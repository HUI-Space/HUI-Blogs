---
title: Android打包
categories: Unity
date: 2022-10-04 11:50:08
tags: Unity Android
---

## 环境

- Unity
- Android Studio
- JDK

## Unity打包

打包步骤：

- 使用UnityHub为对应的Unity版本添加模块，安装对应的Android Build Support
- 打开项目将Unity平台切换到Android平台
- 通过Unity PlayerSetting设置相关打包参数
- 设置Build Settings设置Android相关参数
- 点击Build Settings > Android > Build选择对应打包的目录进行打包即可

注意事项：

- 切记路径全为英文，出现中文则可能导致打包失败
- 切记使用Unity仅在编辑器相关API的时候，添加上对应的宏，否则会导致打包失败
- 引用外部文件时候切记放置到目标目录
- 使用外部文件时候建议新建工程，打包测试是否有未引用的文件（在使用Protobuf时文件未全部引用导致打包出错）
- 持续更新TODO

## Android打包

打包步骤：

- 将Unity中Preferences > External Tools 中的Android SDK切换到Android Studio编辑器对应的SDK路径（此步骤避免导出工程后出现SDK不匹配Android Studio编译错误）
- 勾选Build Settings > Android 中的 Export Project 此时Build按钮切换成Export
- 选择对应的打包目录进行导出即可
- 使用Android Studio打开导出对应的目录，等待编译
- 如果需要给APK签名，点击Build > Generate Signed Bundle or APK，选择APK点击Next输入相关的内容
- 点击Android Studio的Build > Build Bundle(s) /APK(s) >Build APK(s)。 
- 生成的APK包在build > outputs > apk > debug 文件夹下

注意事项：

- Unity与Android Studio使用的SDK最好使用同一个版本的SDK，这样可以避免不必要的bug发生
- 持续更新TODO