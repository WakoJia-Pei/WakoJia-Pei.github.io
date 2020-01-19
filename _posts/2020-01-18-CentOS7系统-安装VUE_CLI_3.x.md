---

title: "CentOS 7 系统：安装VUE CLI 3.x"
layout: post
category: note
tags: [CentOS, vue-cli]
excerpt: "在CentOS系统上安装vue cli 3.x及以上版本时，会因为各种问题导致vue命令不能正确执行"

---

### npm 安装

```bash
$ npm install -g @vue/cli
```

 等待安装完成，成功后使用 `vue -V` 或 `vue --version`查看版本号

1. 如果输出 vue 的版本号，则说明安装成功

  ![lALPDgQ9reo6Pi7M0c0DMA_816_209.png](https://cdn.nlark.com/yuque/0/2020/png/295105/1579309255541-285edb85-effd-4a56-86a4-d5c5330ff5d3.png#align=left&display=inline&height=209&name=lALPDgQ9reo6Pi7M0c0DMA_816_209.png&originHeight=209&originWidth=816&size=27430&status=done&style=none&width=816)

2. 报错 bash: vue: command not found ，原因：

-  npm的指定的node_modules全局路径与安装@vue/cli指定的node_modules路径不一致

   
```bash
# 查询npm的安装路径
$ npm root -g
# 路径不正确时，根据自己的情况重新指定路径
$ npm config set prefix /usr/local/bin
```

- 权限问题，执行 `sudo npm install -g @vue/cli`




- 如果还有问题[请参考](https://www.cnblogs.com/Amos-Turing/p/9203257.html)

