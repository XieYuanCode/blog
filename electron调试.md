
# 4.0 Electron 调试

> So begins a new age of knowledge


---

## 前言

   作为初学`Electron`的同学，[官方文档](https://Electronjs.org/)就是最好的文档

---

> [!NOTE]
> 本文的所有代码示例，都储存在[github](https://github.com/XieYuanCode/electron-demo/tree/master/communication)，你可以clone并运行，尝试做出你自己的修改

---

### 渲染进程调试

渲染进程，即web页面，调试方式与我们平常开发web页面是方式是一样的， 都是使用DevTools进行调试

通过编程的方式在 `BrowserWindow` 的 `webContents` 中调用 `openDevTool()` API来打开它们：

```javascript
const { BrowserWindow } = require('electron')

let win = new BrowserWindow()
win.webContents.openDevTools()
```

有关DevTools的使用方式，google为我们提供了[详细的文档](https://developer.chrome.com/devtools)， 方便我们调试我们的web页面

### 主进程调试

由于Electron的本质一个NodeJs程序，所以主进程的日志默认是在命令行输出的，就如同我们[上一章节](https://gitpress.io/@amber/electron%E8%BF%9B%E7%A8%8B%E9%80%9A%E8%AE%AF)中的情况一样，那如果我们不仅想查看日志，还想调试怎么办呢？

#### DevTools调试

我们打开项目中的package.json, 在 `scripts` 中增加如下脚本：

> [!Tip]
> 这里默认的端口号是5858，你可以改成喜欢的任何端口

```json
"scripts": {
  "start:debug": "electron . --inspect=5858 electron-qucik-start"
}
```

运行后，终端如下：

![Snipaste_2019-07-19_14-44-19.png](https://i.loli.net/2019/07/19/5d3167021b63c85554.png)

之后，打开我们的chrome浏览器， 输入网址[chrome://inspect](chrome://inspect/), 点击Configure按钮，确保配置项中有我们的端口

![1.png](https://i.loli.net/2019/07/19/5d31684cd4bdc46864.png)

![2.png](https://i.loli.net/2019/07/19/5d31684ce3e4e67473.png)

点击Done， 之后我们可以就可以看到我们

![1.png](https://i.loli.net/2019/07/19/5d3169896187c97866.png)

点击 inspect, 就可以打开一个调试主进程的DevTools, 在这里, 我们可以和调试渲染进程一样，在主进程的代码中打断点调试

#### vscode调试

如果你选择的代码编辑器是vscode的话，我们可以在vscode中增加调试脚本，使用vscode调试主进程

使用vscode打开项目根目录，增加一个[.vscode/launch.json](https://github.com/XieYuanCode/electron-demo/blob/master/communication/.vscode/launch.json)文件， 写入如下脚本

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Main Process",
      "type": "node",
      "request": "launch",
      "cwd": "${workspaceRoot}",
      "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/electron",
      "windows": {
        "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/electron.cmd"
      },
      "args" : ["."],
      "outputCapture": "std"
    }
  ]
}
```

使用vscode，在main.js中设置一些断点，按下f5，你应该能够捕获断点信息。

---

**LAST TOPIC** => [Electron 进程通讯](https://gitpress.io/@amber/electron%E8%BF%9B%E7%A8%8B%E9%80%9A%E8%AE%AF)