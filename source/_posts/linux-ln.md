---
categories: linux
tags: linux设置软链接
title: linux设置软链接
---

找到MySQL的安装路径： 
```
which mysql
```
假设找到的是:
```
/home/user1/mysql/bin/mysql
```
建立软连接  
```
cd /usr/bin && ln -fs /home/user1/mysql/bin/mysql mysql
```
