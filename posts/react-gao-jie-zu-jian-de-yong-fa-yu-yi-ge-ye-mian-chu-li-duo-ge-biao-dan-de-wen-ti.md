---
title: 'react 高阶组件的用法与一个页面处理多个表单的问题'
date: 2020-05-05 22:36:24
tags: [react ]
published: true
hideInList: false
feature: 
isTop: false
---

通常一个页面对应一个class，而同一个class里面的form是公用同一个作用域，当需要在一个页面中操作多个不同的class时，就会遇到一些问题：
比如：两个表单都调用了``validateFields`` 时，则会互相影响。``getFieldValue``、`` getFieldsValue`` 虽然可以针对不同的 ``getFieldDecorator``进行取值，但是在表单提交校验的时候，这会连带另一个form一起校验，这样不太友好。

一般的处理方式，可以是使用一个页面中创建两个class，分别放置一个form，这样表单作用域就会各自进行校验。如果需要两个form需要数据交互的校验，则需要使用事件传递等方式通过组件之间通信的props传值，不是很方便；这里还可以使用高阶组件``wrappedComponentRef``来操作；

```
class CustomizedForm extends React.Component { ... }

// use wrappedComponentRef
const EnhancedForm =  Form.create()(CustomizedForm);
<EnhancedForm wrappedComponentRef={(form) => this.form = form} />
this.form // => The instance of CustomizedForm
```
经过``Form.creat``包装之后的组件将会自带 ``this.props.form``属性