
# vscode源码探究

> So begins a new age of knowledge


---
## 前言

  Tip: 本文假设你已经有`electron`基础知识，如果你从头开始学习，请查看我的另一篇介绍`electron`基础的文章: [electron基础学习](https://gitpress.io/@amber/electron%E5%9F%BA%E7%A1%80%E6%A6%82%E5%BF%B5)
    
---

  目的是做一个方便公司业务开发、支持公司特定DSL可视化编辑等需求，决定基于vscode(版本:1.3.3)(下文统一简称**vsc**)进行修改，下文是vscode源码部分的解读

---

### 需求
1. 制作一款特定的可视化编辑器，用于编辑DSL
2. 新增向导功能，用于新建资源
3. 新增新的扩展点，允许用户在新的扩展点直接新增自定义编辑器等

---

### vsc源码目录结构

``` project
// 备注 "+"代表为了实现需求，需要更改或者新增的目录或者文件

vscode    
  ├ build gulp编辑脚本   

  ├ docs 说明文档   

  ├ extensions 内置插件 
     
  ├ out 编译输出目录   

      └ extensions 自定义扩展 +

  ├ resources 平台相关资源文件 

  ├ script 工具脚本   

  ├ src 源码工程 

      ├ typing 声明文件

      ├ vs vsc主要工程

          ├ base 提供了基础的UI控件以及各种工具方法

              ├ common 只需基础的原生js API就可以运行的代码， 无法引用browser、node中的代码

              ├ browser 需要调用浏览器API的代码， 例如dom操作，可以引用common

              ├ node 需要调用node API的代码（如fs模块、path模块）， 可以引用common

              └ test 测试代码

          ├ code vscode主窗体核心代码

              ├ electron-browser 需要引用electron rendered process API的代码， 可以引用common、browser、node中的代码

                  ├ workbench

                      └ workbench.js 修改了扩展的加载方式 +

              └ electron-main 需要引用electron main process API的代码， 可以引用common、browser、node中的代码

          ├ editor "Monaco"编辑器核心以及与vscode的对接

              └ browser

                 └ controller

                    └ coreCommands.ts 支持自定义编辑器的undo、redo + 

          ├ platform 定义依赖注入以及平台基础服务

          ├ workbench 托管"Monaco"编辑器并提供viewlet的框架（如Explorer、Status Bar、Menu Bar等） +

          ├ loader.js 异步模块（amd）加载器， 主要用户加载vscode源代码

          └ ...

      ├ bootstrap-amd.js electron渲染进程入口

      ├ bootstart-fork.js 提供了支持ASAR的功能

      ├ bootstrap-window.js 提供了一系列需要electron ipc通讯的方法

      ├ bootstrap.js 提供一套bootstrap的配置的实现，例如支持使用node_modules.asar、URI helper等

      ├ buildfile.js 生成每个模块的name、描述信息等

      ├ cli.js cli脚本入口

      ├ main.js electron主进程入口

      ├ path.js 提供path相关的工具方法

      └ ...

  └ test 测试工程
```