---
layout: post
title: "SpannableString"
date: 2017-2-10
excerpt: "SpannableString"
tags: Android
comments: true
---

# SpannableString

>TextView通常用来显示普通文本，但是有时候需要对其中某些文本进行样式、事件方面的设置，可以使用SpannableString来实现

```java
SpannableString str = new SpannableString("123456789");
```

可以通过``setSpan()``方法来设置文本的样式、事件等，该方法接收四个参数，分别为``Object what``、``int start``、``int end``、``int flags``，其中flags对SpannableString不生效

- ``what``：指定文本显示样式、事件
- ``start``：显示特效文本的开始位置
- ``end``：显示特效文本的结束位置
- ``flags``：模式，**该参数只对SpannableStringBuilder生效！！！**，有4个值，分别为
	- ``SPAN_INCLUSIVE_INCLUSIVE``：特效包括开始位置、包括结束位置
	- ``SPAN_EXCLUSIVE_EXCLUSIVE``：特效不包括开始位置、不包括结束位置
	- ``SPAN_INCLUSIVE_EXCLUSIVE``：特效包括开始位置、不包括结束位置
	- ``SPAN_EXCLUSIVE_INCLUSIVE``：特效不包括开始位置、包括结束位置

## SpannableString可以实现的效果
1. ``BackgroundColorSpan``：背景色 
	
	![](http://p1.bqimg.com/567571/cc29e2d75e89fc57.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	BackgroundColorSpan bcs = new BackgroundColorSpan(Color.RED);
	str.setSpan(bcs, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
2. ``ClickableSpan``：文本可点击，有点击事件
	
	![](http://i1.piimg.com/567571/0b2bc278769af606.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	ClickableSpan cs = new ClickableSpan() {
	    @Override
	    public void onClick(View widget) {
	        Log.e("widget",""+widget);
	        Toast.makeText(MainActivity.this, "哈哈哈", Toast.LENGTH_SHORT).show();
	    }
	};
	str.setSpan(cs, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setMovementMethod(LinkMovementMethod.getInstance();
	textView.setText(str);
	```
	
	- 注意
		- 使用ClickableSpan的文本如果想真正实现点击作用，必须为TextView设置``setMovementMethod``方法，否则没有点击相应
	
3. ``ForegroundColorSpan``：文本颜色（前景色）
	
	![](http://i1.piimg.com/567571/0443c401e87dff6b.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	ForegroundColorSpan fcs = new ForegroundColorSpan(Color.parseColor("#0099EE"));
	str.setSpan(fcs, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
4. ``MaskFilterSpan``：修饰效果，如模糊(BlurMaskFilter)、浮雕(EmbossMaskFilter)
	
	![](http://p1.bpimg.com/567571/01ac7f0de005c23e.png)
	
	```java
	SpannableString spanText = new SpannableString("MaskFilterSpan -- http://orgcent.com");
	int length = spanText.length();
	//模糊(BlurMaskFilter)
	MaskFilterSpan maskFilterSpan = new MaskFilterSpan(new BlurMaskFilter(3, BlurMaskFilter.Blur.OUTER));
	spanText.setSpan(maskFilterSpan, 0, length - 10, Spannable.SPAN_INCLUSIVE_EXCLUSIVE);
	//浮雕(EmbossMaskFilter)
	maskFilterSpan = new MaskFilterSpan(new EmbossMaskFilter(new float[]{1, 1, 3}, 1.5f, 8, 3));
	spanText.setSpan(maskFilterSpan, length - 10, length, Spannable.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.append("\n");
	textView.append(spanText);
	```
	
5. ``MetricAffectingSpan``：父类，一般不用	
6. ``RasterizerSpan``：光栅效果
	
	暂不清楚如何使用
	
7. ``StrikethroughSpan``：删除线（中划线）
	
	![](http://i1.piimg.com/567571/586fa54709e175e4.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	StrikethroughSpan ss = new StrikethroughSpan();
	str.setSpan(ss, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```

8. ``SuggestionSpan``：相当于占位符
	
	相当于占位符，一般用在EditText输入框中。当双击此文本时，会弹出提示框选择一些建议（推荐的）文字，选中的文本将替换此占位符。在输入法上用的较多。
	
9. ``UnderlineSpan``：下划线
	
	![](http://i1.piimg.com/567571/73d2f00dd4302f1b.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	UnderlineSpan us = new UnderlineSpan();
	str.setSpan(us, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
10. ``AbsoluteSizeSpan``：绝对大小（文本字体）
	
	![](http://p1.bpimg.com/567571/44f3d4ebec93e22c.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	AbsoluteSizeSpan ass = new AbsoluteSizeSpan(10);
	str.setSpan(ass, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	```
	
11. ``DynamicDrawableSpan``：设置图片，基于文本基线或底部对齐。
	
	![](http://p1.bqimg.com/567571/476f87bc1e48f16d.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	DynamicDrawableSpan dds = new DynamicDrawableSpan() {
	    @Override
	    public Drawable getDrawable() {
	        Drawable d = getResources().getDrawable(R.mipmap.ic_launcher);
	        d.setBounds(0, 0, 50, 50);
	        return d;
	    }
	};
	str.setSpan(dds, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
12. ``ImageSpan``：图片
	
	![](http://p1.bqimg.com/567571/166ee2a689af96a5.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	ImageSpan is =new ImageSpan(this,R.mipmap.ic_launcher);
	str.setSpan(is, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
13. ``RelativeSizeSpan``：相对大小（文本字体）
	
	![](http://p1.bqimg.com/567571/0eff228cb60943fb.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	RelativeSizeSpan rss = new RelativeSizeSpan(2);//设置为其他文字2倍
	str.setSpan(rss, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
14. ``ReplacementSpan``：父类，一般不用
15. ``ScaleXSpan``：基于x轴缩放
	
	![](http://p1.bpimg.com/567571/d08aa01897a48e1d.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	ScaleXSpan sxs = new ScaleXSpan(2);//设置X轴方向为其他文本2倍
	str.setSpan(sxs, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
16. ``StyleSpan``：字体样式：粗体、斜体等
	
	![](http://p1.bqimg.com/567571/dc325b7e070f19c7.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	StyleSpan ss = new StyleSpan(Typeface.BOLD_ITALIC);
	str.setSpan(ss, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
	字体样式：
	
	- ``Typeface.NORMAL``：正常
	- ``Typeface.BOLD``：加粗
	- ``Typeface.ITALIC``：斜体
	- ``Typeface.BOLD_ITALIC``：加粗、斜体
	
17. ``SubscriptSpan``：下标（数学公式会用到）
	
	![](http://p1.bqimg.com/567571/b4237943da76b7ac.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	SubscriptSpan ss = new SubscriptSpan();
	str.setSpan(ss, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
	
18. ``SuperscriptSpan``：上标（数学公式会用到）
	
	![](http://i1.piimg.com/567571/cd4938eb831b7c43.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	SuperscriptSpan ss = new SuperscriptSpan();
	str.setSpan(ss, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
19. ``TextAppearanceSpan``：文本外貌（包括字体、大小、样式和颜色）
	
	![](http://p1.bpimg.com/567571/599fb7c5076e9ce5.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	TextAppearanceSpan tas = new TextAppearanceSpan(this, android.R.style.TextAppearance_Medium);
	str.setSpan(tas, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
20. ``TypefaceSpan``：文本字体
	
	![](http://p1.bpimg.com/567571/b4da22d433d9d7d3.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	//若需使用自定义字体，可能要重写类TypefaceSpan
	TypefaceSpan tfs = new TypefaceSpan("monospace");
	str.setSpan(tfs, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setText(str);
	```
	
21. ``URLSpan``：文本超链接
	
	![](http://p1.bpimg.com/567571/7a15dd61b4dde29e.png)
	
	```java
	SpannableString str = new SpannableString("123456789");
	URLSpan us = new URLSpan("http://www.baidu.com");
	str.setSpan(us, 1, 5, Spanned.SPAN_INCLUSIVE_EXCLUSIVE);
	textView.setMovementMethod(LinkMovementMethod.getInstance());//让URLSpan可以点击
	textView.setText(str);
	```

# SpannableStringBuilder

```java
String str = "123456789";
SpannableStringBuilder builder = new SpannableStringBuilder(str);
```

SpannableStringBuilder也可以通过``setSpan()``方法来设置文本的样式、事件等，该方法同样接收四个参数，分别为``Object what``、``int start``、``int end``、``int flags``，其中flags对SpannableStringBuilder生效

- ``SPAN_INCLUSIVE_INCLUSIVE``：特效包括开始位置、包括结束位置
- ``SPAN_EXCLUSIVE_EXCLUSIVE``：特效不包括开始位置、不包括结束位置
- ``SPAN_INCLUSIVE_EXCLUSIVE``：特效包括开始位置、不包括结束位置
- ``SPAN_EXCLUSIVE_INCLUSIVE``：特效不包括开始位置、包括结束位置

注意：如果调用``insert(int where , CharSequence tb)``方法向SpannableStringBuilder中插入字符串，会根据flags来变化

- ``SPAN_INCLUSIVE_INCLUSIVE``：两端插入的字符串都会显示特效
- ``SPAN_EXCLUSIVE_EXCLUSIVE``：两端插入的字符串都不会显示特效
- ``SPAN_INCLUSIVE_EXCLUSIVE``：前端插入的字符串显示特效，后端插入的字符串不显示特效
- ``SPAN_EXCLUSIVE_INCLUSIVE``：前端插入的字符串不显示特效，后端插入的字符串显示特效
- 所有模式在中间插入字符串，字符串都会显示特效，并且原来显示特效的字符依然显示特效