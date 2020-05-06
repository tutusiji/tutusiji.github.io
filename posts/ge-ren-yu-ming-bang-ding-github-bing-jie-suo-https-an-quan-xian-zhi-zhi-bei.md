---
title: '个人域名绑定github并解锁https安全限制指北'
date: 2020-05-05 22:28:19
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
网上的教程比较坑，且很老不用看了。

1. 在github上构建好自己的网站，并在master分支下的Settings
———— Repository name ，修改名称为 tutusiji.github.io

2. 在下面的 GitHub Pages 里面设置自定义域名为 tuziki.com，并在master分支下面创建文件CNAME内容为 tuziki.com

3. 到域名解析商那里修改域名解析：改 `CNAME	www 到 tutusiji.github.io`，改  `CNAME	 * 到 tutusiji.github.io` 或者再加上 `CNAME	@ 到 tutusiji.github.io `

> 到这里域名基本可以访问，但是浏览器会提示安全限制，就是证书没有得到安全认证。去找一个可以提供免费SSL证书的域名商转接一下就好。

4. Cloudflare (https://dash.cloudflare.com) 提供的免费SSL。进行下面几步操作：
    1. 创建CloudFlare帐户，并添加网站
    2. 输入域名、点击扫描、进入面板页面、并选择免费计划，下一步~
    3. 到DNS页面设置域名解析，如图：
![](https://www.tuziki.com/post-images/1588688928570.png)
    4. 到Page Rules页面设置域名重定向：选择 Forwarding URL -- 301
    ```
    http://tuziki.com/ => https://tuziki.com
    http://www.tuziki.com/ => https://tuziki.com
    http://*.tuziki.com/* => https://*.tuziki.com/*
    ```

5. Done and Refresh.