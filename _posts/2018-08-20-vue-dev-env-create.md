---

title: "构建 vue 开发环境"
layout: post
category: note
tags: [vue, vue-cli, webpack]
excerpt: "使用脚手架工具vue-cli和打包工具webpack快速构建vue开发环境"

---

## 前提

完成安装`node.js`和`vue.js`

查看`node、npm、vue`版本, 进入控制台依次输入如下命令(__注意: __`-v`的大小写')

```
node -v
npm -v 
vue -V
```

控制台中如图中输出,则说明都已安装成功

![Alt text](/images/posts/201808/lALPDgQ9qcjVdajNAR7NAbk_441_286.png)

一般`node` 安装成功后, 不用单独去安装`npm`,  `node`的安装包中包括`npm`

> __注意：__ 安装 `VUE CLI (1.x 或 2.x) ` 与 `VUE CLI 3.x 及以上` 版本的行命令有所不同，[安装VUE CLI 4.1.2](http://peiwanjiang.com/note/2020/01/18/CentOS7系统-安装VUE_CLI_3.x/ "安装VUECLI 4.1.2")，安装了3.x及以上版本后，可以实现[界面化创建开发环境](http://peiwanjiang.com/note/2019/05/29/搭建VUE开发环境(@vue-cli-3.x)/ "界面化创建开发环境")

## 开始搭建环境

照着图逐步做就好了

### 1. 初始化

```
vue init webpack vue-ssr-axios-demo
```

![Alt text](/images/posts/201808/lALPDgQ9qae_wuPNAhjNA5w_924_536.png)

__注意__  `Project name` 不能含有***大写字母***

### 2. 初始化完成

根据提示, 依次输入以下命令即可

```
cd vue-ssr-axios-demo
npm run dev
```

![Alt text](/images/posts/201808/lALPDgQ9qafCl_DNAtXNA-g_1000_725.png)
