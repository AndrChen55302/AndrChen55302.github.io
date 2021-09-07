
---
title: computed计算属性
date: 2021-08-31
cover: https://i.loli.net/2020/05/01/gkihqEjXxJ5UZ1C.jpg
tags: computed
categories: vue2.0
---

## 前言
vue template模板中不推荐使用过多的逻辑，这样会导致整个模板过重且难以维护
所以 复杂的逻辑 应该使用 computed计算属性

## 特点
1. 计算值
2. 简化template里面 data属性计算和处理props或$emit的传值，computed(数据联动)。
3. 具有缓存性，页面重新渲染值不变化,计算属性会立即返回之前的计算结果，而不必再次执行函数,这样就避免了一个值多次进行计算，影响代码的执行效率。

## 一个简单的小案例使用一下
![](../../img/vue/computed计算属性/two.png)
![](../../img/vue/computed计算属性/one.png)
### html元素
```vue
	<div>
		<div v-for="(item,index) in result" :key="index">
			科目:{{item.lable}}
			<label>
				分数: <input type="text" v-model="item.marks" />
			</label>
		</div>
		总分数:{{marksNum}}
	</div>
```
### data数据
```vue
result:[
	{lable:'数学',marks:90},
	{lable:'语文',marks:92},
	{lable:'语文',marks:72},
],
```
### computed
```vue
computed:{//计算属性
	marksNum:function(){
		let zongShu = 0 ;//局部声明一个变量  总数
		for(let i=0;i<this.result.length;i++){
			zongShu+=parseInt(this.result[i].marks);
		}
		return zongShu;
	}
}
```
parseInt():将字符串解析为数字
### 为什么要使用parseInt函数
当改变input输入框的值时 因为type='text'的原因，你输入的值就变成了string类型


