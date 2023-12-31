---
title: Hexo环境配置
Date: 2020-08-22
tags: [Hexo, Git, Markdown]
categories: [Sharing]
top: true
---

这一系列分享如何使用Hexo搭建个人博客网站。

俗话说“跑程序一小时，配环境一整天”。

第一步就是要配置环境。这有一个[官方教程](https://hexo.io/zh-cn/docs/)，供参考。其实只要环境配置好了，后续的操作都非常简单。

# 工具总览

我们使用以下的工具来维护博客：

- Typora
- Git
- Node.js, npm
- hexo

# 详细配置

## Git

Git是用来管理/存储代码和部署网页的地方。

需要对git进行的操作

- 安装git

- 配置git账户

- 从github上下载仓库，并日后进行维护。

### 安装git

如何安装git，去网上搜索有很多现成的教程，这里就不赘述了。可参考[廖雪峰git安装教程](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)

### 配置git账户

1. 在本地生成密钥，把密钥添加到远端。这一步是为了将本地机器和远程账户进行链接。可以参考[简书git基本配置](https://www.jianshu.com/p/6e1de95828a8)

疑难解答：

- [x] .ssh/config文件是什么

  https://www.cnblogs.com/foohack/p/10027083.html

- [x] 多个账户配置。注：如果电脑上有多个账户，且没有配置全局账户，以后的操作应该是对于每一个repo都要单独配置账户。

https://www.jianshu.com/p/fbbf6efb50ba

### GitHub Pages连接

若没有GitHub Pages，则需：

1. 在github上新建一个Github Pages仓库
2. 配置仓库链接

若已建立GitHub Pages，则只需：

1. 配置仓库链接

#### 创建新GitHub Pages（可选）

参考[知乎：hexo和git配置](https://zhuanlan.zhihu.com/p/60578464)

#### 配置仓库链接

如何将远程仓库下载到本地，并且配置用户？ [cnblogs：将远程仓库下载到本地](https://www.cnblogs.com/yshang/p/11230209.html)

1. 在本地新建一个文件夹

2. 将本地仓库初始化`git init`

3. 将相应的代码从远程下载下来 `git clone git@github.com:gitacount/blogname.github.io.git`

4. 进入这个仓库的目录下，输入以下代码

   `git config --unset user.name`

   `git config --unset user.email`

   `git config user.name "GitAccount"`

   `git config user.email gitemail@***.***`

:star2: 对于GitHub Pages的仓库配置有个小技巧，就是**使用两个分支**，一个分支用于存储hexo渲染好的静态页面，也就是直接展示给读者的html等文件；另一个分支用于存储源代码，也就是hexo渲染前的文件，这样便于在多台机器上更新博客，或者更换机器后，直接从git上拉取下来就好。

详细教程请参考[haoshuai6: hexo两个分支](https://haoshuai6.github.io/2016-10-28-hexo-github.html)



## Typora

我喜欢使用这个markdown文本编辑器。

Typora比一般markdown编辑器好的地方是：即时展现。也就是它的界面展现给你的就是已经渲染好效果的版本，而不是原始版本的所有字符输入。

原始版本是什么样呢？在键盘上按下`Ctrl+/`就能看见了。再按一次`Ctrl + /`就可以返回展示界面。	

Typora的左侧可以把文章的结构展现出来，而且Typora可以很方便地把markdown导出为pdf或者html等多种结构。在左上角菜单栏里有一个导出（Export）。

### Markdown

markdown类型的文本后缀是.md，是一种文本标记语言。常见于代码仓库的readme文件。长处在于减少排版工作，支持方便输入数学公式，代码，引用，表情包等日常需求。

- [简书：Markdown基础教程](https://www.jianshu.com/p/335db5716248)
- [emoji-cheat-sheet](https://www.webfx.com/tools/emoji-cheat-sheet/)

类似的文件类型还有.rst。

- [cnblogs: markdown vs rst](https://www.cnblogs.com/youxin/p/3597229.html)

- [简书: rst introduction](https://www.jianshu.com/p/1885d5570b37)

hexo可以直接将markdown类型的文本转化为html类型，用做网页。

## Node.js和npm

node.js的简单解释是一种javascript的运行环境，能够使得javascript脱离浏览器运行。

npm(**n**odejs **p**ackage **m**anager)简单说就是nodejs内置的包管理器。可以理解为anaconda之于python的地位。将npm类比为anaconda，nodejs类比为python

### 安装配置

[runoob: nodejs安装配置windows/Linux/Mac](https://www.runoob.com/nodejs/nodejs-install-setup.html)

## Hexo

[hexo](https://hexo.io/zh-cn/docs/)是一个博客框架，能将markdown格式的文件解析生成静态网页。

hexo据说是一个二次元的台湾小哥写的。这里是[Hexo诞生的介绍](https://zespia.tw/blog/2012/10/11/hexo-debut/)。小哥是在玩前端的时候，对现有的博客框架很不满意，于是就自己写了一个。。。。（大佬就是任性，遇到不喜欢的东西就自己写一个。。。类似的故事还有git，linux，latex，matlab等等。我深深地被小哥任性的初衷吸引了，非常种草这个框架。）

安装参考官方文档。

之后就可以去选择自己喜欢的主题，进行写作和装饰啦。

# 引用

1. [Hexo官方安装教程](https://hexo.io/zh-cn/docs/)
2. [廖雪峰git安装教程](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)
3. [简书git基本配置](https://www.jianshu.com/p/6e1de95828a8)
4. [cnblogs: ssh](https://www.cnblogs.com/foohack/p/10027083.html)
5. [简书：Git多账户配置](https://www.jianshu.com/p/fbbf6efb50ba)
6. [知乎：hexo和git配置](https://zhuanlan.zhihu.com/p/60578464)
7. [haoshuai6: hexo两个分支](https://haoshuai6.github.io/2016-10-28-hexo-github.html)
8. [简书：Markdown基础教程](https://www.jianshu.com/p/335db5716248)
9. [emoji-cheat-sheet](https://www.webfx.com/tools/emoji-cheat-sheet/)
10. [cnblogs: markdown vs rst](https://www.cnblogs.com/youxin/p/3597229.html)
11. [简书: rst introduction](https://www.jianshu.com/p/1885d5570b37)
12. [runoob: nodejs安装配置windows/Linux/Mac](https://www.runoob.com/nodejs/nodejs-install-setup.html)
13. [hexo官方介绍](https://hexo.io/zh-cn/docs/)
14. [Hexo的诞生](https://zespia.tw/blog/2012/10/11/hexo-debut/)
