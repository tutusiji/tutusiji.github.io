---
title: 'swiper再度踩坑指南'
date: 2023-05-07 01:29:43
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
swiper3~8众多版本中,如果不能找对版本和使用方式,会很难受.
实现移动端轮播图,并且有缩放图片功能的方法很多
## 第一种,需要考虑横屏滑动,使用swiper 
功能需要完成横竖屏滑动,图片放大功能,左右点击
在vue2项目中使用需要注意的:
安装时需要 npm install swiper@4  才能指定安装4的版本,
其他拓展api方法可以看 https://v1.github.surmon.me/vue-awesome-swiper/  ,以及 swiper3的api,使用时需要注意哪些是4以上才可以使用的 https://www.swiper.com.cn/api/loop/22.html
(有时候不能缩放是因为html文件上设置了禁止缩放,但是如果是按下面的方法操作应该不用考虑这个问题  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">)
```javascript
"swiper": "^4.5.1",
"vant": "^2.10.2",
"vue": "^2.6.14",
"vue-awesome-swiper": "^3.1.3",
```
下面是完整的页面:
其中包含了延时加载和横屏遮罩功能;
以后的这种项目只需要上传好完图片到服务器，即可使用，不用再开发；
填好对应的值，page 服务器文件夹名称，title 图片展名称，num 图片总数， vertical 是否横屏true false ，type 图片格式
https://xxxx.com/#/imageshow?page=dashaRiver&title=xxx图片展&num=61&vertical=false&type=png
```javascript
<template>
    <div>
        <swiper
            class="swiper-container"
            :options="swiperOption"
            ref="imgOverview"
            style="height: 100%;"
        >
            <swiper-slide class="swiper-slide" v-for="img in list" :key="img">
                <div class="swiper-zoom-container">
                    <img :data-src="img" alt="" class="swiper-lazy" />
                    <div class="swiper-lazy-preloader"></div>
                </div>
            </swiper-slide>
            <div
                id="swiper-pagination"
                :class="['swiper-pagination', `${this.pageVertical && 'vv'}`]"
                slot="pagination"
            ></div>
            <!-- 分页 -->
            <div class="btnL swiper-button-prev" slot="button-prev" v-if="!pageVertical"></div>
            <div class="btnR swiper-button-next" slot="button-next" v-if="!pageVertical"></div>
            <div class="btnT swiper-button-prev" slot="button-prev" v-if="pageVertical"></div>
            <div class="btnB swiper-button-next" slot="button-next" v-if="pageVertical"></div>
        </swiper>
        <van-overlay :show="showOverlay" @click="showOverlay = false">
            <div class="wrapper" @click.stop>
                请横屏观看
            </div>
        </van-overlay>
    </div>
</template>

<script>
import 'swiper/dist/css/swiper.min.css'
import { swiper, swiperSlide } from 'vue-awesome-swiper'

export default {
    name: 'showPic',
    data() {
        return {
            current: 0,
            list: [],
            showOverlay: false,
            pageKey: '',
            pageTitle: '',
            pageNum: '',
            pageType: '',
            pageVertical: true,
            swiperOption: {
                direction: this.$route.query.vertical === 'false' ? 'horizontal' : 'vertical',
                width: window.innerWidth,
                height: window.innerHeight,
                // zoom: true,
                loop: true,
                zoom: {
                    minRatio: 1,
                    maxRatio: 10
                },
                lazy: true,
                initialSlide: 0,
                navigation: {
                    nextEl: '.swiper-button-next',
                    prevEl: '.swiper-button-prev'
                },
                pagination: {
                    el: '#swiper-pagination',
                    type: 'fraction'
                }
            }
        }
    },
    components: {
        swiper,
        swiperSlide
    },
    created() {
        const {
            page, title, num, vertical, type
        } = this.$route.query
        if (page) {
            this.pageKey = page
            this.pageTitle = title
            this.pageNum = num
            this.pageVertical = vertical !== 'false'
            this.pageType = type
            document.title = title
            this.showOverlay = vertical === 'true'
            setTimeout(() => {
                this.showOverlay = false
            }, 2200)
            this.cutImg()
        }
    },
    mounted() {},
    methods: {
        cutImg() {
            const list = []
            for (let index = 1; index <= this.pageNum; index++) {
                list.push(
                    `https://xxxx.com/prod/lib/${this.pageKey}/${index}.${this.pageType}`
                )
            }
            this.list = list
        }
    }
}
</script>

<style lang="scss" scoped>
.van-overlay {
    z-index: 10000;
}
.wrapper {
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 30px;
    color:#fff;
}
.btnL,
.btnR,
.btnT,
.btnB {
    position: absolute;
    width: 40px;
    height: 40px;
    margin-top: 0;
}
.btnL {
    top: 50%;
    left: 0;
    transform: translate(0, -50%);
    background: url('../asset/img/arrow-left.png') center no-repeat;
    background-size: 30px;
}
.btnR {
    top: 50%;
    right: 0;
    transform: translate(0, -50%);
    background: url('../asset/img/arrow-right.png') center no-repeat;
    background-size: 30px;
}
.btnT {
    top: 0;
    left: 50%;
    transform: translate(-50%, 0);
    background: url('../asset/img/arrow-up.png') center no-repeat;
    background-size: 30px;
}
.btnB {
    top: auto;
    bottom: 0;
    left: 50%;
    transform: translate(-50%, 0);
    background: url('../asset/img/arrow-down.png') center no-repeat;
    background-size: 30px;
}
// width: 7.5rem;
.swiper-container {
    width: 100%;
    height: 100%;
    .swiper-slide {
        width: 100%;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .swiper-slide img {
        max-width: 100%;
        max-height: 100%;
    }
    .swiper-pagination {
        z-index: 10000;
        position: fixed;
        bottom: 8px;
        left: 50%;
        transform: translate(-50%, 0);
        width: 50px;
        height: 20px;
        text-align: center;
        font-size: 12px;
        &.vv {
            top: 50%;
            left: -28px;
            bottom: 0;
            transform: rotateZ(90deg) translate(0, -50%);
        }
    }
    .swiper-button-prev,
    .swiper-button-next {
        position: fixed;
        outline: none;
        &:active {
            border: none;
        }
        &:focus-visible {
            border: none;
        }
    }
}
</style>
```
## 第二种,不需要考虑横屏滑动,且全屏预览,使用 vant的 ImagePreview组件
图片放大预览，支持函数调用和组件调用两种方式,用vue的vant组件库中的 ImagePreview 组件即可 , https://youzan.github.io/vant/v2/#/zh-CN/image-preview
1. 函数调用
```javascript
import { ImagePreview } from 'vant';
ImagePreview({
  images: [
    'https://img01.yzcdn.cn/vant/apple-1.jpg',
    'https://img01.yzcdn.cn/vant/apple-2.jpg',
  ],
    startPosition: 1,    // 初始位置
    showIndex: false,    // 是否显示页码
    closeable: true,      // 是否显示关闭图标
});需要注意的是,常规调用预览模式,手指点击一下,整个预览状态就会消失,需要开启异步调用方式来阻止,开启异步之后,除非手动关闭,不然会一直存在
const instance = ImagePreview({
       images: this.list,
       asyncClose: true
})

setTimeout(() => {
  instance.close();    // 
}, 2000);2.组件调用
<van-image-preview v-model="show" :images="images" @change="onChange">
  <template v-slot:index>第{{ index }}页</template>
</van-image-preview>
export default {
  data() {
    return {
      show: false,
      index: 0,
      images: [
        'https://img01.yzcdn.cn/vant/apple-1.jpg',
        'https://img01.yzcdn.cn/vant/apple-2.jpg',
      ],
    };
  },
  methods: {
    onChange(index) {
      this.index = index;
    },
  },
};
```
## 第三种,最基础的左右滑屏
即可以用vant 的 Swipe 轮播
也可以用 swiper https://www.swiper.com.cn/api/loop/22.html
项目中已经有了vant,就可以直接用了,毕竟swiper很重