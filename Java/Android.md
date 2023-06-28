# Android 开发（使用Android studio）

## 一.对于Hello World项目结构说明

建议图床

![image-20220313191157226](C:\Users\idear\AppData\Roaming\Typora\typora-user-images\image-20220313191157226.png)

### 1.Project目录

| **文件/文件夹名**      | **作用**          |
|:----------------:|:---------------:|
| build.gradle     | Gradle环境搭建的配置文件 |
| app              | 项目的代码、资源等       |
| local.properties | 指定android的SDK路径 |
| setting.gradle   | 指定项目中引入的模块      |

### 2.app文件夹内

| **文件/文件夹名** | **作用**          |
|:-----------:|:---------------:|
| Build       | 编译时自动生成的文件      |
| libs        | 存放项目中使用的第三方jar包 |
| src         | 存放项目的源码和资源      |

## 二.控件

显示文本信息

```xml
<!--
id:指定id
layout_widht/height:布局内的宽高
text：文本内容
gravity：排列方式
-->
<TextView

          android:id=""
          android:layout_width=""
          android:layout_height=""
          android:text=""
          android:gravity=""/>
```

### 2.Button

按钮控件

### 3.EditText

类似html的input输入框

### 4.ImageView

插入图片文件的控件

### 5.RadioButton

单选的选项按钮

### 6.CheckBox

多选的选择按钮

## 三.布局

### 布局的编写方式

通常有xml文件代码编写和图形化界面编辑

### 1.相对布局

```xml
标签：<RelativeLayout></RelativeLayout>
特点：通过控件之间的相对位置摆放控件
属性：
    1.同级控件之间
<!-- 相对于同级控件之间的位置属性 -->
<!--
android：layout_above=""

                -->
    2.父子级控件之间
<!--
android：centerParent=""

                -->
```

### 2.线性布局

### 3.表格布局

### 4.帧布局

### 5.样式shape

### 6.样式selector

## 四.Activity

### 1.Activity的创建

### 2.Activity之间的信息传递

### 3.Activity的声明周期

## 五.UI进阶

## 六.广播

## 七.服务
