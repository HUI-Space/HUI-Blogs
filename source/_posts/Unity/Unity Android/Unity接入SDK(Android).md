---
title: Unity接入SDK(Android)
categories: Unity
date: 2022-10-05 11:50:08
tags: Unity Android
---

## 网上的教程一大堆，基本上都是一样的，说不到重点，所以这个坑还是得自己来踩。

## 环境

- Android Studio
- Unity
- JDK

## Android端工程开发

### 创建Android的空工程

使用Android Studio创建一个空工程

### 创建一个新的类库

File > New > New Module > Android Library 创建

![](Unity接入SDK(Android)/CreateLibrary_01.jpeg)

![](Unity接入SDK(Android)/CreateLibrary_02.jpeg)

![](Unity接入SDK(Android)/CreateLibrary_03.jpeg)

### 导入Unity的classes.jar包

- 根据Unity打包的版本选择包
  - mono路径：Editor > Data > PlaybackEngines > AndroidPlayer> Variations > mono> Release > Classes > classes.jar
  - il2cpp路径：Editor > Data > PlaybackEngines > AndroidPlayer> Variations > il2cpp> Release > Classes > classes.jar
- 导入到Android Studio的libs文件夹下，路径为 Library(自己命名的类库名称) > libs
- 随后将classes.jar包添加到库当中。右键点击 Add as Library... > OK 等待编译

### 编写Android端的代码

对于Unity版本大于2019.2版本的classes.jar包中不再包含UnityPlayerActivity这个脚本所以需要手动添加到项目当中。实现步骤：

- 找到UnityPlayerActivity.java文件
  - 路径：Editor > Data > PlaybackEngines > AndroidPlayer > Source > com > unity3d > player > UnityPlayerActivity.java
- 此时从com文件夹开始复制到Library(自己命名的类库名称) > java 当中

- 新建一个Java脚本继承并且UnityPlayerActivity。
- 该脚本必须

```java
package com.unity3d.player;

import android.util.Log;

public class SDKActivity extends UnityPlayerActivity
{

    public static String Message = "Message";
    public String Name = "name";

    public static void SetMessage(String message)
    {
        Message = message;
        Log.d(Message, "SetMessage: " + message);
        //java调用unity 第一个参数游戏物体，第二个参数为方法名称，第三个为对应的参数
        UnityPlayer.UnitySendMessage("Test","GetStatic","");
    }

    public static String GetMessage()
    {
        Log.d(Message, "SetMessage: " + Message);
        return Message ;
    }

    public void SetName(String name)
    {
        Name = name;
        Log.d(Name, "SetName: "+name);
        //java调用unity 第一个参数游戏物体，第二个参数为方法名称，第三个为对应的参数
        UnityPlayer.UnitySendMessage("Test","Get","");
    }
    public String GetName()
    {
        Log.d(Name, "SetName: "+ Name);
        return Name;
    }
}
```

### 导出Jar包供Unity使用

因为Unity版本不同所以所作的事情也就不同

导出准备：

- 先将Projec切换到Android
- 查看并修改Gradle Scripts中的build.gradle(自己新建的类库名称)
  - 查看apply plugin 是否是'com.android.library'
  - 查看android相关配置是否正确，不明白配置自行搜索查看
  - 查看dependencies并修改
    - 注释掉fileTree
    - 找到对应的classes.jar包将implementation修改为compileOnly（作用是不将classes.jar打入到.arr当中）
- 如果直接使用Library(类库名称) > build > arr_main_jar > debug > classes.jar
  - 就不需要做任何处理
  - 如果还引用了其他的第三方库，则库需要将Library(类库名称) > build > intermediates > aar_libs_directory > debug > libs 中的文件添加到Unity中

开始导出：

- 生成类库Build > Make Project 
- 生成过后将生成的jar包放到Unity当中，生成路径
  - 路径1：Library(类库名称) > build > outputs > arr > (Library)-debug.arr
  - 路径2：Library(类库名称) > build > arr_main_jar > debug > classes.jar

特殊处理：

如果Unity版本是大于2019.2的那么就需要做以下操作：

- 用压缩软件打开该包 (Library)-debug.arr 或 classes.jar 包 （取决于使用那个包）
- 删除com文件夹中的unity3d文件夹（这步的主要作用是因为unity当中以及有一份该文件了，如果拥有两份则会导致打包错误）

## Unity端工程开发

### 创建文件夹

- 在Asset文件夹下创建Plugins/Android文件夹
- 如果直接使用jar 建议在Plugins/Android文件夹下创建lib文件夹

### 导入包

如果是使用jar包则需要将Andoird工程目录下的，libs/ 、res/ 、AndroidMainFest.xml 都复制到该路径下。

如果是.arr包，只需要使用.arr包即可（里面包含这些文件）

### 修改AndroidMainFest.xml

通过[打包]()中的Unity导出工程的默认AndroidMainFest.xml文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.unity3d.player" xmlns:tools="http://schemas.android.com/tools">
  <application android:extractNativeLibs="true">
    <activity android:name="com.unity3d.player.UnityPlayerActivity" android:theme="@style/UnityThemeSelector" android:screenOrientation="fullUser" android:launchMode="singleTask" android:configChanges="mcc|mnc|locale|touchscreen|keyboard|keyboardHidden|navigation|orientation|screenLayout|uiMode|screenSize|smallestScreenSize|fontScale|layoutDirection|density" android:resizeableActivity="false" android:hardwareAccelerated="false" android:exported="true">
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
      <meta-data android:name="unityplayer.UnityActivity" android:value="true" />
      <meta-data android:name="android.notch_support" android:value="true" />
      <meta-data android:name="android.notch_support" android:value="true" />
    </activity>
    <meta-data android:name="unity.splash-mode" android:value="0" />
    <meta-data android:name="unity.splash-enable" android:value="True" />
    <meta-data android:name="unity.launch-fullscreen" android:value="True" />
    <meta-data android:name="unity.allow-resizable-window" android:value="False" />
    <meta-data android:name="notch.config" android:value="portrait|landscape" />
  </application>
  <uses-feature android:glEsVersion="0x00030000" />
  <uses-feature android:name="android.hardware.vulkan.version" android:required="false" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-feature android:name="android.hardware.touchscreen" android:required="false" />
  <uses-feature android:name="android.hardware.touchscreen.multitouch" android:required="false" />
  <uses-feature android:name="android.hardware.touchscreen.multitouch.distinct" android:required="false" />
</manifest>
```

如果是使用自己的自定义的Activity那么需要修改以下：

- package：修改成自己的定义的package包的位置
- 主Activity（activity android:name="com.unity3d.player.SDKActivity"）需要修改成自定义的Activity的路径
- AndroidMainFest文件只能拥有一份，不然会发生意想不到bug，例如安装app后显示两个图标的问题

如果下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.example.sdklibrary" xmlns:tools="http://schemas.android.com/tools">
  <application android:extractNativeLibs="true">
    <activity android:name="com.example.sdklibrary.SDKActivity" android:theme="@style/UnityThemeSelector" android:screenOrientation="fullUser" android:launchMode="singleTask" android:configChanges="mcc|mnc|locale|touchscreen|keyboard|keyboardHidden|navigation|orientation|screenLayout|uiMode|screenSize|smallestScreenSize|fontScale|layoutDirection|density" android:resizeableActivity="false" android:hardwareAccelerated="false" android:exported="true">
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
      <meta-data android:name="unityplayer.UnityActivity" android:value="true" />
      <meta-data android:name="android.notch_support" android:value="true" />
      <meta-data android:name="android.notch_support" android:value="true" />
    </activity>
    <meta-data android:name="unity.splash-mode" android:value="0" />
    <meta-data android:name="unity.splash-enable" android:value="True" />
    <meta-data android:name="unity.launch-fullscreen" android:value="True" />
    <meta-data android:name="unity.allow-resizable-window" android:value="False" />
    <meta-data android:name="notch.config" android:value="portrait|landscape" />
  </application>
  <uses-feature android:glEsVersion="0x00030000" />
  <uses-feature android:name="android.hardware.vulkan.version" android:required="false" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-feature android:name="android.hardware.touchscreen" android:required="false" />
  <uses-feature android:name="android.hardware.touchscreen.multitouch" android:required="false" />
  <uses-feature android:name="android.hardware.touchscreen.multitouch.distinct" android:required="false" />
</manifest>
```



### 编写Unity代码调用

```C#
using System;
using UnityEngine;
using UnityEngine.UI;

public class SDKTest : MonoBehaviour
{
   private AndroidJavaClass _androidJavaClass;
   private AndroidJavaObject _androidJavaObject;
   
   public Text text;

   public Button btn1;
   public Button btn2;
   public Button btn3;
   public Button btn4;
   
   private void Start()
   {
      _androidJavaClass = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
      _androidJavaObject = _androidJavaClass.GetStatic<AndroidJavaObject>("currentActivity");
      btn1.onClick.AddListener(GetStatic);
      btn2.onClick.AddListener(Get);
      btn3.onClick.AddListener(SetMessage);
      btn4.onClick.AddListener(SetName);
   }
   
   private void GetStatic()
   {
      text.text = _androidJavaObject.GetStatic<String>("Message");
   }
    
   private void Get()
   {
      text.text = _androidJavaObject.Get<String>("Name");
   }

   private void SetMessage()
   {
      _androidJavaObject.CallStatic("SetMessage","按钮3");
   }

   private void SetName()
   {
      _androidJavaObject.Call("SetName","按钮4");
   }
}
```

## 打包

详情请查看[打包]()

## 结束

更加详细的Unity与Android交互请查看[Unity与Android交互]()

完成以上操作基本上就没有什么问题了。