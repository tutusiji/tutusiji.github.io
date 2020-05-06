---
title: '正则表达式常用校验规则'
date: 2020-05-05 22:35:16
tags: [正则]
published: true
hideInList: false
feature: 
isTop: false
---

* 校验11位手机号码  `reg=/^[1][3,4,5,7,8][0-9]{9}$/`
```javascript
function checkPhone(val){
  var reg=/^[1][3,4,5,7,8][0-9]{9}$/
  if (!myreg.test($poneInput.val())) {  
      return false;  
  }
  return true;  
}
```
* 校验短信验证码：`/[0-9]{6}/`

* 身份证校验: `/^[1-9]\d{5}(18|19|20|(3\d))\d{2}((0[1-9])|(1[0-2]))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/`