---
title: 在多台电脑管理一个github-hexo博客
date: 2016-10-23 10:07:41
tags: 学习笔记
---
前言
=============
一般情况下，我们部署好的博客只能在一台电脑上管理，因为要管理博客需要在博客的文件夹下操作。所以我们要找到一个方法共享这个文件夹。而github本身就是一个代码托管仓库，何不就直接将代码托管到github上。

准备
=============
- 两台电脑
- 两台电脑的SSH key都添加上github
- 两台电脑都已经装好Git,node.js,hexo

操作步骤
============

初始化仓库
------------
在终端或者Git Bash（以下统称终端），进入到博客目录下。输入命令
    
    $ git init

这个命令是初始化一个git仓库，这样，这个博客文件夹就变成了一个仓库了。

上传代码
------------
在终端输入

    $ git add .
    $ git commit -m "first commit"
    $ git remote add https://github.com/JereryL/JereryL.github.io.git
    $ git push -u origin master

第三条命令要写自己博客的仓库地址。这样，文件夹中的内容就上传到了github仓库了。进入到github仓库中查看，发现一个问题，这里面的文件原本是public文件夹里面的文件，但现在变成了博客根目录下的所有文件了。而且，打开博客网址，发现出错了。于是，我新建一个分支，用来保存博客根文件夹下的文件，master分支下就用来保存生成的网站文件。在终端输入命令如下

    $ git branch hexo
    $ git checkout hexo
    $ git push origin hexo

这样，就创建了hexo分支并提交上github上了。我们最好把hexo分支设置为默认分支,在github上进入到你的仓库，在上面一栏找到"Settings"，点击进入，在"Option"一栏选择"Branches",修改"Default branch"为hexo,点击"Update"保存修改。这是回到终端，重新提交代码到hexo分支

    $ git add .
    $ git commit -m "branch hexo"
    $ git push origin hexo

这时在github仓库上可以看见提交的代码了。在终端重新生成博客

    $ hexo g
    $ hexo d

进入github仓库的master仓库，可以看见这个分支的代码已经正确生成。

克隆仓库
--------------
到另一台电脑，在终端进入到你想保存仓库的地址，然后输入

    $ git clone https://github.com/JereryL/JereryL.github.io.git

这时，生成了一个目录JereryL.github.io,在这个目录先安装一个包

    $ npm install hexo-deployer-git --save

我们尝试在这里生成一下博客

    $ hexo g
    $ hexo d

在github仓库上发现并不能正确生成网站文件,解决方法是将JereryL.github.io文件夹下的.deploy_git删除，重新生成一下，问题解决。

至此，整个过程完毕。平时修改完博客，只要提交一下文件就行了

    git add .
    git commit -m "备注"
    git push origin hexo

参考自：  
《使用hexo,如果换了电脑怎么更新博客》  
https://www.zhihu.com/question/21193762
