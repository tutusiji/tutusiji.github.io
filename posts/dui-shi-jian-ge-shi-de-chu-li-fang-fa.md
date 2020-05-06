---
title: '对时间格式的处理方法'
date: 2020-05-05 21:36:27
tags: [js时间处理]
published: true
hideInList: false
feature: 
isTop: false
---
适用于业务中的时间耗时展示，处理成对应的时分秒天。主要点在于对时分秒的分开取模上面，将取到的模换算成对应的时分秒或者是进行条件区间判断，比如：一分钟以内展示秒，超过30秒显示1分钟，超过、超过一天或者1小时则不显示秒
```JavaScript
function showTime(time){
    let dateTimes = ''
    const ss = time
    const days = Math.round(ss / (60 * 60 * 24))
    const hours = Math.round((ss % (60 * 60 * 24)) / (60 * 60))
    const minutes = parseInt((ss % (60 * 60)) / 60, 10)
    const seconds = ss % 60
    if (days > 1) {
        // dateTimes = `${days}天${hours}小时${minutes}分${seconds}秒`
        dateTimes = `${days}天${hours}小时${minutes}分`
    } else if (hours > 1) {
        dateTimes = `${hours}小时${minutes}分`
    } else if (minutes > 60) {
        dateTimes = `${minutes}分${seconds}秒`
    } else {
        dateTimes = `${seconds}秒`
    }
    return dateTimes
}
```

