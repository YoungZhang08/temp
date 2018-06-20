### 学习web三月之久，结合自己的布局经历，试问大家是不是也有过本要把自己想放的东西放到自己想放的位置但是却不能如愿，那么下来我就自己的理解和各大博客和微博上比较常用的方法分享。

# ==居中是什么==？
### 在css中居中就是指元素、文本、图片等相对其容器或父元素或浏览器窗口能够居中显示。而根据显示方式的不同，又分为水平居中，垂直居中，水平垂直居中。

#### 每一种居中的解决方式也是不同的，因为元素的存在方式是不同的，有的是块级元素有的是行内元素。我们先看水平居中：

# ==水平居中==
### 行内元素（文本，链接）——在其父元素的CSS样式中设置++text-align：center++，但是，++父元素必须是块级元素++！

## html部分
```
<div id="container">
    <p>我爱学习！</p>
    <a href="www.baidu.com">百度</a>
</div>
```
## css部分

```
*{
    font-size: 30px;
    padding: 0;
    margin: 0;
}
#container{
/*先给父元素居中效果看起来较明显*/
    margin: 0 auto;  
    width: 200px;
    height: 200px;
    background-color: lightblue;
    text-align: center;
}
a{
    text-decoration: none;
    color: red;
}
```

### 这种方法对display设置为inline、inline-block、inline-table、inline-flex的元素都有效。

### 块级元素（照片以及自设为块级元素等）——
### 1. 对于一个==定宽的非浮动块级元素==可以通过设置其margin属性，来达到水平居中的效果。你可以==将其margin-left和margin-right设置为auto==：

### html部分
```
<div id="container">
    <div id="middle">
        aaa
    </div>
</div>
```
### css部分

```
*{
    font-size: 50px;
    padding: 0;
    margin: 0;
}
#container{
    width: 300px;
    height: 300px;
    background-color: lightblue;
    margin: 0 auto;
}
#middle{
    width:200px;
    height: 200px; 
    background-color: lightgreen;
    margin: 0 auto;
}
```

### 在这个例子中父元素container以及其子元素middle均为块级元素，会根据元素的宽度自动为元素左右两边设定等宽的margin。

### 2.对于不定宽的浮动元素也可以采用下面的方法给它水平居中：

### html部分

```
<div id="container">
    <p>我是浮动的</p>
    <p>我也是居中的</p>
</div>
```
### css部分

```
*{
    font-size: 50px;
    padding: 0;
    margin: 0;
}
#container{
    float:left;
    position:relative;
    background-color: lightblue;
    left:50%;
}
p{
    float:left;
    position:relative;
    background-color: lightgreen;
    right:50%;
}
```
### 这个例子中，==父元素和子元素同时向左浮动==，然后==父元素相对于自身的位置向左移动50%==，再然后==子元素相对其父元素向右移动50%==，或者子元素相对左移动-50%。

### 3.多个块级元素的水平居中——==给每个小块均设置display：inline-block==
### html部分

```
<div id="container">
    container
    <div>a</div>
    <div>b</div>
    <div>c</div>
    <div>d</div>
</div>
```
### css部分

```
*{
    font-size: 50px;
    padding: 0;
    margin: 0;
}
#container{
    display: inline-block;
/*由于inline的缘故，可以在父元素设置text-align:center将多个块元素居中 */
    text-align: center;
    width: 100%;
    height: 300px;
    background-color: lightblue;
}
div{
    width: 200px;
    height: 200px;
    background-color: lightgreen;
}
```
# ==垂直居中==
### 行内元素（单行）——

#### 1.当父元素没有设定宽度，而是根据内容自适应时，简单的设置padding就可以达到垂直居中的效果
### html部分

```
<div id="container">
    <p>我爱学习！</p>
</div>
```
### css部分

```
*{
    font-size: 30px;
    padding: 0;
    margin: 0;
    border: 0;
}
#container{
    width: 400px;
    background-color: lightblue;
    padding: 80px;
}
p{
    padding: 30px;
}
```
#### 2.单行文本居中只需要简单地把 想要居中的元素的css属性中line-height 设置为 height 值就可以了。

### 行内元素（多行）——

#### 1.也可以如上述单行文本里头的第一个方法使用padding实现。

#### 2.出现表格单元格的话padding就会失效，这时候需要使用vertical-align属性。

### html部分

```
<table border="1"> 
    <tr>
    	<td>1</td>
    	<td>2</td>
    </tr>
    <tr>
    	<td>3</td>
    	<td>4</td>
    </tr>
    <tr>
    	<td>5</td>
    	<td>6</td>
    </tr>
</table>

```
### css部分

```
*{
    font-size: 30px;
    padding: 0;
    margin: 0;
}
table{
    width: 100px;
    height: 300px;
/*height有没有设定都可以使用*/
}
tr{
    vertical-align: middle;
}
```
### 块级元素——

#### 1.当父元素没有设定宽度，而是根据内容自适应时，简单的设置padding就可以达到垂直居中的效果

#### 2.父元素高度已知,可以通过css3的一个属性来实现，如下：

### html部分

```
<div id="container">
    <div id="child">我爱学习！
    </div>
</div>
```
### css部分

```
*{
    font-size: 30px;
    padding: 0;
    margin: 0;
}
#container{
    height: 600px;
    position: relative;
    background-color: lightblue;
}
#child{
    background-color: lightgreen;
    position: absolute;
    top: 50%;
    width: 200px;
    height: 200px;
    transform: translateY(-50%);
}
```
### 上述代码是通过定位的方式，来实现垂直居中，==子元素参照父元素进行绝对定位，相对于父元素的上边缘向下移动50%（父元素高度的50%），然后通过margin-top元素将子元素向上拉自身的50%==，达到最终的居中。

# ==水平垂直居中==

### 方法一（已知父元素和子元素高度）：

### html部分

```
<div id="container">
    <div id="child">我爱学习！
    </div>
</div>
```

### css部分

```
*{
    font-size: 30px;
    padding: 0;
    margin: 0;
}
#container{
    /*width: 600px;*/
    /*可以有也可以没有*/
    height: 600px;
    position: relative;
    background-color: lightblue;
}
#child{
    background-color: lightgreen;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    width: 50%;
    height: 30%;
    margin: auto;
}
```
### 方法二（父元素高度已知，子元素宽高未知）：

### html部分

```
<div id="container">
    <div id="child">我爱学习！
    </div>
</div>
```
### CSS部分

```
*{
    font-size: 30px;
    padding: 0;
    margin: 0;
}
#container{
    height: 200px;
    position: relative;
    background-color: lightblue;
}
#child{
    background-color: lightgreen;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
```

### 方法三（flexbox,父元素高度已知,子元素宽高随意）

### html部分

```
<div id="container">
    <div id="child">我爱学习！
    </div>
</div>
```

### CSS部分

```
*{
    font-size: 30px;
    padding: 0;
    margin: 0;
}
#container{
    height: 500px;
    background-color: gray;
    display: flex;
    justify-content: center;
    align-items: center;
}
#child{
    background-color: lightgreen;
    
}
```




























