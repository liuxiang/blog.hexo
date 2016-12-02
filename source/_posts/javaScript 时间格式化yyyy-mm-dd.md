title: javaScript 时间格式化yyyy-mm-dd
date: 2016-04-1 00:00:00
categories:  javaScript 时间格式化
tags: [ javaScript 时间格式化 ]


---


# 通用

```
// 对Date的扩展，将 Date 转化为指定格式的String  
// 月(M)、日(d)、小时(h)、分(m)、秒(s)、季度(q) 可以用 1-2 个占位符，  
// 年(y)可以用 1-4 个占位符，毫秒(S)只能用 1 个占位符(是 1-3 位的数字)  
// 例子：  
// (new Date()).Format("yyyy-MM-dd hh:mm:ss.S") ==> 2006-07-02 08:09:04.423  
// (new Date()).Format("yyyy-M-d h:m:s.S")      ==> 2006-7-2 8:9:4.18  
Date.prototype.Format = function(fmt)  
{ //author: meizz  
  var o = {  
    "M+" : this.getMonth()+1,                 //月份  
    "d+" : this.getDate(),                    //日  
    "h+" : this.getHours(),                   //小时  
    "m+" : this.getMinutes(),                 //分  
    "s+" : this.getSeconds(),                 //秒  
    "q+" : Math.floor((this.getMonth()+3)/3), //季度  
    "S"  : this.getMilliseconds()             //毫秒  
  };  
  if(/(y+)/.test(fmt))  
    fmt=fmt.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length));  
  for(var k in o)  
    if(new RegExp("("+ k +")").test(fmt))  
  fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));  
  return fmt;  
}


console.log(new Date().Format("yyyy-MM-dd hh:mm:ss"));

```
# 工具
`Moment.js` http://momentjs.com/docs/

```

moment("2010-10-20 4:30",       "YYYY-MM-DD HH:mm");   // parsed as 4:30 local time
moment("2010-10-20 4:30 +0000", "YYYY-MM-DD HH:mm Z"); // parsed as 4:30 UTC
```


# 自定义工具 dateUtils.js
```

/* 日期时间脚本库方法列表  
 Date.prototype.isLeapYear 判断闰年  
 Date.prototype.FormatString 日期格式化  
 Date.prototype.DateAdd 日期计算  
 Date.prototype.DateDiff 比较日期差  
 Date.prototype.toString 日期转字符串  
 Date.prototype.toArray 日期分割为数组  
 Date.prototype.DatePart 取日期的部分信息  
 Date.prototype.MaxDayOfDate 取日期所在月的最大天数  
 Date.prototype.WeekNumOfYear 判断日期所在年的第几周  
 StringToDate 字符串转日期型  
 IsValidDate 验证日期有效性  
 CheckDateTime 完整日期时间检查  
 daysBetween 日期天数差   
 
 js 代码  */
```
```
//---------------------------------------------------
// 判断闰年
//---------------------------------------------------
Date.prototype.isLeapYear = function()
{
    return (0==this.getYear()%4&&((this.getYear()%100!=0)||(this.getYear()%400==0)));
}
 
//---------------------------------------------------
// 日期格式化
// 格式 YYYY/yyyy/YY/yy 表示年份
// MM/M 月份
// W/w 星期
// dd/DD/d/D 日期
// hh/HH/h/H 时间
// mm/m 分钟
// ss/SS/s/S 秒
//---------------------------------------------------
Date.prototype.Format = function(fmt)
{
    var o = {
        "M+" : this.getMonth()+1,                 //月份
        "d+" : this.getDate(),                    //日
        "h+" : this.getHours(),                   //小时
        "m+" : this.getMinutes(),                 //分
        "s+" : this.getSeconds(),                 //秒
        "q+" : Math.floor((this.getMonth()+3)/3), //季度
        "S"  : this.getMilliseconds()             //毫秒
    };
    if(/(y+)/.test(fmt))
        fmt=fmt.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length));
    for(var k in o)
        if(new RegExp("("+ k +")").test(fmt))
            fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));
    return fmt;
}
 
//+---------------------------------------------------
//| 求两个时间的天数差 日期格式为 YYYY-MM-dd
//+---------------------------------------------------
function daysBetween(DateOne,DateTwo)
{
    var OneMonth = DateOne.substring(5,DateOne.lastIndexOf ('-'));
    var OneDay = DateOne.substring(DateOne.length,DateOne.lastIndexOf ('-')+1);
    var OneYear = DateOne.substring(0,DateOne.indexOf ('-'));
 
    var TwoMonth = DateTwo.substring(5,DateTwo.lastIndexOf ('-'));
    var TwoDay = DateTwo.substring(DateTwo.length,DateTwo.lastIndexOf ('-')+1);
    var TwoYear = DateTwo.substring(0,DateTwo.indexOf ('-'));
 
    var cha=((Date.parse(OneMonth+'/'+OneDay+'/'+OneYear)- Date.parse(TwoMonth+'/'+TwoDay+'/'+TwoYear))/86400000);
    return Math.abs(cha);
}
 
 
//+---------------------------------------------------
//| 日期计算
//+---------------------------------------------------
Date.prototype.DateAdd = function(strInterval, Number) {
    var dtTmp = this;
    switch (strInterval) {
        case 's' :return new Date(Date.parse(dtTmp) + (1000 * Number));
        case 'n' :return new Date(Date.parse(dtTmp) + (60000 * Number));
        case 'h' :return new Date(Date.parse(dtTmp) + (3600000 * Number));
        case 'd' :return new Date(Date.parse(dtTmp) + (86400000 * Number));
        case 'w' :return new Date(Date.parse(dtTmp) + ((86400000 * 7) * Number));
        case 'q' :return new Date(dtTmp.getFullYear(), (dtTmp.getMonth()) + Number*3, dtTmp.getDate(), dtTmp.getHours(), dtTmp.getMinutes(), dtTmp.getSeconds());
        case 'm' :return new Date(dtTmp.getFullYear(), (dtTmp.getMonth()) + Number, dtTmp.getDate(), dtTmp.getHours(), dtTmp.getMinutes(), dtTmp.getSeconds());
        case 'y' :return new Date((dtTmp.getFullYear() + Number), dtTmp.getMonth(), dtTmp.getDate(), dtTmp.getHours(), dtTmp.getMinutes(), dtTmp.getSeconds());
    }
}
 
//+---------------------------------------------------
//| 比较日期差 dtEnd 格式为日期型或者 有效日期格式字符串
//+---------------------------------------------------
Date.prototype.DateDiff = function(strInterval, dtEnd) {
    var dtStart = this;
    if (typeof dtEnd == 'string' )//如果是字符串转换为日期型
    {
        dtEnd = StringToDate(dtEnd);
    }
    switch (strInterval) {
        case 's' :return parseInt((dtEnd - dtStart) / 1000);
        case 'n' :return parseInt((dtEnd - dtStart) / 60000);
        case 'h' :return parseInt((dtEnd - dtStart) / 3600000);
        case 'd' :return parseInt((dtEnd - dtStart) / 86400000);
        case 'w' :return parseInt((dtEnd - dtStart) / (86400000 * 7));
        case 'm' :return (dtEnd.getMonth()+1)+((dtEnd.getFullYear()-dtStart.getFullYear())*12) - (dtStart.getMonth()+1);
        case 'y' :return dtEnd.getFullYear() - dtStart.getFullYear();
    }
}
 
//+---------------------------------------------------
//| 日期输出字符串，重载了系统的toString方法
//+---------------------------------------------------
Date.prototype.toString = function(showWeek)
{
    var myDate= this;
    var str = myDate.toLocaleDateString();
    if (showWeek)
    {
        var Week = ['日','一','二','三','四','五','六'];
        str += ' 星期' + Week[myDate.getDay()];
    }
    return str;
}
 
//+---------------------------------------------------
//| 日期合法性验证
//| 格式为：YYYY-MM-DD或YYYY/MM/DD
//+---------------------------------------------------
function IsValidDate(DateStr)
{
    var sDate=DateStr.replace(/(^\s+|\s+$)/g,''); //去两边空格;
    if(sDate=='') return true;
//如果格式满足YYYY-(/)MM-(/)DD或YYYY-(/)M-(/)DD或YYYY-(/)M-(/)D或YYYY-(/)MM-(/)D就替换为''
//数据库中，合法日期可以是:YYYY-MM/DD(2003-3/21),数据库会自动转换为YYYY-MM-DD格式
    var s = sDate.replace(/[\d]{ 4,4 }[\-/]{ 1 }[\d]{ 1,2 }[\-/]{ 1 }[\d]{ 1,2 }/g,'');
    if (s=='') //说明格式满足YYYY-MM-DD或YYYY-M-DD或YYYY-M-D或YYYY-MM-D
    {
        var t=new Date(sDate.replace(/\-/g,'/'));
        var ar = sDate.split(/[-/:]/);
        if(ar[0] != t.getYear() || ar[1] != t.getMonth()+1 || ar[2] != t.getDate())
        {
//alert('错误的日期格式！格式为：YYYY-MM-DD或YYYY/MM/DD。注意闰年。');
            return false;
        }
    }
    else
    {
//alert('错误的日期格式！格式为：YYYY-MM-DD或YYYY/MM/DD。注意闰年。');
        return false;
    }
    return true;
}
 
//+---------------------------------------------------
//| 日期时间检查
//| 格式为：YYYY-MM-DD HH:MM:SS
//+---------------------------------------------------
function CheckDateTime(str)
{
    var reg = /^(\d+)-(\d{ 1,2 })-(\d{ 1,2 }) (\d{ 1,2 }):(\d{ 1,2 }):(\d{ 1,2 })$/;
    var r = str.match(reg);
    if(r==null)return false;
    r[2]=r[2]-1;
    var d= new Date(r[1],r[2],r[3],r[4],r[5],r[6]);
    if(d.getFullYear()!=r[1])return false;
    if(d.getMonth()!=r[2])return false;
    if(d.getDate()!=r[3])return false;
    if(d.getHours()!=r[4])return false;
    if(d.getMinutes()!=r[5])return false;
    if(d.getSeconds()!=r[6])return false;
    return true;
}
 
//+---------------------------------------------------
//| 把日期分割成数组
//+---------------------------------------------------
Date.prototype.toArray = function()
{
    var myDate = this;
    var myArray = Array();
    myArray[0] = myDate.getFullYear();
    myArray[1] = myDate.getMonth();
    myArray[2] = myDate.getDate();
    myArray[3] = myDate.getHours();
    myArray[4] = myDate.getMinutes();
    myArray[5] = myDate.getSeconds();
    return myArray;
}
 
//+---------------------------------------------------
//| 取得日期数据信息
//| 参数 interval 表示数据类型
//| y 年 m月 d日 w星期 ww周 h时 n分 s秒
//+---------------------------------------------------
Date.prototype.DatePart = function(interval)
{
    var myDate = this;
    var partStr='';
    var Week = ['日','一','二','三','四','五','六'];
    switch (interval)
    {
        case 'y' :partStr = myDate.getFullYear();break;
        case 'm' :partStr = myDate.getMonth()+1;break;
        case 'd' :partStr = myDate.getDate();break;
        case 'w' :partStr = Week[myDate.getDay()];break;
        case 'ww' :partStr = myDate.WeekNumOfYear();break;
        case 'h' :partStr = myDate.getHours();break;
        case 'n' :partStr = myDate.getMinutes();break;
        case 's' :partStr = myDate.getSeconds();break;
    }
    return partStr;
}
 
//+---------------------------------------------------
//| 取得当前日期所在月的最大天数
//+---------------------------------------------------
Date.prototype.MaxDayOfDate = function()
{
    var myDate = this;
    var ary = myDate.toArray();
    var date1 = (new Date(ary[0],ary[1]+1,1));
    var date2 = date1.dateAdd(1,'m',1);
    var result = dateDiff(date1.Format('yyyy-MM-dd'),date2.Format('yyyy-MM-dd'));
    return result;
}
 
//+---------------------------------------------------
//| 取得当前日期所在周是一年中的第几周
//+---------------------------------------------------
Date.prototype.WeekNumOfYear = function()
{
    var myDate = this;
    var ary = myDate.toArray();
    var year = ary[0];
    var month = ary[1]+1;
    var day = ary[2];
    document.write('< script language=VBScript\> \n');
    document.write("myDate = DateValue(''+month+'-'+day+'-'+year+'') \n");
    document.write("result = DatePart('ww', myDate) \n");
    document.write(' \n');
    return result;
}
 
//+---------------------------------------------------
//| 字符串转成日期类型
//| 格式 MM/dd/YYYY MM-dd-YYYY YYYY/MM/dd YYYY-MM-dd
//+---------------------------------------------------
function StringToDate(DateStr)
{
 
    var converted = Date.parse(DateStr);
    var myDate = new Date(converted);
    if (isNaN(myDate))
    {
//var delimCahar = DateStr.indexOf('/')!=-1?'/':'-';
        var arys= DateStr.split('-');
        myDate = new Date(arys[0],--arys[1],arys[2]);
    }
    return myDate;
}
```


**参考**
`jsystem_config/dateUtils.js at 781113cb696fa78140be09d387dc930765d80f79 · lynnchae/jsystem_config`
https://github.com/lynnchae/jsystem_config/blob/781113cb696fa78140be09d387dc930765d80f79/mobanker-config-parent/backStageWeb/src/main/webapp/static/mobanker/js/dateUtils.js


`QmlExplorer/date.js at ae9c040e7fc3add1edf84fb87a01df3ca5f4ca1b · surfsky/QmlExplorer`
https://github.com/surfsky/QmlExplorer/blob/ae9c040e7fc3add1edf84fb87a01df3ca5f4ca1b/QmlExplorer/Js/date.js


`basic/Util.js at c86e3cb5135844310517a66092f43b018f2a5133 · wodezbf/basic`
https://github.com/wodezbf/basic/blob/c86e3cb5135844310517a66092f43b018f2a5133/WebContent/js/util/Util.js



---


# 高级篇
##  IE
假如要兼容 IE6+，通常会这么写。
 
```
function pad(s) { // 补零
  return ('0' + s).slice(-2);
}
 
var dt = new Date();
var date = dt.getFullYear() + '-' + pad(dt.getMonth() + 1) + '-' + pad(dt.getDate());
date += ' ';
date += pad(dt.getHours()) + ':' + pad(dt.getMinutes()) + ':' + pad(dt.getSeconds());


console.log(date); // => 2016-03-25 11:01:01
```
 

大神的文章里是这么写的。
```
var dt = new Date();
var date = [
  [dt.getFullYear(), dt.getMonth() + 1, dt.getDate()].join('-'),
  [dt.getHours(), dt.getMinutes(), dt.getSeconds()].join(':')
].join(' ').replace(/(?=\b\d\b)/g, '0'); // 正则补零 (略微改动)
 
console.log(date); // => 2016-03-25 11:01:01
```


# # 现代浏览器
假如是 IE9+ 或现代浏览器，那就方便多了。
```
var dt = new Date();
dt.setMinutes(dt.getMinutes() - dt.getTimezoneOffset()); // 修正时区偏移
var date = dt.toISOString().slice(0, -5).replace(/[T]/g, ' ');
 
console.log(date); // => 2016-03-25 11:01:01
```


**参考**
`js 生成 yyyy-mm-dd 格式的逼格姿势`

http://www.tuicool.com/articles/jiY3EfJ


---
# 打开浏览器-F12-控制台


- 当前时间
```
Date.now();// 1477644601270
```


- 依据时间
```
new Date("2016-10-28 11:05:00").getTime();// 1477623900000
```


- 验证
```
var dt = new Date(1477623900000);
dt.setMinutes(dt.getMinutes() - dt.getTimezoneOffset()); // 修正时区偏移
var date = dt.toISOString().slice(0, -5).replace(/[T]/g, ' ');
console.log(date); // => 2016-10-28 11:05:00
```
<!-- more -->
