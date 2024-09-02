---
title: Update log
Date: 2021-08-06
---

# 2021-08-07

- all the categories:
  - Learning Notes, Sharing, Reading Summary
- all the tags:
  - Machine Learning, System, Privacy

# 2021-08-06

- RSS无法有效使用

- musics, movies, books, galleries没有页面链接。去掉这几页。附：自定义页面https://www.zhihu.com/question/33324071/answer/58775540

- 评论功能
  
  - gittalk失效，改动了libs/gitalk.js中的
    
    `proxy:  "https://cors-anywhere.azm.workers.dev/https://github.com/login/oauth/access_token"`                  
  
  - 可以使用valine进行评论

- 文章里的markdown超链接功能正常

- post链接post无效，例如CN review notes
  
  - 原因：直接使用doc name进行连接。解决办法不要用任何重名的文档，但是title可以一样。注意文档名不要使用year-month-date-title的形式，貌似hexo会自动纠正这样的命名。直接引用格式../doc-name/index.html
  - 注意：是doc name而不是文章的title
  - 修改方式：两点：
    - 文章内部的链接添加：被链接文章的doc name，而不是title
    - 被链接文章的doc name如何取？可以与文章本身不一致，最好是tag加文章名

- Computational Statistics没有pdf显示？
  
  pdf可以加载进来，但是没有预览功能
  
  使用 `{% pdf path_to_pdf_file/pdf_name.pdf %}`可以实现预览。
  
  如果使用 `[...](path_to_pdf_file/pdf_name.pdf)`则只能实现点击进入新页面查看

- 去掉update date的显示

- 无法正常显示latex公式。在文章开头添加`mathjax: true`

- 增加admin功能

- about页面增加resume功能，只需要在about/index.md中直接进行写作即可

- 把回到顶部的箭头，上一篇，下一篇的箭头改成方的

- 将“评论”功能改为英文：comment

- 改名：将read times改为reading time，将read count改为view times

- 代码改用主题coy，并做了如下更改
  
  background-color: #f7f5ef;
  
  Border-left: #bbb
  
  Box-shadow: #ccc

- 知乎主页信息添加

- ForkMe按钮的账户更新

- 更新404页面

# 2021-08-05

- 色调改为黑白灰为主的简洁风格。重要配色信息
  - navigation的图标和文字：4c4c4c
  - 按钮：border: 1px solid #f2f2f2;
    background: radial-gradient(#bbb, #ccc);
  - Highlight: 3c4858