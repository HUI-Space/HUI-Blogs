---
title: Jenkins安装教程
date: 2024-12-02 10:48:00
categories: Jenkins
tags: Jenkins
---

## 简介

Jenkins是一款开源 CI&CD 软件，用于自动化各种任务，包括构建、测试和部署软件。

Jenkins 支持各种运行方式，可通过系统包、Docker 或者通过一个独立的 Java 程序。

[Jenkins 用户手册](https://www.jenkins.io/zh/doc/)

这里仅记录Window相关的操作

## 环境

### Java

安装Jenkins前请先确保电脑已经有java环境，并且确保java环境和Jenkins版本所匹配。这里使用的是Java 21和Jenkins 2.479.2

[Jenkins 下载地址](https://www.jenkins.io/zh/download/)

## Window安装

### jenkins.msi安装

![image-20241202142344056](Jenkins安装教程/image-20241202142344056.png)

![image-20241202142351474](Jenkins安装教程/image-20241202142351474.png)

![image-20241202142401452](Jenkins安装教程/image-20241202142401452.png)

![image-20241202142416532](Jenkins安装教程/image-20241202142416532.png)

![image-20241202142423692](Jenkins安装教程/image-20241202142423692.png)

![image-20241202142432980](Jenkins安装教程/image-20241202142432980.png)

![image-20241202142439251](Jenkins安装教程/image-20241202142439251.png)

![image-20241202142446532](Jenkins安装教程/image-20241202142446532.png)

![image-20241202142454043](Jenkins安装教程/image-20241202142454043.png)

## 创建管理员用户

打开[Jenkins](http://localhost:9090)后台，端口号为上述设置的端口号。

![image-20241202143041046](Jenkins安装教程/image-20241202143041046.png)

![image-20241202143019006](Jenkins安装教程/image-20241202143019006.png)

![image-20241202143054230](Jenkins安装教程/image-20241202143054230.png)

![image-20241202145755862](Jenkins安装教程/image-20241202145755862.png)

![image-20241202144044533](Jenkins安装教程/image-20241202144044533.png)

创建管理员用户

![image-20241202144131608](Jenkins安装教程/image-20241202144131608.png)

![image-20241202144204926](Jenkins安装教程/image-20241202144204926.png)

![image-20241202144218893](Jenkins安装教程/image-20241202144218893.png)

![image-20241202144239351](Jenkins安装教程/image-20241202144239351.png)

此时Jenkins已经安装完成。

## 常用设置

### 运行或关闭Jenkins

Win+R 运行 compmgmt.msc 

![image-20241202145229679](Jenkins安装教程/image-20241202145229679.png)

 找到下图Jenkins可以修改Jenkins状态。

![image-20241202145336507](Jenkins安装教程/image-20241202145336507.png)

### 修改工作目录

找到Jenkins安装目录下的jenkins.xml并打开。

![image-20241202144856769](Jenkins安装教程/image-20241202144856769.png)

修改上图上的路径到想要保存的目录。例如：J:\Jenkins\JenkinsWorkSpace

![image-20241202145110038](Jenkins安装教程/image-20241202145110038.png)

保存文件，并重启Jenkins即可

### 开启可注册用户

按照下图操作即可

### ![image-20241202154329593](Jenkins安装教程/image-20241202154329593.png)

![image-20241202154343859](Jenkins安装教程/image-20241202154343859.png)

![image-20241202154543032](Jenkins安装教程/image-20241202154543032.png)