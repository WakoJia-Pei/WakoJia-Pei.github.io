---
title: "Vue 渲染echarts图表宽度只能显示100px"
layout: post
category: note
tags: [vue, echarts]
excerpt: "刚刚接触vue不久,在开发过程中遇到不少坑,走了很多弯路,其中一个就是在echarts图表加载渲染时,图表的宽度只能显示100px问题,这篇文章记录解决方案,对于像我一样的初学者来说,可能会有一些参考意义,也是对自己这段学习的总结,以后处理类似问题完全可以成竹在胸了"
---

刚刚接触vue不久,在开发过程中遇到不少坑,走了很多弯路,其中一个就是在echarts图表加载渲染时,图表的宽度只能显示100px问题,这篇文章记录解决方案,对于像我一样的初学者来说,可能会有一些参考意义,也是对自己这段学习的总结,以后处理类似问题完全可以成竹在胸了.

出现这个问题的情况目前来说我只在

```
<el-tabs type="border-card" v-model="tabsName" @tab-click="handleClick">
  <el-tab-pane label="商户">
    <el-select v-model="subSelect" placeholder="商户" @change="handleChangeSelect">
      <el-option
        v-for="item in subSelectValue"
        :key="item"
        :label="item"
        :value="item">
      </el-option>
    </el-select>
    <div id="lineChartEx1" class="chart" v-loading="loading2"></div>
  </el-tab-pane>
  <el-tab-pane label="性别">
    <el-select v-model="subSelect" placeholder="性别" @change="handleChangeSelect">
      <el-option
        v-for="item in subSelectValue"
        :key="item"
        :label="item"
        :value="item">
      </el-option>
    </el-select>
    <div id="lineChartEx2" class="chart" v-loading="loading2"></div>
  </el-tab-pane>
<el-tabs>
```
这一种情况下遇到,在调试时,发现是图表在页面加载时生成的width就是100px,上网查找说是没有resize,我照做之后还是没有解决,

![err-show](/images/posts/201811/vue_echarts_width_100px_show.png)

几经周折之后,总算求到了解决之道

![code](/images/posts/201811/vue_echarts_width_100px.jpg)

__注意__: 插图中的```window.innerWidth / 2 - 20 + 'px'```并不是固定的,完全根据自己实际页面排版进行调整即可.


