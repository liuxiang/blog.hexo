title: 比对css与jquery选择器
date: 2016-12-13 00:00:00
tags: [css选择器]


---
# 第三位开始 ```
    .exam_info > div:nth-child(n+3) {
      color: #a2a2a2;
    }
```


# 最前两位
```
    .exam_info > div:nth-child(-n+2) {
      color: #a2a2a2;
    }
```


# 比对:  Css与jQuery选择器汇总
名称 |   Css    | jQuery
--|--|--|
第一个元素  |    :first-child  |    :first, :first-child
最后个元素  |    :last-child    |  :last, :last-child
偶数的元素  |    :nth-child(even)   |   :even
奇数的元素  |    :nth-child(odd)   |   :odd
大于给定索引值的元素  |    :nth-child(n+2)   |   :gt(0)
小于给定索引值的元素  |    :nth-child(-1n+8)   |   :lt(2)




**参考**
`Css与jQuery选择器转换 gt lt eq - 快乐每一天 - ITeye技术网站`
http://qiaolevip.iteye.com/blog/2189124



`jQuery选择器和选取方法 - MaxIE - 博客园`
http://www.cnblogs.com/MaxIE/p/4078869.html


`这 30 类 CSS 选择器，你必须记在脑袋里！ - 开源中国社区`
http://www.oschina.net/news/57107/30-css-selector-you-should-remeber

