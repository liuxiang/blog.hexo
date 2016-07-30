title: 跨IDE格式化工具editorconfig
date: 2016-3-2 00:00:00
categories:   editorconfig

tags: [ editorconfig ]


---
# 作用
不同IDE使用相同 的代码风格管理` editorconfig `
脱离IDE设置代码样式，主流IDE均支持



# 表现
工程根目录存在`. editorconfig `文件


# 支持 IDE
`集成支持` intellij IDEA,webStorm...
`插件支持` Sublime,eclipse,ATOM,Brackets,notepad++,Xcode...
详见: http://editorconfig.org/


# 示例
任何项目根目录文件 `.editorconfig`
```
# http://editorconfig.org
root = true
 
[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
 
[*.md]
insert_final_newline = false
trim_trailing_whitespace = false
```


`github -  ditorconfig `
https://github.com/editorconfig/editorconfig.github.com

https://github.com/editorconfig/editorconfig-core-js

https://github.com/editorconfig/editorconfig-jetbrains
https://github.com/editorconfig/editorconfig-sublime



`EditorConfig介绍`
http://blog.csdn.net/cengjingcanghai123/article/details/43953307


`WebStorm9的一些奇怪问题`
http://www.zhixing123.cn/php/43313.html



`WebStorm 9 纳入 EditorConfig 支持`

http://www.tuicool.com/articles/IzMFRnF


`使用 EditorConfig来规范代码缩进等的风格以webstorm为例`

http://blog.csdn.net/gextreme/article/details/23794837


<!-- more -->
