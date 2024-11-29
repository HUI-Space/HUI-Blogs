---
title: ICanvasElement
date: 2022-10-11 23:54:23
categories: Unity
tags: UGUI
---
## ICanvasElement

### 简介

使用该接口的元素可以存在于Canvas当中，UI对象如果需要重建就需要继承该接口。

### 源码解析

#### void Rebuild(CanvasUpdate executing)

重构方法

#### Transform transform { get; }

空间坐标

#### void LayoutComplete()

布局重建完成

#### void GraphicUpdateComplete()

图像重建完成

#### bool IsDestroyed()

检查Element是否无效