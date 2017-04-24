---
title: 《拾·微》api接口
date: 2017-04-11 15:40:48
tags: [api,拾·微]
---

# api 接口---《拾·微》
getwi
1.  分类数据: http://gank.io/api/data/数据类型/请求个数/第几页
     * 数据类型： 福利 | Android | iOS | 休息视频 | 拓展资源 | 前端 | all
     * 请求个数： 数字，大于0
     * 第几页：数字，大于0
     * 例：
         * http://gank.io/api/data/Android/10/1
         * http://gank.io/api/data/福利/10/1
         * http://gank.io/api/data/iOS/10/2
         * http://gank.io/api/data/all/20/2

2. 每日数据： http://gank.io/api/day/年/月/日
     * [例]： http://gank.io/api/day/2015/08/06

3. 随机数据：http://gank.io/api/random/data/分类/个数
     * 数据类型：福利 | Android | iOS | 休息视频 | 拓展资源 | 前端
     * 个数： 数字，大于0
         * [例]：http://gank.io/api/random/data/Android/20    




# api 接口---《微·笑》
    
  wifun  
>key = 7bb45624a1ac6a2cd42719716d8f77f6

1. 按照时间查询笑话: 
    * http://japi.juhe.cn/joke/content/list.from?key=7bb45624a1ac6a2cd42719716d8f77f6&page=2&pagesize=10&sort=asc&time=1418745237
2. 按照时间查询趣图: 
    * http://japi.juhe.cn/joke/img/list.from?key=7bb45624a1ac6a2cd42719716d8f77f6&page=2&pagesize=10&sort=asc&time=1418745237
    *   sort	string	是	类型，desc:指定时间之前发布的，asc:指定时间之后发布的
    *  	time	string	是	时间戳（10位），如：1418816972
    *  	page	int	否	当前页数,默认1
    *  	pagesize	int	否	每次返回条数,默认1,最大20
    *  	key	string	是	您申请的key
3. 最新笑话: 
    * http://japi.juhe.cn/joke/content/text.from?key=7bb45624a1ac6a2cd42719716d8f77f6&page=1&pagesize=10
4. 最新趣图: 
    * http://japi.juhe.cn/joke/img/text.from?key=7bb45624a1ac6a2cd42719716d8f77f6&page=1&pagesize=10
    *  	page	int	否	当前页数,默认1
    *  	pagesize	int	否	每次返回条数,默认1,最大20
    *  	key	string	是	您申请的key
5. 随机笑话: 
    * http://v.juhe.cn/joke/randJoke.php?key=7bb45624a1ac6a2cd42719716d8f77f6&type=pic
    *  	key	string	是	你申请的key
    *   type	string	否	类型(pic:趣图,不传默认为笑话)

6. 随机趣图
    * http://v.juhe.cn/joke/randJoke.php?key=7bb45624a1ac6a2cd42719716d8f77f6
    *  	key	string	是	你申请的key
    *   type	string	否	类型(pic:趣图,不传默认为笑话)
 ---
 返回数据含义
 ```
error_code	int	    状态码
reason	  string	提示信息
result	  string	返回内容
content	  string	笑话内容或题目
hashId	  string	笑话或趣图的唯一标识
unixtime	long	笑话或趣图更新的时间戳
url	      string	趣图地址(只有是趣图的时候才返回此字段)
```

---


# api 接口---《微·知》
wiknow
> key =  12150ee73eef17e0eb4eed6d30d9498d


1. 微信精选: http://v.juhe.cn/weixin/query?key=12150ee73eef17e0eb4eed6d30d9498d&ps=10
    *    pno	int	否	当前页数，默认1
    *  	ps	int	否	每页返回条数，最大50，默认20
    *   key	string	是	应用APPKEY
    *  	dtype	string	否	返回数据的格式,xml或json，默认json
    


# 糗事

http://m2.qiushibaike.com/article/list/suggest?page=1&type=refresh&count=30







