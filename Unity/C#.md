# C#语言教程

---

## 一、C#简介

C# 是一个现代的、通用的、面向对象的编程语言，它是由微软（Microsoft）开发的，由 Ecma 和 ISO 核准认可的。我只是用来学习Unity开发而已。

### C#的功能

虽然 C# 的构想十分接近于传统高级语言 C 和 C++，是一门面向对象的编程语言，但是它与 Java 非常相似，有许多强大的编程功能，因此得到广大程序员的青睐。

下面列出 C# 一些重要的功能：

* 布尔条件（Boolean Conditions）
* 自动垃圾回收（Automatic Garbage Collection）
* 标准库（Standard Library）
* 组件版本（Assembly Versioning）
* 属性（Properties）和事件（Events）
* 委托（Delegates）和事件管理（Events Management）
* 易于使用的泛型（Generics）
* 索引器（Indexers）
* 条件编译（Conditional Compilation）
* 简单的多线程（Multithreading）
* LINQ 和 Lambda 表达式
* 集成 Windows

## 二、C#程序结构

---

~~~ C#
using System;
namespace HelloWorldApplication//命名空间声明
{
   class HelloWorld//声明类
   {
      static void Main(string[] args)//主函数
      {
         /* 我的第一个 C# 程序*/ //注释
         Console.WriteLine("Hello World"); //语句
         Console.ReadKey();
      }
   }
}
~~~

一、二行都是用来对命名空间进行声明，命名空间HelloWorldApplication中包含了HelloWorld类