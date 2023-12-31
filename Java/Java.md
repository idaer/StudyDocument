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

### 深浅拷贝（克隆）详解

克隆是指创建一个对象的副本，使得新创建的对象在内容上与原始对象相同。在编程中，克隆是常用的技术之一，它具有以下几个重要用途和优势：

1. **复制对象**：使用克隆可以创建一个与原始对象相同的新对象，包括对象的属性和状态。这样可以在不影响原始对象的情况下，对新对象进行修改、操作、传递等。这在某些场景下非常有用，可以避免重新创建和初始化一个对象。
2. **隔离性与保护**：通过克隆，可以创建一个独立于原始对象的副本。这样，修改克隆对象时，不会影响到原始对象，从而实现了对象之间的隔离性。这对于多线程环境下的并发操作或者保护重要数据具有重要意义。
3. **性能优化**：有时候，通过克隆对象可以提高程序的性能。在某些场景下，对象的创建和初始化过程可能较为耗时，如果需要多次使用这个对象，通过克隆原始对象可以避免重复的创建和初始化过程，从而提高程序的执行效率。
4. **原型模式**：克隆在设计模式中有一个重要的角色，即原型模式。原型模式通过克隆来创建对象的实例，而不是使用传统的构造函数。这样可以提供更灵活的对象创建方式，并且避免了频繁的子类化。

#### 浅拷贝

* 浅拷贝指在克隆操作中，只复制对象本身以及对象内部的基本数据类型的属性，而不复制对象内部的引用类型的属性。
* 浅拷贝仅仅创建了一个新对象，该对象与原始对象共享对同一引用类型属性的访问。如果原始对象的引用类型属性被修改，浅拷贝的对象也会受到影响。
* 在浅拷贝中，新对象和原始对象指向同一块内存区域，因此对其中一个对象进行修改可能会影响到另一个对象。

浅拷贝**只拷贝最表面的对象**，如果对象内引用了其他对象的话，仍然是使用相同的引用地址。

浅拷贝通过实现了`Cloneable`接口的`clone()`方法

```java
//实现Cloneable接口
class Person implements Cloneable {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    //实现clone方法
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class Main {
    public static void main(String[] args) {
        Person person1 = new Person("Alice", 25);
        try {
            // 浅拷贝
            Person person2 = (Person) person1.clone();

            System.out.println(person1.getName() + " " + person1.getAge()); // Alice 25
            System.out.println(person2.getName() + " " + person2.getAge()); // Alice 25

            person2.setName("Bob");
            person2.setAge(30);

            System.out.println(person1.getName() + " " + person1.getAge()); // Alice 25
            System.out.println(person2.getName() + " " + person2.getAge()); // Bob 30
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}
```



#### 深拷贝

* 深拷贝指在克隆操作中，除了复制对象本身以及对象内部的基本数据类型的属性外，还要递归地复制对象内部的引用类型的属性。即深度克隆了所有引用类型的属性。
* 深拷贝创建了一个完全独立的新对象，该对象与原始对象没有任何关联，对新对象和原始对象的修改互不影响。
* 在深拷贝中，新对象和原始对象分别对应不同的内存区域，它们之间不存在引用关系，因此修改其中一个对象不会影响到另一个对象。

深拷贝会一直递归复制对应的对象，其对象内部的引用对象也会被复制。

深拷贝是通过序列化和反序列化来进行复制的。

```java
import java.io.*;


//实现Serializable接口
class Address implements Serializable {
    private String city;
    private String street;

    public Address(String city, String street) {
        this.city = city;
        this.street = street;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public void setStreet(String street) {
        this.street = street;
    }

    public String getCity() {
        return city;
    }

    public String getStreet() {
        return street;
    }
}
//实现Serializable接口
class Person implements Serializable {
    private String name;
    private int age;
   	//内部引用了其他对象
    private Address address;

    public Person(String name, int age, Address address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public Address getAddress() {
        return address;
    }
}

public class Main {
    public static void main(String[] args) {
        Address address = new Address("City", "Street");
        Person person1 = new Person("Alice", 25, address);

        // 深拷贝！！！
        Person person2 = deepCopy(person1);

        System.out.println(person1.getName() + " " + person1.getAge() + " " + person1.getAddress().getCity()); // Alice 25 City
        System.out.println(person2.getName() + " " + person2.getAge() + " " + person2.getAddress().getCity()); // Alice 25 City

        person2.setName("Bob");
        person2.setAge(30);
        person2.getAddress().setCity("New City");

        System.out.println(person1.getName() + " " + person1.getAge() + " " + person1.getAddress().getCity()); // Alice 25 City
        System.out.println(person2.getName() + " " + person2.getAge() + " " + person2.getAddress().getCity()); // Bob 30 New City
    }

    
    //深拷贝具体实现方法
    public static <T extends Serializable> T deepCopy(T object) {
        try {
            //创建字节输出流
            ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
            //以字节输出流创建对象输出流？？
            ObjectOutputStream objectOutputStream = new ObjectOutputStream(byteArrayOutputStream);
            //写入到内存中
            objectOutputStream.writeObject(object);
            objectOutputStream.flush();
            //创建对象输入流
            ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(byteArrayOutputStream.toByteArray());
            //从内存中读取
            ObjectInputStream objectInputStream = new ObjectInputStream(byteArrayInputStream);
            return (T) objectInputStream.readObject();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

在上述示例中，我们创建了一个 Address 类和一个 Person 类，它们都实现了` Serializable` 接口。通过序列化和反序列化操作，我们可以实现深拷贝。在 `deepCopy()` 方法中，我们使用字节流将对象写入到内存中，并从内存中读取出来，从而得到一个新的独立对象。通过调用 `deepCopy()` 方法，可以得到一个新的对象 `person2`，它与原始对象 `person1` 完全独立。在修改 `person2` 的属性时，不会影响到 `person1`。 值得注意的是，要实现深拷贝，**所有相关的类都需要实现 Serializable 接口**。

### JVM垃圾回收（GC）

#### 什么是垃圾

在JVM里，如果一个对象没有了引用，就会成为垃圾。

~~~ java
public void createUser(){
   User user = new User();
   user.setUserName("张三");
   user.print();
}
~~~

在方法执行时，`User`对象存在引用信息，当方法执行完毕退出时，`User`对象再也不可能被任何对象或类引用到，就会成为垃圾。

#### 怎么发现垃圾

* 引用计数法

> 什么是引用？
>
> 在我们使用一个对象的时候，通常都会创建一个对象，然后通过引用变量进行操作。
>
> 如：test就是个引用，持有Test()对象的引用，可以代表Test()对象本身
>
> ~~~ java
> Test test = new Test();
> ~~~



每一个对象都存在一个引用计数器，作用就是每当这个对象被引用时引用计数器就+1，最后通过判断引用此时来决定对象是否回收。

每当引用被设置为null时，或者取消了引用，那么引用计数器就-1。

如果引用计数器为0时，则代表这个对象不可能被引用了，因为这个时候不可能再有别的办法得到这个对象的引用了。这个对象也就没有什么作用了。

-----

从上边的描述里看起来没有什么问题，应该不会出现什么意外，那么意外就来了。

引用计数法最致命的问题就是，不能解决对象的循环引用，什么意思呢，看下边的代码就知道了。

~~~~ java
public static void main(String[] args){
   Test test1 = new Test();
   Test test2 = new Test();
   
   // 赋值引用 test2引用计数器+1
   test1.otherRefrence = test2;
   // 赋值引用 test1引用计数器+1
   test2.otherRefrence = test1;
   
   // 清空
   test1 = null;
   test2 = null;
   
   // otherRefrence取不到了，但是引用还在，而且test1、test2已经game over了
}


public static class Test{
   // 保存其它对象引用
   public Test otherRefrence;
}
~~~~

以上代码足以说明一个问题，`test1`、`test2`被清除掉之后，但是内部还存在一个引用，此时两个对象的引用计数器都还是1，而且都还拿不到，还不能清除，`GC`也不能回收，时间一久最终就会造成了内存泄漏，发生`OutOfMemoryError`。

* 根搜索算法

也叫可达性分析算法。由一个根节点来判断对象是否存在引用。这个根节点被称为`GC Root`，根节点有几种类型：

- 栈帧创建的局部变量
- 静态变量
- 运行时常量池
- `JNI`指针

这些根节点其实都是指向了堆中的某个对象，层层递进，如果其中一层断掉了，那么就证明这个对象已经可以被回收了，因为已经没有引用可以到自己身上了。

如果栈帧局部变量用完了，或者类静态变量用完了，那么指向堆内存对象实例的指针将销毁。

#### 垃圾回收算法

* **标记清除算法**

这种算法非常简单粗暴，就是将堆中的对象实例进行标记，然后进行删除。

看似实现简单实际上有很多的问题：

- 会造成堆空间的内存不连续，中间出现很多空隙。内存过于碎片化。
- 会造成内存使用率低，如果一个大对象需要3块内存空间存放，但是没有任何一段连续的空间能存放下这个大对象，就会造成OOM错误。

* **复制交换算法**

这种算法需要开辟两块一模一样的内存空间用来存放对象，其实就是新生代的`S0`和`S1`区。

这种算法效率较高，并且对内存的使用率也很高，不会造成内存碎片过多的问题。

最大的问题就是需要创建两块一模一样的空间，想想，如果你的JVM堆设置的是2G，如果都用这种算法，那么JVM就要申请4G空间才能正常运行，问题就是太浪费内存资源，不适合大内存区域使用。

* **标记压缩算法**

这种算法和复制交换算法一样，都不会产生内存碎片，并且对内存空间的占用也要比复制交换节省一半的空间。

缺点就是效率较慢，需要对对象进行排序整理后才能完成垃圾回收。目前被用于`Full GC`。

#### 垃圾收集器

在JVM(<=1.8)里共存在6中内置的垃圾收集器：

- **Serial GC（串行垃圾收集器）**：它是单线程执行垃圾回收的，因此效率较低，如果是用在内存小的JVM里，比如几十mb，那用它是个不错的选择，否则会造成应用程序`STW`时间过长，影响使用。
- **SerialOld GC（串行垃圾收集器）**：它和`Serial GC`的区别是回收的区域不同，主要用于老年代的回收，并且采用了复制整理算法，效率更低。
- **Parallel Scavenge GC（并行垃圾收集器）**：它是并行多线程的垃圾收集器，他可以多个线程同时执行垃圾回收，`SWT`时间长，`GC`次数少，多线程GC，效率很高。
- **ParNew GC(并行垃圾收集器)**：它也是并行收集垃圾的，它和`Parallel Scavenge GC`最主要的区别就是`SWT`时间短，`GC`次数多，适合和用户交互的应用程序。
- **Parallel Old(并行垃圾收集器)**：它是和`ParNew GC`的区别是用作于老年代的垃圾收集，并且采用复制整理算法，效率不如`ParNew GC`，但是效率依然比`SerialOld GC`强N倍
- **Concurrent Mark Sweep GC（CMS垃圾收集器）**：它是与用户线程并行执行的垃圾收集器，超短的`SWT`，适合超大内存使用，缺点是会产生`浮动垃圾`、`标记失败`，会降低系统`CPU`效率，并且会产生大量内存碎片，因为主要使用`标记清除`算法。

### 推荐工具

* arthas（阿里巴巴的Java性能监控工具）

## 各版本语言特性·

### Java 8

* Lambda表达式
* Stream流处理
* 支持LocalDate等时间包，以改进原有Date
* 支持Optional来改进Null值的处理
* 优化了HashMap和ConcurrentHashMap

### Java 9

* 支持了JShell，既类似node.js, python一样的命令行工具，对待简单的东西，可以直接命令测试
* 改进Javadoc, 使得Javadoc可以搜索
* 支持了List.of(), Set.of(), Map.of()的方式初始化不可变集合，省略了大量代码，语法糖
* 改进的Stream API，比如ofNullable(),dropWhile(),takeWhile()等
* 接口中的private方法不会被外部实现
* try-with-resources语法自动释放资源

### Java 10

* 支持了局部变量的类型推导，支持了局部变量的var声明

### Java 11

* 简化了启动单个源代码文件的方法，使得小白命令式编译运行Java文件变成更加简单，`java Helloworld.java` 即可
* 重写了HttpClient,提供了新的标准化HttpClient API, 以后不再需要引入apache包的HttpClient或是okhttp就能支持高性能的网络编程
* 支持了在lambda内部使用var声明局部变量

### Java 12

* switch 不仅可以作为语句，也可以作为表达式。

### Java 13

* switch语句增加`yield`语句用于跳出switch块
* `"""`作为文本块表达式

### Java 14

* `instanceof`判断无需类型转换直接进行匹配了
* record类简化数据类型存储（简化实体对象创建）

### Java 15

* Sealed Classes密封类，来规范子类的可继承属性或方法
