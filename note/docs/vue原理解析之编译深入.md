#vue 原理解析之编译深入

##编译入口

在之前分析数据驱动的时候，在 $mount 这个函数内调用 compileToFunctions 吗？下面我们再来看看这个方法

```
 const { render, staticRenderFns } = compileToFunctions(template, {
    shouldDecodeNewlines,
    shouldDecodeNewlinesForHref,
    delimiters: options.delimiters,
    comments: options.comments
  }, this)
  options.render = render
  options.staticRenderFns = staticRenderFns
```


compileToFunctions 方法就是把模板 template 编译生成 render 以及 staticRenderFns，它的定义在 src/platforms/web/compiler/index.js 中：

```
import { baseOptions } from './options'
import { createCompiler } from 'compiler/index'

const { compile, compileToFunctions } = createCompiler(baseOptions)

export { compile, compileToFunctions }
```

可以看到 compileToFunctions 方法实际上是 createCompiler 方法的返回值，该方法接收一个编译配置参数，接下来我们来看一下 createCompiler 方法的定义，在 src/compiler/index.js 中：

```
//createCompilerCreator 允许创建使用alternative 的编译器
//parser/optimizer/codegen, e.g the SSR optimizing compiler.
// 这里我们只是使用默认部分导出一个默认编译器

export const createCompiler = createCompilerCreator(function baseCompile (
  template: string,
  options: CompilerOptions
): CompiledResult {
  const ast = parse(template.trim(), options)
  if (options.optimize !== false) {
    optimize(ast, options)
  }
  const code = generate(ast, options)
  return {
    ast,
    render: code.render,
    staticRenderFns: code.staticRenderFns
  }
})
```
通过上述代码可以看出createCompiler 就是createCompilerCreator（传入一个baseCompile函数），接下来看看createCompilerCreator是一个什么呢：

```
export function createCompilerCreator (baseCompile: Function): Function {
  return function createCompiler (baseOptions: CompilerOptions) {
    function compile (
      template: string,
      options?: CompilerOptions
    ): CompiledResult {
      const finalOptions = Object.create(baseOptions)
      const errors = []
      const tips = []

      let warn = (msg, range, tip) => {
        (tip ? tips : errors).push(msg)
      }

      if (options) {
        if (process.env.NODE_ENV !== 'production' && options.outputSourceRange) {
          // $flow-disable-line
          const leadingSpaceLength = template.match(/^\s*/)[0].length

          warn = (msg, range, tip) => {
            const data: WarningMessage = { msg }
            if (range) {
              if (range.start != null) {
                data.start = range.start + leadingSpaceLength
              }
              if (range.end != null) {
                data.end = range.end + leadingSpaceLength
              }
            }
            (tip ? tips : errors).push(data)
          }
        }
        // merge custom modules
        if (options.modules) {
          finalOptions.modules =
            (baseOptions.modules || []).concat(options.modules)
        }
        // merge custom directives
        if (options.directives) {
          finalOptions.directives = extend(
            Object.create(baseOptions.directives || null),
            options.directives
          )
        }
        // copy other options
        for (const key in options) {
          if (key !== 'modules' && key !== 'directives') {
            finalOptions[key] = options[key]
          }
        }
      }

      finalOptions.warn = warn

      const compiled = baseCompile(template.trim(), finalOptions)
      if (process.env.NODE_ENV !== 'production') {
        detectErrors(compiled.ast, warn)
      }
      compiled.errors = errors
      compiled.tips = tips
      return compiled
    }

    return {
      compile,
      compileToFunctions: createCompileToFunctionFn(compile)
    }
  }
}
```

从上述代码中可以看出是一个柯里化函数

createCompilerCreator() =》createCompiler  =》{
      compile:function(){},
      compileToFunctions: createCompileToFunctionFn(compile)
    }
    
    
最终返回的是一个对象，该对象属性 compileToFunctions对应的就是 $mount 函数调用的 compileToFunctions 方法，它是调用 createCompileToFunctionFn 方法的返回值，我们接下来看一下 createCompileToFunctionFn 方法，它的定义在 src/compiler/to-function/js 中：


```
export function createCompileToFunctionFn (compile: Function): Function {
  const cache = Object.create(null)

  return function compileToFunctions (
    template: string,
    options?: CompilerOptions,
    vm?: Component
  ): CompiledFunctionResult {
    options = extend({}, options)
    const warn = options.warn || baseWarn
    delete options.warn

    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production') {
      // detect possible CSP restriction
      try {
        new Function('return 1')
      } catch (e) {
        if (e.toString().match(/unsafe-eval|CSP/)) {
          warn(
            'It seems you are using the standalone build of Vue.js in an ' +
            'environment with Content Security Policy that prohibits unsafe-eval. ' +
            'The template compiler cannot work in this environment. Consider ' +
            'relaxing the policy to allow unsafe-eval or pre-compiling your ' +
            'templates into render functions.'
          )
        }
      }
    }

    // check cache
    const key = options.delimiters
      ? String(options.delimiters) + template
      : template
    if (cache[key]) {
      return cache[key]
    }

    // compile
    const compiled = compile(template, options)

    // check compilation errors/tips
    if (process.env.NODE_ENV !== 'production') {
      if (compiled.errors && compiled.errors.length) {
        if (options.outputSourceRange) {
          compiled.errors.forEach(e => {
            warn(
              `Error compiling template:\n\n${e.msg}\n\n` +
              generateCodeFrame(template, e.start, e.end),
              vm
            )
          })
        } else {
          warn(
            `Error compiling template:\n\n${template}\n\n` +
            compiled.errors.map(e => `- ${e}`).join('\n') + '\n',
            vm
          )
        }
      }
      if (compiled.tips && compiled.tips.length) {
        if (options.outputSourceRange) {
          compiled.tips.forEach(e => tip(e.msg, vm))
        } else {
          compiled.tips.forEach(msg => tip(msg, vm))
        }
      }
    }

    // turn code into functions
    const res = {}
    const fnGenErrors = []
    res.render = createFunction(compiled.render, fnGenErrors)
    res.staticRenderFns = compiled.staticRenderFns.map(code => {
      return createFunction(code, fnGenErrors)
    })

    // check function generation errors.
    // this should only happen if there is a bug in the compiler itself.
    // mostly for codegen development use
    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production') {
      if ((!compiled.errors || !compiled.errors.length) && fnGenErrors.length) {
        warn(
          `Failed to generate render function:\n\n` +
          fnGenErrors.map(({ err, code }) => `${err.toString()} in\n\n${code}\n`).join('\n'),
          vm
        )
      }
    }

    return (cache[key] = res)
  }
}
```   

至此总算找到了 compileToFunctions 的最终定义，它接收 3 个参数、编译模板 template，编译配置 options 和 Vue 实例 vm。核心的编译过程就一行代码：

```
 const compiled = compile(template, options)
```

compile到底是什么呢，来屡屡逻辑


刚开始 createCompilerCreator（传入baseCompile函数）=》到createCompilerCreator 返回对象 {
      compile:function(){},
      compileToFunctions: createCompileToFunctionFn(compile)
    }====》找到compile ====》const compiled = baseCompile(template.trim(), finalOptions)
    
    
    
compile 函数执行的逻辑是先处理配置参数，真正执行编译过程就一行代码：


```
const compiled = baseCompile(template, finalOptions)
```

baseCompile 在执行 createCompilerCreator 方法时作为参数传入，如下：

```
export const createCompiler = createCompilerCreator(function baseCompile (
  template: string,
  options: CompilerOptions
): CompiledResult {
  const ast = parse(template.trim(), options)
  optimize(ast, options)
  const code = generate(ast, options)
  return {
    ast,
    render: code.render,
    staticRenderFns: code.staticRenderFns
  }
})
```

所以编译的入口我们终于找到了，它主要就是执行了如下几个逻辑:

 const ast = parse(template.trim(), options) ===>解析模板字符串生成 AST
 
 
optimize(ast, options). =====>优化语法树

const code = generate(ast, options) =====》生成代码 