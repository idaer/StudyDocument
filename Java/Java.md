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

* Collection
  * List
    * ArrayList
    * LinkedList
    * *Vector(Old)*
    * *Stack(Old)*
  * Set
    * HashSet
    * LinkedHashSet
* Map
  * HashMap
  * TreeMap
  * HashTable

### 工具类

### 多线程

## 各版本语言特性·
