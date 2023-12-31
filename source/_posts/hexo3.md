---
title: Hexo进阶一：认识基本组件
date: 2021-08-23
tags: [Hexo]
categories: [Sharing]
---

本篇是Hexo进阶内容的第一篇，主要介绍为了DIY Hexo主题风格，大概率会修改的组件/文件。将介绍以下内容：

- 基本文件结构
  - 两个config的yaml文件
  - source文件夹
  - theme文件夹
- web前端“三剑客”：HTML+CSS+JavaScript及开发者工具

本期视频很多内容参考[YouTube Hexo系列视频](https://www.youtube.com/watch?v=Jiwbmyc4nCA&list=PLLAZ4kZ9dFpOMJR6D25ishrSedvsguVSm&index=6)，强烈推荐该系列视频，每一集只有几分钟，讲得深入浅出，实用性很强。之后会出一篇介绍我对Matery主题的个性化配置和修改，还会出一篇介绍hexo使用的优质资源分享。

# 基本文件结构

当你第一次成功运行了一次hexo之后`hexo s`或者`hexo d`，在你的blog文件夹下，会看到以下的文件结构：

```bash
- _config.yml
- source/
	|--- _posts/
	|--- _data/
	|--- drafts/
	|--- about/
	|--- other folders
- themes/
	|--- one or more theme folders, in each folder
	     |--- _config.yml
	     |--- languages/
	          |--- yaml files
	     |--- layout/
	          |--- (_partial/)
	          |--- (_widget/)
	          |--- layout.ejs
	          |--- index.ejs
	          |--- post.ejs
	          |--- other ejs files
	     |--- source/
	          |--- css/
	          |--- js/
	          |--- libs/
	          |--- medias/
	     |--- Others
- package-lock.json
- package.json
- scaffolds/
	|--- several markdown files
- public/
- node_modules/
- db.json
```

作为一名前端小白，我第一次看到这些文件夹时，两眼一抹黑，根本不知道要改哪里。经过好长一段时间的摸索，逐渐搞清楚了。这里会按照重要性的顺序和使用的顺序来进行重点介绍。

首先的首先，我以为前端/hexo的重要三部分就是**“内容+样式+功能”**。一切的一切都是围绕这三项展开。

## 两个config的yaml文件

通常，要DIY自己的网站的第一步就是修改`_config.yml`文件。注意：有两种`_config.yml`文件，

- 第一种在最上层的文件夹里`***.github.io/_config.yml`
- 第二种是在`themes/`文件夹下，和每个theme相匹配，例如`***.github.io/themes/matery/_config.yml`

为了便于区分，我将第一种称为大config，第二种称为小config。第一步修改的是大config。

大config涵盖了网站general的设置，比如网站的title，description，permalink等，详情参考[Hexo官方配置文档](https://hexo.io/zh-cn/docs/configuration.html)。其中，选择使用哪个主题也在大config里设置。将`theme:`属性的值设置为`themes/`文件夹下你希望使用的主题的文件夹名字即可。

小config主要是针对特定的主题进行属性设置，不同主题的小config文件包含的属性也不一样。在接下来的一篇分享，我会针对[Matery主题](https://github.com/blinkfox/hexo-theme-matery)详细介绍如何进行小config的修改。 

这两个文件对内容，样式，功能三个方面都有涉及。

## source文件夹

source文件夹是在主题配置完成后，主要进行文档写作，博客内容更新的地方。其中：

- `_posts/`文件夹存放的是一般的blog markdown文件，会被最终展示到博客上，
- 而`drafts/`下面放的是草稿，这部分内容默认是不会被hexo渲染，也不会显示在网站上的。很方便地用于存储创作中的文章。
- `_data/`文件夹，如[官网介绍](https://hexo.io/zh-cn/docs/data-files.html)，用于存放一些并不在文章内，且是需要重复使用的资料/数据。该文件夹下的文档的调用方式是：使用js/ejs，通过`site.data.filename`来调用。
- 其他诸如`categories/`，`tags/`，`404/`等文件夹，都是规定了相对应的分类页面，标签页面，404页面的内容。可匹配对应页面的在主题里的css, ejs文件做修改。之后也会分享到。

## theme文件夹

theme文件夹是前期对主题进行个性化设置时，主要修改的地方。一个theme主题的构成包括什么？老三样：**“内容+样式+功能”**。theme文件夹下的文件及文件们也对应着这三样。

- 小config文件：是与当前主题相关的general的设置。比如：是否开启当前主题的某种功能。
- `languages/`： 存储属性与对应语言的翻译，用于更改网站的基础显示语言。
- `layout/`：一系列的[ejs文件](https://ejs.bootcss.com/)：是一套简单的模板语言，帮你利用普通的 JavaScript 代码生成 HTML 页面。可以理解为既包含了**内容**，又包含了**功能**，像是HTML和Java Script的综合体。其下的子文件夹无非是为了将同类的文件聚集到一起，增强结构的可读性。譬如：`layout/_partial/`里存放的是部分在不同页面共同使用的网页模块，页眉页脚等。
- `themes/.../source/`这个文件夹包含css的**样式**设置，网页展示使用的图片、logo等，额外安装的功能库等，如[gitalk](https://github.com/gitalk/gitalk/)，[echarts](https://echarts.apache.org/zh/index.html)。

不同的主题下的文件结构可能略有差异，但构成内容大同小异。通常如果只是修改配色，就去到对应的`themes/.../source/css/...`去找到对应的部件/属性进行样式修改。如果要修改显示的内容，则需要到`themes/.../layout/`中找到对应的ejs文件进行修改。如果要修改/增删功能，也需要找到对应的ejs文件进行修改。

## 其他的文件夹

其他的一级文件或者文件夹，很少会在个性化配置中进行更改，此处不赘述。如感兴趣`scaffolds/`文件夹，可以参考[Hexo2: layout是什么](../hexo2/index.html#toc-heading-4)。`public/`文件夹请参考[Hexo2：发布文章](../hexo2/index.html#toc-heading-15)中的`hexo clean`和`hexo generate`命令。

# Web前端“三剑客”

这一部分，是我这个web前端小白入门前端的过程，希望能帮助到同样处境的人。当然前端的知识体系非常庞大，远不止我说的这三个，如果有进一步学习需求的小伙伴，推荐以下资源：

- [Github：前端入门到进阶图文教程](https://github.com/qianguyihao/Web)
- [YouTube: JavaScript in 1 hour](https://www.youtube.com/watch?v=W6NZfCO5SIk)
- [YouTube: Learn HTML in 1 hour](https://www.youtube.com/watch?v=qz0aGYrrlhU&t=7s)
- [YouTube: Node.js in 1 hour](https://www.youtube.com/watch?v=TlB_eWDSMt4)



先来简要介绍一下“三剑客”都是谁，有什么作用，该如何使用。主要参考自[Github：前端入门到进阶图文教程](https://github.com/qianguyihao/Web)。web前端“三剑客”包括：HTML，CSS和JS。

- **HTML**：全称为 HyperText Markup Language，译为**超文本标记语言**。是用来组织网页的元素和**内容**的，也就是各个浏览器直接拿来显示给用户的文档。
  - HyperText：超出了文本的限制，包括图片，音频，视频等
  - Markup：标记语言。会给元素做上标记，表明它是超链接啊，还是一级标题啊，还是图片啊，等等
- [**CSS**](https://www.runoob.com/css/css-intro.html)：全称为**C**ascading **S**tyle **S**heets，译为**层叠样式表**。是定义如何显示HTML元素的，比如字体啊，颜色啊，大小啊，图片的长宽啊等等。负责老三样中的**样式**。
- JS：全称为JavaScript，译为用Java写的脚本？是描述网页的**行为功能**（实现业务逻辑和页面控制）的，实现用户和网页的交互。而hexo里使用的ejs文档，又可译作embedded JavaScript，就是利用JavaScript生成HTML页面的。它和很多大家熟悉的编程语言Python，C/C++，Java等很像，有语法和对象。
  - ECMAScript：JavaScript 的语法标准。包括变量、表达式、运算符、函数、if语句、for语句等。
  - DOM：Document Object Model（文档对象模型），操作页面上的元素的API。比如让盒子移动、变色、改变大小、轮播图等等。
  - BOM：Browser Object Model（浏览器对象模型），操作浏览器部分功能的API。通过BOM可以操作浏览器窗口，比如弹框、控制浏览器跳转、获取浏览器分辨率等等。

总结来说，就是HTML管内容，描述页面结构；CSS管样式，追求审美；JavaScript管功能，实现行为。当我们对这三方面有需求的时候，就去到各自的文件里进行修改即可。

## 开发者工具

最后介绍一个特别好用的工具：浏览器的开发者工具。

我所使用的浏览器FireFox和Google Chrome都具备这个功能。在FireFox/Google Chrom的“更多工具”中有个名为“Web开发者工具”的，Mac上对应的快捷键是`Option+Command+I`。一开始，我并不知道这个工具，修改主题时，那叫一个痛苦，后来得此利器，原地起飞。Web开发者工具不仅方便定位到代码位置，还可以直接调试css样式属性，即时查看效果，另外这些修改并不会动到原始的代码，极其方便进行大量尝试。

这个视频[YouTube: Learn HTML in 1 hour](https://www.youtube.com/watch?v=qz0aGYrrlhU&t=7s)从14:30秒起简要介绍了web开发者工具，无法科学上网的小伙伴也可以参考[FireFox Tools](https://developer.mozilla.org/zh-CN/docs/Tools)。



总结，本篇主要介绍了理论内容，尽可能地为进行主题个性化配置打下理论基础，免得到时候两眼一抹黑，胡同里乱钻。另外，强烈推荐使用**Web开发者工具**，此乃神器！

# 引用

1. [YouTube Hexo系列视频](https://www.youtube.com/watch?v=Jiwbmyc4nCA&list=PLLAZ4kZ9dFpOMJR6D25ishrSedvsguVSm&index=6)
2. [Hexo Matery主题](https://github.com/blinkfox/hexo-theme-matery)
3. [Hexo data文件夹](https://hexo.io/zh-cn/docs/data-files.html)
4. [gitalk](https://github.com/gitalk/gitalk/)
5. [echarts](https://echarts.apache.org/zh/index.html)
6. [Github：前端入门到进阶图文教程](https://github.com/qianguyihao/Web)
7. [YouTube: JavaScript in 1 hour](https://www.youtube.com/watch?v=W6NZfCO5SIk)
8. [YouTube: Learn HTML in 1 hour](https://www.youtube.com/watch?v=qz0aGYrrlhU&t=7s)
9. [YouTube: Node.js in 1 hour](https://www.youtube.com/watch?v=TlB_eWDSMt4)
10. [CSS](https://www.runoob.com/css/css-intro.html)
11. [FireFox Tools](https://developer.mozilla.org/zh-CN/docs/Tools)