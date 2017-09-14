title: Github 搜索技巧
date: 2016-04-16 00:00:00
categories:  Github
tags: [Github，搜索技巧]


---

https://github.com/search
https://github.com/search/advanced


https://help.github.com/categories/search/
`★` https://help.github.com/articles/searching-code/
`国内翻译稿` http://blog.sina.com.cn/s/blog_4e60b09d0102vcso.html


```
octocat in:path
Matches code where "octocat" appears in the path name.
搜索路径中有octocat的代码


octocat in:file,path
Matches code where "octocat" appears in the file contents or the path name.
搜索路径中有octocat的代码或者文件中有octocat的代码
```


```
按照目录结构搜索
By including the path qualifier, you specify that resulting source code must appear at a specific location in a repository. Subfolders are considered during the search, so be as specific as possible. For example:
console path:app/public language:javascript
Finds JavaScript files with the word "console" in an app/public directory (even if they reside inapp/public/js/form-validators).
在app/public directory目录下搜索console关键字
```


```
Search by filename
通过文件名搜索
You can use the filename qualifier if there's a specific file you're looking for. For example:
filename:.vimrc commands
Finds *.vimrc* files with the word "commands" in them.
搜索 文件名匹配*.vimrc* 并且包含commands的代码
minitest filename:test_helper path:test language:ruby
Finds Ruby files containing the word "minitest" named *test_helper* within the *test* directory.
在test目录中搜索包含minitest且文件名匹配"*test_helper*"的代码
```


```
Search by the file extension
根据扩展名来搜索代码
The extension qualifier matches code files with a certain extension. For example:
form path:cgi-bin extension:pm
Matches code with the word "form," under cgi-bin, with the .pm extension.
搜索cgi-bin目录下以pm为扩展名的代码
icon size:>200000 extension:css
Finds files larger than 200 KB that end in .css and have the word "icon" in them.
搜索超过200kb包含icon的css代码
```
---
`searchcode | source code search engine`
https://searchcode.com/


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-5-5/10685161.jpg)


```
Searchcode是一款免费的源代码/文档搜索引擎，项目创建者为Ben Boyter。这里汇聚了Github、Bitbucket、Google Code、Codeplex、Sourceforge、Fedora Project等多家开源站点，拥有超过20万个项目、180亿行源代码，能以特殊字符、语言、仓库和源方式从90多种语言找到函数、API的真实代码。基于此，程序员可以搜索数百亿行软件代码，从中找寻编码技巧。果断Mark！
```


<!-- more -->
