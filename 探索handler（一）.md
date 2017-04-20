---
title: 探索Handler（一）
date: 2016-08-14 14:25:19
tags: [Retrofit2 , android]
---


`
private Handler handler = new Handler(){
@override
public void handleMessage(Message msg){
	super.handleMessage(msg);
}
}
`

This Handler class should be static or leaks might occur
这个handler 类应该被static修饰，否则可能反正内存泄露
since this handler is declared as an inner class, it may prevent the outer class from being garbage collected.
一旦handler被声明成内部类，它会阻止外部类被内存回收
if the handler is using a looper or messsagequeue for a thread other than the main thread,then there is no isses
如果在非主线程使用looper或者消息队列，则不会发生该问题
or you need to fix your handler declaration .
若在主(UI)线程，就必须进行修复
as follows:
declare the handler as a static class. in the outer class, instantiate a weakreference to the outer class and pass this object to your handler when you instantiate the handler 
用static 修饰，并使用weakreference声明外部类的引用
make all reference to members of the outer class using the weakreference object


	static class MHandler extend Handler{
	
	WeakReference<xxActivity> mActivity; 
	
	MyHandler(xxActivity activity) { 
	mActivity = new WeakReference<>(activity); 
	}
	@override
	public void handleMessage(Message msg){
		super.handleMessage(msg);
	}
	...
	}

或者在主线程中新开一个线程，在该线程中调用Looper.prepare()和Looper.looper()创建出handler的环境


而针对这种内存泄露
内部类有一个指向外部类的引用
内存中的对象的引用计数器为0时，就将会被回收
在异步消息处理机制中,要维护一个Looper处理messageQueue，每次循环取出一个message，回调处理信息

但handler一直在处理Message，那就会一直享有外部类的引用，外部类的对象就不能被回收