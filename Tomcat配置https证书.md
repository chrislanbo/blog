---
title: Tomcat配置https证书
date: 2017-03-15 11:19:16
tags: [tomcat,https证书]
---

第一步：为服务器生成证书
使用keytool 为 Tomcat 生成证书，假定目标机器的域名是“ localhost ”， keystore 文件存放在“ `d:\tomcat.keystore` ”，口令为“ `password` ”，使用如下命令生成：
> `keytool -genkey -v -alias tomcat -keyalg RSA   -validity 36500  -keystore d:\tomcat.keystore -dname "CN=localhost,OU=cn,O=cn,L=cn,ST=cn,c=cn" -storepass password -keypass password`

这个tomcat.cer是为了解决不信任时要导入的 ：
> `keytool -export -alias tomcat -keystore d:\tomcat.keystore -file d:\tomcat.cer -storepass password`

第二步：配置Tomcat 服务器 server.xml
```xml
<Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol" SSLEnabled="true"  
    maxThreads="150" scheme="https" secure="true"    
    clientAuth="false" sslProtocol="TLS"    
    keystoreFile="D:/tomcat.keystore" keystorePass="password" >
```  

第三步：访问
```
https://localhost:8443
```