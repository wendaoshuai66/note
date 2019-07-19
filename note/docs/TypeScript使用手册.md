#TypeScript使用手册

##TypeScript介绍

TypeScript 是一种由微软开发的自由和开源的编程语言，它是JavaScript的一个超集，扩展了JavaScript的语法，因此现有的JavaScript代码可与 TypeScript 一起工作无需任何修改，TypeScript 通过类型注解提供编译时的静态类型检查。

###TypeScript与JavaScript的比较


TypeScript与JavaScript相比进步的地方包括：加入注释，让编辑器理解所支持的对象和函数，编译器会移除注释，不会增加开销；增加一个完整的类结构，使之更新是传统的对象语言。最大的好处就是加入了类型检查，可以让我们书写代码的时候更加规范。


###5分钟上手TypeScript [摘抄于官网](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

```
npm install -g typescript

```

####构建你的第一个TypeScript文件

在编辑器，将下面的代码输入到greeter.ts文件里：

```
function greeter(person) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```

####编译代码

我们使用了.ts扩展名，但是这段代码仅仅是JavaScript而已。 你可以直接从现有的JavaScript应用里复制/粘贴这段代码。

在命令行上，运行TypeScript编译器：

```
tsc greeter.ts
```
输出结果为一个greeter.js文件，它包含了和输入文件中相同的JavsScript代码。 一切准备就绪，我们可以运行这个使用TypeScript写的JavaScript应用了！

接下来让我们看看TypeScript工具带来的高级功能。 给 person函数的参数添加: string类型注解，如下：

```
function  greeter(person:string){
    return 'hello ' + person;
}
let usr = "Jane User";
document.body.textContent = greeter(usr);
```

###类型注解

TypeScript里的类型注解是一种轻量级的为函数或变量添加约束的方式。 在这个例子里，我们希望 greeter函数接收一个字符串参数。 然后尝试把 greeter的调用改成传入一个数组：

```
function greeter(person: string) {
    return "Hello, " + person;
}

let user = [0, 1, 2];

document.body.innerHTML = greeter(user);
```

重新编译，你会看到产生了一个错误。

![](https://wendaoshuai66.github.io/study/note/images/ts1.png)

类似地，尝试删除greeter调用的所有参数。 TypeScript会告诉你使用了非期望个数的参数调用了这个函数。 在这两种情况中，TypeScript提供了静态的代码分析，它可以分析代码结构和提供的类型注解。

要注意的是尽管有错误，greeter.js文件还是被创建了。 就算你的代码里有错误，你仍然可以使用TypeScript。但在这种情况下，TypeScript会警告你代码可能不会按预期执行。


####接口

让我们开发这个示例应用。这里我们使用接口来描述一个拥有firstName和lastName字段的对象。 在TypeScript里，只在两个类型内部的结构兼容那么这两个类型就是兼容的。 这就允许我们在实现接口时候只要保证包含了接口要求的结构就可以，而不必明确地使用 implements语句。

```
interface Person{
    firstName:string;
    lastName:string;
}
function greeter(person:Person){
    return "hello"+ person.firstName + person.lastName;
}
let user={firstName:'liu',lastName:'shuai'};

document.body.textContent = greeter(user)
```

####类


最后，让我们使用类来改写这个例子。 TypeScript支持JavaScript的新特性，比如支持基于类的面向对象编程。

让我们创建一个Student类，它带有一个构造函数和一些公共字段。 注意类和接口可以一起共作，程序员可以自行决定抽象的级别。

还要注意的是，在构造函数的参数上使用public等同于创建了同名的成员变量。

```
//创建一个Student的类,它带有一个构造函数和一些公共字段
//在构造函数的参数上使用public等同于创建了同名的成员变量。

class Student{
    fullName :String;
    constructor(public firstName,public middleInitial,public lastName){
        this.fullName = firstName + "" +middleInitial+""+lastName;
    }
}

interface Person{
    firstName;
    lastName;
}

function greeter(person:Person){
    return "hello" + person.firstName + person.lastName
}

const user = new Student('liushuai','m.','User')

document.body.textContent = greeter(user)
```

重新运行tsc greeter.ts，你会看到生成的JavaScript代码和原先的一样。 TypeScript里的类只是JavaScript里常用的基于原型面向对象编程的简写。

```
//创建一个Student的类,它带有一个构造函数和一些公共字段
//在构造函数的参数上使用public等同于创建了同名的成员变量。
var Student = /** @class */ (function () {
    function Student(firstName, middleInitial, lastName) {
        this.firstName = firstName;
        this.middleInitial = middleInitial;
        this.lastName = lastName;
        this.fullName = firstName + "" + middleInitial + "" + lastName;
    }
    return Student;
}());
function greeter(person) {
    return "hello" + person.firstName + person.lastName;
}
var user = new Student('liushuai', 'm.', 'User');
document.body.textContent = greeter(user);

```

##使用VSCode编写TypeScript


VSCode可以配置TypeScript的自动编译。

1.全局安装typescript

```
npm i -g typescript

```

2.生成并配置tsconfig.json

```
tsc --init
```

tsconfig.json说明


官网所有配置选项[传送门](https://www.tslang.cn/docs/handbook/compiler-options.html)

```
{//tsconfig.json文件内容
// 指定需要编译的目录
"include": ["src/**/*"],
// 排除指定目录
"exclude": ["node_modules"],
// 编译选项配置
"compilerOptions": {
"outDir": "./output", // 输出目录
"lib": ["dom","es5","es2015"], // 编译过程中需要引入的库文件的列表
"module": "commonjs", // 采用的模块系统
"moduleResolution": "node",
"target": "es5", // 编译输出目标 ES 版本
"strict": true, // 以严格模式解析
"sourceMap": true, // 是否包含可以用于 debug 的 sourceMap
"emitDecoratorMetadata": true, // 启用设计类型元数据（用于反射）
"experimentalDecorators": true, // 启用装饰器
"allowSyntheticDefaultImports": true, // 允许从没有设置默认导出的模块中默认导入
"isolatedModules": false, // 将每个文件作为单独的模块
"esModuleInterop": true, //导入语法 import foo from "foo"
//【最佳实践】true打开的话对应我们要用任何的Library都需要.d.ts即使是空定义，放心可以下载。
"noImplicitAny": false, // 在表达式和声明上有隐含的any类型时报错
"noImplicitReturns": true, // 不是函数的所有返回路径都有返回值时报错。
"importHelpers": true, // 从 tslib 导入外部帮助库: 比如__extends，__rest等
"listFiles": true, // 编译过程中打印文件名
"removeComments": true, // 移除注释
"suppressImplicitAnyIndexErrors": true,
"allowJs": true, // 允许编译javascript文件
"pretty": true, // 给错误和消息设置样式，使用颜色和上下文

"baseUrl": "./", // 解析非相对模块名的基准目录,相当于加了前缀
// 指定特殊模块的路径
"paths": {
"jquery": [
"node_modules/jquery/dist/jquery"
]
},

// 每当编译器在某一rootDirs的子目录下发现了相对模块导入，它就会尝试从每一个rootDirs中导入。
// 例如场景构建中的会将某些文件夹,输出拷贝到同一个目录下
"rootDirs": [
"src/views",
"generated/templates/views"
]

}
}

```

3.监测文件改动并编译

```
tsc -w
```








