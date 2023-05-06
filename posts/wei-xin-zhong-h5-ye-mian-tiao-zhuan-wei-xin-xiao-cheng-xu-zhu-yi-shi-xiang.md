---
title: '微信中h5页面跳转微信小程序注意事项'
date: 2023-05-07 02:12:19
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
## **具体操作可以看官方文档**
https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/Wechat_Open_Tag.html

## **下面主要总结一些过程中可能遇到的问题:**
1. 域名是https并且备案
2. 公众号JS安全域名里面有登记你的前端域名,https://mp.weixin.qq.com/
3. ip白名单列表里面有登记服务器出口外网IP地址
4. 公众号必须是服务号,订阅号不可以用,非个人主体的
5. 遇到VUE报错 <wx-open-launch-weapp> 未定义,需要新增这个标签,在main.js中添加 Vue.config.ignoredElements.push('wx-open-launch-weapp')  ,如果使用了 Vue.config.ignoredElements=['wx-open-launch-weapp'] 会覆盖所有自定义标签,不可用
6. 微信开发者工具和真机都可以显示出按钮的,chrome浏览器不行
7. 后台接口已经完成签名校验,并返回正确的值,注意前后端使用的appid必须是同一个
8. JSSDK要使用高版本的,https://res2.wx.qq.com/open/js/jweixin-1.6.0.js 
9. 确认config中nonceStr（js中驼峰标准大写S）, timestamp与用以签名中的对应noncestr, timestamp一致
10. 最好开启wx.config的debug模式,方便调试
11. jsApiList:['chooseImage', 'previewImage']（必须有，不然安卓不显示）
12. 微信版本要求为：7.0.12及以上。 系统版本要求为：iOS 10.3及以上、Android 5.0及以上
13. wx.config中的配置项必填的一定是必填的
14. 不能用js来模拟点击，有局限性
15. 样式无法写在外面中，只能在script标签内内链写或者行内样式
16. 无论是内链还是行内 都不支持rem
17. 不会继承样式
18. 如果开发标签内需要使用图片，不能用本地图片，得用外网可以访问的图片，要不然会不显示
19. path是需要跳转的微信小程序的路径，需要在路径后边添加.html。否则跳转不过去
20. html 页面可以用 <template></template> 标签包裹，Vue 页面要改用 <script type="text/wxtag-template"></script> 包裹
21. 后端的加密方法是 hex_sha1
22. username是需要跳转小程序的原始id，以gh_开头

```javascript
<template>
    <div class="home-wrap">
        <div class="top">
            目标ID: {{ this.id }} <br /><br />
            目标路径: {{ this.url }}
        </div>
        <div class="xxbtn">
            <wx-open-launch-weapp id="launch-btn" :username="id" :path="`${url}.html`">
                <script type="text/wxtag-template">
                    <style>.openbtn { display: block;width: 280px;height: 42px;line-height: normal;border: none;background-color: #11961c;color: #fff;font-size: 18px;border-radius: 6px;}</style>
                    <button class="openbtn">打开小程序</button>
                </script>
            </wx-open-launch-weapp>
        </div>
    </div>
</template>
  html 页面可以用 <template></template> 标签包裹，Vue 页面要改用 <script type="text/wxtag-template"></script> 包裹


<script>
import {
    isInWechat, isInApp, getUrlParamValue, getToken
} from '@/common/util/env-helper'
import { getSignData } from '../service/api'

export default {
    name: 'Home',
    data() {
        return {
            isInWechat: isInWechat(),
            isInApp: isInApp(),
            code: getUrlParamValue('code'),
            token: getToken(),
            id: '',
            url: ''
        }
    },
    created() {
        const { id, url } = this.$route.query
        this.id = id
        this.url = url
    },
    async mounted() {
        if (this.id && this.url) {
            await this.pageConfig()
        }
    },
    methods: {
        async pageConfig() {
            console.log('this.isInApp :>> ', this.isInApp)
            if (this.isInApp) {
                // const data = await checkUser()
                // if (data) {
                //     this.$router.push('/record')
                // }
            }
            // if (this.isInWechat) {}
            const params = {
                version: '2.0.0',
                type: 'H5',
                // url: 'https://cxxxxxx.com/szswj/stg2/app/feature/reserve/#/'
                url: window.location.href.split('#')[0]
            }
            const res = await getSignData(params)
            console.log('res', res.data)
            const info = res.data
            const configs = {
                debug: true,
                appId: 'wx123456789',
                ...info,
                jsApiList: [
                    'showOptionMenu',
                    'updateAppMessageShareData',
                    'updateTimelineShareData',
                    'chooseImage',
                    'previewImage'
                ],
                openTagList: ['wx-open-launch-weapp'] // 必填，需要使用的JS接口列表
            }

            console.log('configs---', configs)
            window.wx.config(configs)

            window.wx.ready(() => {
                console.log('ready')
                // window.wx.updateAppMessageShareData(shareData) // “分享给朋友”及“分享到QQ”
                // window.wx.updateTimelineShareData(shareData) // “分享到朋友圈”及“分享到QQ空间”
            })
            window.wx.error(resp => {
                console.error(`[WechatAgent--] 初始化微信sdk时发生异常: ${JSON.stringify(resp)}`)
            })
        }
    }
}
</script>
<style lang="scss" scoped>
.xxbtn{
    margin: 200px auto 0;
    width:280px;
}

</style>
```
**类似案例:**
https://developers.weixin.qq.com/community/develop/article/doc/0006ac9d35cc38455deb68ee350813
https://www.cnblogs.com/Can-daydayup/p/15404964.html#_label1
https://blog.csdn.net/Angelheca/article/details/125444337
https://blog.csdn.net/qq_32684617/article/details/124901270
https://blog.csdn.net/weixin_44265066/article/details/125673122    云函数跳转
