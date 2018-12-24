---

title: "利用GitHub Pages建立自己专属博客以及域名绑定的详细步骤"
category: memo
tags: [site]
excerpt: "鉴于我自身建站经历，在翻阅了各种教程后，终于将这个网站在github pages上搞出来了, 在此, 将详细的过程记录下来，一来防止自己以后忘记, 二来如果有幸可以帮到更多跟我一样的人, 我也是乐见其成的^_^"

---

### 1. 登录[GitHub](https://github.com/)官网

　如果已经有账号直接[登录](https://github.com/login)即可，没有的话需要[注册](https://github.com/join?source=experiment-header-dropdowns-home)

### 2. 创建仓库

![Alt text](/images/posts/201812/lALPDgQ9qaAbZwPNAojNBWY_1382_648.png)
点击右上角个人头像左侧的`+`, 在选项中选择`New repository`到https://github.com/new新建仓库
![Alt text](/images/posts/201812/lALPDgQ9qaBS-fDNAtbNBW4_1390_726.png)


**这个新仓库就将是存放你即将拥有的博客的地方**

__注意__: 仓库名(***Repository name***)不能随便取，取名的格式应该为“用户名.github.io”，而且"**用户名**"必须与"**Owner的名称**"保持一致. 比如: 我的`Owner`是`WakoJia-Pei`, 那么`Repository name`就必须是`WakoJia-Pei.github.io`

### 3. 进入GitHub线上仓库设置

建完仓库后，在当前页面上边选择Settings，进入设置页面，在这里生成github pages
![Alt text](/images/posts/201812/lALPDgQ9qaAnbWnNAnjNBWo_1386_632.png)

### 4. 自动生成博客

进入Setting后, 在最上边的位置Repository name默认即可
![Alt text](/images/posts/201812/lALPDgQ9qaAp2LnNAxDNBWU_1381_784.png)

将网页向下拖, 找到 **GitHub Pages** , 按图照做即可
![Alt text](/images/posts/201812/lALPDgQ9qaArin3NAtLNAvI_754_722.png)

+ `Source`: 选中 `master branch`后点击 `Save`

+ `Custom domain`中默认为空白, 即不与域名绑定, 也不会影响网站的访问; 填入一个已经注册的域名, 点击`Save`后, 则填入的域名与正在建立的这个博客网站已经绑定, 理论上现在在浏览器的地址栏中输入域名就可以访问网站了, 而不需要去访问仓库名了. 但是实际上到这一步我还不能通过访问绑定的域名来访问我的网站, 具体的原因和解决办法我在最后说明

  　如果你点击`Choose a theme`按钮，则选中一个主题后`Select theme`, 如果不想选择主题, 则直接进行__第5步"预览"__即可
  ![Alt text](/images/posts/201812/lALPDgQ9qaAsSHzNAz3NBWY_1382_829.png)  

### 5. 预览

在继续教程前，可以先预览一下页面，但实际你最后做出的效果会和这个比起来好几百倍，但你可以先确认一下能不能显示出页面来, 在浏览器地址栏输入仓库名, 点击回车即可, 比如在浏览器地址栏输入`WakoJia-Pei.github.io`
![Alt text](/images/posts/201812/lALPDgQ9qaAw3K1MzQV3_1399_76.png)

### 6. 将仓库克隆到本地

用git命令或者客户端都行, 达到目的就行, 我是以git为例

```
git clone https://github.com/WakoJia-Pei/WakoJia-Pei.github.io.git
```

### 7. 选择主题模板

这时候，你就该真正考虑一下你的博客主题风格了，如果你前端开发的功底不好，就不建议频繁更换主题了，虽然要改也不是不行，只是要折腾就是了

到这个网站选择你喜欢的模板http://jekyllthemes.org/
![Alt text](/images/posts/201812/lALPDgQ9qaA_WIDNAtnNBWI_1378_729.png)

选择自己喜欢的模板, 点击“**download**”，把它下载下来吧

### 8. 应用主题

打开存放克隆下来仓库的文件夹，把里面的文件`全部都删了（没错），除了隐藏文件夹“.git”不要删`就好了，然后把模板里的东西全部拖到你的原博客仓库里  
	　　 比如：存放克隆下来仓库的文件夹（/home/root/sit/WakoJia-Pei.github.io/ ）, 将下载的主题压缩包里的文件全部拖拽到存放仓库的文件里面
![Alt text](/images/posts/201812/lALPDgQ9qaBBpXXNAvPNBpQ_1684_755.png)

### 9. 配置博客基本信息

先简单介绍一下这个仓库里的内容，你根据自身需要用文本编辑器来修改内容
![Alt text](/images/posts/201812/lALPDgQ9qaBGAuXNAaXNBOU_1253_421.png)

+ index.html：这是博客的主页面，里面的内容就是博客主页了

+ _config.yml：这是博客的基本配置文件，里面有博客的名字，以及存放博主的一些基本信息

+ _layouts：这文件夹里面存放你每个页面的设计，一般有default.html（默认页面）和posts.html（博文页面）

+ _includes：这个文件夹里的的内容将会通用到你博客每个页面，起到一种便利的作用

+ _posts：这里面装的就是博文文件啦，__记住，要用markdown语法写，而且命名格式必须严格按照 "*2018-12-12-MySQL-默认字符集修改.md*" 的格式, __要不然上传会失败的

以上就是一个Jekyll规范的博客的基本内容了，想想也不是很难吧^_^

### 10. 上传到github线上仓库

现在已经把博客基本配置完成了，那么就该把它上传到github公布吧

进入到存放克隆下来仓库的文件夹（/home/root/sit/WakoJia-Pei.github.io/ ）, 执行三条命令

```
git add .
git commit -m '更换网站主题 '
git push
```

### 11. 后记

#### 如果不绑定域名的话,进行到这一步, 那么网站的搭建步骤就全部完成了,之后可以在_posts 文件夹里继续撰写博文，然后按照__第10步 上传到github线上仓库__即可

稍等一会儿，打开网站,按照__第5步操作__,就会发现博客文章已经神奇的出现了^_^


### 12. 绑定域名

在__`第4步 的Custom domain`__中提到已经绑定域名了, 还是不能通过访问域名的方式来访问网站, 这是什么原因呢? 原因很简单, 就是域名的DNS与GitHub pages并没有关联, 我要做的就是将域名的DNS映射到GitHub pages上, 那么首先到自己域名注册机构的官网上, 我是在[Godaddy](https://sg.godaddy.com/zh/)上申请注册的域名,进入到[域名管理器](https://dcc.godaddy.com/manage/)页面
![Alt text](/images/posts/201812/lALPDgQ9qaByXSrNA2rNBYw_1420_874.png)

选择`管理DNS`后, 进入
![Alt text](/images/posts/201812/lALPDgQ9qaB0NQvNA3TNBXw_1404_884.png)

在记录中主要修改类型为A的两行记录, 将值分别修改为`192.30.252.153` 和 `192.30.2525.154`

| 类型    |    名称 | 值 |
:--: | :---:| :---:
A | @ | 192.30.252.153
A | @ | 192.30.252.154

__原本默认的类型A的记录只有一条, 需要手动添加一条A记录, 另外`其他的选项最好不要改动`__

#### 进行到这一步, 基本上所有的步骤就全部完成了, 现在就可以通过域名访问网站了

---

> [参考原文](http://cyzus.github.io/2015/06/21/github-build-blog/):    http://cyzus.github.io/2015/06/21/github-build-blog/  
>`主要是GitHub相较于原文的版本, 页面排版不一样了, 我在参考原文的时候折腾了半天, 所以才将自己的操作步骤记录下来, 仅供参考`
