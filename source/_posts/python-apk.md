---
categories: python
tags: python生成apk
title: python生成apk
---

## 第一种方式：beeware

beeware 官网 ：<https://docs.beeware.org/en/latest/tutorial/tutorial-0.html>

### 1、前提：前提需要下载python3

### 2、可能遇到的问题：

#### 1、只下载了platform-tools的情况下，需要单独下载新版 android sdk,由于android sdk官网已经不支持单独下载了，所以可以下载sdk manager的zip包

#### 2、briefcase build android 过程中会提示无license

可以通过进入sdk manager执行./sdkmanager --license 生成licens文件夹，复制到platform-tools/tools 内

#### 3、可能提示无ndk等问题，需要下载ndk

## 第二种方式：kivy

