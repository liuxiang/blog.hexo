title: Web API文档生成工具apidoc
date: 2016-04-16 02:00:00
categories:  api文档生成工具
tags: [api文档生成工具]


---

`官网`  http://apidocjs.com/
`github` https://github.com/apidoc/apidoc



# 安装 & 使用
1.`npm install apidoc -g`



2.`控制台运行>` ` apidoc `



`运行错误`
```
warn: Please create an apidoc.json configuration file.
error: No files found.
{ Path: '.' }
```


3.`新建空白文件夹->新建文件` `apidoc.json`
```
{
  "name": "example",
  "version": "0.1.0",
  "description": "apiDoc basic example",
  "title": "Custom apiDoc browser title",
  "url" : "https://api.github.com/v1"
}
```


`运行错误`
```
error: No files found.
{ Path: '.' }
```


4.`新建api内容描述文件` ` example.js `
```
/**
* @api {get} /user/:id Request User information
* @apiName GetUser
* @apiGroup User
*
* @apiParam {Number} id Users unique ID.
*
* @apiSuccess {String} firstname Firstname of the User.
 * @apiSuccess {String} lastname  Lastname of the User.
*
* @apiSuccessExample Success-Response:
 *     HTTP/1.1 200 OK
 *     {
 *       "firstname": "John",
 *       "lastname": "Doe"
 *     }
*
* @apiError UserNotFound The id of the User was not found.
*
* @apiErrorExample Error-Response:
 *     HTTP/1.1 404 Not Found
 *     {
 *       "error": "UserNotFound"
 *     }
*/
```


5.`控制台运行>` ` apidoc `
`info: Done.`


6.`正常输出静态目录` `doc`
打开 `index.html`
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-5-5/59379577.jpg)


# 应用 & 技巧


<!-- more -->
