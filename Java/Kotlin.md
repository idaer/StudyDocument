# Kotlin入门

不知道为什么现在才觉得好用的开发语言非常舒服，本身是脚本语言的设置(如动态类型之类的)，但是又可以编译为JVM可以运行的字节码。非常适合快速开发。

## Hello World

```Kotlin
fun main() {
    println("Hello World")
}
```

可以看到，Kotlin反而和C语言在某些方面更相似，省略掉了Java（在快速开发中冗长的）某些部分。是他拥有了类似于脚本语言的性质。但却可以用于某些大型项目的开发（起码Android开发没问题）。

## 语法

```Kotlin
```

### 包声明

```Kotlin
package com.xx.lck

fun main() {
    println("Hello World")
}
```

录与包的结构无需匹配：源代码可以在文件系统的任意位置。也就意味着， **package只是声明了逻辑位置，而不是文件结构位置。** 你可以在Kotlin中通过`com.xx.lck.main()`来调用main方法。但main方法不是真的放在`com/xx/lck`中

### 主函数

main函数作为Kotlin的函数入口

```Kotlin
fun main() {
    println("Hello World")
}
```

也可以将主函数加上参数（从运行前获得的参数）

```Kotlin
fun main(args: Array<String>) {
    println(args)
}
```

#### 默认导入

有多个包会默认导入到每个 Kotlin 文件中：

* kotlin.*
* kotlin.annotation.*
* kotlin.collections.*
* kotlin.comparisons.*
* kotlin.io.*
* kotlin.ranges.*
* kotlin.sequences.*
* kotlin.text.*

### 函数

Kotlin 函数使用 fun 关键字声明：

```Kotlin
fun nofun(a:Int, b:Int) {
    return a+b
}
```

#### 参数

函数参数使用 Pascal 表示法定义——`name: type`。参数用逗号隔开， 每个参数必须有显式类
型。函数参数可以有默认值，当省略相应的参数时使用默认值。这可以减少重载数量：

```Kotlin
fun read(
//参数
b: ByteArray,
//参数默认值
off: Int = 0,
len: Int = b.size,
)
 { /*函数体*/ }

```

### 基本类型

在Java中，*参与运行的变量是不能为空的，否则就会产生NullPoiontException* [^buyin] ，对于所有基本类型而言，可以使用数据类型后`?`将参数类型标记为可空

#### 基本数据类型

在Kotlin中虽然可以创建动态数据类型的变量，但是也存在固定数据类型。不同于 Java 的是，字符不属于数值类型，是一个独立的数据类型。

* Double（64）
* Float（32）
* Long（64）
* Int（32）
* Short（16）
* Byte（8）

##### 常量表示

1. 十进制：123
2. 长整型以大写的 L 结尾：123L
3. 16 进制以 0x 开头：0x0F
4. 2 进制以 0b 开头：0b00001011
5. 浮点数默认Double：123.5, 123.5e10
6. Float使用 f 或者 F 后缀：123.5f

你可以使用*下划线*<kbd>_</kbd> 使数字常量更易读

```Kotlin
//十进制
val oneMillion = 1_000_000
//长整型
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
//16 进制
val hexBytes = 0xFF_EC_DE_5E
//2 进制
val bytes = 0b11010010_01101001_10010100_10010010
```

✳注意：8进制不支持

##### 数值比较

Kotlin 中没有基础数据类型，只有封装的数字类型，你每定义的一个变量，其实 Kotlin 帮你封装了一个对象，这样可以保证不会出现空指针。数字类型也一样，所以在比较两个数字的时候，就有比较数据大小和比较两个对象是否相同的区别了。

在 Kotlin 中，三个等号 === 表示比较对象地址，两个 == 表示比较两个值大小。

```Kotlin
fun main(args: Array<String>) {
    val a: Int = 10000
    println(a === a) // true，值相等，对象地址相等

    //经过了装箱，创建了两个不同的对象
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a

    //虽然经过了装箱，但是值是相等的，都是10000
    println(boxedA === anotherBoxedA) //  false，值相等，对象地址不一样
    println(boxedA == anotherBoxedA) // true，值相等
}
```

#### 布尔

Boolean 类型表示可以有 true 与 false 两个值的布尔对象。Boolean 的可空版 Boolean? 还有 null 值。

布尔值的内置运算有：

* `&&` 逻辑与
* `||` 逻辑或
* `!` 逻辑非

#### 字符

字符用 Char 类型表示。 字符字面值用单引号括起来: '1' 。编码其他字符要用 Unicode 转义序列语法： `\uFF00` 。

##### 转义字符

* `\t`——制表符
* `\b`——退格符
* `\n`——换行(LF)
* `\r`——回车(CR)
* `\'`——\'
* `\"`——\"
* `\\`——\\
* `\$`——\$

```Kotlin
fun main() {
    //sampleStart
    val aChar: Char = 'a'
    println(aChar)
    println('\n') // 输出一个额外的换行符
    println('\uFF00')
    //sampleEnd
}
```

#### 字符串

Kotlin 中字符串用 String 类型表示。 通常，字符串值是双引号（ " ）中的字符序列

```Kotlin
val str="123 asd"
```

字符串的元素——字符可以使用索引运算符访问: s[i] 。 可以使用 for 循环遍历这些字
符：

```Kotlin
fun main() {
    val str = "abcd"
    //sampleStart
    for (c in str) {
        println(c)
    }
    //sampleEnd
}
```

#### 数组
