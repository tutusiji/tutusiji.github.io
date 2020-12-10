---
title: 'vue与react生成前端二维码的QRcode组件使用'
date: 2020-12-10 15:14:04
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
## VUE版本：

### 安装:
``` html
npm install qrcode

import QRCode from 'qrcode'
```

### DOM:
``` html
<canvas id="canvas"></canvas>
```
### 页面调用
``` javascript
this.useqrcode('http://www.baidu.com')

useqrcode(url) {
    const canvas = document.getElementById('canvas')
    QRCode.toCanvas(
        canvas,
        url,
        {
            scale: 5.0,
            height: 180,
            wight: 180
        },
        error => {
            if (error) console.error(error)
            console.log('success!')
        }
    )
}

```

<!-- more -->
## React版本：
### 安装:
``` html
npm install qrcode

import QRCode from 'qrcode.react'
```
### DOM:
``` javascript
{this.renderCanvas(url)}
```
### 渲染:
``` javascript
renderCanvas = url => {
    return (
        <div style={{ width: '200px'}}>
            <p style={{ wordBreak: 'break-all' }}>{url}</p>
            <QRCode
                value={url} //value参数为生成二维码的链接
                size={200} //二维码的宽高尺寸
                fgColor="#000000" //二维码的颜色
            />
        </div>
    )
}
```