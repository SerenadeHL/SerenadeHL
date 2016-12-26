---
layout: post
title: "ExpandableListView"
date: 2016-12-22
excerpt: "ExpandableListView"
tags: Android
comments: true
---

# ExpandableListView

# 继承关系
ListView --> AbsListView --> AdapterView

# 属性
- ``android:childDivider``：指定各组内子类表项之间的分隔条，图片不会完全显示， 分离子列表项的是一条直线
- ``android:childIndicator``：显示在 **子列表** 项旁边的Drawable对像，可以是一个图像
- ``android:childIndicatorEnd``：子列表项指示符的结束约束位置
- ``android:childIndicatorLeft``：子列表项指示符的左边约束位置
- ``android:childIndicatorRight``：子列表项指示符的右边约束位置
- ``android:childIndicatorStart``：子列表项指示符的开始约束位置
- ``android:groupIndicator``：显示在 **组列表** 项旁边的Drawable对像，可以是一个图像
- ``android:indicatorEnd``：组列表项指示器的结束约束位置
- ``android:indicatorLeft``：组列表项指示器的左边约束位置
- ``android:indicatorRight``：组列表项指示器的右边约束位置
- ``android:indicatorStart``：组列表项指示器的开始约束位置

# 指示器的状态图片
- state_expanded：是否是展开状态，注意，该属性在XML中不会提示！但是存在！需要手动写！！

```
<item android:drawable="@drawable/bottom" android:state_expanded="true"></item>
<item android:drawable="@drawable/right"></item>
```

# ExpandableListView使用特定的Adapter
实现ExpandableAdapter的三种方式
1. 扩展BaseExpandableListAdpter实现ExpandableAdapter。
2. 使用SimpleExpandableListAdpater将两个List集合包装成ExpandableAdapter
3. 使用SimpleCursorTreeAdapter将Cursor中的数据包装成SimpleCuroTreeAdapter 本节示例使用的是第一个，扩展BaseExpandableListAdpter，我们需要重写该类中的相关方法。

# ExpandableListView需要数据类型的问题
- 组的数据   List<String>
- 子的数据   List<List<String>>

# ExpandableListView的监听事件
```
elv.setOnChildClickListener(new ExpandableListView.OnChildClickListener() {
    @Override
    public boolean onChildClick(ExpandableListView parent, View v, int groupPosition, int childPosition, long id) {
        //parent：父控件，即ExpandableListView
        //v：所点击的子控件
        //groupPosition：所点击子控件所从属的组的位置
        //childPosition：所点击子控件的位置
        //id：所点击子控件的id
        //返回值：false为子控件不能点击，true为子控件可以点击
        return true;
    }
});
```
