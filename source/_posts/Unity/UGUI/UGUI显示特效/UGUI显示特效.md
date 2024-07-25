---
title: UGUI显示特效
date: 2022-10-11 23:54:23
categories: Unity
tags: UGUI

---

## 默认模式特效不能显示在UGUI上

因为Canvas的默认渲染模式是Screen Space-Overlay，这种模式下的Canvas在屏幕空间中渲染，会显示在场景的最上方，也就是说一切UI都显示在最上层，而粒子系统是在世界空间中渲染的，所以会被遮挡住。

## 实现方式

1.将Canvas的渲染模式修改为Screen Space-Camera并指定一个UI相机

2.修改该相机参数: Clear Flags=>Depth only ; Culling Mask =>UI ; Projection => Orthographic(正交模式)

3.针对粒子系统渲染层级的问题，显示在最前或者后面，可以通过修改Renderer的设置Sorting Layer及Order in Layer来控制。 在UI的Canvas中也会有这个属性，相互配合完成效果 

## UGUI上遮罩粒子系统

### 使用Sprite Mask

1.新建一个空游戏物体，并添加Sprite Mask组件，指定Sprite

2.修改该游戏物体的大小，修改到刚好能后遮罩的大小，（可以将RectTransform 修改为Transform 方便查看遮罩的大小）

3.将粒子系统的Renderer中的属性Masking修改为Visible Inside Mask (表示只显示在遮罩内部（Visible Outside Mask:表示显示在遮罩外部）)

### ParticleEffectForUGUI插件（自行查阅）

https://github.com/mob-sakai/ParticleEffectForUGUI