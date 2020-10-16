---
title: '【算法】二维数组转树形数据结构'
date: 2020-10-16 14:25:19
tags: [算法]
published: true
hideInList: false
feature: 
isTop: false
---
给定一个二维数组转换为树形数据结构。
二维数组如下：
```javascript
const array = [
    ["a", "aa", "aaa", "aaaa"],
    ["b", "bb", "bbb"],
    ["a", "ab", "aba"],
    ["a", "aa", "aab"]
] 
```
目标树形结构如下：
```javascript
const json = [
    {
        "name" : "a",
        "child" : [
            {
                "name" : "aa",
                "child" : [
                    {
                        "name" : "aaa",
                        "child" : [
                            {
                                "name" : "aaaa",
                                "child" : []
                            }
                        ]
                    },
                    {
                        "name" : "aab",
                        "child" : []
                    }
                ]

            },
            {
                "name" : "ab",
                "child" : [
                    {
                        "name": "aba",
                        "child" : []
                    }
                ]

            }
        ]
    },
    {
        "name": "b",
        "child" : [
            {
                "name" : "bb",
                "child" : [
                    {
                        "name" : "bbb",
                        "child" : []
                    }
                ]
            }
        ]
    }

]
```
**某大佬给出的方法及思路：**
1. 首先你要把一个一维数组转换为一个简单的树/列表，就是循环处理好数组中的每个元素，构造出
   `` { name: 'a', children: [] }`` 结构
2. 然后针对这个二维数组可以构造出 4 个树/列表
3. 观察这 4 个中间结果，找到合并的规则（字母开头要一致，公共父节点值）
4. 编写一个合并两个树/列表的函数，应用第三步整理的规则

**我的思路：**
1、合并二维数组为一维数组
2、数组去重
3、对这个一维数组进行改造，对每一项标注出它的父级parentId，单字母的parentId为0，具体实现方法为，将每一项去掉尾字母，在新数组里遍历查找与之匹配的那一项，获取其下标 +1，即为该父级parentId
4、对新数据进行遍历，查找每一项的子集数组，如果存在，则给这一项添加children属性，并赋值，否则返回第一层
```javascript
const array = [
    ["a", "aa", "aaa", "aaaa"],
    ["b", "bb", "bbb"],
    ["a", "ab", "aba"],
    ["a", "aa", "aab"]
] 
const arr = []
array.forEach((item) => {
    item.forEach((i) => {
        arr.push(i)
    })
})
const brr = Array.from(new Set(arr))
const tree = []
// ["a", "aa", "aaa", "aaaa", "b", "bb", "bbb", "ab", "aba", "aab"]
brr.forEach((j, index) => {
    tree.push({
        id: index + 1,
        parentId: checkId(j),
        name: j
    })
})
function checkId(j) {
    const aaa = brr.findIndex(k => j.slice(0, j.length - 1) === k)
    console.log('--', j, aaa + 1)
    return aaa + 1
}
console.log(tree)
// 0: {id: 1, parentId: 0, name: "a"}
// 1: {id: 2, parentId: 1, name: "aa"}
// 2: {id: 3, parentId: 2, name: "aaa"}
// 3: {id: 4, parentId: 3, name: "aaaa"}
// 4: {id: 5, parentId: 0, name: "b"}
// 5: {id: 6, parentId: 5, name: "bb"}
// 6: {id: 7, parentId: 6, name: "bbb"}
// 7: {id: 8, parentId: 1, name: "ab"}
// 8: {id: 9, parentId: 8, name: "aba"}
// 9: {id: 10, parentId: 2, name: "aab"}
const cloneData = tree.slice()
const last = cloneData.filter((father) => {
    const branchArr = cloneData.filter(child => fat her.id === child.parentId) // 返回每一项的子级数组
    // branchArr.length > 0 ? father.children = branchArr : '' // 给父级添加一个children属性，并赋值
    if (branchArr.length > 0) {	
        father.children = branchArr	//如果存在子级，则给父级添加一个children属性，并赋值
    }
    return father.parentId === 0 // 返回第一层
})
console.log(last) // 树形数据
```
