# Php与MySql开发入门中

## PHP 面向对象的介绍

### 概念

在面向对象的程序设计（英语：Object-oriented programming，缩写：OOP）中，对象是一个由信息及对信息进行处理的描述所组成的整体，是对现实世界的抽象。

OOP达到了软件工程的三个目标：重用性 灵活性 扩展性

OOP特点：封装 继承 多态

### 面向对象过程（OOP）的内容 

- **类** − 定义了一件事物的抽象特点。类的定义包含了数据的形式以及对数据的操作。
- **对象** − 是类的实例。
- **成员变量** − 定义在类内部的变量。该变量的值对外是不可见的，但是可以通过成员函数访问，在类被实例化为对象后，该变量即可称为对象的属性。
- **成员函数** − 定义在类的内部，可用于访问对象的数据。
- **继承** − 继承性是子类自动共享父类数据结构和方法的机制，这是类之间的一种关系。在定义和实现一个类的时候，可以在一个已经存在的类的基础之上来进行，把这个已经存在的类所定义的内容作为自己的内容，并加入若干新的内容。
- **父类** − 一个类被其他类继承，可将该类称为父类，或基类，或超类。
- **子类** − 一个类继承其他类称为子类，也可称为派生类。
- **多态** − 多态性是指相同的操作或函数、过程可作用于多种类型的对象上并获得不同的结果。不同的对象，收到同一消息可以产生不同的结果，这种现象称为多态性。
- **重载** − 简单说，就是函数或者方法有同样的名称，但是参数列表不相同的情形，这样的同名不同参数的函数或者方法之间，互相称之为重载函数或者方法。
- **抽象性** − 抽象性是指将具有一致的数据结构（属性）和行为（操作）的对象抽象成类。一个类就是这样一种抽象，它反映了与应用有关的重要性质，而忽略其他一些无关内容。任何类的划分都是主观的，但必须与具体的应用有关。
- **封装** − 封装是指将现实世界中存在的某个客体的属性与行为绑定在一起，并放置在一个逻辑单元内。
- **构造函数** − 主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中。
- **析构函数** − 析构函数(destructor) 与构造函数相反，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。析构函数往往用来做"清理善后" 的工作（例如在建立对象时用new开辟了一片内存空间，应在退出前在析构函数中用delete释放）

### 如何抽象一个类

类的声名

成员属性

成员方法

## 构造方法与析构方法

```
class Person{
    public function __construct($name,$age)
    {
        //当这个类new的时候自动执行
        echo 'hello'.$name;
        echo '<hr/>';
        $this->name = $name;
        $this->age =$age;
    }
    public function data(){
        return $this->age;
    }
    public function __destruct()
    {
        echo 'bye'.$this->name;
        echo '<hr/>';
        // 可以进行资源的释放的操作 例如数据库关闭
        //对象被销毁的执行，没有代码被运行了
    }
}
new Person('first',11);
new Person('second',12);
```

## PHP 魔术方法

### 使用__get可以访问私有属性

```
class Person{
    public $name = 'ls';//共有的
    private $age=23;//私有的
    protected $money =10;//受保护的
    //私有的成员方法，不能再类的外部直接访问
    private function getAge(){
        return $this->age;
    }
    //受保护的成员方法，不能再类的外部直接访问
    protected function getMoney(){
        return $this->money;
    }
    //共有的成员方法
    public function userCard(){
        return '我的钱有'.$this->getMoney().'我的年龄是'.$this->getAge();
    }
     //使用__get可以访问私有属性
    public function __get($name)
    {
        if($name==='age'){
            return '改值不能访问';
        }
    }

}
 $xw = new Person();
echo $xw->age;
```

### __set()：给类的私有属性赋值时调用，传递需设置的属性名。属性值

```
class Person{
    public $name = 'ls';//共有的
    private $age=23;//私有的
    protected $money =10;//受保护的
    //私有的成员方法，不能再类的外部直接访问
    private function getAge(){
        return $this->age;
    }
    //受保护的成员方法，不能再类的外部直接访问
    protected function getMoney(){
        return $this->money;
    }
    //共有的成员方法
    public function userCard(){
        return '我的钱有'.$this->getMoney().'我的年龄是'.$this->getAge();
    }
   //__set()：给类的私有属性赋值时调用，传递需设置的属性名。属性值
    public function __set($name, $value)
    {
        if($name === 'age' && $value === 23){
            $this->age = 30;
        }
    }

}
 $xw = new Person();
$xw->age =23;

echo $xw->userCard();
```

### __isset()：和 __unset()

__isset()：检测对象私有属性时调用，传递检测的属性名，返回isset($this->属性名)。

__unset()：使用unset函数删除对象的私有属性时调用，传递删除的属性名。方法中执行unset($this->属性名)。

### _toString()：使用echo打印对象时调用，返回打印对象时想要显示的内容，返回必须是字符串

```
class Player{
    private $name;
    function __construct( $name ){
        $this->name = $name;
    }

    function __toString(){
        //__toString方法必须加一个return
        return $this->name;
    }
}

$player_1 = new Player( "kobe" );
echo $player_1;
```

### __call()：调用一个类中未定义的方法或者私有、受保护的方法时自动调用__call函数。传递被调用的函数名及参数列表。

```

class Player{
    public $name;
    function __construct( $name ){
        echo "构造函数<br>";
        $this->name = $name;
    }
    private function getName(){
        return $this->$name;
    }

    function __call( $funcName, $funcParams ){
        echo "调用的函数是$funcName, 参数列表是：";
        print_r( $funcParams );
    }
}

$player_1 = new Player( "kobe" );
echo $player_1->setName( "james" );
echo "<br>";
echo $player_1->getName();
echo "<br>";
```

**__clone()：当使用clone关键字克隆一个对象时自动调用，作用是为新克隆的对象初始化赋值**

### _sleep()：对象序列化时自动调用，返回一个数组，数组中的值就是可以序列化的属性。可以定义serialize()序列化时返回的数据

### __wakeup()：对象反序列化时调用，为反序列化新产生的对象进行初始化赋值

### serialize()序列化 unserialize()反序列化

为了传输方便，可以把对象转化程二进制，等到达另一端的时候，再还原成原来的对象。
1、一个对象再网络中传输的时候需要将对象串形化 2、把对象写入文件或者数据库的时候用到串形化

### autolaod()自动加载 可以理解程按需加载

autoload()是专门为很多类不存在设计的，很多框架利用这个实现了类文件的自动加载

```
function __autoload($classname)
{
    require_once $classname . 'php';
}

//当Myclass1不存在的时候，自动调用__autoload()函数，传入参数Myclass1;
$obj1 = new Myclass1();
$obj2 = new Myclass2();
```

## PHP 面向对象的继承和多态

子类中重载父类的方法：在子类中允许重写（覆盖）父类中的方法，在子类中，使用parent访问父类中被覆盖的属性和方法

例如：

```
parent::construct()
parent::fun()
```

重载：方法名一样根据传递的参数不一样调用。

```
class Person{
    public $name;//外部能访问，子类能继承
    private $age;//外部不能访问，子类不能继承
    protected $money;//外部不能访问，子类能继承
    public function __construct($name,$age,$money)
    {
        $this->name=$name;
        $this->age=$age;
        $this->money = $money;
    }
    public function getCard(){
        echo '名字'.$this->name.'年龄'.$this->age.'钱'.$this->money;
    }
}
class Yellow extends Person{
    //重写
    public function __construct($name, $age, $money)
    {
        //重载
        parent::__construct($name, $age, $money);
    }
    //重写
    public function getCard(){
        echo $this->money;//能够继承父类被保护的属性
        //重载
        parent::getCard();
    }
}
$s= new Yellow('xiaowang',18,10);
$s->getCard();
```



## PHP 抽象类与接口

### 抽象方法

我们在类里面定义的没有方法提的方法就是抽象方法。所谓的没有方法体指的是，在声明的时候没有大括号以及其中的内容，而是直接在声明时在方法名后加上分号结束，另外在声明抽象方法时方法还要加一个关键字"abstract"来修饰。

例如：

```
abstract function fun1(); 
abstract function fun2(); 
```

### 抽象类

只要一个类里面有一个方法是抽象方法，那么这个类就定义为抽象类，抽象类也要使用“abstract”关键字来修饰；在抽象类里面可以有不是抽象的方法和成员属性，但只要有一个方法是抽象的方法，这个类就必须声明为抽象类，使用“abstract”修饰。

抽像类的特点：不能实例化的，也就是不能new成对象；若想使用类，就必须定义一个类继承这个抽象类，并定义覆盖父类的抽象方法(实现抽象方法)。

例如：

```
abstract class demo{
var $test;
abstract function fun1();
abstract function fun2();
}
```

1.含有抽象方法的类必须是抽象类。

2.抽象类不一定非的含有抽象方法。

3.抽象类可以含有普通方法。

4.抽象类不能被实例化，必须有一个子类继承并且把抽象类的抽象方法实现。

```
abstract class Person{
    //抽象类中可以有普通方法
    public function eat(){
        echo '吃';
    }
    //抽象方法 没有方法体
    public abstract function run();
}
//抽象类必须有一个子类继承，并且实现抽象类中的抽象方法
class Man extends Person{
    function __construct()
    {
    }
    //抽象类中抽象方法必须在子类中实现
    public function run()
    {
        // TODO: Implement run() method.
        echo 'run';
    }
}
$man =new Man();
$man->eat();
$man->run();
```

### 接口

1.接口的关键字是interface

2.接口可以声明常量也可以抽象一些方法。

3.接口中的方法都是抽象方法，不用abstract定义

4.接口不能被实例化，需要一个类去实现它。

5.一个类不能继承多个类，一个类可以实现多个接口

```
interface Person{
    //接口中可以声明常量，也可以抽象方法，抽象方法不用abstract去定义
    const name = "xiaowang";
    public function run();
    public function eat();
}
interface Study {
    public function study();
}
//一个类不可以继承多个类，但是一个类可以实现多个接口，并且接口中的抽象方法必须有子类实现
class Student implements Person,Study{
    const data = 3.14;
    function run()
    {
        // TODO: Implement run() method.
        echo 'run';
    }
    function eat()
    {
        // TODO: Implement eat() method.
        echo 'eat';
    }
    function study()
    {
        echo 'study';
    }
    function test(){
        return self::data;
    }
    //静态的方法
    static function test1(){
        return self::data;
    }
}
$xw = new Student();
echo $xw::name;
echo $xw->test();
echo $xw::test1();
```









