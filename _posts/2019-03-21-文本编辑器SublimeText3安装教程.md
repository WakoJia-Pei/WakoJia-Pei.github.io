---

title: "CentOS 7系统文本编辑器sublime-text-3安装教程"
layout: post
category: note
tags: [centos]
excerpt: "CentOS 7系统文本编辑器sublime-text-3安装教程"

---

1. 官网下载最新的Linux版安装包

      [http://www.sublimetext.com/3](http://www.sublimetext.com/3)

2. 解压文件包并且移动到指定目录下面

```shell
tar xjf sublime*.tar.bz2 # 解压下载的最新的文件包

sudo mv /home/xtde/Downloads/sublime_text_3 /opt/ # 将解压的文件夹移动到opt目录下面
```
 
3. 配置环境变量

```shell
sudo vim /etc/profile
```

4. 添加内容

```shell
export PATH=/opt/sublime_text_3:$PATH
```

5. 保存退出后，需要执行下面命令使配置文件生效

```shell
source /etc/profile
```

6. 创建快捷方式

```shell
sudo vim /usr/share/applications/sublime_text.desktop
```

7. 编辑内容

```shell
[Desktop Entry]
Version=1.0
Type=Application
Name=Sublime Text
GenericName=Text Editor
Comment=Sophisticated text editor for code, markup and prose
Exec=/opt/sublime_text_3/sublime_text %F
Terminal=false
MimeType=text/plain;
Icon=/opt/sublime_text_3/Icon/48x48/sublime-text.png
Categories=TextEditor;Development;
StartupNotify=true
Actions=Window;Document;

[Desktop Action Window]
Name=New Window
Exec=/opt/sublime_text_3/sublime_text -n
OnlyShowIn=Unity;

[Desktop Action Document]
Name=New File
Exec=/opt/sublime_text_3/sublime_text --command new_file
OnlyShowIn=Unity;

```

8. 保存退出即可

9. 安装package control遇到问题参考地址：[https://www.jianshu.com/p/02665121caf9](https://www.jianshu.com/p/02665121caf9)
