---
title: UIBehaviour
date: 2022-10-11 23:54:23
categories: Unity
tags: UGUI
---

## 简介

UIBehaviour是所有UI组件的基类，UI组件都直接或间接继承该抽象类。

## 代码解析

```
UIBehaviour
#if UNITY_EDITOR									   //编辑器下调用
protected virtual void OnValidate()						//脚本Inspector面板的值出现变化的时候会被调用
protected virtual void Reset()   						//恢复成默认值
#endif
protected virtual void OnRectTransformDimensionsChange()  //当RectTransform变化时调用
protected virtual void OnBeforeTransformParentChanged()	  //当父物体变化之前调用
protected virtual void OnTransformParentChanged()		 //当父物体变化之后调用
protected virtual void OnDidApplyAnimationProperties()	 //当应用动画属性时调用
protected virtual void OnCanvasGroupChanged()			//Canvas Group 变化时候调用
protected virtual void OnCanvasHierarchyChanged()		//当Canvas状态变化时调用，比如禁用Canvas组件
public bool IsDestroyed()							  //是否被销毁

```

