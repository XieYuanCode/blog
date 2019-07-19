
# 3.0 Electron 进程通讯

> So begins a new age of knowledge


---

## 前言

   作为初学`Electron`的同学，[官方文档](https://Electronjs.org/)就是最好的文档

> [!NOTE]
> 本文的所有代码示例，都储存在[github](https://github.com/XieYuanCode/electron-demo/tree/master/communication)，你可以clone并运行，尝试做出你自己的修改

---

> [!Tip]
> 为了方便编辑渲染进程，我们新增`index.html`文件，并修改如下代码

```javascript
// 修改之前的代码
win.loadURL('https://gitpress.io/@amber/electronHelloWorld')

// 修改之后的代码
win.loadFile('index.html')
```
    
---

### 概念

由于Electron的API都会指派给一种进程类型，许多API都只能在主进程或者渲染进程中调用，当你希望调用另外一个进程的API的时候，就需要进行通讯。

通讯分为两类：
* 使用`ipcRenderer` 和 `ipcMain`模块发送消息
* 使用 remote模块进行RPC方式通信

#### IPC通讯



#### RPC通讯
