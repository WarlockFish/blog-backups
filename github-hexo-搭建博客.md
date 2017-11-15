---
title: github+hexo 搭建博客
date: 2017-10-15 23:04:53
tags: [new,hexo,github pages,教程,配置]
categories: github + hexo
copyright: true
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=439915614&auto=0&height=66"></iframe>

#  前言

 本文章讲述如何利用 [github pages](https://github.com/) 和 [hexo](https://hexo.io/) 建博客

<!--more-->
## 准备环境

* 有一个github账号，没有的话去 github https://github.com/ 注册一个；
* 安装node.js、npm
* 安装git
* 安装hexo

本文所使用的环境：

* ubuntu   16.04
* node.js  8.6.0
* git      2.7.4
* hexo     3.3.9

# 搭建github博客

## 创建仓库

在注册完github帐号后，新建一个名为`用户名.github.io`的仓库，比如说，如果你的github用户名是test，那么你就新建`test.github.io`的仓库（必须是你的用户名，其它名称无效），将来你的网站访问地址就是 http://test.github.io 。

![](/photo/注册1.png)

由此可见，每一个github账户最多只能创建一个这样可以直接使用域名访问的仓库。

几个注意的地方：
1. 注册的邮箱一定要验证，否则不会成功；
2. 仓库名字必须是：`username.github.io`，其中`username`是你的用户名；


创建成功后，默认会在你这个仓库里生成一些示例页面，以后你的网站所有代码都是放在这个仓库里啦。

## node.js&&npm 安装

* node官网 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <v>  https://nodejs.org/en/
* npm官网 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<v>   https://www.npmjs.com


ubuntu下安装

``` bash
sudo apt-get install nodejs && npm
```
## git 安装

* git官网下载      &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;<v> https://git-scm.com/downloads/

  Windows: <v>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; https://windows.github.com/

  Mac: <v>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; https://mac.github.com



其实ubuntu下直接安装就好:

``` bash
sudo apt-get install git
```

# 配置SSH key

为什么要配置这个呢？因为你提交代码肯定要拥有你的github权限才可以，但是直接使用用户名和密码太不安全了，所以我们使用ssh key来解决本地和服务器的连接问题。

用git bash执行如下命令：

``` bash
$ cd ~/. ssh #检查本机已存在的ssh密钥
```

如果提示：No such file or directory 说明你是第一次使用git。

``` bash
	ssh-keygen -t rsa -C "邮件地址"
```

然后连续3次回车，最终会生成一个文件在用户目录下，打开用户目录，找到`.ssh\id_rsa.pub`文件，记事本打开并复制里面的内容，打开你的github主页，进入个人设置 -> SSH and GPG keys -> New SSH key：

![](http://image.liuxianan.com/201608/20160818_143914_495_9084.png)

将刚复制的内容粘贴到key那里，title随便填，保存。

## 测试是否成功

  $ ssh -T git@github.com # 注意邮箱地址不改

如果提示`Are you sure you want to continue connecting (yes/no)?`，输入yes，然后会看到：

> Hi liuxianan! You've successfully authenticated, but GitHub does not provide shell access.

看到这个信息说明SSH已配置成功！

此时你还需要配置：

```bash
	$ git config --global user.name "liuxianan"// 你的github用户名，非昵称
	$ git config --global user.email  "xxx@qq.com"// 填写你的github注册邮箱
```
配置完之后输入：

```bash
  $ git config --list #查看已设配置
```
查看username，email是否正确

# 使用hexo写博客

## hexo简介

Hexo是一个简单、快速、强大的基于 Github Pages 的博客发布工具，支持Markdown格式，有众多优秀插件和主题。

官网： http://hexo.io
github: https://github.com/hexojs/hexo

## 原理

由于github pages存放的都是静态文件，博客存放的不只是文章内容，还有文章列表、分类、标签、翻页等动态内容，假如每次写完一篇文章都要手动更新博文目录和相关链接信息，相信谁都会疯掉，所以hexo所做的就是将这些md文件都放在本地，每次写完文章后调用写好的命令来批量完成相关页面的生成，然后再将有改动的页面提交到github。


## 安装


```bash
$ npm install -g hexo
```

## 初始化

在电脑的某个地方新建一个名为hexo的文件夹（名字可以随便取）
```bash
$ cd ~/hexo/
$ hexo init    #初始化文件夹
$ npm install  #安装包
```

hexo安装成功后，hexo文件夹目录为：

```javascript{.line-numbers}
.
├── _config.yml // 网站的配置信息，你可以在此配置大部分的参数。
├── package.json
├── scaffolds // 模板文件夹。当你新建文章时，Hexo会根据scaffold来建立文件。
├── source // 存放用户资源的地方
|   ├── _drafts
|   └── _posts
└── themes // 存放网站的主题。Hexo会根据主题来生成静态页面
```
具体内容可见[hexo建站](https://hexo.io/zh-cn/docs/setup.html)


然后输入下面：
```bash
$ hexo g # 生成public文件夹（浏览器访问资源）
$ hexo s # 启动服务
```

执行以上命令之后，hexo就会在public文件夹生成相关html文件，这些文件将来都是要提交到github去的



`hexo s`是开启本地预览服务，打开浏览器访问 http://localhost:4000 即可看到内容，很多人会碰到浏览器一直在转圈但是就是加载不出来的问题，一般情况下是因为端口占用的缘故，因为4000这个端口太常见了，解决端口冲突问题请参考这篇文章：

http://blog.liuxianan.com/windows-port-bind.html

第一次初始化的时候hexo已经帮我们写了一篇名为 Hello World 的文章，默认的主题比较丑，打开时就是这个样子：

![](/photo/2017.10.15.22.1.png)
## 配对ssr
打开`~\Hexo` 文件夹中的`_config.yml`文件，填写内容
```bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
 type: git
 repository: git@github.com:WarlockFish/WarlockFish.github.io.git     #填入你的github链接，我填的是我的
 branch: master
```
## 修改主题

默认主题很丑，可以来替换一个好看点的主题。这是 [官方主题](https://hexo.io/themes/) 链接

我使用的是 [next](https://github.com/iissnan/hexo-theme-next) .
喜欢使用可以安装：
```bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
记住要在hexo目录中执行上面指令。

修改`hexo`目录中的`_config.yml`中的`theme: landscape`改为`theme: next`，保存退出，
然后执行`hexo g`来重新生成文件。
```bash
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

## 写博客
在`hexo`下

```bash
hexo n "name of the new post"
```
在`~/hexo/source/_posts`下会生成一份博客，Hexo使用MarkDown写作语法。写完后可以使用
```bash
hexo g
hexo s
```
然后可以在本地访问 http://localhost:4000  查看效果，便于更改

## 更新Github
进入目录
```bash
$ cd ./hexo
$ hexo  g  #编译本地内容
$ hexo  d  #上传到github上
```
这样就发布成功了，可以在网络上访问了。


谢谢阅读！！

