---
title: 'vuex中store运用require.context改造为便捷的管理状态器'
date: 2020-12-11 11:21:45
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
## VueX（Vue状态管理模式)

### Vuex 的核心成员列表：
- state 存放状态
- mutations state成员操作
- getters 加工state成员给外界
- actions 异步操作
- modules 模块化状态管理

正常情况下，我们需要在state里面定义需要操作的对象，并赋值，再调取 getter、和 mutations的setter方法来操作数据，定义比较繁琐
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
	state: {
		addAppealForm: {
			enterpriseAppealNotice: false,
			showPicker: false,
			appealType: ''
		},
		signinForm: {
			enterpriseName: '',
			userName: '',
			loginPassword: '',
			contactName: ''
		}
	},
	getters: {
		getAddAppeal(state) {
			return state.addAppealForm
		},
		getSigninForm(state) {
			return state.signinForm
		}
	},
	mutations: {
		setAddAppeal(state, params) {
			state.addAppealForm = params
		},
		setSigninForm(state, params) {
			state.signinForm = params
		}
	}
})

export default store

```
实际业务中写如此多的构建语法会比较繁琐，将模块与vuex所需操作的数据进行剥离，做到类似localStorage的键值对方式存取会更加的方便，这里要用到webpack的api，require.context函数，获取一个特定的上下文，实现自动化导入模块，使得不需要每次显式的调用import导入模块
```javascript
// ./modules/index.js
const files = require.context('.', false, /\.js$/)
const modules = {}

files.keys().forEach((key) => {
	if (key === './index.js') return
	modules[key.replace(/(\.\/|\.js)/g, '')] = files(key).default
})

export default modules

```
```javascript
// index.js
import Vue from 'vue'
import Vuex from 'vuex'
import modules from './modules'

Vue.use(Vuex)

const store = new Vuex.Store({
	modules
})

export default store

```
从而，在实际的业务需求vuex定义的模块中，动态键值对的设计则是通过Object.keys() 调用``Vue.delete(state, key)``  和  ``Vue.set(state, key, data[key])`` 来实现
```javascript
// process.js

import Vue from 'vue'

export default {
	state: null,
	mutations: {
		updateProcessInfo(state, data) {
			if (data === null) {
				Object.keys(state).forEach((key) => {
					Vue.delete(state, key)
				})
			} else {
				Object.keys(data).forEach((key) => {
					Vue.set(state, key, data[key])
				})
			}
		}
	},
	actions: {}
}

```
页面调用：
```javascript
// 存入
this.$store.commit('updateProcessInfo', {processDetail: data})

// 读取
this.$store.state.process.processDetail
```
![](https://www.tuziki.com/post-images/1607692443097.png)


