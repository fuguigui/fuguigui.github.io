---
title: 一次完整的Hexo写作流程
date: 2021-08-17
tags: [Hexo]
categories: [Sharing]
---

推荐有任何疑问点的时候，先去查阅[hexo官方文档](https://hexo.io/zh-cn/docs/writing.html)。但是官方文档，有着工具书共同的弊端：缺乏对日常操作的指导性，难以分清阅读顺序，难以区分各个内容的重要程度和优先级。

我将根据一次hexo更新博客的完整流程，重新整理总结需要的命令操作。

网上也有很多博文进行此方面操作，可参考[知乎Hexo博客写文章及基本操作](https://zhuanlan.zhihu.com/p/156915260)。不同于这些博文，我会额外介绍操作背后的原理。

一次使用hexo更新博客的基础操作流程为：

1. 新建一个文章
2. 文章写作
3. 本地预览更新后的博客
4. 远程部署更新博客

# 新建文章

实际上，hexo渲染一篇post的工作流程是：

1. 拿到一个markdown文件，

2. 根据这个文件的layout类型，

3. 进行样式排版，生成html文件，

4. 最终展示到网页端。

因此，一个`_posts/`文件夹下的markdown文件就会对应到一篇post。所以，第一步是创建一个markdown文件。有两种方法：

- 使用hexo的命令
- 按常规的操作，新建一个markdown文件，并保存到`_posts/`文件夹下即可。

## 使用hexo的命令

官方文档是这么说的：

```bash
$ hexo new [layout] <title>
```

实际上，常用的操作是：

```bash
$ hexo new "我的第一篇文章"
```

输入这行命令背后的操作是：默认生成一个**layout**为post类型的markdown文件，该文件存储为`***.github.io/source/_posts/我的第一篇文章.md`。

## 常规的操作

也可以不使用命令行进行生成，我更习惯于直接在Typora中新建文件，并保存到`***.github.io/source/_posts/`这个位置。

我觉得这样的操作更便捷，因为我常常是打开Typora之后，有多个文档要写，直接`Ctrl + N`新建一个文档，会快很多。当然这种操作也有弊端：

- 需要手动添加文章头部信息Front Matter。添加头部信息Front Matter的操作是：
  - 先输入三个短横线-，`---`，然后回车。
  - 在出现的阴影框里，输入希望添加的属性值，
    - 常用的有`title, date, author`
    - 次常用的有`categories, tags`
    - [Front-Matter official manual](https://hexo.io/zh-cn/docs/front-matter)
    - 其他的还有比如：`mathjax`, `top`等是hexo的一些插件需要使用的参数。

  Front Matter其实就是预先告诉hexo该博文的一些基本属性。可以用文件类比，理解为文件的类型信息，创建日期信息，用什么方式打开，而不涉及具体的文件内容。

- 如果该文章需要展示图片，pdf等其他文件时，需要自行创建一个**同名文件夹**。示例，如果你的markdown文件叫做`_posts/markdown-demo.md`的话，同名文件夹就是`_posts/markdown-demo/`。

  如果你将`***.github.io/_config.yml`中的`post_asset_folder`设置为`true`，那么使用hexo命令，则会自行创建这个同名文件夹。

- 不适用于自定义layout以减少重复工作量的情形。（详细参考下面的layout是什么）

使用hexo命令方式创建文档，会自动生成`title, date`信息。

## layout是什么（进阶内容，可跳过）

感兴趣且可以科学上网的同学，强烈推荐一个[YouTube Hexo系列视频](https://www.youtube.com/watch?v=Jiwbmyc4nCA&list=PLLAZ4kZ9dFpOMJR6D25ishrSedvsguVSm&index=6)。第六集scaffolds和第十二集layout基本就对应本小节内容。

回到官方文档，会看到`hexo new`命令有个可选参数叫做`[layout]`。在文档的下一句说到

> 您可以在命令中指定文章的布局（layout），默认为 `post`，可以通过修改 `_config.yml` 中的 `default_layout` 参数来指定默认布局。

有的博主会介绍说，布局有三种：`post`（文章）、`draft`（草稿）、`page`（页面）。其实不够准确。如果你愿意，你可以有n多种布局。只不过这三种是hexo以及大多数主题theme已经默认帮你写好的。

*那么什么是layout呢？*

layout：英文直译是版面设计。也就是说这个命令告诉hexo，该如何排版。不同的排版使用不同的html网页布局，css样式，展示不同的属性。

*hexo是如何根据这个layout参数值进行后续操作的呢？*

- 首先，每一个markdown文件都有自己的layout。对于没有显示在Front Matter写明layout种类的文件，hexo会自动根据`***.github.io/_config.yml`中的`default_layout:`属性值来决定是什么layout，这个值默认为`post`。也就是说hexo会默认根据post的内容及格式规定对markdown文章进行渲染。

- 一个layout有两个位置的定义来共同决定。

  - 在`***.github.io/scaffolds/`文件夹下，和
- 在`***.github.io/themes/theme-name/layout/`文件夹下。

### scaffolds文件夹


第一个部分`scaffolds/`文件夹下，主要规定新建markdown文件的内容：Front Matter和正文。

举例：可以在该文件夹下，创建一个新的md文件，假设叫做`selflayout.md`，在里面输入

  ```
  ---
  title: {{title}}
  date: {{date}}
  author: hahaha
  layout: {{layout}}
  ---
  
  Demo: self-defined post content
  ```

  这之后，就可以直接使用hexo命令行操作

  ```bash
  hexo new selflayout "demo"
  ```

  在`_posts/`文件夹下，生成一个名为`demo.md`layout为selflayout的文件啦。

  更改`scaffolds/`下的对应markdown文件，可以大大减少很多重复工作。

- 比如你想要创建一个系列博客，共享一些`categories, author`，你就可以新建一个layout，在Front Matter中添加`categories, author`的属性值。
- 再比如所有的post的作者都是同一个人，叫aa，而你不想在每一篇post的Front Matter中都手动添加`author: aa`。就可以选择在`scaffolds/post.md`的Front Matter中添加`author: aa`。这样，每次使用`hexo new file-name`都会自动实现这个功能啦。

### theme的layout文件夹

一个layout的第二部分定义在`themes/theme-name/layout/`文件夹下。这部分主要规定ejs, css等具体的排版样式。hexo渲染一个页面的先后顺序是，所有的页面都建立在`layout.ejs`的基础上，然后根据各自的版面进行渲染，

- 比如index.html的页面是这么生成的，最外层是`layout.ejs`，然后将`layout.ejs`内部的`<%- body %>`替换为`index.ejs`的渲染效果。
- 再比如一篇博客的html页面是这么生成的，最外层是`layout.ejs`，然后将`layout.ejs`内部的`<%- body %>`替换为`post.ejs`的渲染效果。

对应于`_posts/`文件下的每一个markdown文件，如果Front Matter里

- 没有指明layout是什么，会默认根据`_config.yml`里的`default_layout: post`使用`layout.ejs` + `post.ejs`方式进行渲染

- 如果指明了layout是什么（比如我们上面新建了一个叫做`selflayout`的layout），

  - 在`themes/theme-name/layout/`下也存在`selflayout.ejs`文件，就会使用`layout.ejs` + `selflayout.ejs`方式进行渲染
  - 如果在`themes/theme-name/layout/`不存在对应的ejs文件，仍会默认使用`layout.ejs` + `post.ejs`方式进行渲染。

总结来说，**scaffolds文件夹下，会对内容进行默认设置，而theme的layout文件夹下的ejs文件，则会对排版进行设置。**

# 文章写作

对于一般的文字内容排版，只需要按照markdown的写作格式进行写作即可。例如：一级标题，二级标题，emoji输入，加粗斜体等。

我这里添加几个我常用到的额外的功能：插入本地图片，展示本地pdf，展示数学公式，链接到本站其他blog

## 插入本地图片

这里主要参考这篇文章[赵大宝hexo-images](https://fuhailin.github.io/Hexo-images/)，介绍的很详细，我在此基础上稍作补充。在此说明这篇文章里的前四个方法：

- 绝对路径本地引用，方法：

  - 将图片存储于`source/images`文件夹中
  - 在markdown文件中写`![picture description](images/picname.picformat)`
- 相对路径本地引用，方法：

  - 将图片存储于`_posts/`同名文件夹下
  - 同绝对路径引用，在markdown文件中写`![picture description](images/picname.picformat)`
- 标签插件语法引用，方法：

  - 将图片存储于`_posts/`同名文件夹下

  - 在markdown文件中写`{% asset_img image.jpg This is an image %}`
- HTML语法引用
  - 将图片存储于`_posts/`同名文件夹下
  - 在markdown文件中写`<img src="picname.picformat" weight="50%" height="100%" title="picture description" alt="picture alternative description"/>`

### 示例

示例以及优劣势比较。以下是我使用这四种方法展示本地图片的效果：

- 绝对路径

![绝对路径](/images/BD1.png)

- 相对路径

![相对路径](BD1.png)

![相对路径小图](BD1_mini.png)

- 标签插件语法引用

{% asset_img BD1.png 标签插件语法引用 %}

- html引用

<img src="BD1.png" weight="50%" height="50%" title="html引用小图" alt="html引用小图"/>

<img src="BD1.png" height="100%" title="html引用原图" alt="html引用原图"/>

### 优劣势比较

根据我的测试结果：

- 标签插件语法引用失效。没闹明白为什么。如果有知道的同学，我真诚求教。
- html引用小图失效。我的设置是`weight="50%" height="50%"`。依旧没弄明白为什么，求教中。
- 绝对路径和相对路径在引用效果上没有区别。唯一的区别是文件存储位置，是喜欢图片集中存放，还是每个post建一个文件夹存放，因人而异。
- 使用markdown本身的路径引用方式的话，无法在引用时调整图片大小。网页端最终显示的图片大小是图片原本的大小（如果屏幕放得下的话）。想要调整显示页面的图片大小，必须**手动更改图片原本的大小**。比如，我这里相对路径引用的原图和小图，其实对应着两个png文件，一个为原始图片大小为宽1097高537，一个我手动更改为小图，其尺寸为宽500高245。
- 使用html引用方式的优势在于，可以在引用时更改大小（？？？原理上是这样，但是我没有成功？？？）；还可以设置图片的替代描述文字（当图片无法正常显示的时候，用以替代图片的文字显示）。
- **总体来说：我推荐绝对/相对路径引用方式**

## 展示本地pdf

对此网上也有诸多教程，在此参考一篇[csdn博客hexo添加pdf插件](https://blog.csdn.net/u010820857/article/details/82356974)，并另外补充一种markdown的文件引用方式。

- 使用npm插件方式：

  - 安装插件：`npm install --save hexo-pdf`

  - 将pdf文件存储于`_posts/`的同名文件夹下。

  - 在markdown文本里输入：

    ```
    {% pdf file-name.pdf %} 
    ```

  - （如果需要引用网上pdf资源，可以直接使用`{% pdf http://url-to-pdf/file-name.pdf %} `）

- markdown自带的文件引用方式：

  - 将pdf文件存储于`_posts/`的同名文件夹下。

  - 在markdown文本里输入：

    ```
    [pdf file description](file-name.pdf)
    ```

这两种方法都可以使别人获取你的pdf文件，区别在于：

- npm插件方式：会生成内置pdf的小窗口，**读者可以直接阅读到pdf文件内容**。
- markdown自带的方式：会生成一行带有超链接的文字，点击文字，则直接提示下载该文件。这行文字就是种括号里你输入的描述性文字。

我一般希望别人直接阅读到我的pdf内容，所以推荐npm的插件方式。

## 展示数学公式

参考[csdn:hexo博客支持数学公式](https://blog.csdn.net/qq_38496329/article/details/104065659)。

- 安装npm渲染器：`npm install hexo-renderer-kramed --save`

- 修改`***.github.io/_configs.yml`配置文件中

  ```yaml
  mathjax:
    enable: true
  ```

- 在markdown文件的Front Matter加入`mathjax: true`

- 按照markdown语法，在需要添加数学公式的地方，直接按照markdown语法使用`$ formula $`添加行内公式或者`$$ formula $$`添加整行公式。

## 链接到本站其他blog

如何在当前的blog里引用本站其他的blog呢？

本质上就是直接使用markdown的文件链接方式：

```bash
[file description](file path)
```

唯一不同的就是这里的file path，不是指要被引用的markdown文件在`_posts`文件夹的位置，而是**经hexo渲染后生成的html文件的位置**。

举例来说，我这里想引用另一篇介绍hexo的blog，它的原始文件叫做`source/_posts/Hexo1.md`，经hexo渲染后的文件存储为`public/hexo1/index.html`。

而我当前的文件叫做`source/_posts/Hexo2.md`，经hexo渲染后的文件存储为`public/hexo2/index.html`。

如果习惯于markdown文件的书写，可能会直接在本文件里写

```bash
[Hexo1](Hexo1.md)
```

正确的写法应该是，

```bash
[Hexo1](../hexo1/index.html)
```

这是因为hexo在渲染的时候，并不会更改`()`中的内容。如果按照第一种写法，实际上会在`public/hexo2/`文件夹下寻找名为`Hexo1.md`的文件，并不存在。按照第二种写法，则是先去到父目录`public/`然后找到`hexo1/index.html`。

[Hexo1](../hexo1/index.html)

当然，有的小伙伴的blog是用时间来生成路径的，比如`public/2019/07/02/hexo2/index.html`，同理寻找到正确的路径即可。至于如何控制hexo使用时间还是文章标题来命名，在`_config.yml`文件里有一个属性叫做`permalink: `，我这里设置为`:title/`，即按照blog标题生成链接路径。

## 草稿

此外，可以在`source/drafts/`文件夹下，创建markdown文件，写草稿。在这些文档里，正常写作即可，但它们不会被渲染，也就不会显示在最终的博客页面里。

# 发布文章

先列举一些常用的命令

- Clean: `hexo clean`。该命令会删除整个`***.github.io/public/`文件夹

- Generate: `hexo generate`或者简写为`hexo g`。该命令会生成静态文件夹`public/`，也就是从markdown到网页文件html等的转换操作

- Server: 启动服务器常用的是`hexo server`或者简写为`hexo s`。

  - 若需指定端口号则为`hexo server -p 5000`5000可以更改为其他端口号。
  
  该命令会把生成好的静态文件部署到本地的指定端口，之后即可在本地浏览器输入`localhost:4000`即可预览。若指定了端口号，则把4000改为你指定的端口，如上个示例中的`localhost:5000`
  
- deploy：`hexo deploy`或者简写为`hexo d`。该命令将网站部署到服务器上。实际操作是：更新你在github上的仓库`***.github.io`的指定分支（如果你采用Hexo系列第一节说到的两分支方式的话）。这是你最终发表的博客页面，你可以在浏览器上访问`https://***.github.io`来查看更新后的博客啦。

接着，给出一些命令使用的常见套路

- `hexo clean`这个命令其实很少用。使用情况常见于：更新了`_config.yml`文件夹，删除了一些已有博文等。原因就是速度慢，耗费不必要的时间。毕竟它会将整个`public/`文件夹删除，再重新生成。推荐偶尔清理使用即可。
- 通常更新一次博客的套路是先本地预览，再远程部署。即先执行
  - `hexo g; hexo s`，再执行
  - `hexo d`或者`hexo g; hexo d`
- hexo有个特别便利的地方！
  - 在本地预览时，你仍可以更改markdown文件中一般的文字内容，然后直接在浏览器端刷新页面，就能看到实时更改的效果，而不需要再执行一次`hexo g;hexo s`，节省很多时间。常用于预览过程中进行微调操作。我的测试表明，此时更改文章的`categories, tags`等Front-Matter的属性的话，也可以动态刷新，很神奇。
  
  - 但是，有些涉及到更深层次的操作，比如利用到themes文件里的js函数，css样式，文件链接等，可能无法实时更新。此时仍需要重新generate才可以预览最新效果。同理，如果你更改了themes文件夹下面的css文件, ejs文件, yml文件等，通常也需要重新渲染。
  
  - 注意：在执行`hexo s`之后，想要中断操作，使用的是`control + C`快捷键。我老会习惯性地使用`con trol + Z`的快捷键。此时，可能出现端口被占用的错误。解决办法是：1. 找到被占用端口4000的进程号pid，2. kill掉这个进程。
  
    ```
    lsof -i:4000
    kill -9 pid
    ```
  
    
- 如果你采用两个分支的git部署方式。使用`hexo d`只是更新了其中的一个分支，此时仍需要进行常规的git仓库更新操作`git add ... git commit ... git push`这一系列来更新另一分支。

总结来说，最常用的套路就是

1. `hexo g; hexo s`本地预览，再更改，直到满意为止
2. `hexo d`或者`hexo g; hexo d`远程部署。



# 引用

1. [hexo官方文档](https://hexo.io/zh-cn/docs/writing.html)
2. [知乎Hexo博客写文章及基本操作](https://zhuanlan.zhihu.com/p/156915260)
3. [Front-Matter official manual](https://hexo.io/zh-cn/docs/front-matter)
4. [YouTube Hexo系列视频](https://www.youtube.com/watch?v=Jiwbmyc4nCA&list=PLLAZ4kZ9dFpOMJR6D25ishrSedvsguVSm&index=6)
5. [赵大宝hexo-images](https://fuhailin.github.io/Hexo-images/)
6. [csdn博客hexo添加pdf插件](https://blog.csdn.net/u010820857/article/details/82356974)
7. [csdn:hexo博客支持数学公式](https://blog.csdn.net/qq_38496329/article/details/104065659)