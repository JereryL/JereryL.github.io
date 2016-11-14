---
title: ubuntu16.04搭建github+hexo博客
date: 2016-10-18 21:03:33
tags: 学习笔记
---
材料
===================
- ubuntu16.04
- Github,Git，hexo，node.js，ssh key等

生成SSH keys
===================
打开终端,输入  
    $ ssh-keygen -t rsa -C "Your_email@example.com"

回车后会提示：

    Generating public/private rsa key pair.  

继续回车，提示：

    Enter file in which to save the key (~/.ssh/id_rsa):  

这里将SSH keys保存在默认文件夹，直接回车就行了。然后按提示输入你要设置的密码并再次输入确认：

    Enter passphrase (empty 'for' no passphrase):
    Enter same passphrase again:

接下来就会提示成功：

    Your identification has been saved in ~/.ssh/id_rsa.
    Your public key has been saved in ~/.ssh/id_rsa.pub.
    The key fingerprint is:
    ... Your_email@example.com

在 .ssh 目录下创建一个 config 文件:

    $ cd ~/.ssh
    $ gedit config

在文件里面输入如下配置信息：

    Host Yourname.github.com         #这里修改成你自己的 host
    HostName github.com
    PreferredAuthentications publickey   
    IdentityFile ~/.ssh/id_rsa    # 这里填入你前面新建的密钥的路径
    User git

这里输入完成，保存，关闭。回到终端。接下来要查看新建的SSH key是否在运行

    $ ssh-add ~/.ssh/id_rsa
    $ ssh-add -l

如果显示了刚刚创建的SSH key，则表明已经在运行了。我们需要把这个SSH key添加到github，但这里要使用的是id_rsa.public。用gedit打开id_rsa.public，然后复制里面的内容。打开你的github网页，右上角有个下拉菜单，点击，然后选择setting，在setting页面左边一栏选择SSH and GPG keys。接下来在SSH keys那里选择New SSH key。进入页面后，title随便填吧。key就填刚刚复制的一段内容。之后点击Add SSH key完成添加。

回到终端，输入

    $ ssh -T git@github.com
    Hi Youname! You’ve successfully authenticated, but GitHub does not provide shell access.

这就代表已经配对成功了。

安装Node.js
=====================

在终端输入nodejs或者npm，如果没有安装会提示你进行安装，命令如下：

    sudo apt-get update
    sudo apt-get install nodejs
    ln -s /usr/bin/nodejs /usr/bin/node
    sudo apt-get install npm

这里添加了一个用node指向nodejs的软链接，是因为用包管理器安装的话，二进制表文件被叫做 nodejs，但 hexo 用的是 node

安装Hexo
=====================
在终端输入下面的命令安装

    $ npm install -g hexo-cli

安装完后进行初始化，进入你要保存博客的目录

    $ hexo init hexo    #初始化，创建一个你专门存放博客文件的文件夹，我这里把文件夹命名为 hexo，你可以改成你想要的名字
    $ cd hexo              #进入 hexo 目录
    $ npm install          #安装相关依赖
    $ npm install hexo-deployer-git --save

测试博客是否能运行

    $ hexo g
    $ hexo s

终端会显示

    INFO  Start processing
    INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.

在浏览器输入 localhost:40000,如果成功的话便会有页面显示出来，否则会跳到404页面。

将hexo部署到github
=================================
用gedit打开hexo目录下的_config.yml文件

    $ gedit _config.yml

修改deploy字段：
    
    deploy:
      type: git
      repository: https://github.com/JereryL/JereryL.github.io.git
      branch: master

注意：格式必须按照上面所示，否则会报错。保存退出，在终端输入：

    $ hexo g
    $ hexo d

按照提示输入帐号密码，完成后在浏览器输入自己的github博客地址，看是否正常成功。至此，将hexo部署在github的过程已经完成。

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

    hexo g
    hexo s

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

参考自：  
关于在 Ubuntu 上部署 Hexo 到 Github：  
http://www.leyar.me/create-a-blog-with-hexo-in-ubuntu/

史上最详细“截图”搭建Hexo博客并部署到Github  
http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html  

开始使用NexT:  
http://theme-next.iissnan.com/getting-started.html

Markdown 语法说明 (简体中文版):  
http://www.appinn.com/markdown/#code

>>>>>>> 9dbb1880e1f35012c5488d0a41af93ddc39efe69
