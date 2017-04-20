---
title: 'hexo博客出现Template render error: (unknown path)?'
date: 2017-03-31 15:25:02
tags: [hexo]
---

使用`hexo d`等命令都提示`Template render error: (unknown path)?`
可能是刚创建的文章中

```
{{}}，{% %}...

```

等转义失败的符号，需要用```注释