---
title: vue3.0中setup
date: 2021-09-06  
cover:  https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630932759000.png
tags: setup函数
categories: vue3.0
---
## 前言

在我们vue2.0中 我们频繁的使用data,computed,methods,watch等等

来处理我们组件间的逻辑和数据当我们组件“越来越大”的时候，会导致组件难以阅读和理解，
这时通过setup将某一部分抽离成函数，让开发者省心。

## setup函数

> 1. 什么是setup===>Composition API（组合API）的入口
>
> 2. setup基础知识
>
>    > 1): setup所在位置是beforeCreate和created之前的
>    >
>    > 2): 因为在 1）带来的影响 我们如果想要使用setup的话
>    > 									data数据和methods方法都无法使用
>    >
>    > 3): 在setup单独声明一个变量是，如果setup的变量名
>    > 					 	和vue中的data里面变量名一样，setup的优先级更高	

### setup的优先级

setup>create>....其余生命周期

### ref

> ref创建响应式引用
>
> `ref`函数声明称为count的**响应式属性**。 它可以包装任何原始类型或对象，并**返回**它的响应式引用。 传递元素的值将保留在创建引用的值属性中。 例如，如果要访问count引用的值，则需要扩展请求`count.value`.

```vue
<template>
  <h3 @click="addCount()">点击+1:{{count}}</h3>
</template>

<script>
import {ref} from 'vue' //引入ref 声明数据
export default {
  name: "secondHome",
  setup() {//在beforeCreate之前
    const count = ref(0);//声明单个变量
    function addone() {
      count.value++ //先通过.value 拿到声明数据的变量 自加一
    }
    return {
      count,
      addone,
    }
  }
}
</script>
```

#### 声明单个变量

```vue
<template>
    <div>
		 <h2>setup声明的值:{{name}}</h2>
		 <h2>setup声明的值:{{user.username}}</h2>
		 <h2>setup声明的值:{{user.userage}}</h2>
		 <h2>setup声明的值:{{userList}}</h2>
    </div>
</template>

<script>
    import {ref} from 'vue' //引入ref 声明数据
    export default {
        name: "secondHome",
        //------------第一种使用ref声明数据---------------
        setup(){
            console.info('setup')
            /*通过ref 去使用，声明一个响应式变量*/
          const name = ref("张三");
			const user =ref({username:'李四',userage:12})
			const userList =ref([{username:'王五',userage:12},{username:'赵六',userage:12}])
			console.info("setup声明的数据==>",name.value);
			console.info("setup声明的user数据==>",user.value)
			console.info("setup声明的userList数据==>",userList.value)
            return{
                name,
                user,
                userlist
            }
        },
    }
</script>
```

#### 使用reactive声明多个变量

> toRefs()将响应式的对象变为普通对象 再解构，在模板中就可以直接使用属性，不用name,user,userlist

```vue
<template>
    <div>
        <h3>setup声明的值：{{name}}</h3>
        <h3>setup声明的值：{{user}}</h3>
        <h3>setup声明的值：{{userlist}}</h3>
    </div>
</template>

<script>
    import {reactive,toRefs} from 'vue' //引入ref 声明数据
    export default {
        name: "secondHome",

        //------------第二种使用 reactive 声明数据---------------
        setup(){
            console.info('setup')
			const data =reactive({
				name:'张三',
				msg:'goodbye',
				user:{username:'李四',userage:12},
				userList:[{username:'王五',userage:12},{username:'赵六',userage:12}]
			});
            return{
                //toRefs()将响应式的对象变为普通对象 再解构，在模板中就可以直接使用属性，不用name,user,userlist
                ...toRefs(data)
               
            }
        },
    }
</script>
```

### 2.0和3.0生命周期对照

#### 2.0

```vue
2.0生命周期	
beforeCreate(组件创建之前)	
created(组件创建完成)	
beforeMount(组件挂载之前)	
mounted(组件挂载完成)	
beforeUpdate(数据更新，虚拟DOM打补丁之前)	
updated(数据更新，虚拟DOM渲染完成)	
beforeDestroy(组件销毁之前)	
destroyed(组件销毁之后)	
```

#### 3.0

```vue
3.0生命周期
setup(组件创建之前)
setup(组件创建完成)
onBeforeMount(组件挂载之前)
onMounted(组件挂载完成)
onBeforeUpdate(数据更新，虚拟DOM打补丁之前)
onUpdated(数据更新，虚拟DOM渲染完成)
onBeforeUnmount(组件销毁之前)
onUnmounted(组件销毁之后)
```



