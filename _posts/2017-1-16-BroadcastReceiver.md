---
layout: post
title: "BroadcastReceiver"
date: 2017-1-16
excerpt: "BroadcastReceiver"
tags: Android
comments: true
---

# 广播的三要素
- 广播：用于发送广播
- 广播接收者：用于接收广播
- Intent意图对象：用于保存广播的相关消息

# 流程
1. 需要发送广播的地方，把过滤器和信息放入到Intent对象中
2. 调用``context.sendBroadcast(Intent intent)``或``context.sendOrderedBroadcast(Intent intent)``发送广播
3. 当广播发送后，已经注册的广播接收者会监测自己的过滤器，如果符合条件，则处理

# 定义
广播接收者可以接收某个频道(action)发送的广播，发送者可以是``Activity``，也可以是``Service``

# 作用
1. 监听系统的广播，也可以监听自己的广播，并且做出响应
	1. 监听自己的广播(自己发送，自己接收)
	2. 系统广播
2. 可以通过广播接收者将数据返回

# 发送广播
- 广播的分类
	1. 普通广播
		- 过滤器
		- 传递的内容
		- 步骤
			1. 得到Intent意图对象

				```
				Intent intent = new Intent();
				```
				
			2. 添加频道
			
				```
				intent.setAction("this.is.a.boradcast");
				```
				
			3. 添加传递信息
			 
				```
				intent.putExtra("msg","这是一条广播");
				```
				
			4. 发送广播
				
				```
				context.sendBroadcast(Intent intent);
				```
		
	2. 有序广播
		
		可以按照接收的优先级，进行排序，高优先级的先接收
		
		- 过滤器
		- 传递的内容
		- 权限
		- 步骤
			1. 得到Intent意图对象

				```
				Intent intent = new Intent();
				```
				
			2. 添加频道
			
				```
				intent.setAction("this.is.a.boradcast");
				```
				
			3. 添加传递信息
			 
				```
				intent.putExtra("msg","这是一条广播");
				```
				
			4. 发送广播
				
				```
				/**
				 * intent				intent对象
				 * receiverPermisson	权限
				 */
				context.sendOrderedBroadcast(Intent intent, String receiverPermisson);
				```

	3. 粘性广播

# 接收广播
1. 定义一个类，继承BroadcastReceiver
	
	```
	public class MyReceiver extends BroadcastReceiver
	```
	
2. 重写父类的``onReceive(Context context, Intent intent)``方法
	
	```
	@Override
	public void onReceive(Context context, Intent intent) {
	    String msg = intent.getStringExtra("msg");
	    Toast.makeText(context, "接收到了广播,msg：" + msg, Toast.LENGTH_SHORT).show();
	}
	```
	
3. 注册广播
	1. 静态注册
		
		- 在清单文件中注册``<receiver/>``节点
		
			```
			<!--静态方式注册广播接收者，android:name指定当前注册的是哪个接收者，全类名-->
	        <receiver android:name=".MyReceiver">
	            <intent-filter>
	                <action android:name="this.is.a.boradcast" />
	            </intent-filter>
	        </receiver>
			```
			
		- 注意
			- 静态注册是全局的，即使应用程序没有启动，只要没有卸载，就可以监听到指定广播
		
	2. 动态注册
		- 在Activity的``onCreate()``方法中注册
		
			```
			//得到过滤对象
			IntentFilter filter = new IntentFilter();
			//添加过滤信息
			filter.addAction("this.is.a.boradcast");
			//注册广播接收者
			MyReceiver recevier = new MyReceiver();
			registerReceiver(recevier, filter);
			```
			
		- 在Activity的``onDestory()``方法中解除注册
			
			```
			unregisterReceiver(recevier);
			```
			
		- 注意
			- 动态注册是局部的，是应用程序内部的

# 注意
1. 广播接收者的生命周期是非常短暂的，在接收到广播的时候创建，onReceive()方法结束之后销毁
2. 广播接收者中不要做一些耗时的工作，否则会弹出Application No Response错误对话框
3. 最好也不要在广播接收者中创建子线程做耗时的工作，因为广播接收者被销毁后进程就成为了空进程，很容易被系统杀掉
4. 耗时的较长的工作最好放在服务中完成