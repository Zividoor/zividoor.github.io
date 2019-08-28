---
title: node-webkit_1
tags: webkit
---

**起因**

由于项目需要将web程序包装成桌面程序，所以在项目中使用了node-webkit，将web包装成桌面程序。

而现在由于之前包装好的客户端浏览器内核不支持新的web功能，所以需要更新客户端，但由于原先负责客户端包装的员工已经离职，且编写客户端的代码技术信息都已经遗失，需要重新开发包装一个客户端程序。

**方案确认**

根据代码分析，得知之前使用的技术是node-webkit实现的客户端包装，所以决定继续使用node-webkit实现。

node-webkit网站：[NW.js](https://nwjs.io/)

**问题处理**

由于该项目所使用的nw版本不可追踪，所以这里直接使用了 [v0.40.1](https://nwjs.io/blog/v0.40.1/)

在根据官方文档：[document](http://docs.nwjs.io/en/latest/)

配置好package.json文件：

`{

  "name": "BeiBen Client",

  "version": "1.0.0",

  "description": "BeiBen Client",

  "main": "launch/launch.html",

  "author": "Gant",

  "chromium-args": "--disable-direct-write --disable-web-security --allow-file-access-from-files --disable-xss-auditor",

  // "node-remote": "localhost,127.0.0.1",

  "js-flags": "--harmony_proxies --harmony_collections",

  "window": {

​    "title": "BeiBen Client",

  "icon": "launch/images/logo.png",

​    "toolbar": true,

​    "frame": false,

​    "width": 620,

​    "height": 396,

​    "resizable": false,

​    "position": "center"

  },

  "webkit": {

​    "plugin": true

  }

}`

并编根据案例编写好代码之后发现程序能够运行，但在内嵌的web跳转到其他链接时会发生程序崩溃。

之后发现是node-webkit版本问题导致（这个是目前最新版本）

随后替换了版本 [v0.39.3](https://github.com/nwjs/nw.js/tree/nw39)

解决问题。

打包问题待续。。。