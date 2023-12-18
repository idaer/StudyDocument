# Vue.js学习

---

## 一.创建项目

### 1.vue create

```shell
vue create [options] <project-name>
```

options选项有：

* **-p, --preset \<presetName\>**： 忽略提示符并使用已保存的或远程的预设选项
* **-d, --default**： 忽略提示符并使用默认预设选项
* **-i, --inlinePreset \<json\>**： 忽略提示符并使用内联的 JSON 字符串预设选项
* **-m, --packageManager \<command>**： 在安装依赖时使用指定的 npm 客户端
* **-r, --registry \<url>**： 在安装依赖时使用指定的 npm registry
* **-g, --git [message]**： 强制 / 跳过 git 初始化，并可选的指定初始化提交信息
* **-n, --no-git**： 跳过 git 初始化
* **-f, --force**： 覆写目标目录可能存在的配置
* **-c, --clone**： 使用 git clone 获取远程预设选项
* **-x, --proxy**： 使用指定的代理创建项目
* **-b, --bare**： 创建项目时省略默认组件中的新手指导信息
* **-h, --help**： 输出使用帮助信息

接下来就会出现安装选项界面：

```shell
Vue CLI v4.4.6
? Please pick a preset: (Use arrow keys)
❯ default (babel, eslint)
  Manually select features
```

然后在项目创建文件夹内命令行输入：

```shell
npm run serve
```

打开**http://localhost:8080/** ，就可以看到应用界面了 

### 2.vue ui

```shell
$ vue ui
&#x1f680;  Starting GUI...
&#x1f320;  Ready on http://localhost:8000
...
```

执行以上命令会在浏览器打开vue的ui界面，可以在ui界面执行创建项目的操作

![img](https://www.runoob.com/wp-content/uploads/2021/12/6C6FBF13-54BF-4DBC-8019-6442A51C03F3.jpg)

## 二.Vue项目文件结构

| 文件夹/文件       | **说明**                                                                                                                                                                              |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| build        | 项目构建(webpack)相关代码                                                                                                                                                                   |
| config       | 配置目录，包括端口号等。我们初学可以使用默认的。                                                                                                                                                            |
| node_modules | npm 加载的项目依赖模块                                                                                                                                                                       |
| src          | 这里是我们要**开发的目录**，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：assets: 放置一些图片，如logo等。components: 目录里面放了一个组件文件，可以不用。App.vue: 项目入口文件，我们也可以直接将组件写这里，而不使用 components 目录。main.js: 项目的核心文件。index.css: 样式文件。 |
| static       | 静态资源目录，如图片、字体等。                                                                                                                                                                     |
| public       | 公共资源目录。                                                                                                                                                                             |
| test         | 初始测试目录，可删除                                                                                                                                                                          |
| .xxxx文件      | 这些是一些配置文件，包括语法配置，git配置等。                                                                                                                                                            |
| index.html   | 首页入口文件，你可以添加一些 meta 信息或统计代码啥的。                                                                                                                                                      |
| package.json | 项目配置文件。                                                                                                                                                                             |
| README.md    | 项目的说明文档，markdown 格式                                                                                                                                                                 |

## 三. \.Vue文件代码结构分析

```html
<!-- 展示模板 -->
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" />
</template>
<!-- Vue 代码 -->
<script>
/* 从 src/components/HelloWorld.vue 中引入 HelloWorld 组件 */
import HelloWorld from './components/HelloWorld.vue'

//作为组件导出时的参数
export default {
  //作为组件的名字
  name: 'App',
  //该组件所使用的组件
  components: {
    HelloWorld
  }
}
</script>
```

其中，<kbd>\<template\>\</template\></kbd>之中为html相应内容，而<kbd>\<script\>\</script\></kbd>之中为vue的相应代码。

### 1.关于data选项（方法）

**data 选项**是一个函数。Vue 在创建新组件实例的过程中调用此函数。它应该返回一个对象，然后 Vue 会通过响应性系统将其包裹起来，并以 $data 的形式存储在组件实例中。

在\<script\>中：

```javascript
const app = Vue.createApp({
  //在data方法内定义变量
  data() {
    return { 
        //此处定义变量，将会返回到html中
        count: 4 
    }
  }
})
const vm = app.mount('#app')
```

### 3.方法

我们可以在组件中添加方法，使用 **methods** 选项，该选项包含了所需方法的对象。

```javascript
const app = Vue.createApp({
  data() {
    return { count: 4 }
  },
  methods: {
    increment() {
      // `this` 指向该组件实例
      this.count++
    }
  }
})
const vm = app.mount('#app')

document.write(vm.count) // => 4
document.write("<br>")
vm.increment()

document.write(vm.count) // => 5
```

## 四.关于Vue语法

### 1.数据绑定

```html
<div id="app">
  <p>{{ message }}</p>
</div>
```

其中**{{message}}**的会在运行时变成data（）中对应名为message的变量的值

### 2.v-类型的特殊属性

```html
<div v-bind:id="dynamicId"></div>
```

对应标签的v-特殊属性，双引号中所对应的vue变量的值会实现一些特殊效果

例如，在input之中

```html
<div id="app">
    <p>{{ message }}</p>
    <input v-model="message">
</div>

<script>
const app = {
  data() {
    return {
      message: 'Runoob!'
    }
  }
}

Vue.createApp(app).mount('#app')
</script>
```

在input中输入的值也会改变对应变量message的值

## 五.组件

组件（Component）是 Vue.js 最强大的功能之一。

组件可以扩展 HTML 元素，封装可重用的代码。组件系统让我们可以用独立可复用的小组件来构建大型应用，几乎任意类型的应用的界面都可以抽象为一个组件树

**每个 Vue 应用都是通过用 createApp 函数创建的**，传递给 createApp 的选项用于配置根组件。当我们挂载应用时，该组件被用作渲染的起点。

一个应用需要被挂载到一个 DOM 元素中。

以下实例我们将 Vue 应用挂载到 **\<div id="app"\>\</div\>**，应该传入 #app：

```javascript
const RootComponent = { /* 选项 */ }
const app = Vue.createApp(RootComponent)
const vm = app.mount('#app')
```

### 1.注册一个组件

注册一个全局组件语法格式如下：

```html
<div id="app">
    <!-- 此处自动替换为组件 -->
    <runoob></runoob>
</div>

<script>
// 创建一个Vue 应用
const app = Vue.createApp({})

// 定义一个名为 runoob的新全局组件
app.component('runoob', {
    template: '<h1>自定义组件!</h1>'
})

app.mount('#app')
</script>
```

全局注册往往是不够理想的。全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。所以我们可以使用components定义局部组件

```html
<div id="app">
    <runoob-a></runoob-a>
</div>
<script>
//定义组件结构
var runoobA = {
  template: '<h1>自定义组件!</h1>'
}
//注册组件
const app = Vue.createApp({
  components: {
    'runoob-a': runoobA
  }
})

app.mount('#app')
</script>
```

### 2.向组件内部传递参数

prop 是子组件用来接受父组件传递过来的数据的一个自定义属性。

父组件的数据需要通过 props 把数据传给子组件，子组件需要显式地用 props 选项声明 "prop"：

```html
<div id="app">
    <!-- title属性传递到组件内部 -->
  <site-name title="Google"></site-name>
  <site-name title="Runoob"></site-name>
  <site-name title="Taobao"></site-name>
</div>

<script>
const app = Vue.createApp({})

app.component('site-name', {
  //预先定义属性
  props: ['title'],
  //确定组件内部属性传递位置
  template: `<h4>{{ title }}</h4>`
})

app.mount('#app')
</script>
```

组件也可以类似于普通的的html标签动态的导入变量的值

```html
div id="app">
<!--动态循环内部变量，属性值也动态改变-->  
<site-info
    v-for="site in sites"
    :id="site.id"
    :title="site.title"
  ></site-info>
</div>

<script>
const Site = {
  data() {
    return {
      sites: [
        { id: 1, title: 'Google' },
        { id: 2, title: 'Runoob' },
        { id: 3, title: 'Taobao' }
      ]
    }
  }
}

const app = Vue.createApp(Site)

app.component('site-info', {
  props: ['id','title'],
  template: `<h4>{{ id }} - {{ title }}</h4>`
})

app.mount('#app')
</script>
```

## 六.属性

### 1.computed属性

我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。**能够节约系统资源**

使用计算属性：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://unpkg.com/vue@next"></script>
</head>
<body>
<div id="app">
  <p>原始字符串: {{ message }}</p>
  <p>计算后反转字符串: {{ reversedMessage }}</p>
</div>

<script>
const app = {
  data() {
    return {
      message: 'RUNOOB!!'
    }
  },
  //使用方法与method相同
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
}

Vue.createApp(app).mount('#app')
</script>
```

默认computed只有一个返回值的getter，但是也可以手动定义一个初始化属性的setter

```javascript
var vm = new Vue({
  el: '#app',
  data: {
    name: 'Google',
    url: 'http://www.google.com'
  },
  computed: {
    site: {
      // getter
      get: function () {
        return this.name + ' ' + this.url
      },
      // setter
      set: function (newValue) {
        var names = newValue.split(' ')
        this.name = names[0]
        this.url = names[names.length - 1]
      }
    }
  }
})
// 调用 setter， vm.name 和 vm.url 也会被对应更新
vm.site = '菜鸟教程 http://www.runoob.com';
document.write('name: ' + vm.name);
document.write('<br>');
document.write('url: ' + vm.url);
```

### 2.watch属性

当被监视的属性发生改变时执行

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://unpkg.com/vue@next"></script>
</head>
<body>
<div id = "app">
    千米 : <input type = "text" v-model = "kilometers">
    米 : <input type = "text" v-model = "meters">
</div>
<p id="info"></p>    
<script>
const app = {
  data() {
    return {
      kilometers : 0,
      meters:0
    }
  },
  //watch属性的使用方法和method、computer相同
  watch : {
      //监视kilometer改变
      kilometers:function(val) {
          this.kilometers = val;
          this.meters = this.kilometers * 1000
      },
      //监视meter改变
      meters : function (val) {
          this.kilometers = val/ 1000;
          this.meters = val;
      }
  }
}
vm = Vue.createApp(app).mount('#app')
vm.$watch('kilometers', function (newValue, oldValue) {
    // 这个回调将在 vm.kilometers 改变后调用（watch执行后）
    document.getElementById ("info").innerHTML = "修改前值为: " + oldValue + "，修改后值为: " + newValue;
})
</script>
</body>
</html>
```

## 七.样式绑定

class 与 style 是 HTML 元素的属性，用于设置元素的样式，我们可以用 v-bind 来设置样式属性。v-bind 在处理 class 和 style 时， 表达式除了可以使用字符串之外，还可以是对象或数组。

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://unpkg.com/vue@next"></script>
<!-- 绑定class为active的样式 -->
<style>
.active {
    width: 100px;
    height: 100px;
    background: green;
}
</style>
</head>
<body>
<div id="app">
    <!-- 通过v-bind绑定变量和对应的class -->
    <div :class="{ 'active': isActive }"></div>
    <!-- 实例 div class 渲染结果为：<div class="active"></div> -->
</div>

<script>
const app = {
    data() {
      return {
         //isActive为false时，被绑定的元素不显示
         isActive: true
      }
   }
}

Vue.createApp(app).mount('#app')
</script>
</body>
</html>
```

样式绑定也可以使用数组和表达式

```html
<div class="static" :class="[activeClass, errorClass]"></div>
相当于
<div class="static active text-danger"></div>
```

当isActive 为 true 时

```html
<div id="app">
    <!-- 使用了三元表达式，当isActive 为 true 时 -->
    <div class="static" :class="[isActive ? activeClass : '', errorClass]"></div>
    <!--  以上实例 div class 渲染结果为： -->
    <div class="static text-danger"></div>
</div>
```

也可以直接使用v-bind设置内联样式

```html
<div id="app">
    <div :style="{ color: activeColor, fontSize: fontSize + 'px' }">菜鸟教程</div>
</div>
```

## 八.事件处理

我们可以使用 v-on 指令来监听 DOM 事件，从而执行 JavaScript 代码。

**v-on** 指令可以缩写为 **@** 符号。

```javascript
v-on:click="methodName"
或
@click="methodName"
```

也可以同时运行多个方法

```html
<div id="app">
  <!-- 这两个 one() 和 two() 将执行按钮点击事件 -->
  <button @click="one($event), two($event)">
  点我
  </button>
</div>
```

### 1.事件修饰符

vue为了更详细的划分事件，引入了实践修饰符,Vue.js 通过由点 **.** 表示的指令后缀来调用修饰符。

* `.stop` - 阻止冒泡
* `.prevent` - 阻止默认事件
* `.capture` - 阻止捕获
* `.self` - 只监听触发该元素的事件
* `.once` - 只触发一次
* `.left` - 左键事件
* `.right` - 右键事件
* `.middle` - 中间滚轮事件

```html
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>

<!-- click 事件只能点击一次，2.1.4版本新增 -->
<a v-on:click.once="doThis"></a>
```

### 2.按键修饰符

Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：

```html
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```

## 九.自定义指令

除了默认设置的核心指令( v-model 和 v-show ), Vue 也允许注册自定义指令。

下面我们注册一个全局指令 v-focus, 该指令的功能是在页面加载时，元素获得焦点

```html
<div id="app">
    <p>页面载入时，input 元素自动获取焦点：</p>
    <input v-focus>
</div>

<script>
const app = Vue.createApp({})
// 注册一个全局自定义指令 `v-focus`
app.directive('focus', {
  // 当被绑定的元素挂载到 DOM 中时……
  mounted(el) {
    // 聚焦元素
    el.focus()
  }
})
app.mount('#app')
</script>
```

我们也可以在实例使用 directives 选项来注册局部指令，这样指令只能在这个实例中使用：

```html
<div id="app">
    <p>页面载入时，input 元素自动获取焦点：</p>
    <input v-focus>
</div>

<script>
const app = {
   data() {
      return {
      }
   },
   directives: {
      focus: {
         // 指令的定义
         mounted(el) {
            el.focus()
         }
      }
   }
}

Vue.createApp(app).mount('#app')
</script>
```
