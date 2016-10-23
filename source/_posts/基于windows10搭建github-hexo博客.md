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
====================
进入hexo博客所在文件夹，打开_config.yml文件，修改deploy字段如下

    deploy:
    type: git
    repository: https://github.com/JereryL/JereryL.github.io.git
    branch: master

repository要填写你的仓库地址。修改完后，保存退出，回到终端。输入命令如下

    $ hexo g
    $ hexo d

如果是第一次使用，将要求设置用户名和密码，根据提示设置便可以了。至此，hexo博客已经部署到github上了，可以在浏览器输入你的博客地址便可浏览。博客的地址为一开始设置的仓库名字，例如我的是  
JereryL.github.io

更换hexo主题
========================
因为我正在使用的是next主题，所以这里以next主题为例。首先找到next主题的仓库，然后克隆到本机hexo目录下的themes目录，代码如下

    $ cd your-hexo-site
    $ git clone https://github.com/iissnan/hexo-theme-next themes/next

下载完成后，在themes/next下会生成一个_config.yml配置文件，这里称其为主题配置文件，而在hexo目录下的那个_config.yml文件则称为站点配置文件。

启用主题
-------------------
修改站点配置文件：

    theme: next

验证主题，可以在终端输入:

    $ hexo g
    $ hexo s

然后在localhost:4000上查看是否成功修改。

选择主题风格
------------------
主题配置文件里面的Scheme字段提供了选择next主题下的3种风格，要切换的话将前面的#号去掉即可
     
    #scheme: Muse
    scheme: Mist
    #scheme: Pisces

设置语言
----------------
修改站点配置语言，将language设置成zh-Hans即可切换为简体中文

    language: zh-Hans

设置菜单
-----------------------
    修改主题配置文件，修改如下内容，在启用的菜单项去掉#号，在删除的菜单项加上#号即可

    menu:
      home: /
      archives: /archives
      #about: /about
      #categories: /categories
      tags: /tags
      #commonweal: /404.html

设置头像
------------------------------
修改站点配置文件，新增字段 avatar，设置头像的链接地址。例如我的头像放在source/images目录下，则我修改为

    avatar: source/images/avatar.jpg

设置作者昵称
------------------------------
修改站点配置未见的author。

设置站点描述
------------------------
修改站点配置文件，设置description字段即可。

至此，整个站点基本搭建完毕，除此之外，还能加入多说评论系统，但我还没有进行尝试，所以先不做此修改了。最后一点值得注意的是，很多人为了美观，布局，都会删除hello world这篇文章。但不要急着删除，因为在没有其他文章的情况下删除这篇文章，在进行hexo clean后会报错。所以在你写好自己的第一篇文章后才删除这篇文章吧。
