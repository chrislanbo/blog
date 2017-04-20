---
title: '[转] Android 5.0+(RecycleView、CardView、Palette)'
date: 2017-03-06 10:01:42
tags:
---
[转] Android 5.0+(RecycleView、CardView、Palette)

﻿Android L 开发者预览支持库提供两个新的Widgets，RecyclerView和CardView。使用这两个Widgets可以显示复杂的Listview和卡片布局，这两个Widgets默认使用Material design。

RecyclerView

    RecyclerView是一个更高级柔性版本的Listview，RecyclerView是一个能包含很多视图的容器，它能完美的处理循环和滚动。在item动态变化的Listview使用RecyclerView。
RecyclerView使用很简单，因为它提供了：

1、定位item的布局管理器

2、常见的item操作默认动画



你能够灵活的为RecyclerView自定义布局管理器和动画。

使用RecyclerView，必须使用指定一个adapter、定义一个布局管理器。创建adapter必须继承自RecyclerView.Adapter。实施的细节需要看数据类型和需要的视图。



   RecyclerView widget





RecyclerView 提供了 LayoutManager，RecylerView 不负责子 View 的布局

目前提供了

LinearLayoutManager(显示垂直或水平滚动列表中的条目。)

GridLayoutManager(在一个网格显示项)

StaggeredGridLayoutManager（在交错网格显示项。）

以上如果不满足要求，那么你可以继承RecyclerView.LayoutManager


Animations

动画添加和删除项目在RecyclerView默认启用。自定义动画只需要继承RecyclerView.ItemAnimator类并使用RecyclerView.setItemAnimator()方法。



使用：

studio build.gradle  添加

	dependencies {
	compile 'com.android.support:recyclerview-v7:22.2.0’
	}




RecyclerView Demo：

1、布局文件


	<!-- A RecyclerView with some commonly used attributes -->
	<android.support.v7.widget.RecyclerView
	android:id="@+id/my_recycler_view"
	android:scrollbars="vertical"
	android:layout_width="match_parent"
	android:layout_height="match_parent"/>

2、Activity文件


	public class MyActivity extends Activity {
	private RecyclerView mRecyclerView;
	private RecyclerView.Adapter mAdapter;
	private RecyclerView.LayoutManager mLayoutManager;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.my_activity);
	mRecyclerView = (RecyclerView) findViewById(R.id.my_recycler_view);
	// improve performance if you know that changes in content
	// do not change the size of the RecyclerView
	mRecyclerView.setHasFixedSize(true);
	// use a linear layout manager
	mLayoutManager = new LinearLayoutManager(this);
	mRecyclerView.setLayoutManager(mLayoutManager);
	// specify an adapter (see also next example)
	mAdapter = new MyAdapter(myDataset);
	mRecyclerView.setAdapter(mAdapter);
	}
	...
	}

To create a simple adapter:


	public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
	private String[] mDataset;
	// Provide a reference to the type of views that you are using
	// (custom viewholder)
	public static class ViewHolder extends RecyclerView.ViewHolder {
	public TextView mTextView;
	public ViewHolder(TextView v) {
	super(v);
	mTextView = v;
	}
	}
	// Provide a suitable constructor (depends on the kind of dataset)
	public MyAdapter(String[] myDataset) {
	mDataset = myDataset;
	}
	// Create new views (invoked by the layout manager)
	@Override
	public MyAdapter.ViewHolder onCreateViewHolder(ViewGroup parent,int viewType) {
	// create a new view
	View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.my_text_view, null);
	// set the view's size, margins, paddings and layout parameters
	...
	ViewHolder vh = new ViewHolder(v);
	return vh;
	}
	// Replace the contents of a view (invoked by the layout manager)
	@Override
	public void onBindViewHolder(ViewHolder holder, int position) {
	// - get element from your dataset at this position
	// - replace the contents of the view with that element
	holder.mTextView.setText(mDataset[position]);
	}
	// Return the size of your dataset (invoked by the layout manager)
	@Override
	public int getItemCount() {
	return mDataset.length;
	}
	}


3、Recycler adapter


	public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
	private String[] mDataset;
	// Provide a reference to the type of views that you are using
	// (custom viewholder)
	public static class ViewHolder extends RecyclerView.ViewHolder {
	public TextView mTextView;
	public ViewHolder(TextView v) {
	super(v);
	mTextView = v;
	}
	}
	// Provide a suitable constructor (depends on the kind of dataset)
	public MyAdapter(String[] myDataset) {
	mDataset = myDataset;
	}
	// Create new views (invoked by the layout manager)
	@Override
	public MyAdapter.ViewHolder onCreateViewHolder(ViewGroup parent,int viewType) {
	// create a new view
	View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.my_text_view, null);
	// set the view's size, margins, paddings and layout parameters
	...
	ViewHolder vh = new ViewHolder(v);
	return vh;
	}
	// Replace the contents of a view (invoked by the layout manager)
	@Override
	public void onBindViewHolder(ViewHolder holder, int position) {
	// - get element from your dataset at this position
	// - replace the contents of the view with that element
	holder.mTextView.setText(mDataset[position]);
	}
	// Return the size of your dataset (invoked by the layout manager)
	@Override
	public int getItemCount() {
	return mDataset.length;
	}
	}


RecyclerView 的标准化了 ViewHolder， 编写 Adapter 面向的是 ViewHoder 而不在是View 了， 复用的逻辑被封装了， 写起来更加简单。

CardView

CardView继承自FrameLayout类，可以在一个卡片布局中一致性的显示内容，卡片可以包含圆角和阴影。

可以使用android:cardElevation属性，创建一个阴影的卡片。

怎样指定CardView的属性：

1、使用android:cardCornerRadius属性指定圆角半径

2、使用CardView.setRadius 设置圆角半径。

3、使用 android:cardBackgroundColor属性设置卡片颜色
compile 'com.android.support:cardview-v7:22.2.0’

Palette
根据图片来决定标题的颜色和标题栏的背景色，这样视觉上更具有冲击力和新鲜感，而不像统一色调那样呆板。
compile 'com.android.support:palette-v7:22.2.0'

//获取一张位图
Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.img_test);

//获取一个Builder
Palette.Builder from = Palette.from(bitmap);

//生成颜色 Builder.generate().getter方法 这里简单演示
int color =from.generate().getDarkVibrantColor(getResources().getColor(android.R.color.transparent));
mHead.setBackgroundColor(color);