---
title: Android 程序框架设计
date: 2016-07-14 11:25:11
tags: [框架设计,android] 
---
#Android 程序框架设计
###1、主要说明
	框架说明：
	1 基于HTTP协议进行通信
	2 利用json格式传输数据。
	客户端以POST方式用UTF-8编码提交网络请求，主要模块有通用framework,view视图，业务逻辑(系统调度)模块，本地数据处理模块，http模块，工具类。http请求解析，传输到业务处理模块，然后本地处理数据模块进行文件保存,若展示则 通过业务处理模块，最终在view视图上显示。

###2、设计细项
2.1 共通类的设计

	概念完整性：
	- 开发过程中，需求、设计、编码的一致性
	- 整个程序具有统一的风格，比如对话框样式，按钮风格，色调等UI元素
	- 整个程序具体统一的结构，比如不同模块访问网络，它们的调用方式一致，例如异步访问都用回调方式通知结果，相同的功能应该提取成共通模块。
	- 开发人员能很好的执行需求人员和设计人员的意图。
	- 有完整的文档，需求文档，设计文档，测试文档，处理流程的文档等。

2.1.1 Widget设计

	尽量在style文件中定义样式。
	TextView
	EditText
	Button
	Title bar
	Tool bar

### 为什么要提供这些共通控件？ ###
统一字体大小，如App字体不随系统字体变化而变化
统一UI式样，如Button， EditText具有相同的背景等
复用代码

2.1.2 Adapter Items

	- 根据式样，提取需要在AdapterView中显示的Item
	- 简单的复合布局
	- 自绘制，从而提高滑动性能
	- ListView中放Gallery时，提高上下滑动性能
	- 尽量优化绘制
	
### 数据驱动 ###

	Adapter Items提供核心的方法
	- setData(Object data)
	- getData();
	- Adapter#getView实现更加简单
	- 实现简单
	- 不会因为UI变化而变化
下面代码示例了Adapter#getView()方法的实现，它返回BookView，BookView提供方法来接收数据，至于BookView的显示，则根据设置的数据来显示，这就是数据驱动UI。

	@Override
	public View getView(int position, View convertView, ViewGroup parent) {
    if (null == convertView) {
        convertView = new BookView(getContext());
        convertView.setLayoutParameter(new AbsListView.LayoutParameter(150, 150));
    }

    Book book = m_bookList.get(position);
    BookView bookView = (BookView)convertView;
    bookView.setBook(book);

    return convertView;
	}
2.1.3 Dialog

	扩展于Dialog类
	提供Dialog关闭的事件
	Dialog的高度随内容的变化而变化
	可以设置按钮的文字，可见性，字体等方法
	设置按钮点击事件的listener
	要考虑对话框的三个属性：Title, Content area, Action buttons

2.1.4 Utility

	工具类统一放于一个包下面。
	Log
	DateFormat
	Bitmap
	Notification
	Shared Preference
	Environment
	Device
…
2.2 Task管理
线程只是一种机制，保证我们要完成的任务不运行在UI线程（也就是说不阻塞UI），完成的任务才是我们关注的核心，因此，我们可以通过设计，把线程封装，让使用者根本感觉不到是线程，他只用关心他要做的事情就行了。
这里，我们可以设计一种”异步链式调用”的框架，把线程进行了封装。使用都只需要这样用：

	new TaskManager()
	.next(task1)
	.next(task2)
	.next(task3).
	.execute();

这里，task1, task2, task3是顺序执行的，举个例子：我们要访问网络，取得一个图片，使用这个TaskManager我们需要3个task，
task1:显示一个ProgressDialog。
task2:访问网络，创建bitmap。
task3:关闭对话框，显示bitmap。
这一点，可以参考CoreLib工程中的task.TaskManager类。
关于TaskManager，有以下几点需要注意：
-封装了线程
让调用者只关注自己的业务处理
保证顺序链式地执行某一个任务
上一个任务的输出，作为下一个任务的输入
能暂停、恢复任何一个任务

2.3 缓存设计
	
	把内存占用量大的对象存放在缓存中，如bitmap
	利用了Cache类来实现
	利用了AsyncTask类来加载bitmap
	不用再手动释放bitmap内存，该操作有风险
	不用再关心AbsListView的scroll状态
	关于缓存的更多详细细节，请参考[ 请参考CoreLib工程中的cache包 ]。
	这样做，有什么好处， 不用再手动释放bitmap内存，该操作有风险，因为该bitmap是否有View引用，如果当一个View在试图绘制一个已经回收的bitmap，这里会抛出异常。
2.4 线程管理
无消息循环的线程：

	new Thread(null, new Runnable() {
    public void run() {
        // Do you works.
    }	
	}, "Thread_name_xxx").start();
什么情况下使用这种线程：

	做完一件事情就结束，这件事发生频率不高，比如从SD card中读取图片数据
	不需要复用线程
	在使用线程，最好给线程加上名字，这样利用高度与跟踪。
	有消息循环的线程：
	这样的线程拥有消息循环，当消息队列中没有消息时，这个线程会被挂起。我们要做一件事情时，只需要给它发送一个消息就行了。
	这种情况通常是为了复用线程，不用频繁创建线程，比如音乐播放器程序，专门启动一个有消息循环的线程来获得音乐的专辑图片。
	我们通常还要创建一个与这个线程的消息循环（Looper）相关联的Handler，由它来处理消息，注意，这做的事情是运行在后台线程的。

3，程序框架如何设计

	Android程序的结构
	UI层
	数据展示与管理
	用户交互
	绘制
	Adapter
	业务逻辑层
	持久化数据（内存中，相当于全局数据）
	数据加式（数据层的数据有时候需要进行加工成UI层需要的数据）
	数据变化的通知机制
	数据层
	数据访问（DB，文件，网络等）
	缓存（图片，文件等）
	配置文件（sharedpreferences）
下面，我试着画了一下Android程序的结构，如果有不好的地方，欢迎指正。
 
##4、一些基本原则
下面列出一些通常的原则，我们应当在开发过程中遵循，欢迎补充与指正。

4.1 提供initialize()方法
在Activity.onCreate()或者View的构造方法中调用，在以后看代码时，人们通常首先会去找initialize()这样的方法。
Initview();initListener().

4.2 封装点击事件
把View的点击事件，提成方法，这样在listener处只是一个方法调用者，一般的事件封装为：onXXXClick(View v)。

4.3 设计一个BaseActivity类
让所有的Activity都继承自BaseActivity类，这样，我们可以做很多有用的事情
-定义共通属性
显示共通对话框（Progress dialog)
取得top activity
可以手动管理启动的activity

4.4 设计Application类
存全局数据，比top activity, application context。

4.5 异常处理

	- 报告功能是处理异常的精髓
	- 在finally块中执行清理操作
	- 不要用try-catch-finally来判断业务逻辑
	- 考虑设计自定义的异常类

4.6 标注的使用
-重写的方法一定要加@Override
不使用的方法，不要删除，可以标记为@Deprecated，这个做法在维护型的项目中特别有用。

4.7 注册与反注册
-局部广播
各种listener
Service等

4.8 封装Bitmap操作
我们应当把Bitmap操作封装起来，比如从文件加载，保存，网络下载，动态计算sample size等。有了封装后，我们可以对其集中优化。

4.9 绘制处理
一定要注意绘制方面的东西，不要在onDraw()/onTouchEvent()中创建新对象。

