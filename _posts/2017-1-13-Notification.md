---
layout: post
title: "Notification"
date: 2017-1-13
excerpt: "Notification"
tags: Android
comments: true
---

# Notification
位于标题栏下，通知用户的信息，通常是应用程序的推送信息

- 组成
	- 大图标
	- 小图标(必须设置，如果没有大图标，小图标显示在大图标的位置上)
	- 标题
	- 内容
	- 发送时间(不需要指定，系统自动生成)
	- 内容的附加信息

- Builder常用方法
	- ``setAutoCancel(boolean autoCancel)``：设置通知点击后，是否自动删除通知
	- ``setContentInfo(CharSequence info)``：添加通知内容的附加信息
	- ``setContentIntent(PendingIntent intent)``：设置点击通知后，跳转到二级页面
	- ``setContentText(CharSequence text)``：设置通知的内容
	- ``setContentTitle(CharSequence title)``：设置通知的标题
	- ``setDefaults(int defaults)``：设置声音和震动(震动需要添加权限)
	- ``setLargeIcon(Icon icon)``：设置大图标
	- ``setSmallIcon(Icon icon)``：设置小图标
	- ``setPriority(int pri)``：设置通知优先级
	- ``setProgress(int max, int progress, boolean indeterminate)``：设置进度条
	- ``setStyle(Notification.Style style)``：设置样式
	- ``setTicker(CharSequence tickerText)``：设置通知的提示文本信息
	- ``setOngoing(boolean bool)``：设置通知是否一直显示

- NotificationManager方法
	- ``notify(int id, Notification notification)``：发送通知
	- ``cancle(int id)``：清除指定Id的通知
	- ``cancleAll()``：清除所有通知，包括其他应用发送的通知
	
- 普通通知
	1. 得到通知管理器对象NotificationManager
		
		```
		NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
		```
		
	2. 构建通知
		
		```
		NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
		builder.set***();
		Notification notification = builder. build();
		```
		
	3. 发送通知
		
		```
		/**
		 * id			通知的Id(可以为任意数字，但是必须是唯一的)
		 * notification		通知
		 */
		manager.notify(id, notification);
		```
		
- 进度条通知
	1. 得到通知管理器对象NotificationManager
		
		```
		NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
		```
		
	2. 构建通知
		
		```
		NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
		builder.set***();
		/**
		 * max				进度条的最大值
		 * progress			当前进度值
		 * indeterminate		是否为不确定型进度条，true为不确定(模糊)，false为确定(精确)
		 */
		builder.setProgress(max, progress, indeterminate);
		Notification notification = builder. build();
		```
		
	3. 发送通知
		
		```
		/**
		 * id			通知的Id(可以为任意数字，但是必须是唯一的)
		 * notification		通知
		 */
		manager.notify(id, notification);
		```
		
- 列表通知
	1. 得到通知管理器对象NotificationManager
		
		```
		NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
		```
		
	2. 构建通知
		
		```
		NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
		builder.set***();
		//实例化列表通知的样式
		NotificationCompat.InboxStyle style = new NotificationCompat.InboxStyle();
		style.addLine("列表项1").addLine("列表项2");
		//设置通知为列表样式
		builder.setStyle(style);
		Notification notification = builder. build();
		```
		
	3. 发送通知
		
		```
		/**
		 * id			通知的Id(可以为任意数字，但是必须是唯一的)
		 * notification		通知
		 */
		manager.notify(id, notification);
		```
		
- 大图片的通知
	1. 得到通知管理器对象NotificationManager
		
		```
		NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
		```
		
	2. 构建通知
		
		```
		NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
		builder.set***();
		//实例化大图片通知的样式
		NotificationCompat.BigPictureStyle style = new NotificationCompat.BigPictureStyle();
		style.bigPicture(bitmap);
		//设置通知为大图片样式
		builder.setStyle(style);
		Notification notification = builder. build();
		```
		
	3. 发送通知
		
		```
		/**
		 * id			通知的Id(可以为任意数字，但是必须是唯一的)
		 * notification		通知
		 */
		manager.notify(id, notification);
		```

- 自定义通知
	1. 得到通知管理器对象NotificationManager
		
		```
		NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
		```
		
	2. 构建通知
		
		```
		NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
		builder.set***();
		/**
		 * packageName	包名，可以通过getPackageName()获得
		 * resId			布局页面的Id
		 */
		RemoteViews views = new RemoteViews(String packageName, int resId);
		//设置控件上显示的内容
		views.setTextViewText(控件Id, 控件上显示的内容);
		//设置自定义视图
		builder.setContent(views);
		Notification notification = builder. build();
		```
		
	3. 发送通知
		
		```
		/**
		 * id			通知的Id(可以为任意数字，但是必须是唯一的)
		 * notification		通知
		 */
		manager.notify(id, notification);
		```