---
title: '优化嵌套的条件判断语句'
date: 2020-05-05 22:30:36
tags: [代码优化]
published: true
hideInList: false
feature: 
isTop: false
---

> 业务场景：多种状态判断的显示执行语句，需要优化js的嵌套if语句。

```JavaScript
// 操作（SUBMIT = 接件；EXAMINE = 审核；SITE_CHECK = 现场核验；WAIT_DISTRIBUTE = 分发；FOLLOW = 办理；WAIT_GET = 现场领件；APPRAISE = 查看评价）

  if (code === 'SUBMIT') {
    code = 'default'
  } else if (code === 'EXAMINE') {
    code = 'processing'
  } else if (code === 'SITE_CHECK') {
    code = 'warning'
  } else if (code === 'WAIT_DISTRIBUTE') {
    code = 'warning'
  } else if (code === 'FOLLOW') {
    code = 'warning'
  } else if (code === 'WAIT_GET') {
    code = 'warning'
  } else if (code === 'PASSED') {
    code = 'success'
  } else if (code === 'APPRAISE') {
    code = 'success'
  } else if (code === 'NOT_PASSED') {
    code = 'error'
  } else {
    code = 'default'
  }
  return <Badge status={code} text={name} />
```

这里可以直接将多个 ``if... else if ...`` 语句改写为 ``switch`` ,但是这样写其实并没有太多的优化，则需要考虑更加有效率的方法，避免过多的条件检查，可以使用 ``object``

```JavaScript
  const codeList = {
    SUBMIT: 'default',
    EXAMINE: 'processing',
    SITE_CHECK: 'warning',
    WAIT_DISTRIBUTE: 'warning',
    FOLLOW: 'warning',
    WAIT_GET: 'warning',
    PASSED: 'success',
    APPRAISE: 'success',
    NOT_PASSED: 'error'
  }
  code = codeList[code]
  return <Badge status={code} text={name} />
```