---
title: AndroidStudio 版本控制忽略的文件以及操作
date: 2017-03-15 15:55:40
tags: [版本控制]
---


*.iws
.idea/workspace.xml
*.iml
local.properties
.gradle/
.idea/
build/
app/build/

只需要在 .gitignore文件中注释就可以
或者在Settings>VersionControl>IgnoredFiles中添加