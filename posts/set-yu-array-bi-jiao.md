---
title: 'Set() 与 Array() 比较'
date: 2020-05-05 22:31:23
tags: [Array]
published: true
hideInList: false
feature: 
isTop: false
---

React处理数据时经常遇到数据是由接口请求回来，每次触发props更新都会导致重新render，如果需要的数据是需要构造出新的数据，则一般会采用 ``Array``的push方法，但是由于多次的render导致多次push导致数据重复，改为``Set()``数据类型则会自动过滤到重复的数据：

**添加与删除**

``Set()`` 添加数据用 `.add()`方法，删除数据用 `.delete()`
> `add`方法在添加数据时一次只能添加一条数据，`Array`在push时是可以一次添加多条数据
![](https://www.tuziki.com/post-images/1588689109315.png)
![](https://www.tuziki.com/post-images/1588689123302.jpg)

``Array`` 添加数据用 `.push()`添加数据到数组尾部。
向数组的首位添加数据可以用`.unshift()`

合并多个数据用`.concat()` 将一个或者多个数组进行合并成一个新的数组，注意合并数组时被合并的数组数据会自动加到新数组的后面：
![](https://www.tuziki.com/post-images/1588689149697.png)

`Array`在删除时，有多种办法：

* `pop()` 删除数组最后一个元素并返回它
* `shift()` 删除数组第一个元素并返回它
* `arr[arr.length-1]` 选中数组最后一个数据
* `arr.slice(-1) ` 选中指定位置的元素及它以后的所有元素并返回，-1则是倒数第一个，`arrB = arrA.slice()` 为复制数组A给B
* `arr.splice(n,m)` 从第n个元素开始删除原数组中m个元素，并返回这m个元素，`arr.splice(1,arr.length)`为清空数组

**清空数组**
* `arr.splice(0,arr.length)` 使用splice函数
* `arr.length = 0` 给数组的length赋值为0
* `arr = []` 直接赋予新数组 []

*效率比较：*
```
  let a = [];
  let b = [];
  let c = [];
  for (let i = 0; i < 10000; i++) {
    a.push(i);
  }
  console.time('splice');
  a.splice(0, a.length);
  console.timeEnd('splice');

  for (let i = 0; i < 10000; i++) {
    b.push(i);
  }
  console.time('length');
  b.length = 0;
  console.timeEnd('length');

  for (let i = 0; i < 10000; i++) {
    c.push(i);
  }
  console.time('赋值[]');
  c = [];
  console.timeEnd('赋值[]');
```
结果：
```
splice: 1757.174072265625ms
length: 0.06396484375ms
赋值[]: 0.095947265625ms
```
多次测试发现第二种方式最快，第三种其次，大数据量下 第一种最慢。

> 数组的去重 和 排序

``a = Array.from(new Set(a)).sort()`` 先去重，再排序