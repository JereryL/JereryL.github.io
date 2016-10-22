---
title: 基于windows10搭建github+hexo博客
date: 2016-10-22 13:57:39
tags: 学习笔记
---
材料
=============
- windows 10
- Git
- node.js
- github 账号
- hexo

安装Git
==============
进入Git的官方网站下载，我选择setup版本，因为Portable版本比较大，而且听说setup版本跟Portable版本区别不大。  
https://git-scm.com/download/win  
安装的时候有很多选项，我们使用默认配置，一直选择下一步就行了。

安装node.js
==============
进入nodejs的官网下载，安装的时候一直选择下一步默认安装就可以了。  
https://nodejs.org/en/

安装hexo
==============
打开Git Bash，输入如下命令安装hexo

    $ npm install -g hexo-cli
    
进入你想保存博客文件的目录，然后输入以下命令初始化你的博客

    $ hexo init hexo
    
进入到hexo文件夹，安装一个依赖(例如我的hexo安装在d盘下)

    $ cd d:/hexo
    $ npm install hexo-deployer-git --save
    
至此，hexo博客已经生成完毕，接下来就要测试一下，在Git Bash进入hexo目录，然后输入
   
    $ hexo g
    $ hexo s
    
在浏览器输入localhost:4000，可以看见搭建的hexo博客了。

生成SSH key
===============
在Git Bash中输入

    $ ssh-keygen -t rsa -C "你在github上注册的邮箱"

例如，我的账号为"ljhjerery@sina.com",所以我就输入

    $ ssh-keygen -t rsa -C "ljhjerery@sina.com"

按回车后，它会显示如下提示

    Generating public/private rsa key pair.
    Enter file in which to save the key (/c/Users/Vincent/.ssh/id_rsa):
    
按回车就行了，生成的SSH key会保存到括号里面的文件中，例如我的SSH key会生成在/c/Users/Vincent/.ssh/id_rsa文件中。然后它会提示你设置密码，如果不设置就空着。

    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:

到此它会显示提示

    Your identification has been saved in /c/Users/Vincent/.ssh/id_rsa.
    Your public key has been saved in /c/Users/Vincent/.ssh/id_rsa.pub.
    The key fingerprint is:

至此，SSH key已经生成成功。

建立github仓库
=================
进入到注册好的github账号，点击"new repository"。在 "Creat a new repository"页面，设置"Repository name",这个名称比较关键，必须为"你的用户名.github.io",例如我的用户名为"JereryL",所以我就设置为"JereryL.github.io"。修改完毕后，其他设置默认就可以了，然后点击"Creat repository"，创建完毕。

在github添加SSH key
====================
在github页面的右上角有两个下拉菜单，点击右边的那个下拉菜单，选择"Settings"。在“Personal Settings”这一栏中选择“SSH and GPG keys”。在"SSH keys"一栏中点击"New SSH key"，"title"可以随便填。然后将id_rsa.pub里面的内容复制到"Key"里（id_rsa.pub在id_rsa同一个目录里）。最后，点击"Add SSH key"，完成添加。

将hexo部署上github
进入hexo博客所在文件夹，打开_config.yml文件，修改deploy字段如下

    deploy:
    type: git
    repository: https://github.com/JereryL/JereryL.github.io
    branch: master

repository要填写你的仓库地址。
