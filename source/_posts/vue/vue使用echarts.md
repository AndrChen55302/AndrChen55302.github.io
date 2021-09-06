---
title: vue简单使用echarts
date: 2021-09-02     
cover: https://cdn.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg.png #cover 文章图标
---
## 前言
商业级数据图表，它是一个纯JavaScript的图标库，兼容绝大部分的浏览器，底层依赖轻量级的canvas类库ZRender，提供直观，生动，可交互，可高度个性化定制的数据可视化图表。创新的拖拽重计算、数据视图、值域漫游等特性大大增强了用户体验，赋予了用户对数据进行挖掘、整合的能力。

## 通过npm引入echarts
```vue
npm install echarts //引入的是最新版本的echarts  暂为 5.2.0

通过@版本号 可以引入指定版本的echarts
npm install echarts@4.8.0 --save

至于为什么在这里提可以引入指定版本的 是因为
4.9版本以下有地图，5.0版本以上失去这个功能
```

## 使用echarts 三步走
### 1.现在我们想要使用的vue页面里引入
```vue
import * as echarts from 'echarts';//5.0以上的版本
import echarts from 'echarts'; //5.0以下的版本

按需引入
```

### 2.声明一个echarts父级容器
```vue
<div id="echartsFather" style="width: 500px;height: 500px;"></div>
```
### 3.在vue的挂载的生命周期里进行echarts的使用
```vue
2.1根据id找到DOM元素
  let doc=document.getElementById("firstEcharts");
2.2初始化DOM元素
  let myChart = echarts.init(doc);
2.3设置option
myChart.setOption({
  xAxis: {
    type: 'category',
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  yAxis: {
    type: 'value'
  },
  series: [{
    data: [820, 932, 901, 934, 1290, 1330, 1320],
    type: 'line',
    smooth: true
  }]
})
 ```



![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630600328000.png) 
更多内容请访问  echarts:  https://echarts.apache.org/zh/index.html
    
