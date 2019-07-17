
# Electron基础学习

> So begins a new age of knowledge


---

## 前言

   作为初学`Electron`的同学，[官方文档](https://Electronjs.org/)就是最好的文档
    
---

### 定义
   
      Electron， 可以理解是一个内置浏览器的桌面应用程序，这个桌面应用程序可以让你使用node.js，而不是局限在浏览器运行环境中，从而实现许多浏览器无法实现的功能，如I/O读写等。

### 优势
  
  1. 浏览器 + node.js

  优势其一，就是定义中介绍了，可以node.js与`Electron`提供的api，实现许传统桌面应用才能实现的功能

  2. 跨平台
  
  一次开发， 多端运行， `Electron`只需要一份代码，就可以打包出兼容Mac、Windows、Linux平台的引用


  3. 活跃的社区

  `Electron`有着非常活跃的社区和各种各样的官网工具：[Electron社区](https://Electronjs.org/community)

### 配置开发环境

1. #### Mac 

* MacOS

版本要求：10.10(Yosemite)以及以上版本

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
```sh
# 下面这行的命令会打印出NodeJs的版本信息
node -v

# 下面这行的命令会打印出npm的版本信息
npm -v
```

* 代码编辑器

一款适合JavaScript或者TypeScript的编辑器，推荐[vscode](https://code.visualstudio.com/)