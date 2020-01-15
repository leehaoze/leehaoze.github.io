---
title: 创建Electron应用
date: 2020-01-15 23:59:15
tags:
- Electron
---
# 安装NodeJS
有什么问题是神奇的brew解决不了的呢 
终端输入
`brew install node`

# 创建项目文件夹
- 新建文件夹
`mkdir yout-app`
- 初始化
`npm init`
随后一路回车就行
- 编辑配置文件
编辑生成的`package.json`
在`scripts`字段新增一行`"start": "electron ."`

# 安装Electron依赖
又有什么是神奇的npm解决不了的依赖呢
终端输入
`npm install --save-dev electron`

# 创建一个窗口
- 新建入口文件`main.js`
- 引入`electron`模块
`const { app, BrowserWindow } = require('electron')`
`app`负责应用的生命周期，`BrowserWindow`负责创建窗口
- 创建用于显示的`index.html`
创建一个静态页面 内容随意
- 创建窗口
在应用程序`ready`的时候调用创建窗口函数,加载`index.html`文件到窗口中
```
function createWindow () {   
  // 创建浏览器窗口
  let win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  // 加载index.html文件
  win.loadFile('index.html')
}

app.on('ready', createWindow)
```

# 运行
`npm run start`
即可看到窗口

# 小结
Electron其实就是普通的页面+nodejs 结合，electron中有两个进程，一个叫做`渲染进程`,另外一个叫做`主进程`。
顾名思义啊，`渲染进程`负责页面的渲染，可以理解为是个浏览器。
运行`package.json`中定义的`main`脚本的是主进程，该进程负责与操作系统的交互，可以使用NodeJS的API。

`主进程`使用`BrowserWindow`创建页面，每个页面都在自己单独的`渲染进程中运行`。主进程与渲染进程通过`ipc`的方式通信
