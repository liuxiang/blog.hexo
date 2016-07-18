title: hexo YAML [分类 & 标签]编译问题
date: 2015-11-30 00:00:00 #发表日期，一般不改动
categories: hexo #文章文类 
tags: [hexo,YAML]

---

# Error info
```
F:\Tool\node\hexo\blog>hexo g
ERROR Process failed: _posts/思维导图-log4j.md
YAMLException: end of the stream or a document separator is expected at line 4, column 11:
    categories: 思维导图 #文章文类
              ^
    at generateError (F:\Tool\node\hexo\blog\node_modules\hexo\node_modules\js-yaml\lib\js-yaml\loader.js:160:10)
```


# 相关文章
`Hexo Wordpress migration glitches | Glen Smith`
http://blogs.bytecode.com.au/glen/2015/10/23/Hexo-Wordpress-migration-glitches.html


# 测试工具
`YAML parser for JavaScript - JS-YAML`
https://nodeca.github.io/js-yaml/


`Convert JSON to YAML`
http://www.json2yaml.com/


#示例:
```
title: hexo YAML [分类 & 标签]编译问题 #dd
date: 2015-11-30 00:00:00 #发表日期，一般不改动
categories: hexo #文章文类 
tags: [hexo,YAML]
photos:
- ss
- ss
```
# 结果:
```
{ title: 'hexo YAML [分类 & 标签]编译问题',
  date: Mon Nov 30 2015 08:00:00 GMT+0800 (中国标准时间),
  categories: 'hexo',
  tags: [ 'hexo', 'YAML' ],
  photos: [ 'ss', 'ss' ] }
```


# 示例:
```
title:hexo YAML [分类 & 标签]编译问题#dd
date:2015-11-30 00:00:00#发表日期，一般不改动
categories:hexo#文章文类 
tags:[hexo,YAML]
photos
- ss
- ss
```


# 结果:
```
'title:hexo YAML [分类 & 标签]编译问题#dd date:2015-11-30 00:00:00#发表日期，一般不改动 categories:hexo#文章文类 tags:[hexo,YAML] photos - ss - ss'
```

---

# 问题二
```
YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key at line 7, column 1:
```
```
解决: "- "需要带空格
```


# 附赠
```
# 日常笔记中需要保留原图片,且在预览时仅查看在线图片地址. 可以通过如下注释实现
![]( http://***.jpg)
                  < -注意这需要一个空行,否则使用hexo的博客文章中会出现`<!-` `->`标记
<!-- 
IMG图片
-->
```

<!-- more -->
