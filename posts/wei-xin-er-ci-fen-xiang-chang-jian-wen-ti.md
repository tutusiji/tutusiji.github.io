---
title: '微信二次分享常见问题'
date: 2023-05-07 01:52:41
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
### 1、需要调用比较新的JSSDK
```<script src="//res.wx.qq.com/open/js/jweixin-1.6.0.js"></script>```

### 2、是否微信环境判断
```javascript
const isWeChat = () => {
    // window.navigator.userAgent属性包含了浏览器类型、版本、操作系统类型、浏览器引擎类型等信息，这个属性可以用来判断浏览器类型
    const ua = window.navigator.userAgent.toLowerCase()
    // 通过正则表达式匹配ua中是否含有MicroMessenger字符串
    // eslint-disable-next-line
    if (ua.match(/MicroMessenger/i) == 'micromessenger') {
        return true
    }
    return false
}
```
### 3、发送请求拿到后端的校验签名
```javascript
import axios from 'axios'
import { isWeChat } from './env-helper'

if (isWeChat()) {
    // 微信二次分享
    const data = {
        version: '2.0.0',
        type: 'H5',
        url: window.location.href.split('#')[0]
    }
    const shareData = {
        title: '', // 分享标题
        desc: '', // 分享描述
        link: '', // 分享链接，该链接域名或路径必须与当前页面对应的公众号 JS 安全域名一致
        imgUrl: '', // 分享图标
        success: function () {
          // 设置成功
        }
    }
    axios({
        headers: {
            'Content-Type': 'application/json'
        },
        method: 'post',
        url: `//${apiBase}/coupon/weixin/getWeixinShareCfg`,
        data: JSON.stringify(data)
    })
        .then((res) => {
            const info = res.data.data
            window.wx.config({
                  debug: true, // 开启调试模式,调用的所有 api 的返回值会在客户端 alert 出来，若要查看传入的参数，可以在 pc 端打开，参数信息会通过 log 打出，仅在 pc 端时才会打印。
                  appId: '', // 必填，公众号的唯一标识
                  timestamp: , // 必填，生成签名的时间戳
                  nonceStr: '', // 必填，生成签名的随机串
                  signature: '',// 必填，签名
                  ...info,
                  jsApiList: ['updateAppMessageShareData', 'updateTimelineShareData'] // 必填，需要使用的JS接口列表
            })
            window.wx.ready(() => {
                window.wx.updateAppMessageShareData(shareData) // “分享给朋友”及“分享到QQ”
                window.wx.updateTimelineShareData(shareData) // “分享到朋友圈”及“分享到QQ空间”
            })
            window.wx.error((resp) => {
                console.error(`[WechatAgent] 初始化微信sdk时发生异常: ${JSON.stringify(resp)}`)
            })
        })
        .catch((error) => {
            console.error(`[WechatAgent] 调用微信sdk初始化配置接口时发生异常: ${JSON.stringify(error)}`)
      })}
```
这样即为注册微信分享和朋友圈分享成功。

**如果还是分享不成功或者 微信调用时，提示invalid signature，即签名错误解决办法：**
一、接口配置信息配置，配置这项主要用于你自己服务器检验确定信息来自微信服务器。必须配置，但你可以不使用，你能通过这个配置判断信息是不是微信服务器的返回结果，避免网络安全中有人伪装微信服务器给你返回结果。当然如果你觉得自己的公众号程序达不到让人伪装欺诈的地步，你可以配置了不使用。按照接入接口教程还是比较容易完成的。
![](https://www.tuziki.com/post-images/1683396058261.png)
二、JS接口安全域名配置，微信服务器只响应配置的域名服务器进行JS接口调用，并且避免有人截断伪造信息，还需要利用微信公众号的方法进行签名认证。
![](https://www.tuziki.com/post-images/1683396148069.png)
三、配置好两边的服务器后，也会出现签名无效。

配置Js安全接口域名，微信服务器可以保证请求是你的服务器发出的，Signature可以保证数据传输途中没有被人截断修改过。

invalid signature签名错误。建议按如下顺序检查：

1.确认签名算法正确，可用http://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=jsapisign 页面工具进行校验。
2.确认config中nonceStr（js中驼峰标准大写S）, timestamp与用以签名中的对应noncestr, timestamp一致。
3.确认url是页面完整的url(请在当前页面alert(location.href.split('#')[0])确认)，包括'http(s)😕/'部分，以及'？'后面的GET参数部分,但不包括'#'hash后面的部分。
4.确认 config 中的 appid 与用来获取 jsapi_ticket 的 appid 一致。
5.确保一定缓存access_token和jsapi_ticket。
**6.确保你获取用来签名的url是动态获取的，动态页面可参见实例代码中php的实现方式。如果是html的静态页面在前端通过ajax将url传到后台签名，前端需要用js获取当前页面除去'#'hash部分的链接（可用location.href.split('#')[0]获取,而且需要encodeURIComponent），因为页面一旦分享，微信客户端会在你的链接末尾加入其它参数，如果不是动态获取当前链接，将导致分享后的页面签名失败。**

这个过程中最容易出错的是第6步， 如果签名一致，那问题基本出在浏览器访问的url和参与生成签名的url不一致。
第6步中（1）将微信请求的链接提取出来var htmlUrl = location.href.split('#')[0];
            （2）encodeURIComponent(htmlUrl)；传送到后台的URl要经过encodeURIComponent()处理。
```javascript
$(function (){
	var htmlUrl = location.href.split('#')[0];
	$.ajax({type:"POST", url:"${contextPath}RealTimeTicket.do", data:{"code":1, "htmlUrl": encodeURIComponent(htmlUrl)}, dataType:"JSON",
		success: function(res) {
		       if('success' == res.result){
				wx.config({
				    debug: true, 
					appId: res.appId, // 必填，公众号的唯一标识
					timestamp: res.timestamp, // 必填，生成签名的时间戳
					nonceStr: res.nonceStr, // 必填，生成签名的随机串
					signature: res.signature,// 必填，签名
					jsApiList: ["openLocation", "getLocation"] 
					});
						console.log('successful');
					} else {
						alert('ticket获取失败!');
					}
				}
			});
```
（3）Java后台获取URl生成Signature，URL也要经过反encodeURI处理String URL=new java.net.URI(htmlUrl).getPath();这样获取的链接才是微信服务器生成Signature所用的URL。试想一下：你生成Signature用的是你自己获得的URL，微信服务器生成Signature用的是它实际收到请求的URL,两者生成的signature肯定不一样，微信服务器肯定返回你的Signature无效（invalid signature）
 
微信 JS 接口签名校验工具
https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=jsapisign

分享接口及参数
https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html#4

（部分知识点总结来自https://blog.csdn.net/zhh0310235/article/details/102835292）

