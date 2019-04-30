---

title: "CentOS 7更新git"
layout: post
category: note
tags: [centos]
excerpt: "CentOS 7更新git"

---

1. 配置存储库
```shell
sudo vim /etc/yum.repos.d/wandisco-git.repo
```

添加以下字段，ESC，然后:输入wq保存退出。

```shell
[wandisco-git]
name=Wandisco GIT Repository
baseurl=http://opensource.wandisco.com/centos/7/git/$basearch/
enabled=1
gpgcheck=1
gpgkey=http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco
```

1. 使用以下命名了导入存储库GPG密钥
```shell
sudo rpm --import http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco
```

1. 安装
```shell
sudo yum install git
```
