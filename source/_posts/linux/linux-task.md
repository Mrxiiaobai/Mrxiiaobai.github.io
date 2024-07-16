---
categories: linux
tags: linux设置定时任务
title: linux设置定时任务
---

## 1、linux自带crontab、没有需要安装
crontab -l 查看定时任务
crontab -e 编辑定时任务
每五分钟执行  */5 * * * *

每小时执行     0 * * * *

每天执行       0 0 * * *

每周执行       0 0 * * 0

每月执行       0 0 1 * *

每年执行       0 0 1 1 *

## 2、写shell脚本 例如 hello.sh
编写第一个shell文件，#!/bin/bash 是必须要写的，表示要是/bin/bash这个执行脚本的命令执行接下来写的脚本, echo "hello world !!"表示想前端打印一句话，具体看各自需求。


```
#!/bin/bash
echo "hello world"
```

这里假如echo之类的命令不是全局，需要设置软链接,如果后面命令为某文件内命令，需设置具体路径例如

```
dotnet /data/Rat
```


## 3、修改hello.sh权限

```
chmod 777 ./hello.sh
```
## 4、试执行 ./hello.sh,命令窗口会打印出hello world

```
./hello.sh
```

## 5、crontab -e 打开窗口编辑定时任务

```
*/5 * * * * /data/hello.sh
```
表示每5分钟执行一次hello.sh

## 6、重新启动

```
service crond restart
```
出现

```
Redirecting to /bin/systemctl restart crond.service
```
执行
```
systemctl start crond
```

## 7、如何查看有没有定时执行任务

```
grep "hello" /var/log/cron
```
会提示该路径有邮件

```
/var/spool/mail/root
```
通过cat /var/spool/mail/root 查看日志
可以看到有hello world的执行时间
到这已经完成了定时执行shell脚本的功能