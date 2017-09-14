title: js拖拽(drag)相关
date: 2017-3-24 00:00:00
tags: [ drag ]


---


# jquery
`Draggable | jQuery UI`
http://jqueryui.com/draggable/


---
# 插件支持
 - drag

```
$(".layer-map_back").on('mousedown', '.box', function (e) {
  $.extend(document, {'move': true, 'move_target': this}); // 实现拖拽移动
});
```




`jQuery实现平面图区域标记`

http://www.jq22.com/yanshi7677


`以拖拽方式在图片上添加热链接 - jQ酷`
http://www.jqcool.net/picaddlink.html


`js实现简单拖拽功能` `跨区域`
http://www.jq22.com/jquery-info11854


---
# 纯js实现（思路）
`JavaScript实现最简单的拖拽效果 « 张鑫旭-鑫空间-鑫生活`

http://www.zhangxinxu.com/wordpress/2010/03/javascript%E5%AE%9E%E7%8E%B0%E6%9C%80%E7%AE%80%E5%8D%95%E7%9A%84%E6%8B%96%E6%8B%BD%E6%95%88%E6%9E%9C/


`简单的鼠标拖拽效果（感谢张鑫旭的笔记，看完后思路清晰多了）`
http://www.qdfuns.com/notes/19044/b3c57594c1f1b220f1a102d4e9147828.html


---
```
//鼠标按下
div1.onmousemove = function() {
//鼠标移动
document.onmousemove = function(){
//鼠标松开
document.onmouseup = function(){

}
  }
}
```
- 完整
```
//obj需要拖拽的元素
//parent被拖拽元素的父级,用于现在拖出父级区域
//adsorption边缘自动吸附，传入一个数字，如果不传参数表示不自动吸附
function setDrag(obj, parent, adsorption) {
        //定义元素的X，Y轴的位置
        var objX = 0,
        objY = 0;
        //吸附值判断
        if (adsorption == undefined) {
                adsorption = 0;
        }
        //鼠标按下
        obj.onmousedown = function(ev) {
                var oEvnet = ev || event;
                //用鼠标按下的鼠标坐标减去元素的两边的距离得出元素坐标
                objX = oEvnet.clientX - obj.offsetLeft;
                objY = oEvnet.clientY - obj.offsetTop;
                //鼠标移动
                document.onmousemove = function(ev) {
                        var oEvnet = ev || event,
                        //鼠标移动距离减去上面按下鼠标得出的元素距离得出实际需要移动的距离
                        var l = oEvnet.clientX - objX,
                        t = oEvnet.clientY - objY;
                        //防止元素被移除窗口可视区域，所以需要判断移动距离
                        if (l < adsorption) {
                                l = 0;
                        } else if (l > parent.clientWidth - obj.offsetWidth - adsorption) {
                                l = parent.clientWidth - obj.offsetWidth;
                        }
                        if (t < adsorption) {
                                t = 0;
                        } else if (t > parent.clientHeight - obj.offsetHeight - adsorption) {
                                t = parent.clientHeight - obj.offsetHeight;
                        }
                        obj.style.left = l + 'px';
                        obj.style.top = t + 'px';
                }
                //鼠标松开清除事件
                document.onmouseup = function() {
                        document.onmousemove = null;
                        document.onmouseup = null;
                }
        }
}
```


`javaScript  拖拽`-推酷搜索

http://www.tuicool.com/search?kw=javaScript++%E6%8B%96%E6%8B%BD&t=1


`原生javascript实现鼠标拖拽移动元素 - 推酷` ` ★ ★ `
http://www.tuicool.com/articles/EjIZJna


`使用 JavaScript 实现简单的拖拽 - 推酷`  ` ★ `
http://www.tuicool.com/articles/36Zjiaj



`Javascript 中的拖拽实现 - 推酷`
http://www.tuicool.com/articles/7ZzMbuA


`每天一个JavaScript实例-html5拖拽 - 推酷`
http://www.tuicool.com/articles/Y3QZvy6
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/52189036.jpg)


---


`说说 HTML 中的dropEffect 和 effectAllowed`

http://blog.csdn.net/gggg5643/article/details/52135824


---

