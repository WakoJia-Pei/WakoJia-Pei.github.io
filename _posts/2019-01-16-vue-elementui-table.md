---

title: "优雅的使用 element-ui 中的 table 组件"
layout: post
category: note
tags: [element-ui, table]
excerpt: "优雅的使用 element-ui 中的 table 组件"

---

> 参考原文[优雅的使用 element-ui 中的 table 组件](https://juejin.im/post/5a100d09f265da4324800807)

### 动态的创建表头以及填充表格内容

后台返回数据

```
data () {
  return {
    dataList: [
      {a: 1, b: 60.625, c: 1, d: 1, e: 100, f: 100}, 
      {a: 0, b: -38.375, c: 0.05, d: 2, e: 1, f: 1},
      {a: 0.0101, b: -37.375, c: 0.05, d: 3, e: 2, f: 2}
    ],
    dataHeader: []
  }
}
```

预览效果
![Alt text](/images/posts/201901/lALPDgQ9qdq3AcbM2c0DVQ_853_217.png)

其中表头部分并不是固定的,名称和列数都是后台返回, 所以表头需要动态的去获取

```
mounted () {
  // 提炼表头
  let arr = []
  for (let key in this.dataList) {
    for(let j in this.dataList[key]){
      arr.push(j)
    }
  }

  // 将重复的表头去掉
  this.dataHeader = this.$options.methods.unique(arr);
}
```

最后页面显示的代码也与官方文档中有所不同了

```
<el-table :data="dataList">
  <el-table-column v-for="(item, key) in dataHeader" :label="item" :key="key">
    <template slot-scope="scope">
      {{dataList[scope.$index][item]}}
    </template>
  </el-table-column>
</el-table>
```

完整的vue组件结构如下图

![Alt text](/images/posts/201901/lALPDgQ9qdrPkbbNA4LNA_Y_1014_898.png)

