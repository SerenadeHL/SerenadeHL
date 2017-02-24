---
layout: post
title: "WebView"
date: 2017-2-9
excerpt: "WebView"
tags: Android
comments: true
---

# WebView
包含了浏览网页的功能的控件

- 常用的方法
	- ``canGoBack()``：用于检测网页是否可以后退
	- ``goBack()``：WebView后退
	- ``canGoForward()``：用于检测网页是否可以前进
	- ``goForward()``：WebView前进
	- ``stopLoading()``：停止网页的加载
	- ``reload()``：刷新网页
	- ``loadUrl(String url)``：加载网页

# WebViewClient
网页加载的处理器，webView 只有设置了WebViewClient，才能在当前的应用程序中显示网页

# WebChromeClient
webView处理界面功能的，如进度条

4, WebSettings:  webView 设置的