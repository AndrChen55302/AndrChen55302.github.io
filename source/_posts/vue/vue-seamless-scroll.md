---
title: vue使用vue-seamless-scroll实现无缝滚动
date: 2021-09-01
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630741100000.jpg
tags: vue-seamless-scroll
categories: vue2.0
---
## 前言
通过使用 vue-seamless-scroll这个插件可以快速的实现无缝滚动

### 安装
```vue
npm install vue-seamless-scroll --save　
```

### 引入
#### 1.在main.js里全局挂载
```vue 
import vueSeamlessScroll from 'vue-seamless-scroll'
Vue.use(vueSeamlessScroll)
```
#### 2.在要使用该插件的.vue文件中引入
```vue 
import vueSeamlessScroll from 'vue-seamless-scroll'

 components: {//引入插件都需要在这里声明
      vueSeamlessScroll,
    },
```
### 配置参数
```vue
computed: {
      classOption: () => ({
        step: 2.5, // 数值越大速度滚动越快
        step: 0.2, // 数值越大速度滚动越快
        limitMoveNum: 2, // 开始无缝滚动的数据量 this.dataList.length
        hoverStop: true, // 是否开启鼠标悬停stop
        direction: 1, // 0向下 1向上 2向左 3向右
        openWatch: true, // 开启数据实时监控刷新dom
        singleHeight: 0, // 单步运动停止的高度(默认值0是无缝不停止的滚动) direction => 0/1
        singleWidth: 0, // 单步运动停止的宽度(默认值0是无缝不停止的滚动) direction => 2/3
        waitTime: 1000 // 单步运动停止的时间(默认值1000ms)
      }),
    },

```

### 具体实现
```vue
<template>
  <div>
    <vue-seamless-scroll :data="hotList" :class-option="classOption" class="seamless-warp">
      <ul class="scroll-item">
        <li v-for="item in hotList">
          <a >{{item}}</a>
        </li>
      </ul>
    </vue-seamless-scroll>
  </div>
</template>

<script>
  import vueSeamlessScroll from 'vue-seamless-scroll'
  export default {
    data() {
      return {
        hotList: [
          '中国成功发射天绘一号04星 系长征运载火箭第381次飞行',
          '退役军人事务部：正起草退役军人安置条例 争取尽快出台',
          '新华社达累斯萨拉姆7月28日电（记者高竹 李斯博）坦桑尼亚总统哈桑28日在位于坦最大城市达累斯萨拉姆的总统府宣布，启动该国新冠疫苗接种计划。她本人当场接种新冠疫苗。',
          '美联储重申将维持超低利率 应对通货膨胀和肺炎疫情反弹',
          '他曝光美军无人机杀害阿富汗平民 却因泄密获刑45个月',
          '鹤壁淇县灾后重建：村民返家抢救麦子 村里消杀防疫',
          '生态环境部公布打击危险废物环境违法犯罪典型案例',
          '一头须鲸在浙江瑞安滩涂搁浅后续：救援12小时后死亡',
          '台湾新增11例确诊病例 其中10例为本地病例',
          '除了戴口罩 美CDC对接种疫苗者是否需检测也有新说法',
          '台湾本地疫苗开放施打意愿登记 民众疑虑多反应冷淡',
          '迪迩秀：我是怎么得到这管“泄漏病毒”的？多国外长拒绝新冠溯源政治化',
          '第14金+打破世界纪录！中国4朵金花夺游泳接力冠军',
          '全国至少11地机场、港口、车站爆发疫情 张伯礼提4点建议',
          '网红为拍视频穿救生衣偷救生艇 被发现后被救援队怒怼'
        ],
      }
    },
    components: {
      vueSeamlessScroll,
    },
    computed: {
      classOption: () => ({
        step: 2.5, // 数值越大速度滚动越快
        step: 0.2, // 数值越大速度滚动越快
        limitMoveNum: 2, // 开始无缝滚动的数据量 this.dataList.length
        hoverStop: true, // 是否开启鼠标悬停stop
        direction: 1, // 0向下 1向上 2向左 3向右
        openWatch: true, // 开启数据实时监控刷新dom
        singleHeight: 0, // 单步运动停止的高度(默认值0是无缝不停止的滚动) direction => 0/1
        singleWidth: 0, // 单步运动停止的宽度(默认值0是无缝不停止的滚动) direction => 2/3
        waitTime: 1000 // 单步运动停止的时间(默认值1000ms)
      }),
    },

  }
</script>
```

