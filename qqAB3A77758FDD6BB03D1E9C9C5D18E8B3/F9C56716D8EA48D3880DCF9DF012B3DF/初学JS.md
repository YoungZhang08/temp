# JS在页面中的位置
### JS代码可放在html文件中任何位置，但是我们一般放在网页的head或者body部分。
## ==放在<head>部分==
#### 最常用的方式是在页面中head部分防止<script>元素，浏览器解析head部分就会执行这个代码，然后才解析页面的其余部分。
## ==放在body部分==
#### JS代码在网页读取到该语句的时候就会执行。
## - [注意] ：JS作为一种脚本语言可以放在html页面中的任何位置，但是浏览器解释html时是按先后顺序的，所以前面的script就被先执行。比如进行页面显示初始化的JS必须放在head里面，因为初始化都要求提前进行（比如页面body设置CSS等);而如果是通过事件调用执行的function那么对位置没什么要求的。


# 函数
### 基本语法：

```
   function 函数名()
   {
        函数代码;
    }
```
#### 说明：
1. function定义函数的关键字。
2. “函数名”是你为函数取的名字。
3. “函数代码”替换为完成特定功能的代码。


# 输出内容（document.write）
1. 输出内容用“”括起，直接输出“”号内的内容。
2. 通过变量，输出内容。
3. 输出多项内容，内容之间用+号连接。
4. 输出HTML标签，并起作用，标签使用“”括起来。


# 确认(confirm消息对话框）
### confirm消息对话框常用于允许用户做选择的动作。
#### 语法：confirm(str); // str:在消息对话框中要显示的文本  返回值：Boolean值。当点击确定时，返回ture；当点击取消时，返回false。


# 提问（prompt消息对话框）
### 弹出消息对话框，通常用于询问一些需要与用户交互的信息。
#### 语法：prompt(str1,str2); //str1:要显示在消息对话框中的文本，不可修改;  str2：文本框中的内容，可以修改;    返回值：点击确定按钮，文本框中的内容作为函数的返回值；点击取消按钮，将返回null。

# 打开新窗口（window.open）
### open()方法可以查找一个已经存在或者新建的浏览器窗口。
#### 语法：window.open([URL],[窗口名称],[参数字符串])
### 窗口名称
1. "_top"：框架网页中在上部窗口中显示目标网页
2. "_blank"：在新窗口显示目标网页
3. "_self"：在当前网页中的上部窗口中显示目标网页

### 参数
参数 | 值 | 说明
---|--- | --- | ---
top | Number | 窗口顶部离开屏幕顶部的像素数
left | Number | 窗口左端离开屏幕左端的像素数
width | Number | 窗口的宽度
height | Number | 窗口的高度
menubar | yes,no | 窗口有没有菜单
toolbar | yes,no | 窗口有没有工具条
scrollbars | yes,no | 窗口有没有滚动条
status | yes,no | 窗口有没有状态栏

# 关闭窗口（window.close）
#### 用法：window.close();或<窗口对象>.close();

# 认识DOM
### （Document Object Model文档对象模型） DOM将HTML文档呈现为带有元素、属性和文本的树结构（节点树）
#### HTML文档可以说是由节点构成的集合，==三种常见的DOM节点==：
1. 元素节点：<html>,<head>,<body>,<p>等都是元素节点，即标签;
2. 文本节点：向用户展示的内容，如<li></li>中的JavaScript、DOM、CSS等文本。
3. 属性节点：元素属性，标签的属性（如<a>标签的href属性）

### 通过document.getElementById()方法获取元素（==注：获取的元素是一个对象，如果想对元素进行操作，要通过它的属性或方法==）

# innerHTML属性
### (用于获取或替换HTML元素的内容)
### 语法：Object.innerHTML(==注：Object是获取的元素对象，如通过document.getElementById()获取的元素==)

# 改变HTML样式
### 语法：Object.style.property=new style;

### 基本属性表（部分）

属性| 描述
---|---
backgroundColor | 设置元素背景色
height| 设置元素高度
width | 设置元素宽度
color | 设置元素颜色
font | 在一行设置所有的字体属性
font-family | 设置元素字体系列
font-size | 设置元素大小

# 显示和隐藏(display属性)
### 语法：Object.style.display = value;
### (value=none(隐藏)/block(显示))

# 控制类名(className属性)
##### className属性设置或返回元素的class属性
### 语法：object.className=classname;
### 作用：
1. 获取元素的class属性
2. 为网页内的某个元素指定一个css样式来更改元素外观