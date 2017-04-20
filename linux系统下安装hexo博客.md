
---
title: linux系统下安装hexo博客
date: 2016-09-14 11:25:19
tags: [linux,blog,hexo] 
---

1. 使用密令行安装node.js（官网下载好压缩文件后，直接解压可用）
2. 为了全局可以使用需要在系统目录创建软链接

		默认情况下权限为用户权限，无法在系统目录创建软链接
		在系统目录创建链接需要root权限，我们在此设置一个临时权限
		sudo su [在此输入系统密码]
		获取root权限后会变成#
		紧接着 ln -s 源目录 目标路径
		设置好node 和 npm 后测试 

`sudo npm install -g hexo`##安装hexo

创建chrislanbo.github.io文件夹，目录下键入

	hexo init #初始化
	hexo g #生成静态页面
	hexo s 启动本地服务
浏览器中输入http://localhost:4000 可以看到页面
<!--more-->
配置_config.yml文件建立关联

出现这种错误

	bash: /dev/tty: No such device or address
	error: failed to execute prompt script (exit code 1)
	fatal: could not read Username for 'https://github.com': No error
deploy:后面一定要有空格

	deploy:  
	type: git

	repo: https://github.com/chrislanbo/chrislanbo.github.io.git

	branch: master

`hexo s -p 5000 # 如果端口被占用，换端口`

如果deploy上传失败 `Deployer not found: git`

键入`npm install hexo-deployer-git --save`

    hexo clean

    hexo generate

    hexo deploy

一些常用命令：

	hexo new "postName" #新建文章
	
	hexo new page"pageName" #新建页面
	
	hexo generate #生成静态页面至public目录
	
	hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
	
	hexo deploy #将.deploy目录部署到GitHub
	
	hexo help # 查看帮助
	
	hexo version #查看Hexo的版本

