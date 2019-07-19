

# 2.0 Electron HelloWorld

> So begins a new age of knowledge


---

## 前言

   作为初学`Electron`的同学，[官方文档](https://Electronjs.org/)就是最好的文档

---

> [!NOTE]
> 本文的所有代码示例，都储存在[github](https://github.com/XieYuanCode/electron-demo/tree/master/electron-qucik-start)，你可以clone并运行，尝试做出你自己的修改
    
---

### 配置开发环境

1. #### Mac 

* MacOS

版本要求：10.10(Yosemite)以及以上版本

* NodeJs

在[NodeJs官网](https://nodejs.org/en/download/)下载最新稳定版NodeJs并安装，如下命令可以测是否安装成功
```Shell
# 下面这行的命令会打印出NodeJs的版本信息
node -v

# 下面这行的命令会打印出npm的版本信息
npm -v
```

* 代码编辑器

一款适合JavaScript或者TypeScript的编辑器，推荐[vscode](https://code.visualstudio.com/)

2. #### Windows

* Windows

版本要求：Windows 7以及以上版本

* NodeJs

在[NodeJs官网](https://nodejs.org/en/download/)下载最新稳定版NodeJs并安装，如下命令可以测是否安装成功
```sh
# 下面这行的命令会打印出NodeJs的版本信息
node -v

# 下面这行的命令会打印出npm的版本信息
npm -v
```

* 代码编辑器

一款适合JavaScript或者TypeScript的编辑器，推荐[vscode](https://code.visualstudio.com/)

3. #### Linux

* 系统版本

`Electron`支持Ubuntu 12.04、Fedora 21、Debian 8 及其以上版本

* NodeJs

首先，安装NodeJS，对于不同linux分支，安装步骤会有所差异。 假如你使用系统自带的包管理器，比如： apt 或者 pacman，请使用[Node.js 官方Linux安装指引](https://nodejs.org/en/download/package-manager/)。
如下命令可以测是否安装成功
```Shell
# 下面这行的命令会打印出NodeJs的版本信息
node -v

# 下面这行的命令会打印出npm的版本信息
npm -v
```

* 代码编辑器

一款适合JavaScript或者TypeScript的编辑器，推荐[vscode](https://code.visualstudio.com/)

---

### 开发

1. 构建必须资源

在你的应用(此处默认应用名为`electron-quick-start`)目录下运行如下命令，如图，会生成一个package.json文件

```Shell
npm init
```

![asd](https://i.loli.net/2019/07/17/5d2ee32a6ebd987115.gif)

等待完成之后，将会生成如下的一个package.json文件

```json
{
  "name": "electron-qucik-start",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}

```

之后， 我们在`scripts`字段下，增加一个`start`脚本来运行`electron`

```json
{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "electron ."
  }
}

```

在同级目录下，新建main.js个文件，之后，你便拥有了一个最简electron应用的所有资源文件，如下

```
electron-qucik-start/
├── package.json
└── main.js
```

---

### 安装electron

在此，官方推荐的安装方法是把它作为您 app 中的开发依赖项，这使您可以在不同的 app 中使用不同的 Electron 版本。 

在electron-qucik-start目录下运行如下脚本：

```Shell
# 使用npm作为包管理器
npm install --save-dev electron
```

or

```Shell
# 使用yarn作为包管理器
yarn add electron
```

> [!Tip]
> 除此之外，也有其他安装 Electron 的途径。 请咨询[安装指南](https://electronjs.org/docs/tutorial/installation)来了解如何用代理、镜像和自定义缓存


### 开发主进程脚本

前文说到，package.json中main字段就是`electron`的主进程，那就是我们刚新建好的main.js


`electron` 模块所提供的功能都是通过命名空间暴露出来的。 比如说： electron.app负责管理Electron 应用程序的生命周期， electron.BrowserWindow类负责创建窗口。

```javascript
// main.js

// app负责管理Electron
// BrowserWindow类负责创建窗口(也就是我们前面说的渲染进程)
const { app, BrowserWindow } = require('electron')

function createWindow () {   
  // 创建浏览器窗口
  let win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  // 加载特定Url， 也可以使用loadFile加载本地html文件
  win.loadURL('https://gitpress.io/@amber/electronHelloWorld')
}

app.on('ready', createWindow)
```

OK, 现在你已经可以运行你的应用了, 在当前目录执行以下命令

```Shell
# 使用npm作为包管理器
npm start
```

or

```Shell
# 使用yarn作为包管理器
yarn start
```

稍后片刻，你就会发现你的第一个electron应用成功的运行起来了！

让我们继续完善这个示例工程

```javascript
const { app, BrowserWindow } = require('electron')

// 保持对window对象的全局引用，如果不这么做的话，当JavaScript对象被
// 垃圾回收的时候，window对象将会自动的关闭
let win

function createWindow () {
  // 创建浏览器窗口。
  win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  // 加载特定Url， 也可以使用loadFile加载本地html文件
  win.loadURL('https://gitpress.io/@amber/electronHelloWorld')

  // 打开开发者工具
  win.webContents.openDevTools()

  // 当 window 被关闭，这个事件会被触发。
  win.on('closed', () => {
    // 取消引用 window 对象，如果你的应用支持多窗口的话，
    // 通常会把多个 window 对象存放在一个数组里面，
    // 与此同时，你应该删除相应的元素。
    win = null
  })
}

// Electron 会在初始化后并准备
// 创建浏览器窗口时，调用这个函数。
// 部分 API 在 ready 事件触发后才能使用。
app.on('ready', createWindow)

// 当全部窗口关闭时退出。
app.on('window-all-closed', () => {
  // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
  // 否则绝大部分应用及其菜单栏会保持激活。
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  // 在macOS上，当单击dock图标并且没有其他窗口打开时，
  // 通常在应用程序中重新创建一个窗口。
  if (win === null) {
    createWindow()
  }
})

// 在这个文件中，你可以续写应用剩下主进程代码。
// 也可以拆分成几个文件，然后用 require 导入。
```


---

**LAST TOPIC** => [Electron 基础概念](https://gitpress.io/@amber/electron%E5%9F%BA%E7%A1%80%E6%A6%82%E5%BF%B5)

**NEXT TOPIC** => [Electron 进程通讯](https://gitpress.io/@amber/electron%E8%BF%9B%E7%A8%8B%E9%80%9A%E8%AE%AF)
