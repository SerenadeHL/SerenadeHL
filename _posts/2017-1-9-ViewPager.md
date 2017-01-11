---
layout: post
title: "ViewPager"
date: 2017-1-9
excerpt: "ViewPager"
tags: Android
comments: true
---

# ViewPager
- 包：``android.support.v4.view.ViewPager``
- 特点：
    1. ViewPager继承于ViewGroup，所以说ViewPager是一个容器，可以添加多个View
    2. ViewPager需要通过PageAdapter来填充数据
    3. 一般情况下，ViewPager和Fragment一起使用，适配器为``FragmentPagerAdapter``和``FragmentStatePageAdapter``
- 适配器
    - BaseAdapter：基础适配器抽象类
        - 常用子类：ArratAdapter、SimpleAdapter
        - 需要重写的方法：
            - ``int getCount()``：返回数据源的总数
            - ``Object getItem(int position)``：返回指定位置的数据
            - ``long getItemId(int position)``：返回指定位置View对应的Id
            - ``View getView()``：返回每个Item的视图
    - CursorAdapter：游标适配器，是ListView和Cursor之间的桥梁
        - 常用子类：SimpleCursorAdapter
        - 需要重写的方法：
            - ``View newView()``：返回Item对应的视图
            - ``void bindView()``：初始化视图中的控件并且为控件赋值
    - PagerAdapter：是一个抽象类，用于填充ViewPager的适配器
        - 常用子类：FragmentPagerAdapter、FragmentStatePageAdapter
        - 需要重写的方法：
            - ``instantiateItem(ViewGroup,int)``：根据指定的下标，创建ViewPager中的页面
            - ``destroyItem(ViewGroup,int,Object)``：根据指定的下标，移除ViewPager中的页面
            - ``getCount()``：返回当前数据源的总数
            - ``isViewFromObject(View,Object)``：判断当前的ViewPager加载的页面，instantia
- ``FragmentPagerAdapter``使用步骤
	1. 在布局页面中，创建标签``<android.support.v4.view.ViewPager/>``
	2. 在代码中增加要显示的数据源
	3. 创建Adapter继承FragmentPagerAdapter并重写四个方法

        ```
        @Override
        public int getCount() {
            //TODO 返回ViewPager中显示数据的数量
            return data.size();
        }
        @Override
        public Object instantiateItem(ViewGroup container, int position) {
            //TODO 根据指定的下标，创建ViewPager中的页面
            //1. 将当前显示的页面，增加到ViewGroup中
            container.addView(data.get(position));
            //2. 将当前增加的页面，作为结果返回
            return data.get(position);
        }
        @Override
        public void destroyItem(ViewGroup container, int position, Object object) {
            //TODO 根据指定的下标，移除ViewPager中的页面
            //1. 删除super.destroyItem(container, position, object);
            //2. 移除指定位置的图片
            container.removeView(data.get(position));
        }
        @Override
        public boolean isViewFromObject(View view, Object object) {
            //TODO 返回当前显示的视图和instantiateItem方法加载的是否为同一个页面
            return view == object;
        }
        ```
        
    4. 在代码中，定义适配器，并且设置适配器

        ```
        MyAdapter adapter = new MyAdapter(data, this);
        viewPager.setAdapter(adapter);
        ```
        
- ``FragmentStatePagerAdapter``使用的步骤：
	1. 必须要提供构造方法
	2. 必须要重写父类的两个方法
		1. ``getCount()``：得到当前数据源的总数量
		2. ``getItem()``：返回指定下标对应的Fragment
	3. Fragment必须是v4包中的

- ``FragmentPagerAdapter``和``FragmentStatePagerAdapter``
	1. 父类都是PagerAdapter
	2. 针对Fragment来填充数据，每一个页面都是一个Fragment
	3. FragmentPagerAdapter可以有预加载的功能，一般使用在静态Fragment中，会预先加载几个页面存入到内存中，如果数据量大的页面，建议使用FragmentStatePagerAdapter
	4. FragmentStatePagerAdapter只加载自己的页面，如果页面移动，移除的页面会被内存销毁

# PageTitleStrip
- 在布局文件中，把它作为ViewPager的子标签出现
- 通过PagerAdapter中的``getPageTitle(int n)``方法来添加标题

# PagerTabStrip
- ``PagerTabStrip``和``PageTitleStrip``的区别：
	1. PageTitleStrip：不能和用户交互，PagerTabStrip：可以和用户交互
	2. PagerTabStrip：会在标题下面显示一条下划线
- 方法：
	- ``setDrawFullUnderline(boolean)``：是否设置默认下划线
	- ``setBackgroundColor()``：设置``PagerTabStrip``的背景颜色
	- ``setTabIndicatorColor()``：设置指示器颜色
