---
title: CanvasUpdateRegistry
date: 2022-10-11 23:54:23
categories: Unity
tags: UGUI
---
## CanvasUpdateRegistry

### 简介

CanvasUpdateRegistry是一个单例，其主要目的是监听Canvas即将渲染的事件，并调用已注册对象的Rebuild、LayoutComplete、GraphicUpdateComplete方法，而其中的Rebuild方法就是每个UI元素的刷新方法。

### 源码解析

####  protected CanvasUpdateRegistry()

//注册PerformUpdate   Canvas渲染之前，会抛出willRenderCanvases事件，从而调用PerformUpdate

```c#
protected CanvasUpdateRegistry()
{
	Canvas.willRenderCanvases += PerformUpdate;
}
```

#### m_LayoutRebuildQueue

记录需要重建的布局元素

```c#
private readonly IndexedSet<ICanvasElement> m_LayoutRebuildQueue = new IndexedSet<ICanvasElement>();
```

#### m_GraphicRebuildQueue

记录需要重建的图像元素

```c#
private readonly IndexedSet<ICanvasElement> m_GraphicRebuildQueue = new IndexedSet<ICanvasElement>();
```

#### private void PerformUpdate()

* 1.依次遍历m_LayoutRebuildQueue和m_GraphicRebuildQueue两个序列，删除掉不可用的元素。
* 2.重建布局（Layout Rebuild）开始
  * 对m_LayoutRebuildQueue序列按照父对象数量进行升序排序(从子物体一层层向上进行布局重建)。
  * 根据布局的三个阶段Prelayout、Layout、PostLayout依次调用m_LayoutRebuildQueue序列中每一个对象的Rebuild方法。
  * 调用所有元素的LayoutComplete方法
  * 清楚布局重建序列中的所有元素
  * 布局结束后调用组件的裁剪方法ClippingRegistry.Cull()，调用布局注册器中的所有待裁剪的对象的PerformClipping方法进行裁剪
* 2.重建图形（Graphic Rebuild）开始
  * 对m_GraphicRebuildQueue（被标记了Dirty状态的Graphic对象）以PreRender，LatePreRender的参数顺序调用每一个元素（无序）的Rebulid方法
  * 调用所有元素的GraphicUpdateComplete方法
  * 清除图形重建序列中的所有元素

#### private static int ParentCount(Transform child)

获取父物体的数量

#### private static int SortLayoutList(ICanvasElement x, ICanvasElement y)

依据父对象的数量进行排序，父transform少的在前

#### public static void RegisterCanvasElementForLayoutRebuild(ICanvasElement element)

#### public static bool TryRegisterCanvasElementForLayoutRebuild(ICanvasElement element)

#### private bool InternalRegisterCanvasElementForLayoutRebuild(ICanvasElement element)

将给定的元素添加到布局重建的列表中

####  public static bool TryRegisterCanvasElementForGraphicRebuild(ICanvasElement element)

####  private bool InternalRegisterCanvasElementForGraphicRebuild(ICanvasElement element)

####  private void InternalUnRegisterCanvasElementForLayoutRebuild(ICanvasElement element)

将给定的元素添加图像重建列表中

#### public static void UnRegisterCanvasElementForRebuild(ICanvasElement element)

从图形和布局重建列表中删除给定的元素

#### private void InternalUnRegisterCanvasElementForLayoutRebuild(ICanvasElement element)

从布局重建列表中删除给定的元素

#### private void InternalUnRegisterCanvasElementForGraphicRebuild(ICanvasElement element)

从图形重建列表中删除给定的元素

#### public static bool IsRebuildingLayout()

是否布局重建中

#### public static bool IsRebuildingGraphics()

是否图像重建中

