---
categories: linux
tags: linux服务器之间上传文件
title: linux服务器之间上传文件
---

```
scp [参数] <源地址（用户名@IP地址或主机名）>:<文件路径> <目的地址（用户名 @IP 地址或主机名）>:<文件路径>
```


1、文件从本地上传到服务器 
```
scp /home/work/source.txt 192.168.0.10:/home/work/ #把本地的source.txt文件拷贝到192.168.0.10机器上的/home/work目录下
```

2、文件从服务器下载到本地

```
scp work@192.168.0.10:/home/work/source.txt /home/work/  #把192.168.0.10机器上的source.txt文件拷贝到本地的/home/work目录下
```

3、文件从服务器a传到服务器b
```
scp work@192.168.0.10:/home/work/source.txt work@192.168.0.11:/home/work/  #把192.168.0.10机器上的source.txt文件拷贝到192.168.0.11机器的/home/work目录下
```

4、scp -r
```
scp -r /home/work/sourcedir work@192.168.0.10:/home/work/  #拷贝文件夹，加-r参数
```

