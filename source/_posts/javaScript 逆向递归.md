title: javaScript 逆向递归
date: 2016-03-19 00:00:00
categories: javaScript
tags: [ javaScript, 逆向 递归 ]


---


# 处理前数据
```
[
    {
        "bookId": 12,
        "code": "1",
        "id": 56,
        "lever": 0,
        "name": "九年级上（科学）"
    },
    {
        "bookId": 12,
        "code": "1#1",
        "id": 58,
        "lever": 1,
        "name": "生物",
        "parent_id": 56
    },
    {
        "bookId": 12,
        "code": "1#1#1",
        "id": 58,
        "lever": 2,
        "name": "生物-子集",
        "parent_id": 56
    },
    {
        "bookId": 12,
        "code": "1#1#1#1",
        "id": 58,
        "lever": 3,
        "name": "生物-子集-子集",
        "parent_id": 56
    },
    {
        "bookId": 12,
        "code": "1#1#2",
        "id": 58,
        "lever": 2,
        "name": "生物-子集2",
        "parent_id": 56
    },
    {
        "bookId": 12,
        "code": "1#2",
        "id": 59,
        "lever": 1,
        "name": "化学",
        "parent_id": 56
    },
    {
        "bookId": 12,
        "code": "1#3",
        "id": 61,
        "lever": 1,
        "name": "物理",
        "parent_id": 56
    },
    {
        "bookId": 12,
        "code": "1#3#1",
        "id": 62,
        "lever": 2,
        "name": "相对运动学",
        "parent_id": 61
    },
    {
        "bookId": 12,
        "code": "1#2#1",
        "id": 63,
        "lever": 2,
        "name": "碳元素",
        "parent_id": 59
    }
]
```


# 处理后数据
```
[
    {
        "bookId": 12,
        "code": "1",
        "id": 56,
        "lever": 0,
        "name": "九年级上（科学）",
        "subset": [
            {
                "bookId": 12,
                "code": "1#1",
                "id": 58,
                "lever": 1,
                "name": "生物",
                "parent_id": 56,
                "subset": [
                    {
                        "bookId": 12,
                        "code": "1#1#1",
                        "id": 58,
                        "lever": 2,
                        "name": "生物-子集",
                        "parent_id": 56,
                        "subset": [
                            {
                                "bookId": 12,
                                "code": "1#1#1#1",
                                "id": 58,
                                "lever": 3,
                                "name": "生物-子集-子集",
                                "parent_id": 56
                            }
                        ]
                    },
                    {
                        "bookId": 12,
                        "code": "1#1#2",
                        "id": 58,
                        "lever": 2,
                        "name": "生物-子集2",
                        "parent_id": 56
                    }
                ]
            },
            {
                "bookId": 12,
                "code": "1#2",
                "id": 59,
                "lever": 1,
                "name": "化学",
                "parent_id": 56,
                "subset": [
                    {
                        "bookId": 12,
                        "code": "1#2#1",
                        "id": 63,
                        "lever": 2,
                        "name": "碳元素",
                        "parent_id": 59
                    }
                ]
            },
            {
                "bookId": 12,
                "code": "1#3",
                "id": 61,
                "lever": 1,
                "name": "物理",
                "parent_id": 56,
                "subset": [
                    {
                        "bookId": 12,
                        "code": "1#3#1",
                        "id": 62,
                        "lever": 2,
                        "name": "相对运动学",
                        "parent_id": 61
                    }
                ]
            }
        ]
    }
]
```


**格式化工具**  http://tool.oschina.net/codeformat/json


# 示例代码 ```
/**
* Created by Administrator on 2016/1/8.
*/
(function () {
  'use strict';
 
  {
    dataFormatTest();// 启动入口
  }
 
  function dataFormatTest(){
    var tempData = [{ "bookId": 12, "code": "1", "id": 56, "lever": 0, "name": "九年级上（科学）"}, {"bookId": 12, "code": "1#1", "id": 58, "lever": 1, "name": "生物", "parent_id": 56}, {"bookId": 12, "code": "1#1#1", "id": 58, "lever": 2, "name": "生物-子集", "parent_id": 56}, {"bookId": 12, "code": "1#1#1#1", "id": 58, "lever": 3, "name": "生物-子集-子集", "parent_id": 56}, {"bookId": 12, "code": "1#1#2", "id": 58, "lever": 2, "name": "生物-子集2", "parent_id": 56}, { "bookId": 12, "code": "1#2", "id": 59, "lever": 1, "name": "化学", "parent_id": 56}, {"bookId": 12, "code": "1#3", "id": 61, "lever": 1, "name": "物理", "parent_id": 56}, { "bookId": 12, "code": "1#3#1", "id": 62, "lever": 2, "name": "相对运动学", "parent_id": 61}, {"bookId": 12, "code": "1#2#1", "id": 63, "lever": 2, "name": "碳元素", "parent_id": 59}];
    var result = dataFormat(tempData);
    console.log(JSON.stringify(result));
  }
 
  function dataFormat(json) {
    console.log("json:"+JSON.stringify(json));
    var upCode;
    var jsonNew = [];
    for (var i = 0; i < json.length; i++) {
      console.log('json[i].lever):'+json[i].lever);
      console.log('json[0].lever:'+(json[0].lever));
      if(json[i].lever != json[0].lever)
      {
        console.log('continue');
        continue;
      }
 
      // 将当前层数据直接添加
      console.log('push:'+JSON.stringify(json[i]));
      jsonNew.push(json[i]);
 
      // 检查子集是否存在
      var jsonSnippets = snippets(json, i, json[i].code);
      if(jsonSnippets.length>0){
        // [递归]获取下一层子集数据
        var jsonresult = dataFormat(jsonSnippets);
 
        console.log("subset:" + JSON.stringify(jsonresult));
        jsonNew[jsonNew.length - 1]['subset'] = jsonresult;
 
        //if (jsonNew[jsonNew.length - 1]['subset'] instanceof Array) {
        //  console.log("1:" + JSON.stringify(jsonresult));
        //  jsonNew[jsonNew.length - 1]['subset'].push(jsonresult)
        //} else {
        //  console.log("2:" + JSON.stringify(jsonresult));
        //  jsonNew[jsonNew.length - 1]['subset'] = jsonresult;
        //}
      }
      upCode = json[i].code;
    }
    return jsonNew;
  }
 
  function snippets(json,beginIndex,code){
    var jsonSnippets=[];
    for (var i = beginIndex; i < json.length; i++) {
      if(json[i].code.startsWith(code+"#")){
        jsonSnippets.push(json[i]);
      }
    }
    console.log("sn:"+JSON.stringify(jsonSnippets));
    return jsonSnippets;
  }
 
 
})();
```


<!-- more -->


