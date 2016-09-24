title: markdown 输入段落缩进
date: 2016-08-09 00:00:00
tags: [ markdown ]
 
---

# 对应css
```
p {
     text-indent: 2em; /*首行缩进*/
}
```


# 方法一： 全角空格
&emsp; &emsp; 全角空格，切换到全角模式下（一般的中文输入法都是按 shift + space）输入两个空格就行了。这个相对  `$nbsp;`  来说稍微干净一点，而且宽度是整整两个汉字，很整齐。
 
`在 Markdown 语言中，如何实现段首空格的显示 ？ - 中文排版 - 知乎`

http://www.zhihu.com/question/21420126


# 方法二：制表符 ` &emsp; &emsp; `
```
半方大的空白&ensp;或&#8194;
全方大的空白&emsp;或&#8195;
不断行的空白格&nbsp;或&#160;
```


---
`markdown.css 让 HTML 显示成 Markdown 纯文本 见证 CSS 的强大`
https://segmentfault.com/a/1190000000487062



`Html转换为MarkDown样式代码 | Html2MarkDown - aTool在线工具`
http://www.atool.org/html2markdown.php
