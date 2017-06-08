---
title: SSH商城
date: 2017-06-08 09:23:51
tags: [ssh,商城]
---

* 使用SQL Server 2005 启用数据库，附加数据库文件
* 使用MyEclipse运行项目，部署项目到tomcat，检查日志，无报错
* 浏览器输入[http://localhost:8080/shopping/](http://localhost:8080/shopping/index.jsp)
* 页面成功

select * from users

jdbc:jtds:sqlserver://127.0.0.1:1433/shopping

可以在DB Browser 中创建sql连接，直接操作数据库（省的去数据库操作）

	Driver name: sqlserver2005
	Connection URL: jdbc:sqlserver://127.0.0.1:1433
	User name: sa
	Password: sa
	Driver JARs
	add 自己的sqljdbc.jar
	Driver classname: com.microsoft.sqlserver.jdbc.SQLServerDriver
	然后点击test driver测试一下是否连接了