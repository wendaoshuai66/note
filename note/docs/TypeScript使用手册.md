#TypeScript使用手册

##TypeScript介绍

TypeScript 是一种由微软开发的自由和开源的编程语言，它是JavaScript的一个超集，扩展了JavaScript的语法，因此现有的JavaScript代码可与 TypeScript 一起工作无需任何修改，TypeScript 通过类型注解提供编译时的静态类型检查。

####TypeScript与JavaScript的比较


TypeScript与JavaScript相比进步的地方包括：加入注释，让编辑器理解所支持的对象和函数，编译器会移除注释，不会增加开销；增加一个完整的类结构，使之更新是传统的对象语言。最大的好处就是加入了类型检查，可以让我们书写代码的时候更加规范。



####5分钟上手TypeScript [摘抄于官网](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

```
npm install -g typescript

```

#####构建你的第一个TypeScript文件

在编辑器，将下面的代码输入到greeter.ts文件里：

```
function greeter(person) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```

#####编译代码

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

####类型注解

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


#####接口

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

#####类


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

###参考
[VS Code 运行 TypeScript 操作指南(转)](https://www.cnblogs.com/yasepix/p/9294491.html)

[vscode下搭建Typescript编译环境](https://www.jianshu.com/p/4f219f20af75)

[使用vscode写typescript（node.js环境）起手式](https://segmentfault.com/a/1190000016305647)

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

##TS基础系列之-基本数据类型

每种语言都会有属于自己的数据类型，ts的基本数据类型基本上是继承了js，但也在基础之上增加了几个不一样的类型

1.布尔型 true／falses

```
//在js中声明boolean型的方法和ts中的不同之处
let flag = true;
let flag: boolean = true;
```

 2.float,数值型（在js和ts中数字型都是float）
 
 ```
 //在js中声明number型的方法和ts中的不同之处
let age = 26;
let age: number = 26;

 ```
 
 3.字符型 Strings
 
 ```
 //在js中声明number型的方法和ts中的不同之处
let name = 'boren';
let name: string = 'boren';
 ```
 
 4.数组 Array
 
 //在js中声明数组的方法和ts中的不同之处
//1.js中声明数组的两种方式
let city = [];
let city = new Array();
//2.ts中声明数组的两种方式
let city: string[] = [];
let city:Array<string> = []
//在ts中声明数组必须提前指定其数据类型，如果其数组中的元素其数据类型不相同，声明的方式会在后面介绍

5.元组 Tuple


元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 string和number类型的元组。

```
//可以定义数组中元素不相同的数据类型
let people = ['boren',26];//js
let people:[string,number];
people = ['boren',26];//ts
```

6.枚举 Enum



enum类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```
//js中没有此方法，都是以object或者json的形式去实现枚举的特性如：
let school = {

     teacherOne : 'Mars',
     teacherTwo:'yupeng',
     teacherThree:'luxuesong'

} 
console.log(school.teacherOne)//Mars

在ECMA2015，简称es5中Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象
Object.getOwnPropertyDescriptor（school,‘teacherOne’）

// {
// value: Mars,
// writable: true,
// enumerable: true,
// configurable: true
// }

其中这么方法打印出来的对象中key enumerable属性，为可枚举性，在js es5中有三个操作会忽略枚举为 false，for...in、 Object...keys()、 JSON.stringify()；在es6中新增一个方法Object.assign()会忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。关于更多js中对枚举属性的支持，在这里就不一一介绍，例如toString()和length等等其枚举属性为false
//ts中枚举类型的用法
enum classMember = {chenchao,rongbin,chenhua,liurui,luxuesong};
let teacher:classMember = classMember.luxuesong

我们也可以给枚举中的成员进行编号等等，以便于更方便的去找到相应的对象元素
```

声明一个枚举类型

```
enum Color {Red,Green,Blue}
```

编译成es5代码是

```
var Color;
(function (Color) {
    Color[Color["Red"] = 0] = "Red";
    Color[Color["Green"] = 1] = "Green";
    Color[Color["Blue"] = 2] = "Blue";
})(Color || (Color = {}));
```

在声明的时候可以对索引进行赋值


```
enum Color {Red = 1,Green = 4,Blue};  //赋值操作：所赋值的值后面会按照赋值索引继续排列
```

获取枚举类型某个值的索引


```
var c : Color = Color.Blue    //获取Blue的索引
```

7.通用数据类型 Any

当不知道变量的类型是什么的时候，就可以使用any类型

```
var notArr : Array<any> = [1,'12',false]
或

let list4:any[] = ['1',4,6,false]
```

8.空值Void

在js中我们其实对void不是那么的陌生，它被认为是一个操作符，这个操作符可以计算表达式但不会返回任何值，在js中常常出现的位置就是在a标签的链接中，我们不想让页面刷新，更不想链接到某些位置只是简简单单的a标签，有时候会调用一个简单的函数，仅此而已，那么我们就会

```
<a href="javascript:void(0)" onclick="people()">点我</a>
```

在ts中，void类型像是与any类型相反，它表示没有任何类型。Void是对函数进行声明的,定义函数的时候，函数是要有返回值类型的，写了返回值类型如果不返回特定类型的值，就会报错。

```
function test1() : number{
    
}
```

上面代码会报错，因为返回值类型为number，没有返回。

使用void声明函数的返回值类型，表示不需要有返回值。

```

function test2() : void {
    
}
```

9.Never

never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。

下面是一些返回never类型的函数：

```

// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

10.类型断言

在ts中类型断言这种方式还是比较有用处的，其相当于js中的类型转换。但是只在编译的时候起作用。并不会改变其数据的本身结构。

两种写法

其一是“尖括号”语法：

```
//类型断言第一种写法

let someValue :any = 'this is strings';

let strLength :number = (<string>someValue).length;
```

另一个为as语法：

```
let someValue :any = 'this is strings';

let strLength :number = (someValue as string).length;
```

两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在TypeScript里使用JSX时，只有 as语法断言是被允许的。


###总结

通过以上的介绍相信对ts的数据类型已经有了大概的了解，其实相对于js,ts的数据类型并没有做什么变更，只是在声明其数据类型的时候必须明确的指定其相应的数据类型，否则代码编译会报错。虽然 ts的文件是xxx.ts 但由于编译过后和js 没有什么大的不同点，能够很好的运行在浏览器端，其中class、public等等函数及模块编译过后的js能后让你更深入的了解js

用let关键字来代替大家所熟悉的JavaScript关键字var。 let关键字是JavaScript的一个新概念，TypeScript实现了它。 很多常见的问题都可以通过使用 let来解决，所以尽可能地使用let来代替var吧。


##TS基础系列之-函数


###介绍

函数是JavaScript应用程序的基础。 它帮助你实现抽象层，模拟类，信息隐藏和模块。 在TypeScript里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义 行为的地方。 TypeScript为JavaScript函数添加了额外的功能，让我们可以更容易地使用。


1.函数的创建

在 ts 中函数创建也氛围两种匿名函数和有命名的函数

```
//函数的创建
//有名字的函数
function people(x){
    return x;
}

//匿名函数
let z=100;
const people1 =function(x){
    return x+z;
} 
const result  = people1(1)
```


2.函数的参数类型和返回值类型

⚠️只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正

⚠️设定了类型之后必须要返回相对应的类型，否则会报错

⚠️如果函数没有返回任何值，也必须指定返回值类型为 void而不能留空

```
//2.函数的参数类型和返回值类型

/**
 * @param {*} x number
 * @param {*} y number
 * return number
 */

 const add = (x:number,y:number):number=>x+y

 const addResult  = add(1,1);

 //书写完整类型
const myadd :(baseValue:number,increment:number)=>number =(x:number,y:number):number=>x+y;
```

3.函数的可选参数和默认参数

⚠️传递给一个函数的参数个数必须与函数期望的参数个数一致，否则会报错

⚠️可选参数用?argname表示，必须跟在必须参数后面

⚠️没有传递参数或传递的值是undefined，这种叫做默认初始化值的参数

⚠️所有必须参数后面的带默认初始化的参数都是可选的，调用时可省略

⚠️带默认值的参数如果出现在必须参数前面，用户必须明确的传入 undefined 值来获得默认值

⚠️当传入的参数个数不固定时，将所有参数收集到一个变量里和 js 中的 arguments 类似，剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个表达方式为（...）

```

//1.参数个数必须与函数期望的参数个数一致，否则会报错

const myadd :(baseValue:number,increment:number)=>number =(x:number,y:number):number=>x+y;
myadd(1)//报错
myadd(1,2)
myadd(1,2,3)//报错


//2.可选参数用?argname表示，必须跟在必须参数后面

const buildName = (firstName:string,lastName?:string)=>firstName+lastName

console.log(buildName('liu'))//liuundefined
console.log(buildName('liu','shuai'))//liushuai


//没有传递参数或传递的值是undefined，这种叫做默认初始化值的参数

const buildName1 = (firstName:string,lastName?:string)=>firstName+lastName

console.log(buildName1('liu'))//liuundefined
console.log(buildName1('liu',undefined))//liuundefined

//所有必须参数后面的带默认初始化的参数都是可选的，调用时可省略

const buildName2 = (firstName:string,lastName='shuai')=>firstName+lastName

console.log(buildName2('liu'))//liushuai
console.log(buildName2('liu','shuai'))//liushuai

//带默认值的参数如果出现在必须参数前面，用户必须明确的传入 undefined 值来获得默认值

const buildName3 = (lastName='shuai',firstName:string)=>firstName+lastName

console.log(buildName3('shuai','liu'))//liushuai
console.log(buildName3(undefined,'shuai'))//shuai shuai



//当传入的参数个数不固定时，将所有参数收集到一个变量里和 js 中的 arguments 类似，剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个表达方式为（...）

const buildName4 = (firstName:string,...otherName:string[]):string=>firstName+otherName.join('')

console.log(buildName4('liu'))//liu
console.log(buildName4('shuai','age','24'))//shuaiage24

```

4.函数的重载

重载允许一个函数接受不同数量或类型的参数时，作出不同的处理

```
//当传入的参数个数不固定时，将所有参数收集到一个变量里和 js 中的 arguments 类似，剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个表达方式为（...）
var buildName4 = function(firstName) {
    var otherName = [];
    for (var _i = 1; _i < arguments.length; _i++) {
        otherName[_i - 1] = arguments[_i];
    }
    return firstName + otherName.join('');
};
console.log(buildName4('liu'));
console.log(buildName4('shuai', 'age', '24'));
// 我们来实现一下通过传入不同的 type 来实现函数的加操作和乘法操作并返回相应的类型
var compute = function(type) {
    var resetData = [];
    for (var _i = 1; _i < arguments.length; _i++) {
        resetData[_i - 1] = arguments[_i];
    }
    if (type === 1) {
        return resetData.reduce(function(a, b) { return a + b; });
    } else if (type === 2) {
        return String(resetData.reduce(function(a, b) { return a * b; }));
    }
};
console.log(compute(1, 3, 4, 5, 6)); // 18
console.log(compute(2, 3, 4, 5, 6)); // '360'
// 通过上面的实现唯一的缺点就是不能明确通过type返回的相对应的计算的值和类型
var compute1 = function(type) {
    var resetData = [];
    for (var _i = 1; _i < arguments.length; _i++) {
        resetData[_i - 1] = arguments[_i];
    }
    return;
};
var compute2 = function(type) {
    var resetData = [];
    for (var _i = 1; _i < arguments.length; _i++) {
        resetData[_i - 1] = arguments[_i];
    }
    return;
};
var compute3 = function(type) {
    var resetData = [];
    for (var _i = 1; _i < arguments.length; _i++) {
        resetData[_i - 1] = arguments[_i];
    }
    if (type === 1) {
        return resetData.reduce(function(a, b) { return a + b; });
    } else if (type === 2) {
        return String(resetData.reduce(function(a, b) { return a * b; }));
    }
};
console.log(compute3(1, 3, 4, 5, 6)); // 18
console.log(compute3(2, 3, 4, 5, 6)); // '360'
// 上例中，我们重复定义了多次函数 compute，前几次都是函数定义，最后一次是函数实现。
```








 










