---
title: 'Linux常用操作'
date: 2020-05-05 22:33:59
tags: [Linux]
published: true
hideInList: false
feature: 
isTop: false
---

```
查看进程  ps -ef|grep nginx

关闭进程  kill -QUIT 或者 nginx -s stop

启动服务  systemctl start nginx

编辑保存 :x		:wq保存退出		:wq!保存强制退出

删除行 Esc -- 两下d

ng重启 nginx -s reload，重启之前 -t 检测

ng检测 nginx -t，如果-t之后报错可以 nginx -c /etc/nginx/nginx.conf ，再 重启

创建文件 touch xxx.xx

删除文件 rm  xxx.xx   --  y

打开编辑文件 vim 

```
 更多指令：https://blog.csdn.net/jincf2011/article/details/6363301 
