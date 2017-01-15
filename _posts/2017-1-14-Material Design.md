---
layout: post
title: "Material Design"
date: 2017-1-13
excerpt: "Material Design"
tags: Android
comments: true
---

# 什么是Material Design
>Material Design，中文名：材料设计语言，是由Google推出的全新的设计语言，谷歌表示，这种设计语言旨在为手机、平板电脑、台式机和“其他平台”提供更一致、更广泛的“外观和感觉”。

- 使用Material Design包的控件需要在Gradle中添加以下依赖

	```
	compile 'com.android.support:design:版本号'
	```

***

# Design包控件

## CoordinatorLayout
CoordinatorLayout是design包里功能强大的一个控件,搭配上其他的控件可以实现比较复杂的动画和布局效果.它是一个增强版的fragment,控件的前后关系是根据elevation来决定的,不在是添加控件的先后顺序来决定,如果elevation相同,则依然按照添加顺序决定.

### 功能
- 作为顶层布局
- 调度协调子布局

### s

***

## Snackbar
![](http://i1.piimg.com/567571/495ccc6f9c38875a.png)

```
Snackbar snackbar = Snackbar.make(view,"哈哈哈",Snackbar.LENGTH_SHORT);
snackbar.setAction("弹出Toast", new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Toast.makeText(MainActivity.this, "成功", Toast.LENGTH_SHORT).show();
    }
});
snackbar.show();
```
### 方法
- ``make(View view, CharSequence text, int duration)``：生成Snackbar消息
- ``setAction(CharSequence text, OnClickListener listener)``：设置action
- ``show()``：显示SnackBar消息
- ``setActionTextColor(int color)``：设置action文本的颜色
- ``setText(CharSequence message)``：设置Snacbar消息显示的本文
- ``dismiss()``：立即让SnackBar消息消失

### 注意
- SnackBar的第一个参数为一个View对象，必须能找到父布局才能用,也就是说,如果我们使用new View(this)的方法来实例化一个view放进去,虽然类型一样,但是运行时会直接报错,因为这个view找不到父view
- 通常在开发中，SnackBar一般是写在BaseActivity中，这时，而BaseActivity中又不是一定有布局文件,所以也不一定能拿到view或viewGroup，所以要找到一个在任何时候都能使用的View对象，这个View就是DecorView
- 使用了DecorView作为第一个参数后，如果设备中没有虚拟按键，显示效果会很好，但是如果是华为、Nexus等有虚拟按键的设备，SnackBar会被虚拟按键挡住，这里有以下两种解决办法
	1. 计算虚拟按键的高度，使SnackBar在其上方弹出
		
		```
		/**
		 * 获取屏幕原始尺寸高度，包括虚拟功能键高度
		 */
		public static int getRawScreenHeight(Context context) {
		    int dpi = 0;
		    WindowManager windowManager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
		    Display display = windowManager.getDefaultDisplay();
		    DisplayMetrics displayMetrics = new DisplayMetrics();
		    @SuppressWarnings("rawtypes")
		    Class c;
		    try {
		        c = Class.forName("android.view.Display");
		        @SuppressWarnings("unchecked")
		        Method method = c.getMethod("getRealMetrics", DisplayMetrics.class);
		        method.invoke(display, displayMetrics);
		        dpi = displayMetrics.heightPixels;
		    } catch (Exception e) {
		        e.printStackTrace();
		    }
		    return dpi;
		}
		/**
		 * 获取屏幕高度
		 *
		 * @return 返回当前屏幕高度
		 */
		public static int getScreenHeight(Context context) {
		    WindowManager manager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
		    DisplayMetrics metrics = new DisplayMetrics();
		    manager.getDefaultDisplay().getMetrics(metrics);
		    return metrics.heightPixels;
		}
		/**
		 * 获取虚拟按键栏的高度(有虚拟按键栏时有值,没有虚拟按键栏时返回0)
		 */
		public static int getBottomStatusHeight(Context context) {
		    int totalHeight = getRawScreenHeight(context);
		    int contentHeight = getScreenHeight(context);
		    return totalHeight - contentHeight;
		}
		```
		
	2. 弹出SnackBar的同时，隐藏虚拟按键；消失SnackBar的同时显示虚拟按键
		
		```
		//隐藏虚拟按键  
       getWindow().getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION  
               | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION //隐藏虚拟按键栏  
               | View.SYSTEM_UI_FLAG_IMMERSIVE //防止点击屏幕时,隐藏虚拟按键栏又弹了出来  
       );
       //恢复被隐藏的虚拟按键  
       getWindow().getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_VISIBLE);
		```

### 自定义SnackBar
Snackbar的view是由SnackbarLayout实现的，而SnackbarLayout是继承自LinearLayout，所以我们可以自定义布局来实现自定义SnackBar

```
//获取snackbar的View(其实就是SnackbarLayout)
View snackbarview = snackbar.getView();
//将获取的View转换成SnackbarLayout
Snackbar.SnackbarLayout snackbarLayout=(Snackbar.SnackbarLayout)snackbarview;
//加载布局文件新建View
View add_view = LayoutInflater.from(snackbarview.getContext()).inflate(layoutId,null);
//设置新建布局参数
LinearLayout.LayoutParams p = new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT,LinearLayout.LayoutParams.WRAP_CONTENT);
//设置新建布局在Snackbar内垂直居中显示
p.gravity= Gravity.CENTER_VERTICAL;
//将新建布局添加进snackbarLayout相应位置
snackbarLayout.addView(add_view,index,p);
```

### 什么是DecorView
我们先来看一下视图的层级关系

![](http://p1.bqimg.com/567571/c989162a3504ee4c.png)

1. DecorView为整个Window界面的最顶层View
2. DecorView只有一个子元素为LinearLayout。代表整个Window界面，包含通知栏，标题栏，内容显示栏三块区域。
3. LinearLayout里有两个FrameLayout子元素。
4. ``20``为标题栏显示界面。只有一个TextView显示应用的名称。也可以自定义标题栏，载入后的自定义标题栏View将加入FrameLayout中。
5. ``21``为内容栏显示界面。就是setContentView()方法载入的布局界面，加入其中。

- DecorView对象获得方法

	```
	getWindow().getDecorView();
	```

***
	
## FloatingActionButton


***

## NavigationView


***

## AppBarLayout


***

## CollapsingToolbarLayout


***

## TextInputLayout


***

# TabLayout