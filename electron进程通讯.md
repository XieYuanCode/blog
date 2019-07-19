
# 3.0 Electron 进程通讯

> So begins a new age of knowledge


---

## 前言

   作为初学`Electron`的同学，[官方文档](https://Electronjs.org/)就是最好的文档

---

> [!NOTE]
> 本文的所有代码示例，都储存在[github](https://github.com/XieYuanCode/electron-demo/tree/master/communication)，你可以clone并运行，尝试做出你自己的修改

---

> [!Tip]
> 为了方便编辑渲染进程，我们新增[index.html](https://github.com/XieYuanCode/electron-demo/blob/master/communication/index.html)文件，并修改如下代码

```JavaScript
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
`ipcRenderer` 和 `ipcMain` 都是一个 [EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter) 的实例。 你可以使用他们提供的一下方法，在进程之间通讯

##### 同步消息

> [!IMPORTANT]
> 发送同步消息将会阻塞整个渲染进程，你应该避免使用这种方式 - 除非你知道你在做什么, 如果使用同步消息，请通过 `event.returnValue` 设置回复消息

在main.js中增加如下代码

```javascript
const { ipcMain } = require('electron')

// 主进程开始监听消息id为synchronous-message的消息
// 当收到渲染进程发送的消息id为synchronous-message，将执行回调
ipcMain.on("synchronous-message", (event, args) => {
  // 定义返回消息，并通过event.returnValue返回给渲染进程
  // !!! 同步消息会阻塞整个渲染进程，请确定设置返回消息，不然请使用异步消息
  // !!! 同步消息会阻塞整个渲染进程，请确定设置返回消息，不然请使用异步消息
  // !!! 同步消息会阻塞整个渲染进程，请确定设置返回消息，不然请使用异步消息
  // 重要的话，说三遍
  const returnValue = "我是主进程的同步返回消息"
  console.log(`主进程日志-主进程收到channel为synchronous-message的同步消息, args: ${args}, 返回参数: ${returnValue}`)
  event.returnValue = returnValue
})
```

在index.js中添加如下代码
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<script>
  const {
    ipcRenderer
  } = require('electron')

  function sendSyncMessageToMainProcess() {
    console.log("渲染进程日志-渲染进程开始向主进程个发送同步消息！")
    let syncResult = ipcRenderer.sendSync("synchronous-message", "我是从渲染进程发向主进程的同步消息")
    console.log("渲染进程日志-主进程的同步返回消息:", syncResult)
  }
</script>

<body>
  <button onclick="sendSyncMessageToMainProcess()">从向主进程发送同步消息</button>
</body>

</html>
```
运行程序，点击按钮，你可以从devtools和命令行分别看到渲染进程和主进程的日志信息, 如图

![Kapture 2019-07-19 at 12.27.30.gif](https://i.loli.net/2019/07/19/5d3146e4483db40069.gif)

##### 异步消息

在main.js中增加如下代码

```javascript
// 主进程开始监听消息id为asynchronous-message的消息
// 当收到渲染进程发送的消息id为asynchronous-message，将执行回调
ipcMain.on("asynchronous-message", (event, args) => {
  console.log(`主进程日志-主进程收到channel为asynchronous-message的异步消息, args: ${args}`)

  // reply方法用于再给渲染基础在发一个异步消息
  // 此处作用就是回复给渲染进程一个消息
  event.reply('asynchronous-reply', 'message-got')
})
```

在index.html中增加如下代码
```javascript
function sendAsyncMessageToMainProcess() {
  console.log("渲染进程日志-渲染进程开始向主进程个发送异步消息！")
  ipcRenderer.send("asynchronous-message", "我是从渲染进程发向主进程的异步消息")
}

ipcRenderer.on('asynchronous-reply', (event, arg) => {
  console.log("渲染进程日志-渲染进程收到主进程的消息，arg:", arg)
})
```


```html
<!-- index.html -->
<button onclick="sendAsyncMessageToMainProcess()">从向主进程发送异步消息</button>
```

运行程序，点击按钮，你可以从devtools和命令行分别看到渲染进程和主进程的日志信息, 如图

![Kapture 2019-07-19 at 12.27.30.gif](https://i.loli.net/2019/07/19/5d314a48b6fd493111.gif)

#### RPC通讯

> [!NOTE]
> RPC通讯可以让你在渲染进程中使用主进程模块

`remote` 模块为渲染进程（web页面）和主进程通信（IPC）提供了一种简单方法。

在Electron中, GUI 相关的模块 (如 `dialog`、`menu` 等) 仅在主进程中可用, 在渲染进程中不可用。 为了在渲染进程中使用它们, `ipc` 模块是向主进程发送进程间消息所必需的。 使用 `remote` 模块, 你可以调用 main 进程对象的方法, 而不必显式发送进程间消息, 类似于 Java 的 RMI

我们在index.html中增加如下代码

```javascript
const { BrowserWindow } = require('electron').remote

function openNewWindowFromRendererProcess() {
  let win = new BrowserWindow({
    width: 800,
    height: 600
  })
  win.loadURL('https://gitpress.io/@amber/electron%E8%BF%9B%E7%A8%8B%E9%80%9A%E8%AE%AF')
}
```

```html
<button onclick="openNewWindowFromRendererProcess()">在渲染进程中调用BrowserWindow方法打开新窗口</button>
```

运行程序，点击按钮，你可以看到渲染进程打开了一个新的窗口

![Kapture 2019-07-19 at 12.51.54.gif](https://i.loli.net/2019/07/19/5d314c853c0d651861.gif)

---

**LAST TOPIC** => [打造你的第一个electron应用：HelloWorld](https://gitpress.io/@amber/electronHelloWorld)



