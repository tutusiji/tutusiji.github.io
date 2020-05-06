---
title: 'JavaScript -- 定义二维数组'
date: 2020-05-07 00:22:55
tags: [Array]
published: true
hideInList: false
feature: 
isTop: false
---
### 方法一：直接定义并且初始化，这种遇到数量少的情况可以用
``var _TheArray = [["0-1","0-2"],["1-1","1-2"],["2-1","2-2"]]``

### 方法二：未知长度的二维数组
```javascript
var tArray = new Array();  //先声明一维
for(var k=0;k<i;k++){    //一维长度为i,i为变量，可以根据实际情况改变
 
tArray[k]=new Array();  //声明二维，每一个一维数组里面的一个元素都是一个数组；
 
for(var j=0;j<p;j++){   //一维数组里面每个元素数组可以包含的数量p，p也是一个变量；
 
tArray[k][j]="";    //这里将变量初始化，我这边统一初始化为空，后面在用所需的值覆盖里面的值
 }
}
给定义的数组传入所需的值
tArray[6][1]=5；//这样就可以将5的值传入到数组中，覆盖初始化的空
```
### 方法三：在这之前，以上两者方法都有问题，方法二，每次定义都初始化了，虽然后面可以动态修改，但是还是不方法

所以我尝试了一种动态传入值到数组的方法

ps:一些在实践过程中遇到的数组有趣的现象

本来以为二维数组可以像下面这样直接传入值
```javascript
for(var a=0;a<i;a++){
    tArray[a]=(matArray[a],addArray[a]); //matArray[a]和addArray[a]是两个数组，这两个数组直接传入tArray[a]中
};
```
结果是tArray[a]中收到的是后面一个数组的值，matArray[a]的内容被忽略的，如果换一个位置，matArray[a]在后面，则传入的是addArray[a]的值。

思考：简单的例子：
代码如下:
```javascript
var a=[1,2];
var b=[];
b[0]=a;//把数组a作为b数组的元素传入b数组中
alert(b[0][1]);  //2
```
上面是最简单的二维数组，
上面例子换种写法：
代码如下:
```javascript
var b=[];
b[0]=[1,2];//把数组[1,2]作为b数组的元素传入b数组中
alert(b[0][1]);  //2
```

可以看出上面的b[0]=[1,2]是可以用的
代码如下:

```javascript
for(var a=0;a<i;a++){
    tArray[a]=[ matArray[a],addArray[a] ];  //上面例子中的（）修改为[] 就可以成功的组成一个二维数组了
};
总结：方法三：
代码如下:
```javascript
for(var a=0;a<i;a++){
    tArray[a]=[ aArray[a],bArray[a],cArray[a]]; 还可以增加dArray[a],eArray[a]
};
```
这种情况适用于已知几个数组，把他们组合成一个二维数组情况

JS 创建多维数组
```javascript
  var allarray=new Array();
  var res="";
  function loaddata()
  {
   for(var i=0;i<3;i++)
 {
 var starth=i*200;
 var strarw=i*200;
 var endh=(i+1)*200;
 var endw=(i+1)*200;
 allarray[i]=new Array();
 allarray[i][0]=new Array();
 allarray[i][1]=new Array();
 allarray[i][0][0]=starth;
 allarray[i][0][1]=strarw;
  allarray[i][1][0]=endh;
 allarray[i][1][1]=endw;
 }
  for(var i=0;i<allarray.length;i++)
  {
    var sh=allarray[i][0][0];
    var sw=allarray[i][0][1]
     var eh=allarray[i][1][0];
    var ew=allarray[i][1][1]
    res+="第"+i+"个坐标的开始坐标是："+sh+","+sw+"结束坐标是："+eh+","+ew+"<br/>";
  }
  document.getElementById("dv").innerHTML=res;
  }
```