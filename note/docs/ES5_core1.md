# 不知道的JavaScript
##例1
```
if(false){
    var a= 1;
}
console.log(a)//undefined

解释其过程

首先变量提升
var a
if（false）{
	a=1
}
```
###拓展1
```
 function test(){
    console.log(1)
}
var test;//忽略掉test为undefined变量
console.log(test)//function test(){console.log(1)}
```
###拓展2 函数表达式定义的时候函数的名字外部不能被访问，内部不能被修改
```
var p=function test(){
    //只能在函数内部访问
    test =1;
    console.log(typeof test)//function
}
p()
console.log(test)//报错 ，函数表达式外部不能访问test，只能在函数内部访问
```
##例2 this

```
var o={
    foo:function(){
        console.log(11)
    },
    bar(){
        console.log(22)
    }
}
var p1=o.foo.bind({});
new p1();
var p2=o.bar.bind({}) ;//ES6 bind不能被new
new p2();//报错了，p2 is a not construct 
```
###拓展1
```
this.a = 20;
function test(){
    console.log(this.a)
}
var obj ={
    a:40,
}
var result=test.bind(obj)
new result();//undefined  new 会对bind失效

```






