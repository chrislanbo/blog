---
title: Vue条件渲染详解
date: 2017-04-11 17:46:28
tags: [vue]
---

### v-if、v-else、v-else-if
```html
<div v-if="Math.random() > 0.5">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

### template 中 v-if 
template可以当成一个包装元素，一旦这样，便只会渲染标签内的元素


#### 用key

