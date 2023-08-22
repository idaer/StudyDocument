# Electron深入浅出

## 简介

早期桌面应用的开发主要借助原生 C/C++ API 进行，由于需要反复经历编译过程，且无法分离界面 UI 与业务代码，开发调试极为不便。后期出现的 QT 和 WPF 在一定程度上解决了界面代码分离和跨平台的问题，却依然无法避免较长时间的编译过程。近几年伴随互联网行业的迅猛发展，尤其是 NodeJS、Chromium 这类基于 W3C 标准开源应用的不断涌现，原生代码与 Web 浏览器开发逐步走向融合，Electron 正是在这种背景下诞生的。

Electron 是由 Github 开发，通过将**Chromium和NodeJS整合为一个运行时环境**，实现使用 HTML、CSS、JavaScript 构建跨平台的桌面应用程序的目的。Electron 源于 2013 年 Github 社区提供的开源编辑器 Atom，后于 2014 年在社区开源，并在 2016 年的 5 月和 8 月，通过了 Mac App Store 和 Windows Store 的上架许可，VSCode、Skype 等著名开源或商业应用程序，都是基于 Electron 打造。为了方便编写测试用例，笔者在 Github 搭建了一个简单的 Electron 种子项目Octopus，读者可以基于此来运行本文涉及的示例代码。

## 开始

[自己看官方文档，写挺好的](https://www.electronjs.org/zh/docs/latest/tutorial/quick-start)

## 主进程与渲染进程

* **主进程**（main process）管理所有的 web 页面以及相应的渲染进程，它通过BrowserWindow来创建视图页面。
* **渲染进程**（renderer processes）用来运行页面，每个渲染进程都对应自己的BrowserWindow实例，如果实例被销毁那么渲染进程就会被终止。

任何 Electron 应用程序的入口都是 `main.js` 文件。 这个文件控制了主进程，它运行在一个完整的Node.js环境中，负责控制您应用的生命周期，显示原生界面，执行特殊操作并管理渲染器进程。

Electron 分别在主进程和渲染进程提供了大量 API，可以通过require语句方便的将这些 API 包含在当前模块使用。但是 Electron 提供的 API 只能用于指定进程类型，即某些 API 只能用于渲染进程，而某些只能用于主进程，例如上面提到的`BrowserWindow`就只能用于主进程。

```javascript
//main.js内
const { BrowserWindow } = require("electron");

ccc = new BrowserWindow();
```

但是，Electron 通过remote模块暴露一些主进程的 API，如果需要在渲染进程中创建一个`BrowserWindow`实例，那么就可以借助这个 `remote` 模块：

```javascript
const { remote } = require("electron"); // 获取remote模块
const { BrowserWindow } = remote; // 从remote当中获取BrowserWindow

const browserWindow = new BrowserWindow(); // 实例化获取的BrowserWindow
```

Electron 可以使用所有 NodeJS 上提供的 API，同样只需要简单的`require`一下。

```js
const fs = require("fs");

const root = fs.readdirSync("/");
```

当然，NodeJS 上数以万计的 npm 包也同样在 Electron 可用，当然，如果是涉及到底层 C/C++的模块还需要单独进行编译，虽然这样的模块在 npm 仓库里并不多。

既然 Electron 本质是一个浏览器 + 跨平台中间件的组合，因此常用的前端调试技术也适用于 Electron，这里可以通过<kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>I</kbd>手动开启 Chromium 的调试控制台，或者通过下面代码在开发模式下自动打开：

```js
mainWindow.webContents.openDevTools(); // 开启调试模式
```

## 模块

[各种模块不如自己翻文档](https://www.electronjs.org/zh/docs/latest/api/app)
