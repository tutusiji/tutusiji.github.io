---
title: '【算法】整数反转'
date: 2020-10-16 14:17:22
tags: [算法]
published: true
hideInList: false
feature: 
isTop: false
---
问题：给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
> 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
```javascript
var reverse = function(x) {
    const str = '' + x;
    let arr = []
    let operator = ''
    if (str.startsWith('-') || str.startsWith('+')) {
        arr = str.slice(1, str.length).split('');
        operator = str[0]
    } else {
        arr = String(x).split('');
    }

    const result = Number(operator + arr.reverse().join(''));
    
    if (result < (-2) ** 31 || result > 2 ** 31 -1) {
        return 0;
    }
    return result
};
```
示例 1:
输入: 123    输出: 321

示例 2:
输入: -123   输出: -321

示例 3:
输入: 120   输出: 21




