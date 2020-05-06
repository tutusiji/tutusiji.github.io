---
title: 'react ant 中处理table列内容太多换行'
date: 2020-05-05 22:30:03
tags: [react ]
published: true
hideInList: false
feature: 
isTop: false
---
```   JavaScript   
render: (text, record) =>{
    let snArray = [];
    snArray = text.split('+');
    let br = <br/>;
    let result = null;
    if (snArray.length < 2) {
        return text;
    }
    for (let i = 0; i < snArray.length; i++) {
        if (i === 0) {
            result = snArray[i];
        } else {
            result = (<span>{result}{br}{snArray[i]}</span>);
        }
    }
    return <div>{result}</div > ;
}
```