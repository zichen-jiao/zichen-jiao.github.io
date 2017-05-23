---
layout: post
title: 使用Hexo + Github Pages搭建个人独立博客
catalog: true
date: 2017-04-28 17:15:59
subtitle: 搭建个人独立博客
header-img: "img/in-post/使用Hexo-Github-Pages搭建个人独立博客/demo.png"
tags:
- 学习
- 博客
---

# 搭建博客
@(帮助文档)[帮助, 杂记]
参考文章：[手把手教你使用Hexo + Github Pages搭建个人独立博客](https://segmentfault.com/a/1190000004947261)进行搭建,并进行记录。

# 系统环境配置
要使用Hexo，需要在你的系统中支持Nodejs以及Git，如果还没有，那就开始安装吧！

## 安装Node.js 
- [下载地址][下载Node.js]
## 安装Git 
- [下载地址][下载Git]
## 安装Hexo

```
$ cd d:/hexo
// 该句是为了获取最高权限,防止出现
// Error：Please try running this command again as root/Administrator.
$ sudo chown -R $USER /usr/local 
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo g # 或者hexo generate
$ hexo s # 或者hexo server，可以在http://localhost:4000/ 查看
```

这里有必要提下Hexo常用的几个命令：
1. `hexo generate (hexo g)` 生成静态文件，会在当前目录下生成一个新的叫做public的文件夹
2. `hexo server (hexo s)` 启动本地web服务，用于博客的预览
3. `hexo deploy (hexo d)` 部署播客到远端（比如github, heroku等平台）
另外还有其他几个常用命令：

```
$ hexo new "postName" #新建文章
$ hexo new page "pageName" #新建页面
```
常用简写

```
$ hexo n == hexo new
$ hexo g == hexo generate
$ hexo s == hexo server
$ hexo d == hexo deploy
```
常用组合

```
$ hexo d -g #生成部署
$ hexo s -g #生成预览
```
现在我们打开http://localhost:4000/已经可以看到一篇内置的blog了。
![Alt text](/img/in-post/post_常用adb/1493280020740.png)

目前我安装所用的本地环境如下：(可以通过`hexo -v`查看)

```
hexo: 3.3.1
hexo-cli: 1.0.2
os: Darwin 16.5.0 darwin x64
http_parser: 2.7.0
node: 6.10.2
v8: 5.1.281.98
uv: 1.9.1
zlib: 1.2.11
ares: 1.10.1-DEV
icu: 58.2
modules: 48
openssl: 1.0.2k
```
## Hexo主题设置
### 安装主题：
可在https://hexo.io/themes/ 选择自己喜欢的主题
以hexo-beantech主题为例进行说明。
https://github.com/YenYuHsuan/hexo-theme-beantech/
```
$ hexo clean
$ git clone https://github.com/YenYuHsuan/hexo-theme-beantech.git ./hexo-beantech
$ cd hexo-beantech
$ npm install
```
### 启用主题
视情况而定,是否需要执行此步骤。
修改Hexo目录下的`_config.yml`配置文件中的`theme`属性，将其设置为material。
### 更新主题
```
$ cd themes/yilia
$ git pull
$ hexo g # 生成
$ hexo s # 启动本地web服务器
```
现在打开http://localhost:4000/ ，会看到我们已经应用了一个新的主题。
## Github Pages设置
### 什么是Github Pages
[GitHub Pages](https://pages.github.com) 本用于介绍托管在GitHub的项目，不过，由于他的空间免费稳定，用来做搭建一个博客再好不过了。

每个帐号只能有一个仓库来存放个人主页，而且仓库的名字必须是`username/username.github.io`，这是特殊的命名约定。你可以通过http://username.github.io 来访问你的个人主页。
### 创建自己的Github Pages
在这里我创建了一个github repo叫做 [zichen-jiao/zichen-jiao.github.io](https://github.com/zichen-jiao/zichen-jiao.github.io) 创建完成之后，需要有一次提交(git commit)操作，然后就可以通过链接http://zichen-jiao.github.io/ 访问了。（现在还没有内容，别着急）

### 部署Hexo到Github Pages
这一步恐怕是最关键的一步了，让我们把在本地web环境下预览到的博客部署到github上，然后就可以直接通过http://zichen-jiao.github.io/访问了。不过很多教程文章对这个步骤语焉不详，这里着重说下。
首先需要明白所谓部署到github的原理。
1. 之前步骤中在Github上创建的那个特别的repo（zichen-jiao.github.io）一个最大的特点就是其master中的html静态文件，可以通过链接http://zichen-jiao.github.io来直接访问。
2. Hexo -g 会生成一个静态网站（第一次会生成一个public目录），这个静态文件可以直接访问。
3. 需要将hexo生成的静态网站，提交(git commit)到github上。

明白了原理，怎么做自然就清晰了。
### 使用hexo deploy部署
hexo deploy可以部署到很多平台，具体可以参考这个链接. 如果部署到github，需要在配置文件_config.xml中作如下修改：

```
deploy:
  type: git
  repo: git@github.com:zichen-jiao/zichen-jiao.github.io.git
  branch: master
```
然后再命令中执行

```
hexo d
```
即可完成部署。
注意需要提前安装一个扩展：

```
$ npm install hexo-deployer-git --save
```
### 使用git命令行部署
不幸的是，上述命令虽然简单方便，但是偶尔会有莫名其妙的问题出现，因此，我们也可以追本溯源，使用git命令来完成部署的工作。
#### clone github repo

```
cd /Users/zichen.jiao/hexo/blog/hexo-beantech
git clone https://github.com/zichen-jiao/zichen-jiao.github.io.git .deploy/zichen-jiao.github.io
```
 将我们之前创建的repo克隆到本地，新建一个目录叫做.deploy用于存放克隆的代码。
#### 创建一个deploy脚本文件
先执行：

```
git config --global user.email "zichen615@yeah.net"
git config --global user.name "zichen-jiao"
```
然后：

```
 hexo generate
 cp -R public/* .deploy/zichen-jiao.github.io
 cd .deploy/zichen-jiao.github.io
 git add .
 git commit -m "update"
 git push origin master
```
简单解释一下，`hexo generate`生成public文件夹下的新内容，然后将其拷贝至zichen-jiao.github.io的git目录下，然后使用`git commit`命令提交代码到zichen-jiao.github.io这个repo的master branch上。

需要部署的时候，执行这段脚本就可以了,比如可以将其保存为deploy.sh。执行过程中可能需要让你输入Github账户的用户名及密码，按照提示操作即可。
### Hexo 主题配置
每个不同的主题会需要不同的配置，主题配置文件在主题目录下的_config.yml。

```
# Header
menu:
  主页: /
  所有文章: /archives
  # 随笔: /tags/随笔

# SubNav
subnav:
  github: "#"
  weibo: "#"
  rss: "#"
  zhihu: "#"
  #douban: "#"
  #mail: "#"
  #facebook: "#"
  #google: "#"
  #twitter: "#"
  #linkedin: "#"

rss: /atom.xml

# Content
excerpt_link: more
fancybox: true
mathjax: true

# Miscellaneous
google_analytics: ''
favicon: /favicon.png

#你的头像url
avatar: ""
#是否开启分享
share: true
#是否开启多说评论，填写你在多说申请的项目名称 duoshuo: duoshuo-key
#若使用disqus，请在博客config文件中填写disqus_shortname，并关闭多说评论
duoshuo: true
#是否开启云标签
tagcloud: true

#是否开启友情链接
#不开启——
#friends: false

#是否开启“关于我”。
#不开启——
#aboutme: false
#开启——
aboutme: 我是谁，我从哪里来，我到哪里去？我就是我，是颜色不一样的吃货…
```

[下载Node.js]:https://nodejs.org/en/
[下载Git]:http://git-scm.com/download/