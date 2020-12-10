---
title: 'VUE中watch 和 computed的区别详解'
date: 2020-12-09 16:55:45
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
``watch``: 监视,能够监听到数据的变化,只要数据变化的时候,都会自定执行对应的方法,其中可以检测的数据来源分为三部分 data , computed , props
### 侦听属性watch：
>
+ 不支持缓存，数据变，直接会触发相应的操作
+ watch支持异步
+ 监听的函数接收两个参数，第一个参数是最新的值；第二个参数是输入之前的值
+ 当一个属性发生变化时，需要执行对应的操作；一对多
+ 监听数据必须是data中声明过或者父组件传递过来的props中的数据，当数据变化时，触发其他操作，函数有两个参数：
>

<!-- more -->

``computed``:  计算属性,存在一个计算缓存的特性,每一次计算之后,只要里面的逻辑不发生变化,每一次重复调用,都会使用上一次执行的结果,能够节省计算的时间
当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch,通常更好的是使用 computed 属性而不是命令式的 watch 回调

### 计算属性computed：
> 
- 支持缓存，只有依赖数据发生改变，才会重新进行计算
- 不支持异步，当computed内有异步操作时无效，无法监听数据的变化
- computed 属性值会默认走缓存，计算属性是基于它们的响应式依赖进行缓存的，也就是基于data中声明过或者父组件传递的props中的数据通过计算得到的值
如果一个属性是由其他属性计算而来的，这个属性依赖其他属性，是一个多对一或者一对一，一般用 computed
如果computed 属性属性值是函数，那么默认会走get方法；函数的返回值就是属性的属性值；在computed中的，属性都有一个get和一个set方法，当数据变化时，调用set方法。

immediate：组件加载立即触发回调函数执行
``` javascript
watch: {
  firstName: {
    handler(newName, oldName) {
      this.fullName = newName + ' ' + this.lastName;
    },
    // 代表在wacth里声明了firstName这个方法之后立即执行handler方法
    immediate: true
  }
}
```
deep: deep的意思就是深入观察，监听器会一层层的往下遍历，给对象的所有属性都加上这个监听器，但是这样性能开销就会非常大了，任何修改obj里面任何一个属性都会触发这个监听器里的 handler
```javascript
watch: {
  obj: {
    handler(newName, oldName) {
      console.log('obj.a changed');
    },
    immediate: true,
    deep: true
  }
}
```
优化：我们可以使用字符串的形式监听
```javascript
watch: {
  'obj.a': {
    handler(newName, oldName) {
      console.log('obj.a changed');
    },
    immediate: true,
    // deep: true
  }
}
```
这样Vue.js才会一层一层解析下去，直到遇到属性a，然后才给a设置监听函数。






