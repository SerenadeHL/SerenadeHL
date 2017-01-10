---
layout: post
title: "ViewPager"
date: 2017-1-9
excerpt: "ViewPager"
tags: Android
comments: true
---

# ViewPager
- 包：``android.support.v4.view.ViewPager
- 特点：
    1. ViewPager继承于ViewGroup，所以说ViewPager是一个容器，可以添加多个View
    2. ViewPager需要通过PageAdapter来填充数据
    3. 一般情况下，ViewPager和Fragment一起使用，适配器为``FragmentPageAdapter``和``FragmentStatePageAdapter``
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
        - 常用子类：FragmentPageAdapter、FragmentStatePageAdapter
        - 需要重写的方法：
            - ``instantiateItem(ViewGroup,int)``：根据指定的下标，创建ViewPager中的页面
            - ``destroyItem(ViewGroup,int,Object)``：根据指定的下标，移除ViewPager中的页面
            - ``getCount()``：返回当前数据源的总数
            - ``isViewFromObject(View,Object)``：判断当前的ViewPager加载的页面，instantia
- 使用步骤
	1. 在布局页面中，创建标签``<android.support.v4.view.ViewPager/>``
	2. 在代码中增加要显示的数据源
	3. 创建Adapter继承FragmentPageAdapter并重写四个方法

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