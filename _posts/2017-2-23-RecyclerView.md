---
layout: post
title: "RecyclerView"
date: 2017-2-23
excerpt: "RecyclerView"
tags: Android
comments: true
---

# RecyclerView
>官方介绍：在有限的窗口中展示大量的数据，关心View的回收和利用，更加灵活的视图，是ListView的升级版
>
>在Android 5.0后引进的，需要导入support-v7包进行使用

- 控制显示的方式，通过布局管理器：LayoutManager
	- ``LinearLayoutManager``：ListView方式显示数据
	- ``StaggeredGridLayoutManager``：瀑布流方式显示数据
	- ``GridLayoutManager``：GridView方式显示数据
- 为Item的增加和删除添加动画效果：ItemAnimator
- 为Item之间设置间隔(可以绘制)：ItemDecoration

## 使用方式
1. 导入依赖
	
	```
	compile 'com.android.support:recylerview-v7:23.3.0'
	```
	
2. 在布局页面中引入``<android.support.v7.widget.RecylerView/>``
3. 在代码中：
	1. 初始化数据源
	2. 定义适配器
		- 继承RecylerView.Adapter<继承RecylerView.ViewHolder>
		- 重写父类方法
	3. 声明布局管理器LayoutManager并设置给RecylerView