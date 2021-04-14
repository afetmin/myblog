---
title: electron和react关联开发使用的小工具
date: 2021-04-14 15:51:55
tags: electron
categories: 工具
---

## concurrently
当执行多个命令时使用，跨平台，输出友好。
如：
```json
"dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \"cross-env BROWSER=none npm start\"",
```

## wait-on
等待文件，端口，网络资源就绪后执行命令。
如：
```json
"dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \"cross-env BROWSER=none npm start\"",
```
等待3000端口内容就绪后执行```electron .```。

## cross-env
跨平台设置环境变量
```json
cross-env BROWSER=none npm start
```
执行后面的命令时不打开浏览器