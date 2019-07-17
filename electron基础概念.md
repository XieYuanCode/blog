
# 1.0 Electron 基础概念

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

---

### 应用架构

#### 主进程与多进程架构

在Electron的应用架构中，包含着有且唯一的主进程(`Main Porcess`)和多个渲染进程(`Renderer Process`)
![](https://i.loli.net/2019/07/17/5d2ed79aa55e122768.png)

* 主进程(`Main Porcess`)

主进程即package.json中main字段指定的脚本,在应用中有且只有唯一一个主进程，在主进程中，使用`BrowserWindow`实例创建页面，每个页面对应一个渲染进程

**<font color=yellow>主进程负责维护渲染进程，即渲染进程的创建、销毁</font>**

* 渲染进程(`Renderer Porcess`)

被主进程使用`BrowserWindow`实例创建，每个`Electron`中的web页面运行在它自己的渲染进程中。

渲染进程无法调用`Electron`GUI相关的API， 如[Dialog](https://electronjs.org/docs/api/dialog)、[Menu](https://electronjs.org/docs/api/menu)等，因为在 web 页面里操作原生的 GUI 资源是非常危险的， 所以当你希望在web页面调用GUI接口，必须与主进程进行通讯，参考[此文](https://gitpress.io/@amber/electron%E8%BF%9B%E7%A8%8B%E9%80%9A%E8%AE%AF)

**<font color=yellow>渲染进程只关心自己渲染的web页面</font>**