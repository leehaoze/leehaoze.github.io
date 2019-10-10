---
title: 搭建Electron+React应用
date: 2019-10-10 17:34:14
tags:
- Electron
- React
---
参考自 `https://juejin.im/post/5c83e225e51d453b117ba825`

# 创建React项目
`yarn create react-app APP_NAME`

# 添加Electron
`cd APP_NAME`
`yarn add electron --dev`

# 配置Electron入口文件
React入口文件为`public/index.html`,在`public`下新建一个Electron应用的入口文件`electron.js`

# 添加工具库，实现一行命令启动Electron + React
`yarn add concurrently --dev`
`yarn add wait-on --dev`
在`package.json`中新加一个脚本命令
`"electron-dev": "concurrently \"BROWSER=none react-scripts start\" \"wait-on http://localhost:3000 && electron .\""`

# Electron 入口文件代码
```
const {app, BrowserWindow} = require('electron');
const isDev = require('electron-is-dev');
const path = require('path');

let mainWindow;

function createWindow() {
    mainWindow = new BrowserWindow({
        width: 960,
        height: 600,
    });

    if (isDev) {
        mainWindow.loadURL('http://localhost:3000/');
    } else {
        mainWindow.loadFile('index.html');
    }

    mainWindow.webContents.openDevTools();
    mainWindow.on('closed', function () {
        mainWindow = null
    })
}

app.on('ready', createWindow);

app.on('window-all-closed', function () {
    if (process.platform !== 'darwin') {
        app.quit()
    }
});

app.on('activate', function () {
    if (mainWindow === null) {
        createWindow()
    }
});
```

# 启动应用
`yarn electron-dev`
