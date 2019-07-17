

# 2.0 Electron HelloWorld

> So begins a new age of knowledge


[TOC]

---

## 前言

   作为初学`Electron`的同学，[官方文档](https://Electronjs.org/)就是最好的文档
    
---

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

---

### 开发

1. 构建必须资源

在你的应用(此处默认应用名为`electron-quick-start`)目录下运行如下命令，如图，会生成一个package.json文件

```sh
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

在同级目录下，新建main.js与index.html两个文件，之后，你便拥有了一个最简electron应用的所有资源文件，如下

```
electron-qucik-start/
├── package.json
├── main.js
└── index.html
```

---

### 安装electron

在此，官方推荐的安装方法是把它作为您 app 中的开发依赖项，这使您可以在不同的 app 中使用不同的 Electron 版本。 

在electron-qucik-start目录下运行如下脚本：

```sh
# 使用npm作为包管理器
npm install --save-dev electron
```

or

```sh
# 使用yarn作为包管理器
yarn add electron
```

> [!Tip]
> 除此之外，也有其他安装 Electron 的途径。 请咨询[安装指南](https://electronjs.org/docs/tutorial/installation)来了解如何用代理、镜像和自定义缓存