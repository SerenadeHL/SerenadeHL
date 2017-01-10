---
layout: post
title: "ActionBar"
date: 2017-1-9
excerpt: "ActionBar"
tags: Android
comments: true
---

# ActionBar
ActionBar是Android3.0之后出现的，在Android3.0之前叫TitleBar，显示在标题栏上，可以显示图标，名称，按钮

- ``showAsAction``
	- ``ifRoom``：如果有空间则显示, 如果没空间不显示
	- ``always``：一直显示在actionBar上
	- ``never``：不显示在ActionBar上，只会在溢出列表中显示，而且只显示标题，所以在定义item的时候，最好把标题都带上。    
	- ``withText``：withText值示意ActionBar要显示文本标题。ActionBar会尽可能的显示这个标题，但是，如果图标有效并且受到ActionBar空间的限制，文本标题有可能显示不全。
	- ``collapseActionView``：声明了这个操作视窗应该被折叠到一个按钮中，当用户选择这个按钮时，这个操作视窗展开。否则，这个操作视窗在默认的情况下是可见的，并且即便在用于不适用的时候，也要占据操作栏的有效空间，一般要配合ifRoom一起使用才会有效果。

# ActionBar提供的主要功能
1. 显示菜单项的选项
2. 可以使应用程序的图标实现返回上一级的功能
3. 提供了交互的View
4. 实现了导航的切换(Tab)，切换Fragment
5. 下拉导航

# 创建ActionItem方式
1. 在``res/menu``中创建菜单文件，并声明``<item/>``节点
2. 在Activity中调用父类的``onCreateOptionsMenu()``方法加载菜单布局
3. 在Activity中调用父类的``onOptionsItemSelected()``方法实现每个选项的点击

# 启动应用程序的导航图标
- 作用
	- 可以让当前App的图标作为可以点击的图标
	- 得到ActionBar对象：
	- ``getActionBar()``：API 11以上
	- ``getSupportActionBar()``：API 11以下
	- ``setDisplayHomeAsUpEnabled(true)``：设置应用程序的图标为可点击的, 并且图标左侧会出现一个向左的箭头
	- ``setHomeButtonEnabled(true)``：设置应用程序的图标为可点击的，API 14以上
- 得到导航图标的ID:
	- ``android.R.id.home``：处理事件的方法在``onOptionsItemSelected()``

# ActionBar中常用的方法
- ``isShowing()``：判断当前ActionBar是否正在显示
- ``hide()``：隐藏ActionBar
- ``show()``：显示ActionBar

# 去除ActionBar的两种方式
1. 在清单文件中, 为<application/>或<activity/> 添加主题:``android:theme="@android:style/Theme.Light.NoTitleBar" ``
2. 在Activity的onCreate() 方法中, 第一执行:``requestWindowFeature(Window.FEATURE_NO_TITLE);``，该方法一定要执行在``setContentView()``之前！！！

# ActionView
- 作用：
	- 可编辑的动作项，如``SearchView``可以直接显示在ActionBar上
- 添加方式(两种)：
	1. ``actionViewClass``：
		- 步骤：
			1. 在菜单页面中，通过``android:actionViewClass="android.widget.SearchView"``引用
			2. 在Activity的``onCreateOptionsMenu()``方法中：
         	
				```
         		//得到SearchView所在的菜单Item
         		MenuItem item = menu.findItem(R.id.***);
         		//通过菜单Item对象，得到SearchView控件
         		SearchView searchView = (SearchView) item.getActionView();
         		//为SearchView设置监听器
         		searchView.setOnClickListener();
				```
				
	2. ``actionViewClass``：
		- 步骤：
			1. 在菜单页面中, 通过``android:actionLayout="@layout/****"``引用
			2. 在Activity的``onCreateOptionsMenu()``方法中:
		  		
		  		```
		  		//得到View所在的菜单Item
				MenuItem item = menu.findItem(R.id.****);
				//通过菜单Item对象, 得到view控件
				View view = item.getActionView();
				//得到View中的控件
				EditText et = (EditText)view.findViewById(R.id.et);
				Button button = (Button) view.findViewById(R.id.button);
				//为searchView添加事件
				button.setOnClickListener(.....);
		  		```

# Tab导航功能
通过选项标签来切换Fragment：

1. 得到ActionBar对象，并且设置导航模式为``TABS``

	```
	ActionBar actionBar = getActionBar();
	/**
	 * mode  模式
	 * NAVIGATION_MODE_STANDARD   标准模式
	 * NAVIGATION_MODE_LIST       标签模式
	 * NAVIGATION_MODE_TABS       Tab标签模式
	 */
	actionBar.setNavigationMode(int mode);
	```

2. 让当前类实现``TabListener``接口，重写3个方法

	```
	@Override
	public void onTabSelected(ActionBar.Tab tab, FragmentTransaction ft) {
	    //TODO 选择Tab的事件
	}
	
	@Override
	public void onTabUnselected(ActionBar.Tab tab, FragmentTransaction ft) {
	    // TODO 取消选择Tab事件
	}
	
	@Override
	public void onTabReselected(ActionBar.Tab tab, FragmentTransaction ft) {
	    //TODO 重新选择Tab事件
	}
	```

3. 创建每个Tab项，并且增加到actionBar中

	```
	ActionBar.Tab tab = actionBar.newTab();
	tab.setText("新闻");//设置显示的文字
	tab.setIcon(R.mipmap.ic_launcher);//设置显示的图标
	tab.setTabListener(this);//设置监听器
	//将创建的Tab项添加到ActionBar当中
	/**
	 * tab          Tab项
	 * setSelected  设置是否默认选中
	 */
	actionBar.addTab(tab,true);
	```