# Java语言深入

## 三大基本特性（面向对象共有的）

### 封装

封装（Encapsulation）将类的某些信息隐藏在类内部，不允许外部程序直接访问，而是通过**该类提供的方法**来实现对隐藏信息的操作和访问。（保证数据安全）

1. 数据隐藏：类的数据只能通过类提供的接口进行访问，对外部不可见。
2. 接口访问：类的接口是公开的，可以被其他类访问和调用。
3. 安全性：封装可以提高代码的安全性，防止外部的错误操作对类的数据产生影响。
4. 可维护性：封装可以隐藏内部的实现细节，使得类的实现可以更加灵活和易于维护。

### 继承

继承（Inheritance）是指子类可以继承父类的属性和方法，同时也可以扩展父类的功能。继承可以提高代码的重用性和可扩展性，同时也可以使代码更加简洁和易于理解。

1. 代码重用：子类可以继承父类的属性和方法，避免了代码的重复编写。
2. 功能扩展：子类可以扩展父类的功能，使得代码更加灵活和可扩展。
3. 继承层次：多层继承可以形成继承层次结构，使得代码的组织和管理更加清晰。

### 多态

多态（Polymorphism）是指同一个方法可以被不同的对象调用，产生不同的结果。多态可以提高代码的灵活性和可扩展性，同时也可以使代码更加简洁和易于理解。

1. 方法重载：同一个方法名可以定义多个不同的参数列表，产生方法重载。
2. 方法重写：子类可以重写父类的方法，产生方法重写。
3. 向上转型：子类对象可以向上转型为父类对象，使得同一个方法可以被不同的对象调用。
4. 运行时绑定：**方法调用**的具体实现是在运行时动态绑定的，而不是在编译时静态绑定的。（不依据**定义时/声明时的类型**来判断）

```java
//声明的父子类
class Animal{
    public String name="Animal";

    public String getName() {
        System.out.println ("Animal的方法getName被调用");
        return name;
    }
    public void animalSeeName(){
        System.out.println ("animalSeeName方法调用");
        System.out.println (name);
    }
}

class Dog extends Animal{
    public String name="Dog";

    public String getName() {
        System.out.println ("Dog的方法getName被调用");
        return name;
    }
    public void dogSeeName(){
        System.out.println ("dogSeeName方法调用");
        System.out.println (name);
    }
}

//属性的静态绑定
public static void main(String[] args) {
    Animal animal=new Dog();
    Dog dog =new Dog();
    System.out.println ("animal.name: "+animal.name);//在编译时animal绑定的是Animal类，所以绑定的是Animal的name属性。
    System.out.println ("dog.name: "+dog.name);//dog编译时绑定的是Dog类，所以绑定的是Dog的name属性。
}
/***
 * 输出结果
 * animal.name:Animal
 * dog.name:Dog
 * ***/
//方法的动态绑定
public static void main(String[] args) {
    Animal animal=new Dog();
    Dog dog =new Dog();

    animal.getName ();//animal编译时绑定的是Animal类的getName方法，但到了运行时，由于它实际指向是一个Dog对象，所以执行的是Dog类的getName方法。
    dog.getName ();
}

/***
 * 输出结果
 * Dog的方法getName被凋用
 * Dog的方法getName被凋用
 * ***/

```

## 其他特点

### 泛型

Java 泛型（generics）是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制，该机制允许开发者在编译时检测到非法的类型。泛型的本质是参数化类型，也就是说*所操作的数据类型被指定为一个参数*。

### 集合

* 集合就是一个放**数据对象引用**的容器
* 集合类存放的都是对象的引用(类似指针)
* 集合类型主要有3种：Set(集）、List(列表）和Map(映射)。

#### 类型

![集合类型](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ad00c1881fc242cda37aead3e3c7969b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

* Collection（单列集合）
  * List（有序，元素可重复）
    * ArrayList
    * LinkedList
    * *Vector(Old)*（线程安全，效率低）
    * *Stack(Old)*
  * Set（无序，元素唯一）
    * HashSet
    * LinkedHashSet
  * Queue（先进先出）
    * ArrayQueue
    * PriorityQueue
* Map（双列集合，K-V结构）
  * HashMap（哈希表，线程不安全）
  * ConcurrentHashMap（哈希表，线程安全）
  * TreeMap（红黑树）
  * HashTable（哈希表，线程安全）（Old）

#### 接口讲解

* Iterable接口
  * hashNext():是否到集合尾部
  * next():跳转到下一个
* Collection接口
* Queue接口
* Dueue接口
* List接口
* Set接口
* Map接口

#### 实现类

* ArrayList

ArrayList是Java集合框架中的一个类，实现了List接口。它是一种基于动态数组实现的可变长度序列。

* LinkedList

LinkedList是Java集合框架中的一个类，实现了List接口和Deque（双端队列）接口。它是一种基于链表实现的可变长度序列。

* HashSet

HashSet 是基于 HashMap 实现的，HashSet的值存放于HashMap的key上，HashMap的value统一为PRSENT（常量，值为`new Object()`），因此 HashSet 的实现比较简单，相关 HashSet 的操作，基本上都是直接调用底层 HashMap 的相关方法来完成，HashSet 不允许重复的值。

### 工具类

* Collections类：包含了一系列对合格的工具方法

### 多线程

#### 线程和进程的区别

* **程序**：开发写的代码称之为程序。程序就是一堆代码，一组数据和指令集，是一个静态的概念。
* **进程(Process)** ：将程序运行起来，我们称之为进程。进程是执行程序的一次执行过程，它是动态的概念。进程存在生命周期，也就是说程序随着程序的终止而销毁。进程之间是通过TCP/IP端口实现交互的。
* **线程(Thread)** ：线程是进程中的实际运作的单位，是进程的一条流水线，是程序的实际执行者，是最小的执行单位。通常在一个进程中可以包含若干个线程，当然一个进程中至少有一个线程。线程是CPU调度和执行的最小单位。

#### 进程的状态

* **就绪态**：当进程获取出CPU外所有的资源后，只要再获得CPU就能执行程序，这时的状态叫做就绪态。在一个系统中处于就绪态的进程会有多个，通常把这些排成一个队列，这个就叫就绪队列。
* **运行态**：当进程已获得CPU操作权限，正在运行，这个时间就是运行态。在单核系统中，同一个时间只能有一个运行态，多核系统中，会有多个运行态。
* **阻塞态**：正在执行的进程，等待某个事件而无法继续运行时，便被系统剥夺了CPU的操作权限，这时就是阻塞态。引起阻塞的原因有很多，比如：等待I/O操作、被更高的优先级的进程剥夺了CPU权限等。

#### 线程的实现方法

* 继承Thread类
  * 重写`run()`方法，编写线程执行体
  * 实例化对象，调用`start()`方法启动线程
* 实现`Runnable`接口
* 实现`Callable`接口（不常用）

## 各版本语言特性·

### Java8

* Lambda表达式
* Stream流处理
* 支持LocalDate等时间包，以改进原有Date
* 支持Optional来改进Null值的处理
* 优化了HashMap和ConcurrentHashMap

### Java9

* 支持了JShell，既类似node.js, python一样的命令行工具，对待简单的东西，可以直接命令测试
* 改进Javadoc, 使得Javadoc可以搜索
* 支持了List.of(), Set.of(), Map.of()的方式初始化不可变集合，省略了大量代码，语法糖
* 改进的Stream API，比如ofNullable(),dropWhile(),takeWhile()等
* 接口中的private方法不会被外部实现
* try-with-resources语法自动释放资源

### Java10

* 支持了局部变量的类型推导，支持了局部变量的var声明

### Java11

* 简化了启动单个源代码文件的方法，使得小白命令式编译运行Java文件变成更加简单，`java Helloworld.java` 即可
* 重写了HttpClient,提供了新的标准化HttpClient API, 以后不再需要引入apache包的HttpClient或是okhttp就能支持高性能的网络编程
* 支持了在lambda内部使用var声明局部变量

### Java12

* switch 不仅可以作为语句，也可以作为表达式。

### Java13

* switch语句增加`yield`语句用于跳出switch块
* `"""`作为文本块表达式

### Java14

* `instanceof`判断无需类型转换直接进行匹配了
* record类简化数据类型存储（简化实体对象创建）

### Java15

* Sealed Classes密封类，来规范子类的可继承属性或方法
