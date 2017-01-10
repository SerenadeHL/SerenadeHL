---
layout: post
title: "ContentProvider"
date: 2017-1-5
excerpt: "ContentProvider"
tags: Android
comments: true
---

# 什么是ContentProvider
- 是所有应用程序之间数据存储和检索的桥梁
- 是应用程序之间数据共享的唯一途径
- 注意：
    - 如果想要访问内容提供者提供的数据，那么需要通过ContentResolver对象获取数据内容
    - 如果想要共享自己的应用程序的数据，那么需要自定义一个内容提供者，把自己的数据共享出去

# 作用
实现多个应用程序之间数据的共享

# URI
统一资源标识符

- 格式
    - ``content://应用程序包名.provider/表名``

# 分析
- ``ContentResolver``：内容解析器，负责解析ContentProvider暴露出来的数据
- ``ContentProvider``：内容提供者，负责暴露数据

# 自定义ContentProvider
- 需求：
    - 创建数据库：SQLiteOpenHelper
    - 暴露数据：ContentProvider
- 创建时间：
    - ``Application -> onCreate()``方法，当程序创建的时候调用
    - ``Activity -> onCreate()``方法，当打开对应Activity的时候调用
    - ``ContentProvider -> onCreate()``方法，当ContentProvider创建时候调用
- 出现的先后顺序：
    - ContentProvider的``onCreate()``方法 --> Application的``onCreate()``方法 --> Activity的``onCreate()``方法
- 使用方法：
    - 创建数据库
    - 创建以各类去继承ContentProvider
        - onCreate()：打开数据库，获取数据库操作工具类对象
            - getContext()可以获得上下文对象
    - 设计路径结构
        - 开发者通常通过追加指向单个表的路径来根据权限创建内容URI。例如，如果您有两个表：table1和table2，则可以通过合并上一示例中的权限来生成内容URI：com.example.appname.provider/table1和com.example.appname.provider/table2。路径并不限定于单个段，也无需为每一级路径都创建一个表。
    - 创建静态代码块，给匹配器添加匹配Uri

        ```
        //优先执行
        static{
            //创建对象不匹配
            matcher = new UriMatcher(UriMatcher.NO_MATCH);
            //content://authority/表名
            matcher.addURI(AUTHORITY, tableName, CODE);
        }
        ```
        其中AUTHORITY为常量字符串，CODE为常量整数，二者都为全局变量，AUTHORITY为`content://authority/`,CODE用于与完整Uri对应，方便使用switch语句判断
    - 重写操作方法
        - ``query()``、``insert()``、``update()``、``delete()``
        - 匹配器 匹配传递过来的Uri 区分操作表
             int code = UriMacther.match(uri)
        - 具体操作数据库
             SQLiteDataBase 操作数据库
    - 声明ContentProvider

        ```
        <provider
        android:name="com.example.android_day14_contentprovider_01.MyProvider"  
        //Uri 中间部分需要声明  
        //在同一个手机中，中间部分不能相同
        android:authorities="com.example.myprovider.provider"
        //是否支持其他程序访问
        android:exported="true"
        android:permission=""      读写权限
        android:readPermission=""  读权限
        android:writePermission="" 写权限>
        ```
    - 自定义权限
        - 权限就是一个字符串！但是这个字符串必须声明！
        
	        ```
	        <permission
	        android:name="this is a permission"
	         android:label="这是自定义权限的说明"
	        android:protectionLevel="normal" >
	        </permission>
	        ```
