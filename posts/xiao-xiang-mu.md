---
title: '小项目'
date: 2020-05-05 22:44:23
tags: []
published: true
hideInList: true
feature: 
isTop: false
---
### 【Good Code】

* [DataEditH5](http://souxy.com/demo/edit.html)

### 【Old Idea】
* 2011年 -- 基于地理位置的LBS商户美食推荐系统
* 2012年 -- 第四方物流与供应链系统（类似现在的滴滴货车和货嘀运力调度中心http://data.tf56.com/huodi.html?city=1）
* 2012年 —- 知识共享系统


### 【project】数据可视化配置平台
基于react + dva + redux的可视化配置数据平台的前端工作流程设计方案（不断完善中）

解决需求：

+ 解决管理后台动态可配置

+ 解决数据图表动态添加修改可配置

+ 解决数据源与数据图表可配置

> 落地产品：类似阿里DataV，和常见的H5制作工具，灵活且功能实用。

前端流程设计：
https://kdocs.cn/l/sKOMtserU?f=101
[文档] 大数据平台构建流程.pom

### 【project】打造一套完整的访客系统，从前端到后台

> 应用场景：从用户端的访客预约信息提交，到信息管理，再到线下访客机的实体验证，认证反馈。从软件到硬件，用户端到现场端，解决这一问题。

1. 用户角色：访客信息录入—— H5、小程序、pc web 管理端

2. 访客角色：访客信息查询——H5、小程序

3. 访客机：访客角色验证、访客卡领取、用户&访客（团）批处理

技术实现：

前端：vue + vuex + taro = H5 & 小程序； react 管理端

后台：nodejs + mangoDB

<!-- 访客系统   会议预定系统   场馆预订系统  圣诞晚会抽票  摇奖系统  商品选购下订单  数据展示定制化平台 -->

<!-- ### 【project】赛车拉力***
实现平台：微信小游戏，实时对战3v3，2v2，个人障碍赛，个人竞速赛

故事一：巴音布鲁克赛道

故事二：罪恶都市赛道

故事伞：乡村赛道

实现技术栈：JS，webscoket，webgl3D，wechat SDK，nodejs，重力场

> 模型设计、技术选型可以参考 https://bruno-simon.com/  ，操控可以参考微信小游戏模式、王者荣耀、吃鸡模式
https://greensock.com/showcase/

 -->

### 【project】网址收藏夹
类似百度网址个性页。
功能列表：

1、可以导入html格式的网址收藏夹

2、可以手动添加、删除、修改、查询，管理已有的收藏夹地址

3、可以灵活拖动网址

4、每个网址会自动检测该地址是否可以打开，若可以打开，则提取该网址ico作为图标

5、所见即所得的交互方式

> 可参考百度网址收藏夹
https://www.baidu.com/
https://withpinbox.com/
http://yijee.esgao.cn/profile
chrome://bookmarks/


### 【project】工作日历系统
可以用来管理日常工作的录入，类似mac系统自带的日历系统和google日历

常用功能：

1、工作颗粒度录入，以及该工作的项目归属

2、团队项目管理

3、数据统计分析、报表输出

3、集成工时系统

> 参考google日历
https://calendar.google.com/calendar/r 

> https://fullcalendar.io/  
> https://ui.toast.com/tui-calendar

<!-- ### 【project】 精灵盒子

一种集成内置全息投影、AR效果的，正方体，大概45cm * 45cm * 45cm，里面的游戏，可以渲染成为场景，沙盘，建筑或者塔防游戏，可以给人以AR或者全息投影的真实效果，通过内置计算模块和六面投影屏幕，从外向内看到的就是很逼真的场景，用户可以通过手机app、或者游戏手柄来操作里面的人物进行任务闯关，值得实现的游戏有《纪念碑谷》、《生化危机》 -->

### 【project】 手势识别、声音识别来操作窗帘

用手挥动来开关窗帘。用屋内摄像头捕捉识别人体手势，传送到后台进行分析，通过机器学习、手势识别技术，分析出是哪一种手势，再传出指令调动窗帘上安装的电机执行打开或者收起的效果。
> 人体手势=>摄像头=>后端计算识别手势指令=>发出开关指令=>电机转动

用声音来操控窗帘。
> 人体声音=>麦克风=>后端计算识别声音指令=>发出开关指令=>电机转动

从前端传输到后端的以中心计算的设计模式，也可以改为终端识别即可执行指令，即将摄像模块、计算模块、控制模块集成一体到终端中，在终端中完成一系列的操作。不需要进行网络传输，保证在离线模式下也可以正常使用。在大规模识别计算场景中，将计算服务放在终端，也是一种很不错的模式，远端只需要定期对终端进行服务升级。

### 【project】 AI迷宫终结者

先对迷宫算法进行策略算法实现，通过Tensorflow框架进行迷宫地图的算法模型训练，让其能力演变为对任意迷宫地图都能够做到图形识别，提取迷宫模型骨骼图亦或是点阵图，达到算法能够识别进行的程度。交互方式上，从起点或者图中任意一点划出一条红线连接到终点。

> 用户上传迷宫图=>迷宫模型提取=>算法计算最优路径=>绘制路线
<img src="https://img-bbs.csdn.net/upload/201508/04/1438700761_871230.jpg">
<!-- 
https://tensorflow.google.cn/js/models
http://blog.sciencenet.cn/blog-671857-567654.html
https://www.samyzaf.com/ML/rl/qmaze.html  
http://www.webhek.com/apps/PathFinding/
-->

### 【project】 AI五子棋

使用CSS3开启GPU硬件加速提升网站动画渲染性能
2014年03月12日 | 彬Go
http://blog.bingo929.com/transform-translate3d-translatez-transition-gpu-hardware-acceleration.html


纯JS 智能五子棋 初级版 
http://blog.sina.com.cn/s/blog_74d6cedd0100ywow.html
纯JS 智能五子棋 out版 
http://blog.sina.com.cn/s/blog_74d6cedd0100ywhv.html

纯JS开发五子棋游戏，JavaScript五子棋初级版
http://www.veryhuo.com/down/html/44497.html

三三禁手
https://zhidao.baidu.com/question/17134706.html

### 【project】 翰林贴，按笔画书写汉字，带动画

http://bishun.shufaji.com/0x963F.html

### 【星图】 寻找星座的位置

构建webgl版的3D找星座网站

### 【音乐聚合】 各大类音乐网站资源的聚合页

提供一个可以播放的界面，并展示来源网站

优化推荐算法，来提升推荐的效果

http://yuhuiyu.com/paihangbang/jingdian/
http://www.zuiqin.com/?name=%E5%90%BB%E5%88%AB&type=netease
http://ww1.azp2.com/mp3/
