---
title: mock-api的使用
date: 2017-04-24 16:11:25
tags: [mock-api,vue,前端]
categories:
---


npm install -g mock-api

然后创建一个文件夹test（随意）
存放json文件，然后使用命令 mock-api serve test（此处为文件夹名）

>mock-api -h
Usage: mock-api [options] [command]

    Commands:
           serve [options] [path]  本地API模拟器
    
    Options:
        -h, --help     output usage information
        -V, --version  output the version number




>mock-api serve -h

Usage: serve [options] [path]

  本地API模拟器

  Options:

    -h, --help                     output usage information
    -p, --port <port>              服务器端口，默认10086
    -d, --delay <delay>            模拟网络延迟，默认不延迟
    -s, --status <status>          返回的HTTP状态码，默认200
    -S, --staticPath <staticPath>  静态文件服务目录，默认./static

  Examples:

    $ mock-api serve path/to/folder
    $ mock-api serve path/to/folder -p 8000
    $ mock-api serve path/to/folder -p 8000 -d 2000
    $ mock-api serve path/to/folder -s 400
    $ mock-api serve path/to/folder -S path/to/static




## 在vue中的使用

配置到package.json 文件中

scripts 下
"start": "mock-api serve server"

之后启动就使用 
npm run start

