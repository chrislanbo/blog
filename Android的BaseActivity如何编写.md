---
title: Android的BaseActivity如何编写
date: 2016-09-14 14:25:19
tags:
---

BaseActivity如何编写
主要用于重写一些共有的逻辑

##在BaseActivity的生命周期中复写友盟数据统计方法



##保存Context对象，保存activity栈
BaseActivity.java

	mContext=this;
	Config.setActivityState(this);

Config.java

	public static void setActivityState(Activity activity){
	currentContext = activity;
	activity.setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
	//自定义栈
	UUApp.getInstance().addActivity(activity);
	}

BaseActivity.java

	@Override
    protected void onDestroy() {
        super.onDestroy();
        // 在BaseActivity中的Activity栈中移除Activity
        UUApp.getInstance().removeActivity(this);
    }

##在生命周期方法中对事件总线框架EventBus进行注册和反注册
<!--more-->
BaseActivity.java

	onCreate(Bundle savedInstanceState){
	..
	EventBus.getDefault().register(this);
	..
	}
	
	onDestroy(){
	..
	EventBus.getDefault().register(this);
	..
	}


##onResume更新登录信息


##setContentView重写加载布局文件的逻辑，统一App页面布局

	@Override
    public void setContentView(int layoutResID) {
        View view = getLayoutInflater().inflate(R.layout.activity_base_layout, null);
        super.setContentView(view);

        if (Build.VERSION.SDK_INT == Build.VERSION_CODES.KITKAT) {
            view.setFitsSystemWindows(true);
            setTranslucentStatus(true);
            SystemBarTintManager tintManager = new SystemBarTintManager(this);
            tintManager.setStatusBarTintEnabled(true);
            tintManager.setStatusBarTintResource(R.color.colorPrimaryDark);

        }

        initDefaultView(layoutResID);
        initDefaultToolBar();

    }

	/**
	     * 初始化默认的布局组件
	     *
	     * @param layoutResID
	     */
    private void initDefaultView(int layoutResID) {
        mProgressLayout = (ProgressLayout) findViewById(R.id.progress_layout);
        mProgressLayout.setAttachActivity(this);
        mProgressLayout.setUseSlideBack(false);
        mToolbarContainer = (FrameLayout) findViewById(R.id.toolbar_container);
        mDefaultToolBar = (Toolbar) findViewById(R.id.default_toolbar);
        mToolbarTitle = (TextView) findViewById(R.id.toolbar_title);
        mRvRight = (RippleView) findViewById(R.id.rv_right);
        mRightOptButton = (TextView) findViewById(R.id.right_opt_button);
        mContentContainer = (FrameLayout) findViewById(R.id.fl_content_container);

        View childView = layoutInflater.inflate(layoutResID, null);
        mContentContainer.addView(childView, 0);
    }

	/**
	     * 初始化默认的ToolBar
	     */
    private void initDefaultToolBar() {
        if (mDefaultToolBar != null) {
            String label = getTitle().toString();
            setTitle(label);
            setSupportActionBar(mDefaultToolBar);
            mDefaultToolBar.setNavigationIcon(R.mipmap.toolbar_back_icn_transparent);
        }
    }


##执行共有的UI操作，比如显示Toast，显示SnakeBar，显示Progress，显示Dialog等
	public void showToast(String text) {
        if (text != null && !text.trim().equals("")) {
            Toast.makeText(getApplicationContext(), text, Toast.LENGTH_SHORT).show();
        }
    }

	public void showProgress(boolean canCancled, final Config.ProgressCancelListener listener) {
        Config.showProgressDialog(mContext, canCancled, listener);
    }

    public void dismissProgress() {
        Config.dismissProgress();
    }

	public void showResponseCommonMsg(HeaderCommon.ResponseCommonMsg msg) {
        if (msg.getMsg() != null && msg.getMsg().length() > 0) {
            if (msg.hasShowType()) {
                if (msg.getShowType().equals(HeaderCommon.ResponseCommonMsgShowType.TOAST)) {
                    showSnackBarMsg(msg.getMsg());
                } else if (msg.getShowType().equals(HeaderCommon.ResponseCommonMsgShowType.WINDOW)) {
                    showResponseCommonMsg(msg, null);
                }
            } else {
                showSnackBarMsg(msg.getMsg());
            }
        }
    }