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
		- 监测电池电量(动态注册)
			- action：``Intent.ACTION_BATTERY_CHANGED``
			- 静态注册：``<action android:name="android.intent.action.BATTERY_CHANGED"/>``
				
				```
				//1. 得到当前的系统电量等级
				int level = intent.getIntExtra("level", 100);
				//2. 得到系统总电量
				int total = intent.getIntExtra("scale", 100);
				//3. 计算当前电量的百分比
				int current = level * 100 / total;
				Toast.makeText(context, "当前电量为：" + current, Toast.LENGTH_SHORT).show();
				```
				
		- 监测网络状态(动态注册)
			- action：``ConnectivityManager.CONNECTIVITY_ACTION``或``android.net.conn.CONNECTIVITY_CHANGE``
			- 静态注册：``<action android:name="android.net.conn.CONNECTIVITY_CHANGE" />``
			- 权限：``<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />``
				
				```
				//1. 获取链接管理器
				ConnectivityManager manager = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);
				//2. 获取网络状态
				NetworkInfo info = manager.getActiveNetworkInfo();
				if (info != null) {
				    if (info.isConnected()) {
				        Toast.makeText(context, "网络连接成功", Toast.LENGTH_SHORT).show();
				
				    } else {
				        Toast.makeText(context, "网络连接异常", Toast.LENGTH_SHORT).show();
				    }
				} else if (info == null) {
				    Toast.makeText(context, "网络连接异常", Toast.LENGTH_SHORT).show();
				}
				```
				
		- 监测用户拨打电话(静态注册)
			- action：``Intent.ACTION_NEW_OUTGOING_CALL``或``android.intent.action.NEW_OUTGOING_CALL``
			- 静态注册：``<action android:name="android.intent.action.NEW_OUTGOING_CALL"/>``
			- 权限：``<uses-permission android:name="android.permission.PROCESS_OUTGOING_CALLS" />``
				
				```
				
				```
				
		- 监测来电状态(静态注册)
			- action：``android.intent.action.PHONE_STATE``
			- 静态注册：``<action android:name="android.intent.action.PHONE_STATE" />``
			- 权限：``<uses-permission android:name="android.permission.READ_PHONE_STATE"/>``
				
				```
				//1. 获取电话管理器对象
				TelephonyManager manager = (TelephonyManager) context.getSystemService(Context.TELEPHONY_SERVICE);
				//2. 获得来电号码
				Bundle bundle = intent.getExtras();
				String number = bundle.getString(TelephonyManager.EXTRA_INCOMING_NUMBER);
				//3. 获取电话的状态
				int state = manager.getCallState();
				switch (state) {
				//挂断状态
				case TelephonyManager.CALL_STATE_IDLE:
				    Log.e("挂断：",number);
				    break;
				//接听状态
				case TelephonyManager.CALL_STATE_OFFHOOK:
				    Log.e("接听：",number);
				    break;
				//响铃状态
				case TelephonyManager.CALL_STATE_RINGING:
				    Log.e("响铃：",number);
				    break;
            }
				```
				
		- 监测接收的短信
			- action：``android.provider.Telephony.SMS_RECEIVED``
			- 静态监测：``<action android:name="android.provider.Telephony.SMS_RECEIVED" />``
			- 权限：``<uses-permission android:name="android.permission.RECEIVE_SMS" />``
				
				```
				//1. 获取Bundle对象
				Bundle bundle = intent.getExtras();
				//2. 获取短信Object数组
				Object[] pdus = (Object[]) bundle.get("pdus");
				//3. 循环读取数据
				for (Object pdu : pdus) {
				    SmsMessage message = SmsMessage.createFromPdu((byte[]) pdu, null);
				    //获取短信的发送者
				    String sender = message.getDisplayOriginatingAddress();
				    //获取短信的内容
				    String body = message.getMessageBody();
				    //获取短信的接收时间
				    long millis = message.getTimestampMillis();
				    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
				    String date = sdf.format(new Date(millis));
				    Log.e("sender",sender);
				    Log.e("body",body);
				    Log.e("date",date);
				}
				```
				
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
			
			5. 拦截广播
				
				```
				abortBroadcast();
				```

	3. 粘性广播
		- 发送会一直存在于系统的消息中, 直到处理该广播的接收者出现为止

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
1. 广播接收者的生命周期是非常短暂的，只有10s，在接收到广播的时候创建，onReceive()方法结束之后销毁
2. 广播接收者中不要做一些耗时的工作，否则会弹出Application No Response错误对话框，因为它本身是主线程
3. 最好也不要在广播接收者中创建子线程做耗时的工作，因为广播接收者被销毁后线程就成为了空线程，很容易被系统杀掉
4. 耗时的较长的工作最好放在服务中完成，我们只需要在广播接收者中开启服务即可