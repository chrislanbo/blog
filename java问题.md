---
title: java问题
date: 2016-07-14 11:25:19
tags:  
---
#java问题

cursor 的解释： 类似于resultset
想要数据库的某一列数据getLong（列索引 ）

自定义一个类继承SQLiteOpenHelper

想要操作数据库需要得到SQLitedatabase的实例对象
然后将sql语句搞定，但是数据库不会被创建

之后创建自定义类的实例对象，调用getreadabledatabase（）该数据库就会被创建

查询query（）返回一个cursor结构集合
cursor！=null && cursor。getcount（）>0
看需求选择关不关资源

2打开指定应用的数据库

改变文件权限SQLiteDatabase.openDatabase()
chmod 777 account.db

内容提供者就是为了将私有的数据库内容暴露出来
其他应用可以通过contentresolver内容解析者访问，不论是不是有文件读取权限
ContentProvider


2-

URL--URI 的区别
url大部分指的是网址	www.baidu.com/news.html

uri大部分是自定义的路径		

自定义异常
throw new Exception ("这个异常时啥啥啥，自己重新搞定 ");

过滤pub日志，存在的话，说明contentprovider逻辑正确
路径之前需要有content://
getContentResolver（）.query（。。。。。）;

作用是将短信和联系人的数据库暴露出来让其他文件获取


进行cursor 操作的时候getString（列）是自定义查询的列索引，不是默认列索引（如果查询的是所有列，就是默认列索引）
4--短信备份
创建xml文件
获得xml序列化器		
Xml.newserializer（）

短信还原
xmlpull解析
[1]由于短信的数据库已经通过内容的提供者暴露出来 所以我们直接通过内容解析者去操作数据库
联系人删除了之后，具体数据没有删除掉，删掉的是索引
