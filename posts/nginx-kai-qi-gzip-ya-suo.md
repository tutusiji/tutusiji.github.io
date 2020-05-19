---
title: 'nginx开启gzip压缩'
date: 2020-05-14 19:53:36
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
一般情况下在nginx.conf文件里做以下配置改动就好
```nginx
http {
    ##
    # `gzip` Settings

    gzip on;
    gzip_disable "msie6";

    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_min_length 256;
    gzip_types text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;
}
```