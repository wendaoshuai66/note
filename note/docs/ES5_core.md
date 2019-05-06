# ES5的核心技术

## this

### 为什么要用this

直接上代码

```
 function identify() {
        return this.name.toUpperCase();
      }
      function speak() {
        var greeting = "Hello, I'm " + identify.call( this );
        console.log( greeting)
      }
      var me = {
        name: "Kyle"
      };
      var you = {
        name: "Reader"
      };
      identify.call( me ); // KYLE
      identify.call( you ); // READER
      speak.call( me ); // Hello, 我是 KYLE
      speak.call( you ); // Hello, 我是 READER
      
      如果不用this
      function identify(context) {
       return context.name.toUpperCase();
      }
      function speak(context) {
       var greeting = "Hello, I'm " + identify( context );
       console.log( greeting );
      }
      identify( you ); // READER
      speak( me ); //hello, 我是 KYLE
```

this提供了更优雅的隐式方式’传递‘一个对象的引用，可以将API设计的简洁易用复用

### this 并不像我们所想的那样指向函数本身

我们想要记录一下函数 foo 被调用的次数，思考一下下面的代码:

```
      function foo(num) {
      console.log( "foo: " + num );
      // 记录 foo 被调用的次数
      this.count++;
      }
      foo.count = 0;
      var i;
      for (i=0; i<10; i++) {
        if (i > 5) {
          foo( i );
        }
      }
        foo: 6
        foo: 7
        foo: 8
        foo: 9
      console.log( foo.count );//0---->为什么
      
      解决此的两种方法：
      方法一：
      function foo(num) {
      console.log( "foo: " + num );
      // 记录 foo 被调用的次数
      foo.count++;
      }
      foo.count = 0;
      var i;
      for (i=0; i<10; i++) {
        if (i > 5) {
          foo( i );
        }
      }
        foo: 6
        foo: 7
        foo: 8
        foo: 9
      console.log( foo.count )//4
      方法二：
      function foo(num) {
        console.log( "foo: " + num );
        // 记录 foo 被调用的次数
        this.count++;
      }
      foo.count = 0;
      var i;
      for (i=0; i<10; i++) {
        if (i > 5) {
          foo.call(foo, i );
        }
      }
      console.log(foo.count)
```

执行 foo.count = 0 时，的确向函数对象 foo 添加了一个属性 count。但是函数内部代码this.count 中的 this 并不是指向那个函数对象，所以虽然属性名相同，根对象却并不相同

### this的作用域

this 指向函数的作用域。这个问题有点复杂，因为在某种情况下它是正确的，但是在其他情况下它却是错误的

> *需要明确的是，this 在任何情况下都不指向函数的词法作用域*

### this到底是什么

> *this 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调用时的各种条件。this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。*

### this调用位置

调用位置就是函数在代码中被调用的位置（而不是声明的位置）,大白话谁调用指向谁

#### this谁调用指向谁

```
this.a = 20;
var p={
  a:30,
  test:function(){
  //this--->p
  alert(this.a)
}
}
//s--->window
var s =p.test;
s()//20
接着复杂
this.a = 20;
var p={
  a:30,
  test:function(){
    //this--->p
    alert(this.a)
    function s(){
      this.a=60;
      alert(this.a)
    }
    return s;
  }
}
//s--->window
var s =p.test();
s()//60
如果去掉this.a=60‘
s()//20
```

### this绑定规则--默认绑定

独立函数调用

声明在全局作用域中的变量（比如 var a = 2）就是全局对象的一个同名属性。它们本质上就是同一个东西，并不是通过复制得到的

```
function foo() {
	console.log(this.a)
}
var a = 2;
foo()//2
```

如果使用严格模式

```
function foo() {
"use strict";
console.log( this.a );
}
var a = 2;
foo();// TypeError: this is undefined
```

如果严格模式写成这样

```
function foo() {
console.log( this.a );
}
var a = 2;
(function () {
"use strict";
foo()//2
})()
```

总结：this绑定规则完全取决于调用位置，但是只有foo()运行在非strict mode默认绑定才能绑定到全局对象；严格模式下与 foo()的调用位置无关



### 绑定规则----隐式绑定

调用的位置是否有上下文对象，或者说是否被某个对象所拥有或者包含

```
function foo() {
    console.log( this.a );
}
var obj={
    a:1,
    foo:foo
}
obj.foo();//1
```

```
调用位置会使用 obj 上下文来引用函数
当函数引用有上下文的对象时，隐式绑定规则会把函数调用的this绑定到上下文对象
对象引用链中只有最顶层或者说最后一层会影响调用的位置，废话不多说上代码

function foo() {
          console.log( this.a );
      }
      var obj2={
          a:42,
          foo:foo
      }
      var obj1={
          a:2,
          obj2:obj2
      }
      obj1.obj2.foo();//42
```

一个最常见的this绑定问题就是被隐式绑定的函数会丢失绑定对象，也就是说它会应用默认绑定，从而this绑定到全局对象或者说undefined取决于是否严格模式

```
function foo() {
    console.log(this.a)
}
var obj = {
    a:1,
    foo:foo
}
var s= obj.foo;//函数别名
var a='globle';//a是全局对象的属性
s()//globle
```

传入回调函数 参数传递是隐式赋值

```
 function foo() {
      console.log(this.a)
  }
  function dofoo(fn) {
      fn()
  }
  var obj={
      a:1,
      foo:foo
  }
  dofoo(obj.foo)//undefined
```

把函数传入语言内置的函数而不是传入你自己的函数

```
function foo() {
    //undefined
    console.log(this.a)
}
var obj={
    a:1,
    foo:foo
}
setTimeout(obj.foo,1000)
```

回调函数丢失 this 绑定是非常常见的

### 绑定规则---显示绑定

使用call 和apply --->它们的第一个参数是对象，它们会把这个对象绑定到this上---->称之为显示绑定,上代码

```
function foo() {
     console.log(this)
 }
 // var obj={a:1}
 foo.call(obj)
 通过foo.call,调用foo----->强制把foo的this--》绑定到----》obj
 
 你传入了一个原始值（字符串类型、布尔类型或者数字类型）来当作 this 的绑定对象，这个原始值会被转换成它的对象形式（也就是 new String(..)、new Boolean(..) 或者new Number(..)）。这通常被称为“装箱”
```

#### 硬绑定

```
function foo() {
    console.log(this.a)
}
var obj={
    a:12,
    foo:foo
}
var bar = function () {
    foo.call(obj)
}
   setTimeout(bar,1000)
```

硬绑定典型应用场景就是创建一个包裹函数，传入所有的参数并返回接收到的所有值，上代码

```
function foo(dosomething) {
    console.log(this.a , dosomething)
    return this.a + dosomething
}
var obj = {
    a:121
}
var bar =function () {
    return foo.apply(obj,arguments)
}
var b= bar(3)
console.log(b)
```

另一种方法,创建一个i重复使用的辅助函数

```
function bind(fn,obj) {
   return function () {
       return fn.apply(obj,arguments)
   }
}
var obj = {
    a:121
}
var bar = bind( foo, obj );
var b = bar( 3 );
console.log( b );
bind(..) 会返回一个硬编码的新函数，它会把参数设置为 this 的上下文并调用原始函数
```

#### API调用的‘上下文’

第三方库的许多函数，以及js语言和宿主环境中许多新的内置函数都提供了可选参数，通常被称为‘上下文’，其作用和bind一样

```
// function foo(el) {
//     console.log( el, this.id );
// }
// var obj = {
//     id: "awesome"
// };
// // 调用 foo(..) 时把 this 绑定到 obj
// [1, 2, 3].forEach( foo, obj );
// // 1 awesome 2 awesome 3 awesome
```

### 绑定规则----》new绑定

使用 new 来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。

1. 创建（或者说构造）一个全新的对象。
2. 这个新对象会被执行 [[ 原型 ]] 连接。
3. 这个新对象会绑定到函数调用的 this。
4. 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象

