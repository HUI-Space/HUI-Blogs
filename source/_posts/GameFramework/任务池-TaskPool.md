---
title: 任务池-TaskPool
categories: GameFramework
tags: GameFramework
date: 2023-02-11 12:06:49
---

## 作用

在GF当中任务池的主要作用是用于网络请求、下载、加载资源。它其实也是引用池的一种表现

## 结构

- TaskPool
  - TaskBase
  - ITaskAgent
  - StartTaskStatus
  - TaskStatus
  - TaskPool
  - TaskInfo

### 抽象部分

#### TaskBase任务池基类

该类的主要作用是抽象出任务的相关参数、例如任务优先级、序列编号、标签、自定义数据、是否完成。并且该抽象类类继承了IReference并实现其方法。

### ITaskAgent代理接口

该接口主要作用是抽象出了，获取任务，任务初始化，开始处理任务，任务代理轮询，关闭并清理任务代理，停止正在处理的任务并重置任务代理。

### 枚举部分

StartTaskStatus：开始处理任务的所有状态

TaskStatus：当前任务的状态。

### 对象部分

#### TaskPool任务池

主要关注以下几个容器

```c#
private readonly Stack<ITaskAgent<T>> m_FreeAgents;
private readonly GameFrameworkLinkedList<ITaskAgent<T>> m_WorkingAgents;
private readonly GameFrameworkLinkedList<T> m_WaitingTasks;
```

- m_FreeAgents：存储维护所有空闲的任务代理。
- m_WorkingAgents：存储所有正在工作状态的代理
- m_WaitingTasks：存储所有等待的任务。

主要关注以下几个方法：

- AddTask：增加任务，会根据任务的优先级来将其添加到对应的位置。
- Update：任务池轮询，其主要作用是调用ProcessRunningTasks和ProcessWaitingTasks这两个方法。
- ProcessRunningTasks：运行任务，遍历所有工作的代理。判断工作是否完成，未完成则继续执行，任务完成则做一些回收处理。
- ProcessWaitingTasks：主要作用是开始运行任务。如果当前有空闲代理，则取出此代理并加到工作代理链表中，通过代理控制当前任务执行。如果不出错的情况下，就会归到运行中的任务继续处理，直到结束

## 使用方法

可以参考GF的WebRequest(网络请求), Download(下载), LoadResource(加载资源)

## 总结

使用任务的主要作用还是为了让任务能够有秩序的运行，并且有效的避免了GC，需要的时候进行获取，不需要是进行回收。同时也拥有很高的扩展性，不在写重复的代码。