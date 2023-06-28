# Javascript学习

## 简介

Js最初本身就是一个针对网页的开发语言，并且开发时间很短。所以这个开发语言很烂。非常有用，但是并不美观。并且内容也非常分散不好描述。

## 语法

### 数据类型

Javascript本身就是一门有用动态类型的的语言，同意变量可以用作不同类型。但Javascript本身也是存在基本数据类型的。

**基本类型**

- String
- Number
- Boolean
- Null
- Undefined
- Symbol

**对象类型**

- Object
- Array
- Function
- 以及特殊对象RegExp（正则）和Date（日期）

### 注释

\/\/代表单行注释

\/\* \*\/代表多行注释

```javascript
//这是单行注释

/*
这是多行注释
*/
```

### 变量和常量

#### 命名标准

- 变量必须以字母开头
- 变量也能以 $ 和 _ 符号开头（不推荐）
- 变量名称对大小写敏感

#### 声明变量

在基本的Js中，变量使用**var**关键字进行声明。如果要同时声明多个变量。可以在关键字后使用逗号把不同的变量名隔开。

在ES2015中引入了**let、const**两个新关键字。let和var作用类似，但是let关键字创建的变量作用域在**\{\}**的块内。而var创建的变量在整个文本内。

<br/>

### 运算符

一般运算符（二元）

- 赋值=
- 加法+
- ...

特殊运算符

- 自增++

- 自减--

- 复合运算符+=、-=....

- 级联运算符+、+=（连接字符串）

- ？

- 逻辑运算

- 类型运算

- 位运算

### 函数

#### 函数声明

函数就是包裹在花括号中的代码块，前面使用了关键词 function：

```javascript
function function_name(var argument_1,var argument_2...)
{
  //执行代码
  return argument_1;
  //如果无需返回值则不写return
}
```

#### 函数调用

如果需要调用函数，需要在函数阔海能包含声明的所有参数。

```javascript
function_name(rgument_1,argument_2...);
```

### 对象

在Js中的对象和一般的面向对象语言（Java、CPP）不同。在Js中不需要先创建类，在实例化的方式创建一个对象。而是通过以下类似的语法创建对象。

```javascript
var Person={
  //对象的属性
  name："zms",
  age:69,
  //对象的方法
  movement:function(){
    return "running!"
  }
}

//访问对象的属性
Person.age
//访问对象的方法
Person.movement()
```

#### this关键字

在函数定义中，this 引用该函数的“拥有者”。也就是对象本身，当在对象中需要调用对象本身的一些属性和方法时，就可以使用这个关键字。

```javascript
var Person={
  firstName："zms",
  lastName:"zms",
  introduciton:function() {
  return this.firstName+this.lastName;
}
```

而在某些其他场景，还有别的语义

- 在方法中，this 指的是所有者对象。、
- 在事件中，this 指的是接收事件的元素。（Web）

### 循环

#### for循环

```javascript
for(/*初始化条件*/var i=0;/*结束条件*/var i<5;/*每次循环后执行*/i++)
{
  //循环过程中执行的语句
}
```

#### while循环

```javascript
while(/*保持循环条件*/)
{
  //循环中执行的语句
}
```

#### do/while循环

do/while是while的变体，和while不同的是它会在判断循环条件之前执行一次循环体

```javascript
do
{
//循环中执行的语句
}
while(/*保持循环条件*/)
```

### 条件

#### if、else和else if

只有当指定条件为 true 时，该语句才会执行代码。

```javascript
if(/*判断语句*/)
{
//判断语句条件为真时执行的代码
}
```

如果需要在条件为false的时候执行其他代码，可以使用else作为if的补充

```javascript
if(/*判断语句*/)
{
//判断语句条件为真时执行的代码
}
else
{
//判断语句条件为假时执行的代码
}
```

当有多个条件需要满足时，可以使用else if

```javascript
if(/*判断语句1*/)
{
//判断语句1条件为真时执行的代码
}
else if(/*判断语句*/)
{
//判断语句2条件为真时执行的代码
}
else
{
//以上判断语句条件都为假时执行的代码
}
```

#### switch

switch 语句来选择要执行的多个代码块之一

```javascript
switch(n)
{
    case 1:
        //执行代码块 1
        break;
    case 2:
        //执行代码块 2
        break;
    default:
        //与 case 1 和 case 2 不同时执行的代码
}
```

对switch括号内的表达式n（通常为变量）与case 中的值进行比较，若匹配成功，则该case以下的代码都会被执行。

**注意：**一般开发中，不允许执行其他case的代码，通常会使用`break`关键字作中断处理。

#### break和continue

**break关键字**不光是可以用于中断switch语句，还可以用来中断循环。break 语句跳出循环后，会继续执行该循环之后的代码（如果有的话）

**continue 语句**中断当前的循环中的迭代，然后继续循环下一个迭代。
