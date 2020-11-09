---
title: Hexo配置加闲聊
Author: Gui
Date: 2020-08-22
tags: [hexo, blog]
categories: misc
---



俗话说“跑程序一小时，配环境一整天”。

第一步就是要配置环境。这有一个[官方教程](https://hexo.io/zh-cn/docs/)，可以参考

其实只要环境配置好了，后续的操作都非常简单。

# Tools总览

我们这里维护博客一共需要以下的工具：

- Typora
- Git
- Node.js, npm
- hexo



# 详细配置

## Git

Git是用来管理/存储代码和部署网页的地方。

需要对git进行的操作

- [x] 安装git
- [x] 配置git账户
- [x] 从github上下载仓库，并日后维护。

### 安装git

### 配置git账户

在本地生成密钥，把密钥添加到远端，达到本地远程的链接效果。

可以参考[link](https://www.jianshu.com/p/25587e049d54)

如果电脑上有多个账户，且没有配置全局账户，以后的操作应该是对于每一个repo都要单独配置账户。

- [x] .ssh/config文件是什么

  https://www.cnblogs.com/foohack/p/10027083.html

- [x] 多个账户配置

https://www.jianshu.com/p/fbbf6efb50ba



### 仓库配置

- [x] 如何将远程仓库下载到本地，并且配置用户？

[将远程仓库下载到本地](https://www.cnblogs.com/yshang/p/11230209.html)

1. 在本地新建一个文件夹
2. 将本地仓库初始化`git init`

3. 将相应的代码从远程下载下来 `git clone git@github.com:guiyuanhood/guiyuanhood.github.io.git`

4. 进入这个仓库的目录下，输入以下代码

   `git config --unset user.name`

   `git config --unset user.email`

   `git config user.name "Losermoto"`

   `git config user.email andywongpku@gmail.com`

   

## Typora

我喜欢使用这个markdown文本编辑器。

Typora比一般markdown编辑器好的地方是：即时展现。也就是它的界面展现给你的就是已经渲染好效果的版本，而不是原始版本。

原始版本是什么样呢？在键盘上按下`Ctrl+/`就能看见了。再按一次`Ctrl + /`就可以返回展示界面。	

Typora的左侧可以把文章的结构展现出来，而且Typora可以很方便地把markdown导出为pdf或者html等多种结构。在左上角菜单栏里有一个导出（Export）。

### Markdown

markdown类型的文本后缀是.md，是一种文本标记语言。常见于代码仓库的readme文件。类似的文件类型还有.rst。

[markdown vs rst](https://www.cnblogs.com/youxin/p/3597229.html)

[rst intro](https://www.jianshu.com/p/1885d5570b37)

hexo可以直接将markdown类型的文本转化为html类型，用做网页。



支线任务

- [ ] [基础教程](https://www.jianshu.com/p/335db5716248)
- [ ] 特别好玩的是，markdown插入emoji非常方便（程序员的小浪漫），比如:kissing: 这里是[emoji的链接](https://www.webfx.com/tools/emoji-cheat-sheet/)



## Node.js和npm

node.js的简单解释是一种javascript的运行环境，能够使得javascript脱离浏览器运行。

支线任务

- [ ] 具体javascrpit是什么，请自行百度。建议现在先不看，可以作为支线任务。其他还可以看html, css. html, css, javascript几乎是前端标配。

npm简单说就是nodejs内置的包管理器。可以理解为anaconda之于python的地位。npm就是anaconda，nodejs就是python

### 安装

![nodejs](./pic/nodejs.png)

## Hexo

[hexo](https://hexo.io/zh-cn/docs/)是一个博客框架，能将markdown格式的文件解析生成静态网页。

hexo据说是一个很二次元的台湾小哥写的。这里是[Hexo诞生的介绍](https://zespia.tw/blog/2012/10/11/hexo-debut/)。小哥是在玩前端的时候，对现有的博客框架很不满意，于是就自己写了一个。。。。（大佬就是任性，遇到不喜欢的东西就自己写一个。。。类似的故事还有git，linux，latex，matlab等等。我深深地被小哥任性的初衷吸引了，非常种草这个框架。）



安装参考官方文档。

之后就可以去选择自己喜欢的主题，并且进行修饰啦。