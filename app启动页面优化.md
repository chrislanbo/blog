---
title: app启动页面优化
date: 2016-09-14 14:24:21
tags:
---


##启动页面优化
启动页面主要用于页面展示，加载网络配置信息，跳转到首页

* 加载H5页面
* 加载请求用户信息
* 定时跳转，无网络也会跳转首页

网络跳转优化

	timer = new Timer();
	timerTask = new TimerTask() {
	@Override
	public void run() {
	if (isComplete) {
	L.i("等待2s完成，取消timer任务...");
	cancelTimer();
	return;
	}
	if (isLoadStartQueryComplete && isLoadUserInfoComplete) {
	// 已经显示过引导图
	L.i("StartActivity中网络请求完成，执行跳转逻辑.....");
	handler.sendEmptyMessage(eventWhat);
	L.i("StartActivity中网络请求完成，取消timer任务...");
	cancelTimer();
	}
	}
	};
	timer.schedule(timerTask, 0, 100);

创建一个timer任务，每隔100ms执行一次，判断这两个拉取网络配置信息的异步消息是否已经完成，若已经完成的话则发送handler消息跳转主界面，若2秒钟之内都没有完成拉取任务，则直接跳转同时取消timer任务。
<!--more-->
页面特效让用户不关注加载时间

	//渐变展示启动屏
        AlphaAnimation alphaAnimation = new AlphaAnimation(0.3f,1.0f);
        alphaAnimation.setDuration(2000);
        contentView.startAnimation(alphaAnimation);
        alphaAnimation.setAnimationListener(new AnimationListener() {

            @Override
            public void onAnimationStart(Animation animation) {
                L.i("启动页面动画开始执行...")
            }

            @Override
            public void onAnimationEnd(Animation arg0) {
                L.i("启动页面动画执行完毕...");
            }

            @Override
            public void onAnimationRepeat(Animation animation) {
                L.i("启动页面动画重复执行...");
            }


        });
contentView就是我们启动页Activity的主View，所以这里的执行效果就是我们的启动页Activity的界面的透明度逐渐增加，这样在用户看来由于有了一层loading效果，加载速度显得很快。

##启动页面黑屏处理
App启动页面为图片或者是drawable文件

	<item name="Android:windowBackground">@drawable/start_background</item>
设置设置整个App的style文件，设置Android:windowBackground属性为图片或者是drawable文件，这样App在启动过程中就是显示的是启动页面的图片或者是是drawable文件;

App启动页面为Layout文件，无法为App的style设置background，退而求其次主流的方式是设置白色背景

	<item name="Android:windowBackground">@color/c10</item>

这里的@color/c10就是我们定义的白色色值，这样经过处理之后App启动时就不会出现黑屏的效果了。

## Application启动速度优化 

* 业务逻辑不放在application
* 尽量不以静态变量的方式保存应用数据
* 尽量不使用dex分包方案
* 文件，数据库的操作尽量


##启动页面屏蔽返回按键


	@Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (keyCode == KeyEvent.KEYCODE_BACK) {
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }

重写onKeyDown方法，首先判断用户按下的是否是返回按键，若是的话则直接返回true，屏蔽返回按键的后续执行逻辑。
