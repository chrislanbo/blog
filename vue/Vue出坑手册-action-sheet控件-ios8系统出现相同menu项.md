---
title: Vue出坑手册-action-sheet控件 ios8系统出现相同menu项
date: 2017-05-25 14:07:29
tags: [vue]
categories: [vue]
---


情境：

	手机：iphone SE
	版本：ios8

问题：
	
	使用vue中的action-sheet控件加载菜单页面，菜单只有一个item条目，但是显示出了两个一模一样的item条目

查看代码发现
```javascript
// vue代码
	if (body.accountList) {
	  for (let i in body.accountList) {
	    menus.push(body.accountList[i]['acctLogoDesc'])
	  }
	  this.menus = menus
	  this.accountList = body.accountList
	}
```


然后使用传统for循环即可
```
	if (body.accountList) {
      for (let i = 0; i < body.accountList.length; i++) {
        menus.push(body.accountList[i]['acctLogoDesc'])
      }
      this.menus = menus
      this.accountList = body.accountList
    }
```
 
所以vue中使用for-in 还是有点问题的