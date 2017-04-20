---
title: log使用正则表达式过滤无用标签
date: 2016-09-14 14:23:47
tags: [logcat,android]
---
`^(?!.*(OpenGLRenderer|dalvikvm|EGL_genymotion|libEGL)).*$`

就可以过滤掉名称为OpenGLRenderer、dalvikvm、EGL_genymotion、libEGL的信息