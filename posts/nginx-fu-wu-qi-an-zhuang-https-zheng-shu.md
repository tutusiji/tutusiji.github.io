---
title: 'Nginx 服务器安装HTTPS证书'
date: 2020-05-05 22:37:32
tags: [Nginx ]
published: true
hideInList: false
feature: 
isTop: false
---

1、生成域名对应的证书

2、在ng服务器中创建一个ssl的文件夹来存放证书文件和密钥文件

3、修改nginx.conf配置，将ssl_certificate 和 ssl_certificate_key 分别指向对应的路径

4、到ng中，nginx -t 测试是否配置成功

5、重启ng，nginx -s reload，即可

https://cloud.tencent.com/document/product/400/35244
