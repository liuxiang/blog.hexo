title: flex、table、fixed布局差异
date: 2016-08-24 00:00:00
tags: [flex/table/fixed布局差异]


---
# 差异
`display: flex;` 弹性布局,可直接操作宽度控制等分
`display: table;` 表格布局,不可直接操作宽度,盒子宽度会被内容撑开. (可使用min-width,max-width辅助)

` table-layout:fixed; ` 弹性表格,需要设置自身宽度,才能再对子集设置宽度


# 解决问题
在保留边框的情况下，仅内容垂直居中。
- 使用 `display: table;`


`display: table;`中内容不冲破（撑开）父容器，内容支持换行。 
-  （使用min-width,max-width辅助） 或 （使用 ` table-layout:fixed; `）


# 示例效果 & 代码
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/92840580.jpg)
<!--
-->

-  html
```
   <div class="test_panel">
      <div class="test">aaa</div>
      <div class="test">bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb</div>
      <div class="test">ccc</div>
      <div class="test">ddd</div>
      <div class="test">eee</div>
    </div>
 
    <div class="test_panel_table">
      <div class="test_table">aaa</div>
      <div class="test_table">bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb</div>
      <div class="test_table">ccc</div>
      <div class="test_table">ddd</div>
      <div class="test_table">eee</div>
    </div>
 
    <table class="test_panel_table2">
      <tr>
        <td class="test_table2">aaa</td>
        <td class="test_table2">bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb</td>
        <td class="test_table2">ccc</td>
        <td class="test_table2">ddd</td>
        <td class="test_table2">eee</td>
      </tr>
    </table>
 
    <table class="fixed">
      <tr>
        <td>text</td>
        <td>text</td>
        <td>text</td>
      </tr>
    </table>
```
- css(需要虽小屏幕才能看到效果-因为没设置基础宽度)
```
  .test_panel{
    display: flex;
    align-items: center; /* 与子 级 margin: auto;效果一样.取其一 */


    .test{
      flex:1;
      text-align: center;
      //width:20vw;
      //width:20%;
      overflow: hidden;
      justify-content: center;
      flex-direction: column;
      border:1px solid blue;
       margin: auto; /* 与父级   align-items: center ;效果一样.取其一 */
    }
  }
 
  .test_panel_table{
    display: table;
 
    .test_table{
      display: table-cell;
      width:1%;
      text-align: center;
      border:1px solid blue;
      vertical-align:middle;
 
      /* 代替width:20vw */
      max-width: 20vw;
      min-width: 20vw;
    }
  }
 
  .test_panel_table2{
    table-layout: fixed;
    width: 100vw;
 
    .test_table2{
      overflow: hidden;
      width:20%;
      border:1px solid blue;
      text-align: center;
      vertical-align:middle;
    }
  }
 
  table.fixed {table-layout:fixed; width:90px;}/*Setting the table width is important!*/
  table.fixed td {overflow:hidden;border:1px solid blue;}/*Hide text outside the cell.*/
  table.fixed td:nth-of-type(1) {width:20px;}/*Setting the width of column 1.*/
  table.fixed td:nth-of-type(2) {width:30px;}/*Setting the width of column 2.*/
  table.fixed td:nth-of-type(3) {width:40px;}/*Setting the width of column 3.*/
```


---


`Why is width: 100% not working on div {display: table-cell}? - table的属性~蛮特别 - 十五言`
http://www.15yan.com/story/5dwn4VmtPQM/


`html - Fixed Table Cell Width - Stack Overflow`
http://stackoverflow.com/questions/4185814/fixed-table-cell-width


