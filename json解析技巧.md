---
title: json解析技巧
date: 2016-09-21 15:23:27
tags: [json, 总结]
---


List<Car> cars   返回填充对象的集合
Car[] cars 返回对象的数组

<!--more-->
	{
	    "data": [
	        {
	            "children": [
	                {
	                    "id": 10007,
	                    "title": "北京",
	                    "type": 1,
	                    "url": "/10007/list_1.json"
	                },
	                {
	                    "id": 10006,
	                    "title": "中国",
	                    "type": 1,
	                    "url": "/10006/list_1.json"
	                },
	                {
	                    "id": 10008,
	                    "title": "国际",
	                    "type": 1,
	                    "url": "/10008/list_1.json"
	                },
	                {
	                    "id": 10010,
	                    "title": "体育",
	                    "type": 1,
	                    "url": "/10010/list_1.json"
	                },
	                {
	                    "id": 10091,
	                    "title": "生活",
	                    "type": 1,
	                    "url": "/10091/list_1.json"
	                },
	                {
	                    "id": 10012,
	                    "title": "旅游",
	                    "type": 1,
	                    "url": "/10012/list_1.json"
	                },
	                {
	                    "id": 10095,
	                    "title": "科技",
	                    "type": 1,
	                    "url": "/10095/list_1.json"
	                },
	                {
	                    "id": 10009,
	                    "title": "军事",
	                    "type": 1,
	                    "url": "/10009/list_1.json"
	                },
	                {
	                    "id": 10093,
	                    "title": "时尚",
	                    "type": 1,
	                    "url": "/10093/list_1.json"
	                },
	                {
	                    "id": 10011,
	                    "title": "财经",
	                    "type": 1,
	                    "url": "/10011/list_1.json"
	                },
	                {
	                    "id": 10094,
	                    "title": "育儿",
	                    "type": 1,
	                    "url": "/10094/list_1.json"
	                },
	                {
	                    "id": 10105,
	                    "title": "汽车",
	                    "type": 1,
	                    "url": "/10105/list_1.json"
	                }
	            ],
	            "id": 10000,
	            "title": "新闻",
	            "type": 1
	        },
	        {
	            "id": 10002,
	            "title": "专题",
	            "type": 10,
	            "url": "/10006/list_1.json",
	            "url1": "/10007/list1_1.json"
	        },
	        {
	            "id": 10003,
	            "title": "组图",
	            "type": 2,
	            "url": "/10008/list_1.json"
	        },
	        {
	            "dayurl": "",
	            "excurl": "",
	            "id": 10004,
	            "title": "互动",
	            "type": 3,
	            "weekurl": ""
	        }
	    ],
	    "extend": [
	        10007,
	        10006,
	        10008,
	        10014,
	        10012,
	        10091,
	        10009,
	        10010,
	        10095
	    ],
	    "retcode": 200
	}

===========


	public class NewCenter {
	
		public List<NewItem> data;	
		public List<String> extend;
		public int retcode;
		public class NewItem{
			public List<Children> children;
			public int id;
			public String title;
			public String type;
			public String url;
			public String url1;
			public String dayurl;
			public String excurl;
			public String weekurl;
		}
		public class Children{
			public int id;
			public String title;
			public String type;
			public String url;
		}
	}


1. 父为json对象子json数组 用List(子为数据就用String，子为对象就用class)包裹 集合名为key
2. 父为json数组子json对象 用class包裹(子为数据就写成变量【多个对象写一个里面】，子为数组用List包裹)
3. 父为json对象子json对象 
}