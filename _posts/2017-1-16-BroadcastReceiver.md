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
		
		```
		
		```
		
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
				
			2. 设置频道
			
				```
				intent.setAction("this is a boradcast");
				```
				
			3. 设置内容
			 
				```
				intent.putExtra("msg","这是一条广播");
				```
				
			4. 发送广播
				
				```
				context.sendBroadcast(Intent intent)
				```
		
	2. 有序广播
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
        Toast.makeText(context, "接收到了广播,msg:" + msg, Toast.LENGTH_SHORT).show();
    }
	```
	
3. 注册广播
	1. 静态注册
		
		```
		<!--静态方式注册广播接收者，android:name指定当前注册的是哪个接收者，全类名-->
        <receiver android:name=".MyReceiver">
            <intent-filter>
                <action android:name="this is a boradcast" />
            </intent-filter>
        </receiver>
		```
		
	2. 动态注册
		
		```
		IntentFilter filter = new IntentFilter("this is a boradcast");
       registerReceiver(new MyReceiver(),filter);
		```