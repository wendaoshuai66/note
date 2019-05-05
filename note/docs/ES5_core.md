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



### this谁调用指向谁

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



