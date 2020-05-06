---
title: 'JS数组常用方法总结 some()、every()、find()、findIndex()、filter()、includes()、map()、fill()'
date: 2020-05-05 22:22:22
tags: [数组]
published: true
hideInList: false
feature: 
isTop: false
---

> some() 会遍历数组中第一个与条件符合的并返回true

```JavaScript
// 1. some使用 es6新增方法，和es5的数组forEach类似
var data = [
    { id: 1, name: 'wzj' },
    { id: 2, name: 'zr' },
    { id: 3, name: 'dxy' }
];

// 找出 id 为 2 的对象
var result = data.some(function (value) {
    // 若没有返回值，则会一直循环遍历，类似forEach
    console.log(value);

    return value.id == 2;
});

console.log(result);//true

```
> find: 快速查找返回对象中匹配到key的item

```JavaScript
const dataItem = DataObject.find(item => item.key === "keys")
```

> find: 合并两个数组中的目标值

```
const listA =[{
      id: 'a1',
      type: 'yyy1'
    },{
      id: 'a2',
      type: 'yyy2'
    },{
      id: 'a3',
      type: 'yyy3'
    }]
const listB =[{
      id: 'a1',
      name: 'xxx1'
    },{
      id: 'a2',
      name: 'xxx2'
    },{
      id: 'a3',
      name: 'xxx3'
    }]
    listA.map(i=>{
      const targetItem = listB.find(item =>item.id === i.id )
      i.name = targetItem.name
      return listA
    })

    console.log( listA)
```
![](https://www.tuziki.com/post-images/1588688590104.png)