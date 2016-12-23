---
layout: post
title: "AsyncTask"
date: 2016-12-22
excerpt: "AsyncTask"
tags: Android
comments: true
---

# 什么是ANR？
- 造成的原因：在Android中，一个应用默认是一个线程，而一个线程中会自带一个线程！这个默认线程，称之为主线程或者UI线程，它主要负责用户界面交互逻辑！要求它实时保持较高的反应！如果在主线程中做了耗时操作！例如：I/O网络请求等，那么会出现卡顿的效果，更严重的是会报ANR程序无响应的错误！
- Activity中耗时操作时间为5秒，BroadcastReceiver中10秒，注意：此说法只是理论的！
- 解决方法：开启子线程进行耗时操作，注意！子线程中无法更新除了ProgressBar之外的UI，主线程中不能进行耗时操作，操作完成后通过Handler Message进行线程间通信告知主线程更新UI
![](http://p1.bpimg.com/567571/53233365cc53d07f.png)

# Android使用线程的原则
1. 主线程：可以更新UI，不能执行耗时的操作，不能被阻塞，不能执行网络请求。
2. 子线程：可以执行耗时操作，可以执行网络请求，不能操作UI。
3. 线程不能随便开启，原因有二：1)线程可控性非常差！2)线程多了会消耗系统性能，造成程序反应缓慢，卡顿等等！
4. 但是！在Android中的主线程中，由于它有着特殊的用途(负责UI界面的更新)，导致它无法做过多的耗时操作！那么如果有耗时操作，就需要开启子线程！而且，在Android有明确的规定，在Android3.0以后，网络请求必须写在子线程中！否则报错NetworkOnMainThreadException！

# Handler机制
线程间的通信，将子线程的数据传递给主线程，android中实现了handler机制，这个类就是AsyncTask

# 传统的线程Thread和Runnable在Android中的使用
- 创建一个子线程！重写内部的run方法
- 在子线程中进行耗时操作！I/O网络请求、SDcard、数据库遍历！
- 主线程中创建一个Handler
```
Handler handler = new Handler(){
public void handleMessage(Message msg){
}
}
```
- 在子线程完成耗时操作的地方，创建一个Message
```
Message msg = new Message()
```
- 子线程调用发信的方法
```
handler.sendMessage(msg)
```
- 主线程处理返回数据的方法
```
handleMessage(Message msg)
```

# AsyncTask是什么？
使用子线程执行耗时的操作，通过回调方法把结果返回给主线程，AsyncTask较传统的线程不同的是！它在Android中，使用的是线程+Handler结合成的一个类！使用异步任务，不用再自己写Handler、Message进行线程间通信！直接可以在异步任务类的指定方法中，进行UI控件的更新！

# 如何使用AsyncTask
1. 定义一个类，继承AsyncTask，同时声明3个泛型
```
public class myTask extends AsyncTask<Params,Progress,Result>
```
	- ``Params``：子线程执行任务的请求参数，通常是一个String
	- ``Progress``：子线程执行任务的进度，通常是一个Integer
	- ``Result``：子线程执行任务的结果返回类型，根据具体需求而定
2. 重写父类的方法
	- onPreExecute()：初始化
		- 此方法第一个执行！
   		- 此方法执行在主线程中！
   		- 此方法一般用于准备工作！例如：集合初始化！Dialog提示框弹出！
	- doInBackground()：必须要重写
		- 返回值：泛型参数三
		- 此方法第二个执行！
		- 此方法执行在子线程中！可以做耗时操作！他不能更新UI！
		- 此方法return会把数据return到onPostExecut(Result)的参数result里
		- 此方法可以调用publishProgress(泛型参数2)更新进度！当然！也可以传递数据！
	- onProgressUpdate(泛型参数一...Params)：更新进度
		- 此方法不会主动执行，只有其他方法调用publishProgress()才会执行此方法！
		- 此方法执行在主线程中！
		- 此方法通常用来更新使用！例如：进度条！
	- onPostExecute()：
		- 此方法当doInBackground()方法return以后执行，并会获取return返回的数据！
		- 此方法执行在主线程中！可以进行U更新！
		- 注意：如果在onPreExecute()中开启了Dialog，那么要在此处关闭！
	- onCanceled(泛型参数三)
		- 此方法执行在主线程中！
		- 此方法只有在关闭异步任务(调用cancel(boolean)方法时)时调用此方法！
		- 一般用于关闭后给用户进行提示！
		- 参数！参数就是doInBackground()的返回数据!

# AsyncTask开启和关闭
- 开启
	- 对象必须在主线程中创建
	- 对象不能重复使用
	- 如果对象正在执行，不能再次调用execute()
	- 对象不能手动调用onPreExecut
	- 开启异步任务，必须在主线程中调用execute()方法！
- 关闭
	- AsyncTask.cancel(boolean)
	- true表示尝试去关闭正在运行的异步任务
	- false表示不尝试去关闭正在运行的异步任务
	- true跟false最终都能实现不调用onPostExecute()
	- isCanceled():是否关闭
	- onCanceled():当关闭成功后会回调此方法

	
# 注意事项
1. **创建异步任务对象，必须在主线程中！**
2. **异步任务的对象不能重复使用！**
3. **异步任务对象开启方法必须在主线程中执行**
4. **不能手动调用异步任务四个方法**