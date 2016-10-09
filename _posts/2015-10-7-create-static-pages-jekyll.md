---
layout: post
title:  "使用Jekyll创建静态网页，并托管在Github.io"
date:   2015-10-07 18:51:59
categories: jekyll update
---

良心的github给每个用户提供了一个repository (username.github.io)来托管静态网页，名为Github Pages，用于管理project，写blog等。同时git pages与很火的静态网站生成工具jekyll深度集成，使得创建多页面网站变得很容易。比较出名的使用Github Pages+jekyll的项目可以参考[jekyll examples in github](https://github.com/showcases/github-pages-examples),这里列出几个:

[Microsoft](http://microsoft.github.io)

[IBM](http://ibm.github.io)

[Electron](http://electron.atom.io)

本文从零开始详细介绍如何用jekyll生成一个满意的程序员blog页面，并托管在Github Pages上(RE0：从零开始的blog创建:))

注：本文使用windows系统(Win7-64bit)，使用linux的同学仅供参考。

## 注册github并配置git bash

### 注册github
不多说，假设用户名是madoka，注册邮箱是magicmadoka@gmail.com好了。

### git bash
git bash是windows下可以使用git commands的一种terminal。在[这里](https://git-scm.com/downloads)下载并安装。装好之后打开git bash，默认的home(~)目录在c/User/current_user。你可以使用`cd ~`直接到这一目录。git bash和linux bash的操作比较像，这里就不展开了。需要注意的是如果是第一次使用，需要配置一下账户及ssh key。输入：
{% highlight git %}
git config --global user.name "madoka"
git config --global user.email "magicmadoka@gmail.com"
{% endhighlight %}
让git bash知道你是谁。接下来配置ssh key，用于将你的本地repo push到远程服务器。ssh key的检查，创建以及配置可以参考[这里](https://help.github.com/articles/checking-for-existing-ssh-keys/)。好，到现在为止，你的git bash已经配置好可以使用了。我们先来建一个文件夹GitPages供以后使用，在git bash中输入
{% highlight git %}
cd ~
mkdir GitPages
{% endhighlight %}

## 安装并配置jekyll

### 安装
安装参考[这里](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)的Requirements部分。记得先安装ruby，再装bundle。Ruby是一个编程语言，我还没研究过。但是安装ruby会安装好网页编程三件套Apache+PHP+MySql，可以用来调试网站。并且会安装工具gem，用来下载安装其他工具，感觉跟python里的pip差不多。装好之后记得将ruby的bin目录放到系统变量Path中，否则git bash找不到。比如我的目录是`C:\Bitnami\rubystack-2.2.5-5\ruby\bin`。

### 创建本地repository
这里要创建一个你博客网站(以后称为site)的本地存放目录，假设该目录叫MyBlogs。在git bash中输入
{% highlight git %}
cd ~\GitPages
mkdir MyBlogs
cd MyBlogs
git init
{% endhighlight %}
创建并修改Gemfile文件
{% highlight git %}
touch Gemfile
{% endhighlight %}
找到并打开Gemfile，copy以下代码
{% highlight git %}
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
{% endhighlight %}
安装dependencies。在git bash中输入
{% highlight git %}
bundle install
bundle update
{% endhighlight %}
这里注意bundle是在上一步装好了的，别忘了。这里的bundle update以后可以经常用一用，用于和Github Pages的server同步使用的软件。

### 创建site文件
在git bash中输入：
{% highlight git %}
bundle exec jekyll new . --force
{% endhighlight %}
可以看到MyBlogs文件夹中多了很多文件和文件夹，这些都有自己的作用。比如_post文件夹存放的是你写的博客文件，这里的文件可以使用Markdown来写。_site文件夹(暂时看不到)存放的是网站的jekyll编译结果，你可以想象成是你写的源文件编译成了网站的可执行文件，最后在浏览器上显示的页面都存在_site文件夹。

## 网站调试与设计

### 网站本地测试
在git bash中输入：
{% highlight git %}
bundle exec jekyll serve
{% endhighlight %}
来编译网站，如果没有问题，应该显示类似于以下的信息：
{% highlight git %}
$ bundle exec jekyll serve
Configuration file: C:/Users/Nan-admin/Git/Git-pages/GitPages/_config.yml
            Source: C:/Users/Nan-admin/Git/Git-pages/GitPages
       Destination: C:/Users/Nan-admin/Git/Git-pages/GitPages/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.616 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'C:/Users/Nan-admin/Git/Git-pages/GitPages'
Configuration file: C:/Users/Nan-admin/Git/Git-pages/GitPages/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
{% endhighlight %}
Server address那里对应的就是本地网站的地址，在浏览器中输入http://127.0.0.1:4000/ 就可以看到网站啦~

### 使用Jekyll模板
这时候的网站是很丑的，自己做一个漂亮的模板又烦，这时候就果断使用别人的模板吧~ 网上有很多好看又免费的模板可以使用，可以在
这些网站找到[\[JekyllThemes\]](http://themes.jekyllrc.org/)   
[\[jekyllthemes.io\]](https://jekyllthemes.io/)  
[\[Jekyllthemes.org\]](http://jekyllthemes.org/)  
本文使用JekyllThemes里的[Clean](http://themes.jekyllrc.org/clean/)模板，[Github地址](https://github.com/knaman2609/clean)。

fork clone模板，在git bash中输入：
{% highlight git %}
cd ~\GitPages
git clone https://github.com/knaman2609/clean.git
{% endhighlight %}
会将clean模板下载到`~\GitPages\clean`中。到clean文件夹中copy所有文件，paste到`~\GitPages\MyBlogs`中，该合并的合并，该替换的替换。好啦，现在再用`bundle exec jekyll serve`看看，我们的blog是不是好看多了呢？

这个时候可以在_post文件夹下建更多的文件，每个文件都是一个独立的blog post。可以参考模板自带的2015-09-26-clean-theme.md来写markdown代码。

可以修改_sass文件夹，_layout文件夹和_includes文件夹下的内容来让网站的外观更符合你的要求。比如如果你觉得Blog显示的宽度太窄，可以修改_layout.scss下面post段下面所有的`max-width`属性，比如从540px改成840px。别忘了修改_base.scss下blockquote中的`max-width`属性，它影响的是代码highlight的显示宽度。

注意服务器在运行的时候你是可以任意修改文件的，修改结果会即时编译到site中，在浏览器中刷新就可以看到修改结果。

## 托管到Github Pages
当我们做好了网站，需要上传到互联网上，这里使用Github Pages给我们提供的服务器来做这件事。登录你的github website，建立一个repository叫做madoka.github.io。这里注意必须是username.github.io的形式，并且不要用readme来初始化。这个madoka.github.io就是我们存放网站的远程地址啦。那么现在需要做的就是将做好的网站push过来。在git bash中输入：
{% highlight git %}
cd ~\GitPages\MyBlogs
git add .
git commit -m "first blogs"
git remote add origin https://github.com/madoka/madoka.github.io.git
git push -u origin master
{% endhighlight %}
你的blog就上传到网上啦~在浏览器输入madoka.github.io，是不是就看到做好的blog了？注意以后每次修改本地网站文件都要执行上面的git命令来更新远程server，但`git remote add origin https://github.com/madoka/madoka.github.io.git`这一步就不需要了。至于这些命令的含义，可以看看这个视频[GitHub Pages and Jekyll Beginner Video](https://www.youtube.com/watch?v=nN6QuNqmAwk)

## 一些资源
[git commands guides](https://git-scm.com/docs)：详细的git commands reference document   
[Introduction to Blogging with Jekyll](https://www.youtube.com/watch?v=c5WkxIzK0eA)：Jekyll的presentation   
[Getting Started With Jekyll, The Static Site Generator](https://www.youtube.com/watch?v=iWowJBRMtpc)：比较详细的Jekyll系列教程   
[Jekyll document](https://jekyllrb.com/docs/home/)：官方的Jekyll文档   
[CSS document](http://www.w3schools.com/css/default.asp)：CSS文档
