Java Web 尚硅谷

[学习视频地址](https://www.bilibili.com/video/BV1Y7411K7zz)

## 一.html

基本规范

![image-20220126204658884](https://s2.loli.net/2022/05/01/IxaQJsFZf7jpEdv.png)

注释：

```html
<!-- -->
```

浏览器查看源码可见

## 标签

![](https://s2.loli.net/2022/05/01/oKrGxVyqjZNAEYW.png)

+ 单标签

![image-20220126205848457](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220126205848457.png)

```html
  <hr/> 水平线
```



+ 双标签

![image-20220126205908009](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220126205908009.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>hello</title>
</head>
<body onclick="alert('大佬')" bgcolor="#faebd7"><!-- 基本属性和事件属性-->
    hello
    <hr/>
<button onclick="alert('大佬')" > hello</button>
</body>
</html>
```

属性必须有值 必须加双引号

浏览器会帮你解释 当时还是要保证语法正确

### 1.字体标签

![image-20220126212211789](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220126212211789.png)

### 2. 字符实体

常用的特殊字符集

类似于转义字符

+ 在html 空白连续空格 只会显示一个

  那么就需要用多个 ```&nbsp; ```表示多个空格

### 3.标题标签

h1 到 h6

```html
	<h1 align="left">标题1</h1> <!-- 默认左对齐-->
    <h2 align="right">标题2</h2>
    <h3 align="center">标题3</h3>
    <h4>标题4</h4>
    <h5>标题5</h5>
    <h6>标题6</h6>
```

### 4.超链接

```html
<a href="https://www.baidu.com">百度</a><br/>
<a href="https://www.baidu.com" target="_self">百度</a><br/> <!--当前页面跳转 -->
<a href="https://www.baidu.com" target="_blank">百度</a> <!--新页面跳转 -->

target 还可以选定自己的区域 参考 8.iframe标签
```

必须加https://

错误：

![image-20220127171928670](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220127171928670.png)

### 5.列表标签

+ 有序列表

+ 无序列表

```html
	有序列表<ol></ol>
	无序列表<ul></ul>
	<li> abc <li>列表项  
```

```html
<body>
    <ul>
        <li>a</li>
        <li>b</li>
        <li>c</li>
        <li>d</li>
    </ul>
    
    <ol>
        <li>a</li>
        <li>b</li>
        <li>c</li>
        <li>d</li>
    </ol>
</body>
```

### 6. img标签

```html
<img src="" width="" heigth="">
```

src属性设置路径

##### 路径

在web中路径两种分为 

+ 相对路径

  .    表示当前文件所在的目录

  ..  表示当前文件所在的上一级目录

  文件名  表示当前文件所在的目录的文件 相当于./文件名 省略了./

+ 绝对路径

  格式：http://ip:port/工程名/资源路径



![image-20220127174410965](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220127174410965.png)

在javaSE也分为这两种

+ 相对路径

  从工程名开始算

+ 绝对路径

  盘符+文件名

```<img src="../img/img.png" width="500" heigth="500" border="1" alt="找不到图片">```

alt属性：找不到路径图片时的代替显示的文本

border属性：边框大小 默认0

### 7.表格

```htm
<table align="center" border="1" width="300" height="300" cellspacing="0">
    <tr>
        <td align="center"><b>1.1</b></td>
        <th>1.2</th>
        <th>1.3</th>
    </tr>
    <tr>
        <td>3.1</td>
        <td>2.2</td>
        <td>2.3</td>
    </tr>
    <tr>
        <td>3.1</td>
        <td>3.2</td>
        <td>3.3</td>
    </tr>
    </table>
```

```<th></th>```表头相当于``` <td align="center"><b>1.1</b></td>``` b加粗和居中

```cellspacing="0"```单元格间距为0

 ```html
 <body>
 <table align="center" border="1" width="300" height="300" cellspacing="0">
     <tr >
         <th colspan="2">1.1</th>
         <th>1.2</th>
 
     </tr>
     <tr>
         <td colspan="2">3.1</td>
         <td>2.2</td>
 
     </tr>
     <tr>
         <td rowspan="2">3.1</td>
         <td>3.2</td>
         <td>3.3</td>
     </tr>
     <tr>
         <td>4.2</td>
         <td>4.3</td>
     </tr>
     </table>
 </body>
 ```

跨行  和 跨列 可以对同一个td使用。

### 8.iframe框架标签（内嵌窗口）

在页面开辟一个小区域  比如b站pc端选集位置

```html
<body>
    我是一个单独的完整的页面<br/>
    <iframe src="demo标题.html" width="500" height="400"></iframe>
</body>
```

![image-20220127195900925](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220127195900925.png)

```html
<iframe src="demo标题.html" width="500" height="400" name="abc"></iframe>

<ul>
    <li ><a href="demo列表.html" target="abc">demo列表.html</a></li>
    <li ><a href="demo标题.html"  target="abc">demo标题.html</a></li>
    <li ><a href="表格.html"  target="abc" >表格.html</a></li>
</ul>
```

![image-20220127205127797](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220127205127797.png)

使用 name和target 联合， 改变iframe内容  

### 9.表单

收集信息 发送给服务器

 1.文本框
 ```htlm    
 <input type="text" value="默认值"><br>  
 ```
 2.密码框（掩码）
 ```htlm  
 <input type="password" value="默认值">  
 ```
 3.单选按钮 
 ```htlm
 	<input type="radio" name="sex" > 
	<input type="radio" name="sex"><br>	
 ```
 4.下拉选择
 ```htlm  
 		<select>  
		<option>abc</option>  
		<option>abc</option>  
		<option>abc</option>  
		</select><br>  
 ```
 5.复选框  
 ```htlm  
 		<input type="checkbox">abc   (abc 为举例)
		<input type="checkbox">abc  
		<input type="checkbox">abc<br>  
 ```
 6.文本域
 ```htlm  
 	<textarea row="5" cols="50">  
					abc  
	</textarea><br>  
 ```
 7.文件上传   
 ```htlm  
 	<input type="file">  
 ```
 8.提交按钮
 ```htlm  
 	<input type="submit" value="abc"><!--acb会呈现在按钮上 -->
 ```
 9.重置按钮
 ```htlm  
 	<input type="reset" value="abc">
 ```
 10.如何跳转网页
 ```htlm  
 	<form action="receive.html">
 	<input type="submit" value="注册">
 	</form>
 ```

![image-20220128092132365](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128092132365.png)

![image-20220128092723260](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128092723260.png)

11. 提交

+ get

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128094217694.png)

注意：

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128094251663.png)



区别：

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128094731598.png)

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128094627599.png)

### 10其他

			<p>这是一个段落</p>
			<div>这是一个块</div>
			<span>密码必须是6位</span>
			<span>用户名不为空</span>
			<label>这个是标签 </label>

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128095104991.png)

## 二.CSS

![image-20220128101128556](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128101128556.png)

+ 行内样式：放在标记的style属性里，行内样式优先级最高

```html
<body>
    <div style="border: 1px solid red;"> div标签1</div>
    <div style="border: 1px solid red;"> div标签2</div>
    <span style="border: 1px solid red;"> span标签1</span>
    <span> span标签2</span>
</body>
```

+ 页内样式：放在head之间，用style标记

  style 里的属于css 注释使用```/* 注释 */```

![image](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128100731831.png)

+ 外部样式：放在独立的.css文件中，在网页上用link标签引入

![image-20220128102639261](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128102639261.png)

+ CSS选择器（*****）

1. 标记（标签）选择器 如上
2. id选择器

![image-20220128103747597](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128103747597.png)3. class选择器

一个标签只能使用一个id选择器，但是可以使用多个class

ID和class的优先级不同，当你同时使用两个时，ID优先作用

![image-20220128104547374](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128104547374.png)

4. 组合选择器

![image-20220128112029056](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128112029056.png)

下图是 是这个类 然后  后代必须a标签， a的后代必须有span标签，满足所有条件的使用这个样式。

![image-20220203174903212](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203174903212.png)

## 三.JavaScript

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128112856807.png)

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128113005349.png)

### 1.特点
1.交互性(它可以做的就是信息的动态交互)
2.安全性 (不允许直接访问本地硬盘)
3.跨平台性(只要是可以解释JS的浏览器都可以执行，和操作系统无关)

### 2.使用方式

+ 使用script标签
```
<script type="text/javascript">
alert("hello javaScript")
</script>
```
+ 使用script标签 单独引入js代码文件 与css类似

![image-20220128114844337](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128114844337.png)

![注意](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128115018913.png)

可以再写一个script标签（先执行前面的）

### 3.变量

​		![image-20220128142315864](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128142315864.png)

![image-20220128142530473](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128142530473.png)

```html
<head>
    <meta charset="UTF-8">
    <title>变量</title>
    <script type="text/javascript">
       	var i;
        i=12;
        alert(typeof (i));
        i="asd";
        alert(typeof (i));
    </script>
</head>
```

![image-20220128143903142](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128143903142.png)

![image-20220128144110912](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128144110912.png)

```html
<script type="text/javascript">
       var a=1;
       var b="a";
       alert(a* );
    </script>
```

![image-20220128144402575](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128144402575.png)

NaN 是非数字 非数值。

### 4.关系运算

基本与Java一样

不一样的：

![image-20220128144626934](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128144626934.png)

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128144811810.png)

### 5.逻辑运算

![image-20220128145318803](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128145318803.png)

nan也是

![image-20220128145355855](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128145355855.png)

进行布尔值的且和或的运算。当运算到某一个变量就得出最终结果之后，就返回哪个变量

### 6.数组

![image-20220128164722170](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128164722170.png)

元素类型可以不一致

```java
<script type="text/javascript" >
        var arr =[];
        alert(arr.length); // 0
        arr[0] =12;
        alert(arr[0]); //12
        alert(arr.length); //1
        arr[3]="a";
        alert(arr[2]);//undefined
        alert(arr.length);//4
    </script>
```

读操作辉自动扩容，读的时候越界不会扩容（undefined）

### 7.函数

![image-20220128170144666](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128170144666.png)

返回值 直接retun 不需要说明类型，形参直接写名字， 不需要说明类型

```html
    <script type="text/javascript">
        function fun() {
            alert("无参函数调用")
        }
       // fun();

        function fun2(a,b) {
            alert("有参函数调用 a="+a+"b="+b)
        }
       // fun2(1,5);
        function sum(num1,num2){
            return num1+num2;
        }
        alert(sum(100,30)) //130

    </script>
```

![image-20220128170958633](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128170958633.png)

![image-20220128171158561](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128171158561.png)

+ java 函数可以重载 **js不可以函数重载**，会覆盖前面的函数

+ 隐形参数

  就是在function函数中不需要定义,但却可以直接用来获取所有参数的变量。操作类似数组
  像java中的可变长参数

![image-20220128171713176](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128171713176.png)

```html
<script type="text/javascript">
    function fun(){
        alert(arguments);
        alert(arguments.length);
        for (var i= 0 ;i<arguments.length;i++){
            alert(arguments[i]);
        }
    }
   	fun();//[object Arguments]  0
    fun(1,"g",true);//[object Arguments] 3
</script>
```

就算是fun(a,b,c);有参数 也不会影响arguments使用。a 就是arguments[0];

![image-20220128172911862](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128172911862.png)

如果 数值和字符串相加  和java一样，就是拼接 

### 8. js的自定义对象

+ object形式的自定义对象

对象的定义：

​	var 变量名 = new Object(); //对象实例（）

​	变量名.属性名 =值；//定义一个属性

​	变量名.函数名 = function（）{}//定义一个函数

对象的访问：

​	变量名.属性名 

​	变量名.函数名

![image-20220128174917894](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128174917894.png)

+ {}花括号形式的自定义对象

![image-20220128195639187](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128195639187.png)

属性和方法之间，属性和属性之间 用逗号隔开 最后一个不用。

对象的访问：

​	变量名.属性名 

​	变量名.函数名

### 9.js的事件

电脑输入设备和页面进行的交互的响应叫事件。点击，双击，划过 键盘点击等等

![image-20220128200534256](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128200534256.png)

![image-20220128200920821](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220128200920821.png)

+ onload

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      
  </head>
  <!--静态注册onload-->
  <!--tip:最外层是双引号那么里面就得用单引号不然解析不了-->
  <body onload="alert('静态注册onload事件');">
  
  </body>
  </html
  ```

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onloadFun() {
              alert('静态注册onload事件,大量代码');
          }
      </script>
  </head>
  <!--静态注册onload-->
  <body onload=onloadFun()>
  
  </body>
  </html>
  ```

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          window.onload =function (){//固定格式
              alert('动态注册onload事件');
          }
      </script>
  </head>
  <body>
  </body>
  </html>
  ```
  
+ onclick

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onclickFun(){
              alert("静态注册按钮1onclick")
          }
          //动态注册onclick事件
          window.onload = function (){
  
              /*
              * document js语言提供的对象（文档） 表示整个页面
              * Element 元素 就是标签
              * 通过id属性获取标签
              */
  
              //获取标签对象
              var btnObj =document.getElementById("btn02")
              //通过标签对象.事件名 =function(){}
              btnObj.onclick =function (){
                  alert("动态注册的onclick");
              }
          }
      </script>
  </head>
  <body>
      <!--静态注册onclick事件-->
      <button onclick="onclickFun()">按钮1</button>
      <button id="btn02">按钮2</button>
  </body>
  </html>
  ```

+ onblur

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onblurFun(){
              console.log("静态注册onblur");
              //类似sount
              //console 是js提高的向浏览器的控制台打印输出，用于调试
          }
  
          window.onload = function (){
              var obj=document.getElementById("psw");
              obj.onblur=function (){
                  console.log("动态注册onblur");
              }
          }
      </script>
  </head>
  <body>
  用户名：<input type="text" onblur="onblurFun()"><br/>
  密码：<input type="password" id="psw"><br/>
  
  
  </body>
  </html>
  ```

+ onchange

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript"> 
          function onchangeFun() {
              alert("静态注册onchange,男神已经改变了")
          }
          window.onload =function (){
              var nanshengobj = document.getElementById("nansheng");
              nanshengobj.onchange =function (){
                  alert("动态注册onchange,男神已经改变了")
              }
          }
      </script>
  </head>
  <body>
      选择你心中的男神
      <select onchange="onchangeFun()">
          <option>--d--</option>
          <option>--o--</option>
          <option>--n--</option>
          <option>--g--</option>
      </select>
      选择你心中的男神2
      <select id="nansheng">
          <option>--z--</option>
          <option>--i--</option>
          <option>--z--</option>
          <option>--h--</option>
      </select>
  
  </body>
  </html>

+ onsubmit

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onsubmitFun() {
              //验证所有表单信息是否合法，如果有不合适的就提醒并阻止提交
                alert("静态注册表单提交事件");
                return false;
          }
          window.onload =function (){
              var subObj =document.getElementById("submit");
              subObj.onsubmit =function (){
                  alert("动注册表单提交事件");
                  return false;
              }
          }
      </script>
  </head>
  <body>
  <!--
  return false 可以阻止表单提交
  <form action="http://localhost:8080" method="get" onsubmit="return false">
  
  -->
  <form action="http://localhost:8080" method="get" onsubmit="return onsubmitFun()">
      <input type="submit" value="静态注册"/>
  </form>
  <form action="http://localhost:8080" method="get" id="submit">
      <input type="submit" value="动态注册"/>
  </form>
  </body>
  </html>
  ```

### 10.DOM模型(重点)
![image-20220129152407853](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220129152407853.png)
html就是html文档

#### （1）.document对象

![image-20220129152838930](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220129152838930.png)

![image-20220129153024272](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220129153024272.png)

document对象方法介绍：

![image-20220129153338511](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220129153338511.png)

![image-20220129153656150](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220129153656150.png)

id使用需要唯一

#### （2）.验证用户名输入合法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
    function onclickFun() {
        //获取这个标签对象 uname就是一个dom对象
        var uname = document.getElementById("username");
        /*
        alert(uname.id); //uname
        alert(uname.type) //text
        */
        //可以读输入框的值
        //alert(uname.value)
        var unameText = uname.value;
        //正则表达式验证 字母 下划线 数字 组成5到12位
        var patt = /^\w{5,12}$/;
       // alert(typeof (patt));//object
        if (patt.test(unameText)){
            alert("输入合法");

        }else {
            alert("输入不合法");
        }
    }
    </script>
</head>
<body>
    用户名：<input type="text" id="username" value="输入用户名"/>
    <button onclick="onclickFun()">校验</button>

</body>
</html>
```

改进版： 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
    function onclickFun() {
        var uname = document.getElementById("username");
        var unameText = uname.value;
        //正则表达式验证 字母 下划线 数字 组成5到12位
        var patt = /^\w{5,12}$/;
        var unameSpanObj = document.getElementById("userSpan");
        //innerText 表示起始标签和结束标签的内容
         // alert(unameSpanObj.innerHTML);
         // alert(typeof (patt));//object
        if (patt.test(unameText)){
            unameSpanObj.innerHTML="<img src=\"right.png\" width=\"15\" height=\"15\"/>";

        }else {
            unameSpanObj.innerHTML="<img src=\"wrong.png\" width=\"15\" height=\"15\"/>";
        }
    }
    </script>
</head>
<body>
    用户名：<input type="text" id="username" value="输入用户名" onblur="onclickFun()"/>
    <span id="userSpan" style="color: red;">
       <!-- <img src="right.png" width="15" height="15"/>-->

    </span>

   <!-- <button onclick="onclickFun()">校验</button>-->

</body>
</html>
```



#### （3）.正则表达式

```html
 <script type="text/javascript">
        //是否包含字母e
        var patt =new RegExp("e");
        var patt = /e/; //一样与上面
        //要求是否包含a或b或c
        var a =/[abc]/;
        //要求是否包含a或b或d或e............
        var b =/[a-z]/;
        //是否包含任意数字
        var c =/[0-9]/;
        // \w元字符用于查找单词字符。
        //单词字符包括:a-z.A-Z.0-9, 以及下划线,包含_ (下划线)字符。
        var d=/\w/;
        //是否包含至少一个a
        var e=/a+/;


        //是否包含0个或多个a
        var f =/a*/;
        //是否包含0个或1个a 他们都是检索到符合条件就会停止
        var G =/a?/; // 123654aaaa78 返回t
        //是否包含连续3个a x个n
        var h =/a{3}/;
        //是否包含 至少3个连续的a 最多5个连续的a
        var i =/a{3,5}/; //"aaaaaaaaaa" 也会返回t，看var h
        //必须以a结尾
        var j= /a$/;
        //必须以a开头
        var j= /^a/;
        //从头到尾都必须完全匹配 至少3个连续的a 最多5个连续的a
        var h =/^a{3,5}$/

       // alert(h.test("1aa2aa"))
    </script>
```

#### （4）getElementsByName

![image-20220130101759601](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130101759601.png)

![image-20220130101737976](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130101737976.png)

是有序的 是从上到下的顺序

![image-20220130102238308](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130102238308.png)

与ById的区别就是 获得的是一个对象数组，然后对数组里的对象的属性进行操作

#### （5）getElementsByTagName

![image-20220130103207246](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130103207246.png)

3个查询方法

+ 优先级：

id>name>tagname

+ 加载完成后才可以查询

  代码从前到后执行，如果不在点击方法里，在外面查到是null. 如下：

  ![image-20220130105229445](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130105229445.png)

  解决方法：

  页面加载完 会执行这个方法

  ![image-20220130105354163](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130105354163.png)

#### （6）节点的常用属性和方法

 	![image-20220130105908354](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130105908354.png)

![image-20220130105954855](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130105954855.png)

![image-20220130110006378](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130110006378.png)

理解：

![image-20220130111930521](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130111930521.png)

city 有9个孩子节点

空格算成了单独的文本节点，属性和文本都算在标签节点里面了

只要获取孩子节点，具体的文字包括li的属性都是孙节点了

#### （7）. 添加子节点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        window.onload=function (){
            //使用js代码来创建html标签 并展示在页面上
            var divObj =document.createElement("div"); //在内存中
            divObj.innerHTML= "donggei"; //在内存中 没放在body
            document.body.appendChild(divObj);
        }


    </script>
</head>
<body>

</body>
</html>
```

## 三.jQuery

### 1. helloWorld

![image-20220130115137686](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130115137686.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery.js"></script>
    <script type="text/javascript">
         // alert($); 有值 说明引入成功
         $(function (){  //页面加载完成后.... ，相当于 window.onload =function (){
             var $btnObj =$("#btnId"); //表示 按id查询 jQ对象 命名前面加一个$
             $btnObj.click(function (){
                 alert("JQuery 的单击事件")
             });
         })
    </script>
</head>
<body>
<button id="btnId">按钮</button>
  
</body>
</html>
```

### 2.函数核心介绍

![image-20220130143548374](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130143548374.png)

![image-20220130144042677](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130144042677.png)

![image-20220130144019707](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130144019707.png)如下：



![image-20220130143904199](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130143904199.png)

![image-20220130145128728](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130145128728.png)

按**标签名**查：

![image-20220130145517373](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130145517373.png)



![image-20220130145402534](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130145402534.png)

### 3.区分dom对象和JQurey对象

![image-20220130162254797](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130162254797.png)

 

![image-20220130162352894](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130162352894.png)

+ **jQuery对象是dom对象的数组+ jQuery提供的一系列功能函数。** 

  ``` $a[0] 其实是一个dom对象```（命名默认前面有$，是jq对象） 

+ ![image-20220130163447548](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130163447548.png)

+ 相互转化

  ![image-20220130163613510](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130163613510.png)

  

### 4.Jquery选择器

#### （1）.基本选择器

  ![image-20220130172549984](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130172549984.png)

  ```html
  $("p.myClass")
  ```

  

![image-20220130174227463](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130174227463.png)

![image-20220130174353134](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130174353134.png)

顺序和页面标签的顺序有关 

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<style type="text/css">
			div, span, p {
			    width: 140px;
			    height: 140px;
			    margin: 5px;
			    background: #aaa;
			    border: #000 1px solid;
			    float: left;
			    font-size: 17px;
			    font-family: Verdana;
			}
			
			div.mini {
			    width: 55px;
			    height: 55px;
			    background-color: #aaa;
			    font-size: 12px;
			}
			
			div.hide {
			    display: none;
			}
		</style>
		<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
		<script type="text/javascript">
			
				//1.选择 id 为 one 的元素 "background-color","#bbffaa"
				$(function (){
					$("#btn1").click(function (){
						//css 方法设置和获取样式
						$("#one").css("background-color","red");

					})
				})
				//2.选择 class 为 mini 的所有元素
				$(function (){
					$("#btn2").click(function (){
						//css 方法设置和获取样式
						$(".mini").css("background-color","#bbffaa")

					})

				})

				//3.选择 元素名是 div 的所有元素 
				$(function (){
					$("#btn3").click(function (){
						//css 方法设置和获取样式
						$("div").css("background-color","#bbffaa")

					})
				})

				//4.选择所有的元素 
				$(function (){
					$("#btn4").click(function (){
						$("*").css("background-color","#bbffaa")

					})
				})

				//5.选择所有的 span 元素和id为two的元素   
				$(function (){
					$("#btn5").click(function (){
						$("span,#two").css("background-color","#bbffaa")

					})
				})




		</script>
	</head>
	<body>
<!-- 	<div>
		<h1>基本选择器</h1>
	</div>	 -->	
		<input type="button" value="选择 id 为 one 的元素" id="btn1" />
		<input type="button" value="选择 class 为 mini 的所有元素" id="btn2" />
		<input type="button" value="选择 元素名是 div 的所有元素" id="btn3" />
		<input type="button" value="选择 所有的元素" id="btn4" />
		<input type="button" value="选择 所有的 span 元素和id为two的元素" id="btn5" />
		
		<br>
		<div class="one" id="one">
			id 为 one,class 为 one 的div
			<div class="mini">class为mini</div>
		</div>
		<div class="one" id="two" title="test">
			id为two,class为one,title为test的div
			<div class="mini" title="other">class为mini,title为other</div>
			<div class="mini" title="test">class为mini,title为test</div>
		</div>
		<div class="one">
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini"></div>
		</div>
		<div class="one">
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini" title="tesst">class为mini,title为tesst</div>
		</div>
		<div style="display:none;" class="none">style的display为"none"的div</div>
		<div class="hide">class为"hide"的div</div>
		<div>
			包含input的type为"hidden"的div<input type="hidden" size="8">
		</div>
		<span class="one" id="span">^^span元素^^</span>
	</body>
</html>
```

#### （2）.层级选择器

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<style type="text/css">
			div, span, p {
			    width: 140px;
			    height: 140px;
			    margin: 5px;
			    background: #aaa;
			    border: #000 1px solid;
			    float: left;
			    font-size: 17px;
			    font-family: Verdana;
			}
			
			div.mini {
			    width: 55px;
			    height: 55px;
			    background-color: #aaa;
			    font-size: 12px;
			}
			
			div.hide {
			    display: none;
			}			
		</style>
		<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
		<script type="text/javascript">
			$(document).ready(function(){  //以前的是简写
				//1.选择 body 内的所有 div 元素 
				$("#btn1").click(function(){
					$("body div").css("background", "#bbffaa");
				});

				//2.在 body 内, 选择div子元素  
				$("#btn2").click(function(){
					$("body>div").css("background", "#bbffaa");
				});

				//3.选择 id 为 one 的下一个 div 元素 
				$("#btn3").click(function(){
					$("#one+div").css("background", "#bbffaa");
				});

				//4.选择 id 为 two 的元素后面的所有 div 兄弟元素
				$("#btn4").click(function(){
					$("#two~div").css("background", "#bbffaa");
				});
			});
		</script>
	</head>
	<body>	
	
<!-- 	<div>
		<h1>层级选择器:根据元素的层级关系选择元素</h1>
		ancestor descendant  ：
		parent > child 		   ：
		prev + next 		   ：
		prev ~ siblings 	   ：
	</div>	 -->
		<input type="button" value="选择 body 内的所有 div 元素" id="btn1" />
		<input type="button" value="在 body 内, 选择div子元素" id="btn2" />
		<input type="button" value="选择 id 为 one 的下一个 div 元素" id="btn3" />
		<input type="button" value="选择 id 为 two 的元素后面的所有 div 兄弟元素" id="btn4" />
		<br><br>
		<div class="one" id="one">
			id 为 one,class 为 one 的div
			<div class="mini">class为mini</div>
		</div>
		<div class="one" id="two" title="test">
			id为two,class为one,title为test的div
			<div class="mini" title="other">class为mini,title为other</div>
			<div class="mini" title="test">class为mini,title为test</div>
		</div>
		<div class="one">
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini"></div>
		</div>
		<div class="one">
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini">class为mini</div>
			<div class="mini" title="tesst">class为mini,title为tesst</div>
		</div>
		<div style="display:none;" class="none">style的display为"none"的div</div>
		<div class="hide">class为"hide"的div</div>
		<div>
			包含input的type为"hidden"的div<input type="hidden" size="8">
		</div>
		<span id="span">^^span元素^^</span>
	</body>
</html>
```

#### （3）.基本的过滤选择器

使用时查看CHM。

![image-20220130204243670](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130204243670.png)

![image-20220130213136542](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220130213136542.png)

#### （4）.内容过滤选择器

![image-20220131101131134](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220131101131134.png)

#### （5）.属性过滤选择器

![image-20220131102522651](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220131102522651.png)

#### （6）.表单过滤选择器

注意点： 

+ 可用和不可用属性

+ 相当于dom对象的value属性 （！！表单项中的）

​		不传参数是获取，传参是赋值。val("xxxx")

![image-20220201192645781](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220201192645781.png)

+ 遍历方法

  ![image-20220201194028121](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220201194028121.png)

#### （7）.表单对象属性过滤选择器

### 5.JQuery元素筛选

![image-20220201195447000](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220201195447000.png)

![image-20220202103352069](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220202103352069.png)

![image-20220202103021189](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220202103021189.png)

![image-20220202103107404](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220202103107404.png)

### 6.jquery的属性操作

![image-20220202110443707](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220202110443707.png)

+ text()文本 包括后代节点中的文本

+ 注意：val 是设置获取表单项的value 。div,span......不是表单项。

+ val方法还可以同时设置多个表单项的选中状态: 加【】

  ![image-20220202113358417](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220202113358417.png)

![image-20220202120141401](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220202120141401.png)

**prop解决了  attr（checked） 可能 返回undefined的问题**

下图是attr获取和设置普通属性，

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220202114711921.png)

### 7.DOM的增删改

![image-20220203092457546](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203092457546.png)

![image-20220203092548869](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203092548869.png)

![image-20220203092827204](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203092827204.png)

创建一个h1插入到两个div中

![image-20220203095356151](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203095356151.png)

### 8.css样式操作

![image-20220203134500883](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203134500883.png)

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<style type="text/css">
	
	div{
		width:100px;
		height:260px;
	}
/*
	div 表示限制只能div使用 .border表示给border类使用
*/
	div.border{
		border: 2px white solid;
	}
	
	div.redDiv{
		background-color: red;
	}
	
	div.blackDiv{
		border: 5px blue solid;
	}
	
</style>

<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
<script type="text/javascript">
	

	$(function(){
		/*
CSS
css(name|pro|[,val|fn])       读写匹配元素的样式属性。 
								a.css('color')取出a元素的color
								a.css('color',"red")设置a元素的color为red

CSS 类

addClass(class|fn) 			为元素添加一个class值;<div class="mini big">
removeClass([class|fn]) 	删除元素的class值；传递一个具体的class值，就会删除具体的某个class
							a.removeClass()：移除所有的class值

**/
		
		var $divEle = $('div:first');
		
		$('#btn01').click(function(){
			//addClass() - 向被选元素添加一个或多个类
			$divEle.addClass("redDiv blackDiv");
		});
		
		$('#btn02').click(function(){
			//removeClass() - 从被选元素删除一个或多个类 
			$divEle.removeClass()
		});

		
		$('#btn03').click(function(){
			//toggleClass() - 对被选元素进行添加/删除类的切换操作 
			//切换就是如果具有该类那么删除，如果没有那么添加上
			$divEle.toggleClass("redDiv");
		});
		
		$('#btn04').click(function(){
			//offset() - 返回第一个匹配元素相对于文档的位置。
			var os = $divEle.offset();
			//注意通过offset获取到的是一个对象，这个对象有两个属性top表示顶边距，left表示左边距
			alert("顶边距："+os.top+" 左边距："+os.left);
			
			//调用offset设置元素位置时，也需要传递一个js对象，对象有两个属性top和left
			//offset({ top: 10, left: 30 });
			 $divEle.offset({
				 top:50,
				 left:60
			 }); 
		});
		
	})
</script>
</head>
<body>

	<table align="center">
		<tr>
			<td>
				<div class="border">
				</div>
			</td>
			
			<td>
				<div class="btn">
					<input type="button" value="addClass()" id="btn01"/>
					<input type="button" value="removeClass()" id="btn02"/>
					<input type="button" value="toggleClass()" id="btn03"/>
					<input type="button" value="offset()" id="btn04"/>
				</div>
			</td>
		</tr>
	</table>
	
	<br /> <br />
	
	<br /> <br />
	
</body>
</html>
```

### 9.JQuery动画



![image-20220203161643202](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203161643202.png)

![image-20220203162057844](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203162057844.png)

![image-20220203161350905](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203161350905.png)

![image-20220203162157325](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203162157325.png)

透明度：0~1；

### 10.原生js和jQuery页面加载完成之后的区别

![image-20220203192214264](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203192214264.png)

![image-20220203193015331](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203193015331.png)

如下，原生js会等img 和iframe 都加载完

![image-20220203192112076](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203192112076.png)

![image-20220203192822307](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203192822307.png)

### 11.JQuery中常用事件处理方法

![image-20220203211523359](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203211523359.png)

![image-20220203213448210](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203213448210.png)

![image-20220206203858898](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206203858898.png)

![image-20220203212058270](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203212058270.png)

![image-20220203213034586](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203213034586.png)

无参数就是全部解除绑定

### 12.事件的冒泡

![image-20220203214215701](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203214215701.png)

比如 div 里面 有个span ，span和div都有点击事件， 你点击了span，那么span和div会先后触发点击事件

![image-20220203214413862](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203214413862.png)

span事件里加return false解决。

### 13.事件对象

![image-20220203215011567](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203215011567.png)

下图就是这个js对象里的信息:

![image-20220203215218569](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203215218569.png)

使用举例：

可以实现丰富的功能

![image-20220203215638321](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220203215638321.png)

### 14.练习—图片跟随 

![image-20220204105230645](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204105230645.png)

左上到右下会闪，因为进入了大图片，页面以为出去了小图片，大图片hide，然后页面又以为进入了小图片。解决：

![image-20220204110208820](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204110208820.png)

# 四.XML

 ![image-20220204152311330](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204152311330.png)

![image-20220204153116345](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204153116345.png)

![image-20220204153511237](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204153511237.png)

![image-20220204153721972](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204153721972.png)

![image-20220204154146385](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204154146385.png)

### 1.dom4j解析技术

![image-20220204161104864](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204161104864.png)

![image-20220204161201817](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204161201817.png)

```java
package com.donggei.pojo;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;
import org.junit.Test;

import java.math.BigDecimal;
import java.util.List;

/**
 * @className: Dom4jTest
 * @description: TODO 类描述
 * @author: Dong
 * @date: 2022/2/4
 **/
public class Dom4jTest {
    @Test
    public void test1() throws DocumentException {
        //创建一个SaxReader输入流，去读取xml配置文件，生成bocument对象
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("src/books.xml");
        System.out.println(document);
    }

    @Test
    public void test2() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("src/books.xml");
        //通过根元素获取book标签对象
        Element rootElement = document.getRootElement();
        // System.out.println(rootElement);
        List<Element> books = rootElement.elements("book");
        for (Element book : books) {
            //asXML 把标签对象，转化成标签字符串
            //System.out.println(book.asXML());
            Element nameElement = book.element("name");

            //System.out.println(nameElement.asXML());
            //<name>java编程思想</name>
            //<name>葵花宝典</name>

            // getText();可T以获取标签中的文本内容
            String nameElementText = nameElement.getText();
            //System.out.println(nameElementText);
            //java编程思想
            //葵花宝典

            //直接获取指定标签名的文本内容
            String price = book.elementText("price");
            System.out.println(price);
            String author = book.elementText("author");
            //获取属性值
            String sn = book.attributeValue("sn");
            Book book1 = new Book(sn, nameElementText, author, new BigDecimal(price));
            System.out.println(book1);
            //Book{sn='null', name='java编程思想', author='华仔', price=9.9}
            //Book{sn='null', name='葵花宝典', author='班长', price=5.5}
        }

    }

}
```

![image-20220204170732499](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204170732499.png)

tip:使用了junit单元测试框架，[教程](https://www.liaoxuefeng.com/wiki/1252599548343744/1304048154181666)

# 五.Tomcat

### 1.概念

![image-20220204170254147](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204170254147.png)

![image-20220204170359211](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204170359211.png)

![image-20220204170520502](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204170520502.png)

### 2.web资源分类

![image-20220204170846555](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204170846555.png)

### 3.常用的web服务器

![image-20220204170931597](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204170931597.png)

### 4.Tomcat服务器和Servlet版本的对应关系

![image-20220204171232371](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204171232371.png)

### 5.tomcat安装和使用

找到你需要用的Tomcat版本对应的zip压缩包,解压到需要安装的目录即可。

![image-20220204172714555](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204172714555.png)

webapps里 一个目录 就是一个工程

tip：钝化理解为序列化，把对象写在磁盘上。

![image-20220204173206779](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204173206779.png)

方法二：这个方法好处是启动时发生错误 可以查看错误提示

![image-20220204174356817](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204174356817.png)

ctrl+c 或者**bin下的shutdown.bat** 或者直接关cmd窗口退出

### 6.修改端口号

![image-20220204175121767](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204175121767.png)

![image-20220204174838240](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204174838240.png)

![image-20220204175445325](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204175445325.png)

### 7.部署web工程到Tomcat

方法一：

直接把文件拷贝到webapps里，一个目录就是一个工程（项目）

方法二：

![image-20220204195550579](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220204195550579.png)

相当于 映射过去的

手拖页面文件到浏览器，和输入地址访问的区别：

![image-20220205094413878](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205094413878.png)

tomcat（服务器）中还有其他工程，上图中省略了。

 ![image-20220205102729516](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205102729516.png)

这是因为在tomcat中的conf下的web.xml中配置了，一直下拉到最后可以看到```<welcome-file-list>```标签内容

### 8.idea配置tomcat

![image-20220205104750390](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205104750390.png)

![image-20220205104623870](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205104623870.png)

new 工程：

![image-20220205190405524](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205190405524.png)

![image-20220205190421285](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205190421285.png)

![image-20220205190430659](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205190430659.png)

在WEB-INF下创建lib 目录

目录介绍：

![image-20220205191157076](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205191157076.png)

热部署：

![image-20220205203128702](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205203128702.png)

#### 快速生成servlet

帮你快速写：

如果没有这个选项，到Project Structure中的Facets,选择中对应的web，把最下面的Source Roots打勾。

![image-20220206115614790](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206115614790.png)

![image-20220206115816142](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206115816142.png)



#### 控制台乱码：

#### 添加这句话

```
-Dfile.encoding=UTF-8
```

![image-20220207220456204](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207220456204.png)

![image-20220207220343125](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207220343125.png)

### 9.使用maven配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">


</web-app>
```



使用maven配置后

![image-20220206104201723](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206104201723.png)

## 六 .servlet

[狂神讲的maven配置和Servlet可以作为补充](https://www.cnblogs.com/judy198/p/15008553.html)

  ```xml
  <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
          <dependency>
              <groupId>javax.servlet</groupId>
             <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
          </dependency>
          <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
          <dependency>
              <groupId>javax.servlet.jsp</groupId>
              <artifactId>javax.servlet.jsp-api</artifactId>
              <version>2.3.3</version>
              <scope>provided</scope>
          </dependency>
  ```

![image-20220205203538856](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205203538856.png)

![image-20220205204159613](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205204159613.png)

![image-20220205205000791](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205205000791.png)

![image-20220205205349853](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205205349853.png)

继承servlet后 重写的方法后关注的生命周期，一般继承Httpservlet主要写service方法。

### 1.servlet继承体系

![image-20220205212311644](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205212311644.png)

### 2.ServletConfig类 3个作用

在init方法中使用：

![image-20220205214238041](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205214238041.png)

获取web.xml中的对应的参数

![image-20220205214506705](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205214506705.png)

注意：如下：只在单独的helloServlet2 中可以获取，因为web.xml中就写在了单独的某个Servlet。

![image-20220205221151897](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205221151897.png)

![image-20220205214619080](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205214619080.png)

![image-20220205214740200](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205214740200.png)

![image-20220205220817991](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205220817991.png)

也可以在其他方法中使用

### 3.ServletContext类4个作用

![image-20220206124133993](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206124133993.png)



![image-20220205222336227](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205222336227.png)

![image-20220205222517627](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205222517627.png)

ServletContext只能获取context-prarm, 上一节提到的init-param是ServletConfig才能获取的。

![image-20220205223723593](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220205223723593.png)

idea发布的 对应工程的web目录 就是把源码 web目录下的内容+字节码部署到这个路径下了

这个路径是编译完后的字节文件路 径，是下面out文件的输出路径

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = getServletContext();
        servletContext.setAttribute("key1","value1");

    }
```

context对象的作用就是 每一个Servlet是共用的，相互之间都可以拿到配置的信息。

### 4.HttpServletRequest

每次请求进来就会创建一个  

![image-20220206172231306](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206172231306.png)

注意：URL = 协议 + 主机 + 端口号 + URI ![image-20220206172523967](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206172523967.png)

```java
@WebServlet(name = "RequestAPIServlet", value = "/RequestAPIServlet")
public class RequestAPIServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println(request.getRequestURI());
        System.out.println(request.getRequestURL());
        System.out.println("客户端IP地址："+request.getRemoteHost());
        String requestHeader = request.getHeader("User-Agent");
        System.out.println(requestHeader);
        System.out.println(request.getMethod());
        ///servlet_demo01/RequestAPIServlet
        //http://localhost:8080/servlet_demo01/RequestAPIServlet
        //客户端IP地址：127.0.0.1
        //Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:89.0) Gecko/20100101 Firefox/89.0
        //GET
        
        
    }
```

参数处理：

![image-20220206181205539](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206181205539.png)

![image-20220206181254806](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206181254806.png)

![image-20220206181546292](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206181546292.png)

乱码问题：

![image-20220206181927523](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206181927523.png)

![image-20220206182053611](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206182053611.png)

注意：**设置字符集必须要放在最前面。先获取参数，后设置，无效。**

### 5.请求转发

+ 浏览器地址没有变

+ 他们是一次请求

+ 他们共享request域中的数据

+ 可以转发web-inf目录下（web-inf目录是不可以浏览器地址直接访问的）

+ 不可访问工程以外的地址

  ![image-20220206184701554](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206184701554.png)

  ![image-20220206184718822](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206184718822.png)



![image-20220206182615238](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206182615238.png)

![image-20220206183144539](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206183144539.png)

```java
public class Servlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        // 获取请求的参数（办事的材料）查看
        String username = req.getParameter("username");
        System.out.println("在Servlet1（柜台1）中查看参数（材料）：" + username);

        // 给材料 盖一个章，并传递到Servlet2（柜台 2）去查看
        req.setAttribute("key1","柜台1的章");

        // 问路：Servlet2（柜台 2）怎么走
        /**
         * 请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到IDEA代码的web目录<br/>
         *
         */
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("/servlet2");
//        RequestDispatcher requestDispatcher = req.getRequestDispatcher("http://www.baidu.com");

        // 走向Sevlet2（柜台 2）
        requestDispatcher.forward(req,resp);

    }
}
```

请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到IDEA代码的web目录

请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到IDEA代码的web目录

请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到IDEA代码的web目录

```java
public class Servlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取请求的参数（办事的材料）查看
        String username = req.getParameter("username");
        System.out.println("在Servlet2（柜台2）中查看参数（材料）：" + username);

        // 查看 柜台1 是否有盖章
        Object key1 = req.getAttribute("key1");
        System.out.println("柜台1是否有章：" + key1);

        // 处理自己的业务
        System.out.println("Servlet2 处理自己的业务 ");
    }
}

```

### 6.base标签的作用

解决的问题就是

正常使用的a标签跳转，跳转后地址栏改变，然后点击回到上一页面，正常运行。

如果是**请求转发**过来的，点击回到上一页，因为地址栏没有改动到当前页面，所以会发生错误。

页面有base标签时，点击回到上一页，先参考设置的地址，作为当前地址，而不是地址栏的地址。

![image-20220206194438790](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206194438790.png)



![image-20220206194542293](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206194542293.png)

![image-20220206194812745](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206194812745.png)

![image-20220206195021767](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206195021767.png)

### 7.Web中 / 的意义（路径）

![image-20220206200451653](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206200451653.png)

请求转发中的 / 是到工程名的 ，是和重定向不一样的

(工程名路径也叫上下文路径)

### 8.HttpServletResponse

 ![image-20220206211002521](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206211002521.png)

![image-20220206211653469](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206211653469.png)

不能同时使用，控制台会报错。

解决乱码：

设置响应头的内容都需要在获取流对象之前才可以

```java
 @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       // System.out.println(response.getCharacterEncoding());//ISO-8859-1
        response.setCharacterEncoding("utf-8");
        //浏览器使用的GBK
        //让浏览器也使用utf-8
        response.setHeader("Content-Type","text/html;charset = utf-8");
        //resp.setContentType("text/html;charset=utf-8") 用这一句也能解决 他相当于上面两句
        PrintWriter writer = response.getWriter();
        writer.write("response");
        writer.write("数据");
    }
```

![image-20220206221829422](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206221829422.png)

### 9.请求重定向

![image-20220206222349863](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206222349863.png)

![image-20220206222937812](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206222937812.png)

![image-20220206223148328](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206223148328.png)

![image-20220207000242544](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207000242544.png)

这里的req.setAttribute和getServletContext().setAttribute 不一样，重定向 Context()的Attribute是可以获取到的

![image-20220207000358541](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207000358541.png)

推荐使用的方法：

![image-20220207102548369](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207102548369.png)

## 七.HTTP协议

 ![image-20220206124546119](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206124546119.png)

### 1.请求：

![image-20220206164642813](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206164642813.png)

![image-20220206164555326](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206164555326.png)

![image-20220206131820879](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206131820879.png)

![image-20220206164428184](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206164428184.png)

两种请求的区分：

![image-20220206165401704](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206165401704.png)

### 2.响应

![image-20220206165638434](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206165638434.png)

![image-20220206170022566](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206170022566.png)

![image-20220206170231764](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206170231764.png)

### 3.MIME

![image-20220206170358216](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220206170358216.png)

## 八.IDEA中Debug调试的使用

![image-20220208105208445](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208105208445.png)

![image-20220208111341617](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208111341617.png)

![image-20220208112332086](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208112332086.png)

![image-20220208114242271](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208114242271.png)

![image-20220208114309566](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208114309566.png)

## 九.JSP

使用Servlet回传HTML页面，编码比较麻烦。

疑问：不太明白为什么不直接请求转发？难到是因为不能加自己的可变数据？

弹幕：因为这是请求和回传的差别吧，请求是用servlet接受，回传（响应）需要servlet程序根据请求来回传（动态），老师写的那个就是为了模仿回传的流程。不要纠结可以直接1.html跳转（只是我的想法）

![image-20220208144417374](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208144417374.png)

![image-20220208144717975](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208144717975.png)

## ![image-20220208145210207](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208145210207.png)

![image-20220208191545292](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208191545292.png)

![image-20220208192353278](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208192353278.png)

观察翻译出来的Servlet程序的源代码，不难发现。其底层实现，也是通过输出流。把html页面数据回传
给客户端。

### 1.jsp头部的page指令2

大部分都不需要改

![image-20220208194001354](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208194001354.png)

![image-20220208195145673](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208195145673.png)

![image-20220208195134851](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208195134851.png)

![image-20220208195716572](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208195716572.png)

### 2.jsp常用脚本

#### （1）说明脚本-极少用

![image-20220208202829181](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208202829181.png)



.java文件一般的地址：

![image-20220208202625853](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208202625853.png)

#### （2）表达式脚本-常用

![image-20220208203405772](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208203405772.png)

![image-20220208210027445](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208210027445.png)

特点3：eg:

![image-20220208210210529](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208210210529.png)

#### （3）代码脚本

![image-20220208210821352](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208210821352.png)

也是翻译在_jspService方法中。

![image-20220208213332238](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208213332238.png)

特点3例：

![image-20220208213226442](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208213226442.png)

特点4例：

![image-20220208213500184](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208213500184.png)

![image-20220208213726296](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220208213726296.png)

没<% %>包起来的直接out.write了。

#### 测试源码及自动生成的.java文件。

```html
<%@ page import="java.util.Map" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--练习：
--%>

<%--1、声明类属性--%>
    <%!
        private Integer id;
        private String name;
        private static Map<String,Object> map;
    %>
<%--2、声明static静态代码块--%>
    <%!
        static {
            map = new HashMap<String,Object>();
            map.put("key1", "value1");
            map.put("key2", "value2");
            map.put("key3", "value3");
        }
    %>
<%--3、声明类方法--%>
    <%!
        public int abc(){
            return 12;
        }
    %>
<%--4、声明内部类--%>
    <%!
        public static class A {
            private Integer id = 12;
            private String abc = "abc";
        }
    %>

<%--练习：
1.输出整型
2.输出浮点型
3.输出字符串
4.输出对象    --%>
    <%=12 %> <br>

    <%=12.12 %> <br>

    <%="我是字符串" %> <br>

    <%=map%> <br>

    <%=request.getParameter("username")%>


<%--练习：--%>
<%--1.代码脚本----if 语句--%>
    <%
        int i = 13 ;
        if (i == 12) {
    %>
            <h1>国哥好帅</h1>
    <%
        } else {
    %>
        <h1>国哥又骗人了！</h1>
    <%
        }
    %>
<br>
<%--2.代码脚本----for 循环语句--%>
    <table border="1" cellspacing="0">
    <%
        for (int j = 0; j < 10; j++) {
    %>
        <tr>
            <td>第 <%=j + 1%>行</td>
        </tr>
    <%
        }
    %>
    </table>
<%--3.翻译后java文件中_jspService方法内的代码都可以写--%>
    <%
        String username = request.getParameter("username");
        System.out.println("用户名的请求参数值是：" + username);
    %>


</body>
</html>
```

```java
/*
 * Generated by the Jasper component of Apache Tomcat
 * Version: Apache Tomcat/9.0.53
 * Generated at: 2022-02-08 12:20:00 UTC
 * Note: The last modified time of this file was set to
 *       the last modified time of the source file after
 *       generation to assist with modification tracking.
 */
package org.apache.jsp;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.jsp.*;
import java.util.Map;
import java.util.HashMap;

public final class a_jsp extends org.apache.jasper.runtime.HttpJspBase
    implements org.apache.jasper.runtime.JspSourceDependent,
                 org.apache.jasper.runtime.JspSourceImports {


    private Integer id;
    private String name;
    private static Map<String,Object> map;


    static {
        map = new HashMap<String,Object>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");
    }


    public int abc(){
        return 12;
    }


    public static class A {
        private Integer id = 12;
        private String abc = "abc";
    }

  private static final javax.servlet.jsp.JspFactory _jspxFactory =
          javax.servlet.jsp.JspFactory.getDefaultFactory();

  private static java.util.Map<java.lang.String,java.lang.Long> _jspx_dependants;

  private static final java.util.Set<java.lang.String> _jspx_imports_packages;

  private static final java.util.Set<java.lang.String> _jspx_imports_classes;

  static {
    _jspx_imports_packages = new java.util.HashSet<>();
    _jspx_imports_packages.add("javax.servlet");
    _jspx_imports_packages.add("javax.servlet.http");
    _jspx_imports_packages.add("javax.servlet.jsp");
    _jspx_imports_classes = new java.util.HashSet<>();
    _jspx_imports_classes.add("java.util.Map");
    _jspx_imports_classes.add("java.util.HashMap");
  }

  private volatile javax.el.ExpressionFactory _el_expressionfactory;
  private volatile org.apache.tomcat.InstanceManager _jsp_instancemanager;

  public java.util.Map<java.lang.String,java.lang.Long> getDependants() {
    return _jspx_dependants;
  }

  public java.util.Set<java.lang.String> getPackageImports() {
    return _jspx_imports_packages;
  }

  public java.util.Set<java.lang.String> getClassImports() {
    return _jspx_imports_classes;
  }

  public javax.el.ExpressionFactory _jsp_getExpressionFactory() {
    if (_el_expressionfactory == null) {
      synchronized (this) {
        if (_el_expressionfactory == null) {
          _el_expressionfactory = _jspxFactory.getJspApplicationContext(getServletConfig().getServletContext()).getExpressionFactory();
        }
      }
    }
    return _el_expressionfactory;
  }

  public org.apache.tomcat.InstanceManager _jsp_getInstanceManager() {
    if (_jsp_instancemanager == null) {
      synchronized (this) {
        if (_jsp_instancemanager == null) {
          _jsp_instancemanager = org.apache.jasper.runtime.InstanceManagerFactory.getInstanceManager(getServletConfig());
        }
      }
    }
    return _jsp_instancemanager;
  }

  public void _jspInit() {
  }

  public void _jspDestroy() {
  }

  public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
      throws java.io.IOException, javax.servlet.ServletException {

    if (!javax.servlet.DispatcherType.ERROR.equals(request.getDispatcherType())) {
      final java.lang.String _jspx_method = request.getMethod();
      if ("OPTIONS".equals(_jspx_method)) {
        response.setHeader("Allow","GET, HEAD, POST, OPTIONS");
        return;
      }
      if (!"GET".equals(_jspx_method) && !"POST".equals(_jspx_method) && !"HEAD".equals(_jspx_method)) {
        response.setHeader("Allow","GET, HEAD, POST, OPTIONS");
        response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED, "JSP 只允许 GET、POST 或 HEAD。Jasper 还允许 OPTIONS");
        return;
      }
    }

    final javax.servlet.jsp.PageContext pageContext;
    javax.servlet.http.HttpSession session = null;
    final javax.servlet.ServletContext application;
    final javax.servlet.ServletConfig config;
    javax.servlet.jsp.JspWriter out = null;
    final java.lang.Object page = this;
    javax.servlet.jsp.JspWriter _jspx_out = null;
    javax.servlet.jsp.PageContext _jspx_page_context = null;


    try {
      response.setContentType("text/html;charset=UTF-8");
      pageContext = _jspxFactory.getPageContext(this, request, response,
      			null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;

      out.write("\r\n");
      out.write("\r\n");
      out.write("\r\n");
      out.write("<html>\r\n");
      out.write("<head>\r\n");
      out.write("    <title>Title</title>\r\n");
      out.write("</head>\r\n");
      out.write("<body>\r\n");
      out.write("\r\n");
      out.write("\r\n");
      out.write('\r');
      out.write('\n');
      out.write('\r');
      out.write('\n');
      out.write('\r');
      out.write('\n');
      out.write('\r');
      out.write('\n');
      out.write('\r');
      out.write('\n');
      out.write('\r');
      out.write('\n');
      out.write('\r');
      out.write('\n');
      out.write("\r\n");
      out.write("\r\n");
      out.write('\r');
      out.write('\n');
      out.print(12 );
      out.write(" <br>\r\n");
      out.write("\r\n");
      out.print(12.12 );
      out.write(" <br>\r\n");
      out.write("\r\n");
      out.print("我是字符串" );
      out.write(" <br>\r\n");
      out.write("\r\n");
      out.print(map);
      out.write(" <br>\r\n");
      out.write("\r\n");
      out.print(request.getParameter("username"));
      out.write("\r\n");
      out.write("\r\n");
      out.write("\r\n");
      out.write('\r');
      out.write('\n');
      out.write('\r');
      out.write('\n');

    int i = 13 ;
    if (i == 12) {

      out.write("\r\n");
      out.write("<h1>国哥好帅</h1>\r\n");

} else {

      out.write("\r\n");
      out.write("<h1>国哥又骗人了！</h1>\r\n");

    }

      out.write("\r\n");
      out.write("<br>\r\n");
      out.write("\r\n");
      out.write("<table border=\"1\" cellspacing=\"0\">\r\n");
      out.write("    ");

        for (int j = 0; j < 10; j++) {
    
      out.write("\r\n");
      out.write("    <tr>\r\n");
      out.write("        <td>第 ");
      out.print(j + 1);
      out.write("行</td>\r\n");
      out.write("    </tr>\r\n");
      out.write("    ");

        }
    
      out.write("\r\n");
      out.write("</table>\r\n");
      out.write('\r');
      out.write('\n');

    String username = request.getParameter("username");
    System.out.println("用户名的请求参数值是：" + username);

      out.write("\r\n");
      out.write("\r\n");
      out.write("\r\n");
      out.write("</body>\r\n");
      out.write("</html>");
    } catch (java.lang.Throwable t) {
      if (!(t instanceof javax.servlet.jsp.SkipPageException)){
        out = _jspx_out;
        if (out != null && out.getBufferSize() != 0)
          try {
            if (response.isCommitted()) {
              out.flush();
            } else {
              out.clearBuffer();
            }
          } catch (java.io.IOException e) {}
        if (_jspx_page_context != null) _jspx_page_context.handlePageException(t);
        else throw new ServletException(t);
      }
    } finally {
      _jspxFactory.releasePageContext(_jspx_page_context);
    }
  }
}
```
#### （4）JSP的三种注释

![image-20220209163808447](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209163808447.png)

### 3.JSP中的九大内置对象

jsp中的内置对象，是指Tomcat在翻译jsp 页面成为Servlet源代码后，内部提供的九大对象。叫内置对象。

![image-20220209170117129](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209170117129.png)

第九个需要这样：默认是flase

![image-20220209170151556](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209170151556.png)

### 4.jsp四大域对象

![image-20220209170902014](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209170902014.png)

例一：

![image-20220209171625282](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209171625282.png)

![image-20220209172109379](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209172109379.png)

例二：

![image-20220209173745603](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209173745603.png)

![image-20220209173827339](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209173827339.png)

 例三：

单独访问2

![image-20220209174938507](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209174938507.png)

例四：

关闭浏览器后 再打开单独访问2

![image-20220209175200725](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209175200725.png)

例五：

重启服务器后 再单独访问2

![image-20220209175231636](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209175231636.png)

能用小的 就不用大的。

![image-20220209180022958](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209180022958.png)

### 5.jsp中的out输出和response.getWriter输出的区别

先后区别

一般的HTML是out，就是空白区域写的。

默认out是会写在最后的。

你可以 手动执行out.flush() ，在resp输出代码前,加out.flush() ，输入页面就是你的代码顺序了。

![image-20220209181518298](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209181518298.png)

![image-20220209182211059](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209182211059.png)

### 6.out的两种输出，print源码是调用的write(String.valueOf)。

如果out.write('1') 会直接输出。

![image-20220209183807443](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220209183807443.png)

深入源码，浅出结论:在jsp页面中，可以统一使用out.print()来进行输出。

### 7.jsp常用标签

 #### （1）静态包含（目前 一般都使用静态）

一个项目里不同的页面会有很多相同的地方。同一管理，使用到了包含。

![image-20220210103520107](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210103520107.png)

![image-20220210103658816](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210103658816.png)

使用：

![image-20220210104502966](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210104502966.png)

footer.jsp:

![image-20220210104540224](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210104540224.png)

![image-20220210104621205](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210104621205.png)

本质：在.java文件中

原来的内容 拷贝到静态包含的位置，不会把被包含的jsp翻译成_jsp.java

![image-20220210104808825](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210104808825.png)

#### （2）动态包含

![image-20220210105159272](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210105159272.png)

![image-20220210111617326](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210111617326.png)

传递参数：

![image-20220210111656503](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210111656503.png)

![image-20220210111753251](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210111753251.png)

本质：

![image-20220210110314633](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210110314633.png)

![image-20220210111356838](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210111356838.png)

调用的同一个对象 所有out缓冲区一样，输出顺序也就和代码一样。

#### （3）转发

和之前的 一样。

![image-20220210112429756](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210112429756.png)

### 8.jsp的使用

jsp方便了数据的展示，使用Servlet回传数据麻烦。

把JSP理解成一个servlet程序就行了，jsp页面也是翻译成servlet程序的，记住jsp页面的作用：回传 html 页面的数据，代码可读性和可维护性比servlet程序更好

![image-20220210151030008](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210151030008.png)

![image-20220213180425254](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213180425254.png)

![image-20220213180511636](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213180511636.png)

**写完后访问这个Servlet**

### 9.listener监听器

![image-20220210152931363](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210152931363.png)

一共8个，只有这个还有用。

#### ServletContextL istener监听器

![image-20220210153329893](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210153329893.png)

![image-20220210153554491](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210153554491.png)

步骤：

![image-20220210153707556](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210153707556.png)

![image-20220210153850175](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210153850175.png)

注解 @WebListener 

学习Spring时会用到

## 十.EL表达式

**el替换了jsp的<%= %>**

![image-20220210154437341](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210154437341.png)

![image-20220210154817560](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210154817560.png)

如果key是没有set的：

![image-20220210154923399](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210154923399.png)v

![image-20220210155207025](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210155207025.png)

![image-20220210155253117](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210155253117.png)

先存在域里（或者传过来），再输出。

应该使用了jsp-api这个包

### 1.EL表达式搜索域数据的顺序

当四个域中都有相同的key的数据的时候，EL表达式会按照四个域的从小到大的顺序去进行搜索，找到就输出。

从小范围到大范围。**pageContext，request，session， application**

后面在第4点时，讲到了相同的key时，也可以指定某个域。

 ![image-20220210164020860](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210164020860.png)

### 2.c)EL表达式输出Bean的普通属性，数组属性。List 集合属性,map集合属性

EL表达式会根据name去User类里寻找这个name的get方法，此时会自动把name首字母大写并加上get前缀，一旦找到与之匹配的方法，El表达式就会认为这就是要访问的属性，并返回属性的值。

所以说：在Person对象里 只写一个get方法，不写类的成员变量都能使用！！

![image-20220210174324144](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210174324144.png)

还有一点注意：如果一个成员变量是boolean类型，自动生成的get方法 ，命名是is什么什么。 这样的话EL表达式会根据ok的类型，自动去找isOk 这个方法。

![image-20220213172320778](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213172320778.png)

![image-20220213172604194](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213172604194.png)

### 3.EL表达式—运算

![image-20220210195649962](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210195649962.png)

![image-20220210195955189](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210195955189.png)

![image-20220210200724308](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210200724308.png)

![image-20220210200701338](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210200701338.png)

![image-20220210201550499](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210201550499.png)

```html
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.Map" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <%
//        1、值为null值的时候，为空
        request.setAttribute("emptyNull", null);
//        2、值为空串的时候，为空
        request.setAttribute("emptyStr", "");
//        3、值是Object类型数组，长度为零的时候
        request.setAttribute("emptyArr", new Object[]{});
//        4、list集合，元素个数为零
        List<String> list = new ArrayList<>();
//        list.add("abc");
        request.setAttribute("emptyList", list);
//        5、map集合，元素个数为零
        Map<String,Object> map = new HashMap<String, Object>();
//        map.put("key1", "value1");
        request.setAttribute("emptyMap", map);
    %>
    ${ empty emptyNull } <br/>
    ${ empty emptyStr } <br/>
    ${ empty emptyArr } <br/>
    ${ empty emptyList } <br/>
    ${ empty emptyMap } <br/>

    <hr>
    ${ 12 != 12 ? "A":"B" }

</body>
</html>
```

![image-20220210204447469](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210204447469.png)

![image-20220210204757451](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220210204757451.png)

双引号 也可以

### 4.EL表达式的11个隐含对象

![image-20220211095636235](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211095636235.png)

![image-20220211095738603](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211095738603.png)

pageContext的使用：

![image-20220211105004035](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211105004035.png)

![image-20220211110210557](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211110210557.png)

两个一样：

为什么.scheme 就相当于.getScheme。因为输出数据时 会默认的找这个的get+首字母大写的那个方法

![image-20220211105233173](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211105233173.png)

4个域的使用：

![image-20220211103504695](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211103504695.png)

![image-20220211103439001](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211103439001.png)

param：

![image-20220211111359671](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211111359671.png)

![image-20220211111454217](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211111454217.png)

key 是String V是String[]

![image-20220211111941409](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211111941409.png)

![image-20220211112221622](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211112221622.png)

输出  wzg168

当遇到要获取复选框中的数据的场景时可以使用这个参数

![image-20220211112837572](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211112837572.png)

header:

取请求头的某一个  因为User-Agent有特殊字符 所以用大括号

![image-20220211152502223](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220211152502223.png)

cookie:

键是String类型。cookie.JSESSIONID就是找到了JSESSIONID的值，.name就是getName

![image-20220212103118098](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220212103118098.png)



initparam:

读取配置文件![image-20220212103035573](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220212103035573.png)

![image-20220212102843543](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220212102843543.png)

## 十一.JSTL标签库

**el替换jsp的 <%= %>，jstl替换jsp的 <% %>**

![image-20220212173324558](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220212173324558.png)

![image-20220212173913950](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220212173913950.png)

使用前先引入

![image-20220212174037801](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220212174037801.png)

![image-20220212194122164](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220212194122164.png)

### 1.set标签

![image-20220213114027584](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213114027584.png)

### 2.if标签

如果成立，则中间的输出

![image-20220213114401952](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213114401952.png)

### 3. ```<c:choose> <c:when> <c:otherwise>```标签

![image-20220213114910476](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213114910476.png)

![image-20220213150811117](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213150811117.png)

![image-20220213154008854](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213154008854.png)

### 4.forEach标签

#### 遍历1到10

![image-20220213154711117](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213154711117.png)

#### 遍历数组

![image-20220213155619437](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213155619437.png)

#### 遍历Map集合

![image-20220213160251066](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213160251066.png)

#### 遍历List集合

![image-20220213160747881](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213160747881.png)

![image-20220213162540691](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213162540691.png)

![image-20220213162801597](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213162801597.png)

####  forEach标签所有属性组合使用介绍

begin和end:

![image-20220213163344406](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213163344406.png)

![image-20220213163403622](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213163403622.png)

step:

![image-20220213163553238](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213163553238.png)

varStatus: 变量状态

分析源码可得：

可以使用的方法和作用：

![image-20220213172757823](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213172757823.png)

使用：status.Current ................................

![image-20220213171444418](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220213171444418.png)

如果一个成员变量是boolean类型，自动生成的get方法 ，命名是is什么什么。 这样的话EL表达式会根据first的类型，自动去找isFirst这个方法。

## 十二.Cookie

cookie 保存在客户端

![image-20220303193837252](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220303193837252.png)

#### 1.创建cookie

![image-20220303204522432](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220303204522432.png)

```java
protected void creatCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    Cookie cookie = new Cookie("key1", "value1");
    resp.addCookie(cookie);

    resp.getWriter().write("cookie创建成功");
}
```

![image-20220303210015344](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220303210015344.png)

#### 2.服务器获取cookie

![image-20220303213432229](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220303213432229.png)

```java
   protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        Cookie[] cookies = req.getCookies();
        Cookie iwantcookie =null;
        for (Cookie cookie : cookies) {
          //  resp.getWriter().write(cookie.getName()+"="+cookie.getValue()+"<br/>");
            if (cookie.getName().equals("key1")){
                iwantcookie=cookie;
            }
        }
        if (iwantcookie != null){
            resp.getWriter().write(iwantcookie.getValue());
        }
    }
```

通常写一个工具类：

```java
public class CookieUtils {
    public static Cookie findCookie(String name, Cookie[] cookies) {
        if (name == null || cookies.length == 0 || cookies == null) {
            return null;
        }
        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())) {
                return cookie;
            }
        }
        return null;
    }
}
```

#### 3.修改Cookie的值

方法一：（不常用）

相当于覆盖了

![image-20220304092510070](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304092510070.png)

方法二：

![image-20220304092644959](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304092644959.png)

#### 4.Cookie生命周期控制

![image-20220304094332714](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304094332714.png)

cookie.setMaxAge的默认值是-1 , 在控制台显示为Session

![image-20220304100527911](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304100527911.png)

key1.setMaxAge(0);马上删除, 响应里的细节

![image-20220304101159118](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304101159118.png)

这里生命截止时间 是格林时间（格林威治标准时间，GMT),  东八区[GMT+8]

![image-20220304103011501](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304103011501.png)

#### 5.Cookie的path属性

有效过滤cookie,路径满足条件 resp才会发送cookie

![image-20220304104759663](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304104759663.png)

默认值是当前路径

![image-20220304105241790](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304105241790.png)

#### 以上的测试代码

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="pragma" content="no-cache" />
<meta http-equiv="cache-control" content="no-cache" />
<meta http-equiv="Expires" content="0" />
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Cookie</title>
<style type="text/css">

   ul li {
      list-style: none;
   }
   
</style>
</head>
<body>
   <iframe name="target" width="500" height="500" style="float: left;"></iframe>
   <div style="float: left;">
      <ul>
         <li><a href="cookieServlet?action=creatCookie" target="target">Cookie的创建</a></li>
         <li><a href="cookieServlet?action=getCookie" target="target">Cookie的获取</a></li>
         <li><a href="cookieServlet?action=updateCookie" target="target">Cookie值的修改</a></li>
         <li>Cookie的存活周期</li>
         <li>
            <ul>
               <li><a href="cookieServlet?action=defaultLife" target="target">Cookie的默认存活时间（会话）</a></li>
               <li><a href="cookieServlet?action=deleteNow" target="target">Cookie立即删除</a></li>
               <li><a href="cookieServlet?action=lift3600" target="target">Cookie存活3600秒（1小时）</a></li>
            </ul>
         </li>
         <li><a href="cookieServlet?action=testPath" target="target">Cookie的路径设置</a></li>
         <li><a href="" target="target">Cookie的用户免登录练习</a></li>
      </ul>
   </div>
</body>
</html>
```

```java
@WebServlet(name = "CookieServlet", value = "/cookieServlet")
public class CookieServlet extends BaseServlet{
    protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        Cookie[] cookies = req.getCookies();
        Cookie iwantcookie =null;
        for (Cookie cookie : cookies) {
          //  resp.getWriter().write(cookie.getName()+"="+cookie.getValue()+"<br/>");
            if (cookie.getName().equals("key1")){
                iwantcookie=cookie;
            }
        }
        if (iwantcookie != null){
            resp.getWriter().write(iwantcookie.getValue());
        }
    }
    protected void creatCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("key1", "value1");
        resp.addCookie(cookie);

        resp.getWriter().write("cookie创建成功");
    }
    protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie key1 = CookieUtils.findCookie("key1", req.getCookies());
        key1.setValue("newValue1");
        //通知客户端保存
        resp.addCookie(key1);
    }
    protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("keyLife", "ValueLift");
        cookie.setMaxAge(-1);//cookie 的默认值就是这个，表示浏览器一关cookie就会被删除
        resp.addCookie(cookie);
    }
    protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie key1 = CookieUtils.findCookie("key1", req.getCookies());
        key1.setMaxAge(0); //马上删除
        resp.addCookie(key1);
        resp.getWriter().write("key1已经删除");
    }

    protected void lift3600(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("lift3600", "lift3600");
        cookie.setMaxAge(60*60);//一个小时后删除
        resp.addCookie(cookie);
        resp.getWriter().write("创建了一个存活时间是1小时的cookie");
    }

    protected void testPath(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("testPath", "testPath");
        //getContextPath 获取工程路径
        cookie.setPath(req.getContextPath()+"/abc");
        resp.addCookie(cookie);
        resp.getWriter().write("创建了一个有路径的cookie");
    }
}
```

```java
{
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    protected void doPost(HttpServletRequest req, HttpServletResponse resp)  {



        String action = req.getParameter("action");
        try {
            req.setCharacterEncoding("utf-8");
            //解决中文乱码问题
            resp.setContentType("text/html; charset=utf-8");
            // 获取action业务鉴别字符串，获取相应的业务 方法反射对象
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class, HttpServletResponse.class);
            //System.out.println(method);
            // 调用目标业务 方法
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}
```

#### 6.练习——免用户名登入

![image-20220304191714515](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304191714515.png)

```java
package com.example.cookie_session.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name = "LoginServlet", value = "/loginServlet")
public class LoginServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        if ("admin".equals(username) && "123456".equals(password)) {
            //登录 成功
            Cookie cookie = new Cookie("username", username);
            cookie.setMaxAge(60 * 60 * 24 * 7);//当前Cookie一周内有效
            resp.addCookie(cookie);
            System.out.println("登录 成功11");
        } else {
//            登录 失败
            System.out.println("登录 失败");
        }

    }
}
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="http://localhost:8080/cookie_session/loginServlet" method="get">
        用户名：<input type="text" name="username" value="${cookie.username.value}"> <br>
        密码：<input type="password" name="password"> <br>
        <input type="submit" value="登录">
    </form>
</body>
</html>
```

## 十三.Session

cookie 保存在客户端  session保存在服务器

![image-20220304170154969](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304170154969.png)

#### 1.获取和创建

![image-20220304170838475](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304170838475.png)

#### 2.session域数据存取

#### 3.session生命周期控制

session的默认生命周期在tomcat的web.xml 文件中配置过了。一般是30分钟。

![image-20220304181039348](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304181039348.png)

![image-20220304181123583](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304181123583.png)

![image-20220304181208774](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304181208774.png)

修改单独session:

```java
// 先获取Session对象
HttpSession session = req.getSession();
// 设置当前Session3秒后超时
session.setMaxInactiveInterval(3);
```

![image-20220304181839998](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304181839998.png)

![image-20220304181940337](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304181940337.png)

相当于 cookie的0，马上删除。

+ 一直点，一直创建新的Session

![image-20220304181746804](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304181746804.png)

```Java
public class SessionServlet extends BaseServlet {
    /**
     * 往Session中保存数据
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void setAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getSession().setAttribute("key1", "value1");
        resp.getWriter().write("已经往Session中保存了数据");

    }

    protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取了Session的默认超时时长
        int maxInactiveInterval = req.getSession().getMaxInactiveInterval();

        resp.getWriter().write("Session的默认超时时长为：" + maxInactiveInterval + " 秒 ");

    }

    protected void life3(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 先获取Session对象
        HttpSession session = req.getSession();
        // 设置当前Session3秒后超时
        session.setMaxInactiveInterval(3);

        resp.getWriter().write("当前Session已经设置为3秒后超时");
    }

    protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 先获取Session对象
        HttpSession session = req.getSession();
        // 让Session会话马上超时
        session.invalidate();

        resp.getWriter().write("Session已经设置为超时（无效）");
    }

    /**
     * 获取Session域中的数据
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void getAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Object attribute = req.getSession().getAttribute("key1");
        resp.getWriter().write("从Session中获取出key1的数据是：" + attribute);
    }



    protected void createOrGetSession(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 创建和获取Session会话对象
        HttpSession session = req.getSession();
        // 判断 当前Session会话，是否是新创建出来的
        boolean isNew = session.isNew();
        // 获取Session会话的唯一标识 id
        String id = session.getId();

        resp.getWriter().write("得到的Session，它的id是：" + id + " <br /> ");
        resp.getWriter().write("这个Session是否是新创建的：" + isNew + " <br /> ");
    }
}
```

#### 4.浏览器和Session之间关联的技术

Session技术，底层其实是基于Cookie技术来实现的。

![image-20220304192649228](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304192649228.png)

所以浏览器关闭后这个session就找不到了，因为这个session对应的cookie已经删除，cookie默认在浏览器关闭后删除。

![image-20220304214853919](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220304214853919.png)

Tomcat内存中还有上次session 的id对应的session内容 但是新的的请求本身没有id值（cookie中JSESSION）,所以Tomcat重新创建  所有session要有超时时间。

cookie用来自动登录，session用来保持登录状态

因为http是无状态的，没法保存信息，但每次浏览器请求数据时，可能需要携带大量重复信息

所以就需要cookie和session来存储这些信息【比如登陆的用户信息等】

其中cookie是客户端用于记录，session用于服务端记录

## 十四.Filter

![image-20220305160643394](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305160643394.png)

#### 1.使用

![image-20220305161011925](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305161011925.png)

![image-20220305163127065](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305163127065.png)

```java
package com.donggei.servlet;

import javax.servlet.*;

import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class AdminFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    /**
     * doFilter方法，专门用于拦截请求。可以做权限检查
     * @param request
     * @param response
     * @param chain
     * @throws IOException
     * @throws ServletException
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest) request;
        Object user = req.getSession().getAttribute("user");
        if (user == null){
            //说明没有登入
            req.getRequestDispatcher("login").forward(request,response);
        }else {
           //这行代码很重要 没有得话将不会继续访问应该访问的页面
            //让程序继续往下访问用户的目标资源
            chain.doFilter(request,response);
        }
    }

    @Override
    public void destroy() {

    }
}
```

```xml
<filter>
        <filter-name>AdminFilter</filter-name>
        <filter-class>com.donggei.servlet.AdminFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>AdminFilter</filter-name>
        <url-pattern>/a.jsp</url-pattern>
        <url-pattern>/manager</url-pattern>
    </filter-mapping>
```

规定那些路径要检查，检查的Filter类是那个。

![image-20220305165411440](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305165411440.png)

![image-20220305165808793](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305165808793.png)

#### 2.生命周期

![image-20220305171029225](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305171029225.png)

#### 3.FilterConfig类

![image-20220305171456217](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305171456217.png)

![image-20220305171821496](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305171821496.png)

![image-20220305171639399](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305171639399.png)

#### 4.FilteChain类

chain：链   过滤器链（多个过滤器如何工作）

![image-20220305172356199](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305172356199.png)

![image-20220305172924710](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305172924710.png)

![image-20220305173124931](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305173124931.png)

#### 6.Filter的拦截路径

精确匹配

![image-20220305174017603](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305174017603.png)

目录匹配

![image-20220305174024550](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305174024550.png)

后缀名匹配

![image-20220305174638965](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305174638965.png)

例:

![image-20220305174308257](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305174308257.png)

## 十五.事务管理

### 1.知识储备

如果忘记了事务的操作 点击：[java中的数据库事务处理](https://www.cnblogs.com/zzzzw/p/4869334.html)

事务管理遇到的问题：

【图是Controller 就是Servlet】 

![image-20220306151920252](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220306151920252.png)

解决方法：

![image-20220306152554571](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220306152554571.png)

过滤器的典型应用：

![image-20220306153108096](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220306153108096.png)

![image-20220306153553607](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220306153553607.png)

![image-20220306154323753](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220306154323753.png)

### 2.Threadlocal

一个线程绑定了一个值，单独线程内不管怎么操作不会影响这个线程的数据传递



![image-20220305190321498](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220305190321498.png)

### 3.例子：

服务层出错，抛到了web（Servlet）层

```java
public abstract class BaseDao {

    //使用DbUtils操作数据库
    private QueryRunner queryRunner = new QueryRunner();

    /**
     * update() 方法用来执行：Insert\Update\Delete语句
     *
     * @return 如果返回-1,说明执行失败<br/>返回其他表示影响的行数
     */
    public int update(String sql, Object... args) {
        Connection connection = JdbcUtils.getConnection();
        try {
            return queryRunner.update(connection, sql, args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }

    /**
     * 查询返回一个javaBean的sql语句
     *
     * @param type 返回的对象类型
     * @param sql  执行的sql语句
     * @param args sql对应的参数值
     * @param <T>  返回的类型的泛型
     * @return
     */
    public <T> T queryForOne(Class<T> type, String sql, Object... args) {
        Connection con = JdbcUtils.getConnection();
        try {
            return queryRunner.query(con, sql, new BeanHandler<T>(type), args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }

    }
    /**
     * 查询返回多个javaBean的sql语句
     *
     * @param type 返回的对象类型
     * @param sql  执行的sql语句
     * @param args sql对应的参数值
     * @param <T>  返回的类型的泛型
     * @return
     */
    public <T> List<T> queryForList(Class<T> type, String sql, Object... args) {
        Connection con = JdbcUtils.getConnection();
        try {
            return queryRunner.query(con, sql, new BeanListHandler<T>(type), args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
    /**
     * 执行返回一行一列的sql语句
     * @param sql   执行的sql语句
     * @param args  sql对应的参数值
     * @return
     */
    public Object queryForSingleValue(String sql, Object... args){

        Connection conn = JdbcUtils.getConnection();

        try {
            return queryRunner.query(conn, sql, new ScalarHandler(), args);
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }

    }

}
```

```java
 try {
            orderId = orderService.createOrder(cart, userId);
            JdbcUtils.commitAndClose();
        } catch (Exception e) {
            JdbcUtils.rollbackAndClose();
            e.printStackTrace();
        }
```

只是对dao层的Base进行了修改，继承了BaseDao的DaoImpl类，执行数据库操作时如果发生了异常，就会先高层Service抛出

RuntimeException（运行时异常一般是不用捕捉的，我们这里对这个Service进行了捕捉）,发生异常时就回滚，整个Service结束后才提交事务。

Runtime异常会自己先上抛出；需要我们手动捕捉

![image-20220306212914360](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220306212914360.png)



```java
public class JdbcUtils {

    private static DruidDataSource dataSource;
    private static ThreadLocal<Connection> conns = new ThreadLocal<Connection>();

    static {
        try {
            Properties properties = new Properties();
            // 读取 jdbc.properties属性配置文件
            InputStream inputStream = JdbcUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
            // 从流中加载数据
            properties.load(inputStream);
            // 创建 数据库连接 池
            dataSource = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取数据库连接池中的连接
     * @return 如果返回null,说明获取连接失败<br/>有值就是获取连接成功
     */
    public static Connection getConnection(){
        Connection conn = conns.get();
        if (conn == null) {
            try {
                conn = dataSource.getConnection();//从数据库连接池中获取连接
                conns.set(conn); // 保存到ThreadLocal对象中，供后面的jdbc操作使用
                conn.setAutoCommit(false); // 设置为手动管理事务
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return conn;
    }

    /**
     * 提交事务，并关闭释放连接
     */
    public static void commitAndClose(){
        Connection connection = conns.get();
        if (connection != null) { // 如果不等于null，说明 之前使用过连接，操作过数据库
            try {
                connection.commit(); // 提交 事务
            } catch (SQLException e) {
                e.printStackTrace();
            } finally {
                try {
                    connection.close(); // 关闭连接，资源资源
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        // 一定要执行remove操作，否则就会出错。（因为Tomcat服务器底层使用了线程池技术）
        conns.remove();
    }

    /**
     * 回滚事务，并关闭释放连接
     */
    public static void rollbackAndClose(){
        Connection connection = conns.get();
        if (connection != null) { // 如果不等于null，说明 之前使用过连接，操作过数据库
            try {
                connection.rollback();//回滚事务
            } catch (SQLException e) {
                e.printStackTrace();
            } finally {
                try {
                    connection.close(); // 关闭连接，资源资源
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        // 一定要执行remove操作，否则就会出错。（因为Tomcat服务器底层使用了线程池技术）
        conns.remove();
    }
}
```

### 4.实际项目中的应用

例子中的方法 很笨拙，每次用Service都需要异常处理

使用Filter过滤器统一给所有的Service方法都加上try-catch。来进行实现的管理。

![image-20220307141550021](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307141550021.png)

创建事务过滤器

```java
public class TransactionFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        try {
            filterChain.doFilter(servletRequest,servletResponse);
            JdbcUtils.commitAndClose();// 提交事务
        } catch (Exception e) {
            JdbcUtils.rollbackAndClose();//回滚事务
            e.printStackTrace();
           	
            //把异常抛给Tomcat管理展示友好的错误页面
            throw new RuntimeException(e);
            
        }
    }

    @Override
    public void destroy() {

    }
}
```

所有请求都加上了事务管理

```xml
<filter>
    <filter-name>TransactionFilter</filter-name>
    <filter-class>com.donggei.filter.TransactionFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>TransactionFilter</filter-name>
    <!-- /* 表示当前工程下所有请求 -->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

**特别注意的是**：如果 从 BaseDao 一层一层向外抛的过程中 有try..catch..的地方 还要再向外抛出

eg:BaseServlet中 

```java
public abstract class BaseServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    protected void doPost(HttpServletRequest req, HttpServletResponse resp)  {
        String action = req.getParameter("action");
        try {
            // 获取action业务鉴别字符串，获取相应的业务 方法反射对象
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class, HttpServletResponse.class);
            //System.out.println(method);
            // 调用目标业务 方法
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
            //  ！！增加的部分！！
            // 向上抛给filter
            throw new RuntimeException(e);
        }
    }

}
```

#### 错误页面

```java
  try {
            filterChain.doFilter(servletRequest,servletResponse);
            JdbcUtils.commitAndClose();// 提交事务
        } catch (Exception e) {
            JdbcUtils.rollbackAndClose();//回滚事务
            e.printStackTrace();
           	
            //把异常抛给Tomcat管理展示友好的错误页面
           throw new RuntimeException(e);
            
        }
```

然后再在web.xml配置中

```xml
<!--error-page标签配置，服务器出错之后，自动跳转的页面-->
<error-page>
    <!--error-code表示错误类型  是固定的-->
    <error-code>500</error-code>
    <!--location标签表示。要跳转去的页面路径-->
    <location>/pages/error/error500.jsp</location>
</error-page>

<!--error-page标签配置，服务器出错之后，自动跳转的页面-->
<error-page>
    <!--error-code是错误类型-->
    <error-code>404</error-code>
    <!--location标签表示。要跳转去的页面路径-->
    <location>/pages/error/error404.jsp</location>
</error-page>
```

## 十六.监听器—Listener

[相关教程地址](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/)

![image-20220308110815997](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20220308110815997.png)


> ①ServletContextListener
> 作用：监听ServletContext对象的创建与销毁
>
> ​	tomcat容器启动时 就会创建Servlet上下文对象
>
> ​	tomcat关闭时销毁
>
> ②HttpSessionListener
> 作用：监听HttpSession对象的创建与销毁
> ③ServletRequestListener
> 作用：监听ServletRequest对象的创建与销毁

> ④ServletContextAttributeListener
> 作用：监听ServletContext中属性的创建、修改和销毁
> ⑤HttpSessionAttributeListener
> 作用：监听HttpSession中属性的创建、修改和销毁
> ⑥ServletRequestAttributeListener
> 作用：监听ServletRequest中属性的创建、修改和销毁

> ⑦HttpSessionBindingListener
> 作用：监听某个对象在Session域中的创建与移除
> ⑧HttpSessionActivationListener
> 作用：监听某个对象在Session中的序列化与反序列化。
>
> 序列化:钝化到磁盘时              反序列化:活化到内存时

![image-20220308125526434](D:\学习\截图\image-20220308125526434-16484793857311.png)

![image-20220308125614662](D:\学习\截图\image-20220308125614662.png)





## 十七.JSON

![image-20220307183305401](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307183305401.png)

### 1.定义

![image-20220307184407356](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307184407356.png)

### 2.访问

alert(typeof(jsonObj));// object  json就是一个对象

> json本身是一个对象。
> json中的key我们可以理解为是对象中的一个属性。
> json中的key访问就跟访问对象的属性一样: json 对象.key

![image-20220307185438584](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307185438584.png)

下面例子中和上面定义时是相同的

json中的数组 和 arr 是一样的，也有相同的操作

![image-20220307185542229](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307185542229.png)

值是数组的取法

![image-20220307185746273](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307185746273.png)

值是 一个json的取法

![image-20220307221811919](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307221811919.png)

值是一个 json数组的情况

![image-20220307222456942](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307222456942.png)

### 3.json的两个常用方法

 json的存在有两种形式。

+ 一种是:对象的形式存在，我们叫它json对象。
+ 一种是:字符串的形式存在，我们叫它json字符串。

> 一般我们要操作json中的数据的时候，需要json对象的格式。
> 一般我们要 在客户端和服务器之间进行数据交换的时候，使用json字符串。
>
> JSON.stringify()把json对象转换成为json字符串
> JSON.parse()把json字符串转换成为json对象

![image-20220307225731756](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307225731756.png)

就是没有格式的 没有换行的 直接输出 原本的内容

![image-20220307230443502](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307230443502.png)

### 4.JSON在java中的使用

   使用了google的Gson![image-20220307231106153](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220307231106153.png)

```java
package com.atguigu.pojo;

public class Person {

    private Integer id;
    private String name;

    public Person() {
    }

    public Person(Integer id, String name) {
        this.id = id;
        this.name = name;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}

```



+ javaBean 和json的互转

  ```java
   @Test
      public void test1(){
          Person person = new Person(1,"好帅!"); 
          // 创建Gson对象实例
          Gson gson = new Gson();
          // toJson方法可以把java对象转换成为json字符串
          String personJsonString = gson.toJson(person); //{"id":1,"name":"好帅!"}
          System.out.println(personJsonString);
          // fromJson把json字符串转换回Java对象
          // 第一个参数是json字符串
          // 第二个参数是转换回去原来的Java对象类型
          Person person1 = gson.fromJson(personJsonString, Person.class);
          System.out.println(person1);
      }
  ```

+ List 和json的互转

  先写一个空的类 需要基础com.google.gson.reflect.TypeToken，并且传入确定的参数 ，后面转化时的type需要用到（这个方法可以）

  ```java
  package com.atguigu.json;
  
  import com.atguigu.pojo.Person;
  import com.google.gson.reflect.TypeToken;
  
  import java.util.ArrayList;
  
  public class PersonListType extends TypeToken<ArrayList<Person>> {
  }
  ```

  ```java
  public void test2() {
          List<Person> personList = new ArrayList<>();
  
          personList.add(new Person(1, "AA"));
          personList.add(new Person(2, "BB"));
  
          Gson gson = new Gson();
  
          // 把List转换为json字符串
          String personListJsonString = gson.toJson(personList);
          System.out.println(personListJsonString);
  		//[{"id":1,"name": "AA"},{"id":2,"name":"BB"}]
          List<Person> list = gson.fromJson(personListJsonString, new PersonListType().getType());
          System.out.println(list);
          Person person = list.get(0);
          System.out.println(person);
      }
  
  
  
  ```

+ map和json的互转

  也可以写成 匿名内部类

  ```java
  package com.atguigu.json;
  
  import com.atguigu.pojo.Person;
  import com.google.gson.reflect.TypeToken;
  
  import java.util.HashMap;
  
  public class PersonMapType extends TypeToken<HashMap<Integer, Person>> {
  }
  ```

  ```java
  @Test
      public void test3(){
          Map<Integer,Person> personMap = new HashMap<>();
  
          personMap.put(1, new Person(1, "A好帅"));
          personMap.put(2, new Person(2, "B也好帅"));
  
          Gson gson = new Gson();
          // 把 map 集合转换成为 json字符串
          String personMapJsonString = gson.toJson(personMap); 
          System.out.println(personMapJsonString);
  //{"1":{"id":1,"name":"国哥好帅"},"2":{"id":2,"name":"康师傅也好帅"}}
  
  
  //        Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new PersonMapType().getType());
          Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new TypeToken<HashMap<Integer,Person>>(){}.getType());
  
          System.out.println(personMap2);
          Person p = personMap2.get(1);
          System.out.println(p);
  
      }
  ```

  













## 文件上传

![image-20220214103144423](D:\学习\截图\image-20220214103144423.png)

![image-20220214112643221](D:\学习\截图\image-20220214112643221.png)

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  <form action="http://localhost:8080/servlet_demo01/uploadServlet" method="post" enctype="multipart/form-data">
    用户名: <input type="text" name="username"><br>
      头像:<input type="file" name="photo"><br>
      <input type="submit" value="上传">
  </form>

</body>
</html>
```

```java
@WebServlet(name = "UploadServlet", value = "/uploadServlet")
public class UploadServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("上传");
    }
    
}
```

![image-20220214133131469](D:\学习\截图\image-20220214133131469.png)

提交表单后：

```java
@WebServlet(name = "UploadServlet", value = "/uploadServlet")
public class UploadServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //multipart把多个类型的标签数据都合到一起了，都是二进制流传输的
        System.out.println("上传");
        ServletInputStream inputStream = req.getInputStream();
        byte[] buffer =new byte[1024000];
        int read = inputStream.read(buffer);
        System.out.println(new String(buffer, 0, read));
        //读取的就是请求头  
        //-----------------------------372510948234374483034027680384
        //Content-Disposition: form-data; name="username"
        //
        //das
        //-----------------------------372510948234374483034027680384
        //Content-Disposition: form-data; name="photo"; filename="20210803085403.png"
        //Content-Type: image/png
        //
        //�PNG
        //
        //IHDR  r     K�H�    IDATx��Y�Ǖ&x�c���3$�� A�%Q�K��T��ں�a����L��z�1˙�9�R3g��K�]��,I��*�I��w 3������]wO�DI@��
        //D�/�\�f��>�ׂ��?GQ��P�P�X�R�B�(�V�M�v���{�VVVh}}�i``�*�
        //i���bU|v����Y(�r=����'�/ԁ�tccC��z�NCCCv��������������������xT����8��@F0�!
        //%�3�O�|�ľ�.l,�d�a��8!Q_�%��6�%�^;�M?�>.//���cccT)W�@�z���*P�}v�a�~�7��@]�W�����w�2H��7�~Z�$p�Kz��S�^S�ݦ_��]z�ͷ��w���^�:M��-bE�m��]j57iqqQ��h4�om�֯6��>��WO�9�r��>!�]rي,�[���
        //��9����&�G�����i��͡�C$������Xi�>@�z�:}Ȍ���hE��w�M; 6:%����"w� �߁�U��=c3ҿ@�;��9��R;�����U�Nm��mA��G�L}3��DpH�P^��!�0��C���!:��ie1���  �n2J��'�Y������F(D�L�0Y��g
        //bO�������!$�C*q�S�C��ũ�b�Hz�L*��L#�LY�=1��.A�QxJ�%O�� �RID�DJ9Ԥ̤�.i�2v�'�k�DVtJ�p �{ւ���A��c�F�.D7��w� @��T��hlb�������@�J�C�f�F�eܳdvR�K���;����z��;u|lˣ{֙� zD5m{���{�繭R�<���������2B�]14Pd{���Y�^��3���A�|>���¨�q"G��;U�J��*6�mW[��a��!Eǎ<���������s���Ƚ3NE 3c��yD�$GΝa�X�%[ǳ�?�
        //A/)�嚒   On#��!�tb��a*�'RE���������� N���Yο잀�cRe��mp�t��YR�U�BN�O�Ѓk�䫹 �� �����p�#�vۀ{t�˾}���#7�WM�+�t�
        //�e�l6�>�Iw��M�Cڧ��v�M��rO��$�>׺v�* ��J����������w�HD�H�muؾ8W.�Ѩ�X޻i�V�L�l�j
        //�n�c�(���VWd^�k�z
        //ݍ��Y��$�A�����c��ї�ۂ�l�+�\ޑ ���x���%i����%FJ-���m\Rʼ�4�2q�J��#��KD�?�Ff��H�  ��� ޹{%�;�|�K��������򾧜��%?3��/aݏ�P4i�����-$�=���V�Kss�2F�'U^4QH�A�L1�E��zDRL#E���rr�wxP!g�bOF ��=B�F�U�>҈��E�l�bz<OvK{���j��>����@�O���  ��ǎ[N��U�����
        //`AZ��D����#����}wm�؂�ߍ�����+���u ��m�MW�9�L��ۡ8>>!bg?����]�aٷ�c����o.����9���9yW��_�Gy$����[K�
        //Sw�1����b��$*(�nr6�0
        //���qY]����v�u������G�Uu�*�N�!R"ݝ;&u� d궾��������S�>�*?�Z^E����p�5} �))���+�n�Xv���ǷU�WO
        //<��s��x���F\�k��C\�W*�d�b�D�r���cw�q(q'E�v6�@*�Ɓ��F�@ȉzj�i���>]�3vǊ6gŵ^1��ǣ�:���F�P�j5J�/��3��ܟ��F�d�\qb�ay�mu?蕞�>�n�2�]<���֧��;Vۋ��е(�D����B����������������OL�����`g{�x|�Ȏ�+�<~$X^�,�Q;��i�����'a7v�ҬjM���V�"���� ����H��������F����>�R��0�l��{Q6�����=�Oj�]�ot�do��<�Bj�I��`��M��bG��U����?�󮉷��=�6P�T�����F{xxxxxxxxxxxxxx<��4��I�B���O2�q<�����#<>^d����MI�q�-/��ݟ����vJP�b�{+�T��@�J5��ʤ�K��D\^D]D�t#
        //
        //%>U�I�y��8�у�7Aأ� �~��4��h�gD��GLU�G�jB����4w�O��_�4��&E`��:BN��u�&�O�f�l�^��wb���o_C::A�+�m�&t����_�G:�0g��~��r瑇���������������OD��RiB���qދǹ�Y!�a��aڵ��EYQ������#HD�8
        // �h*����m�<
        //��H�l�����=>�t׫ldN�\�|N�}`����&����_�X�TT+��,"����<t�O�  ����]�{kF��2��+]!'/�aVl����إ6Hg|����s�S��a�G���5}�~�<<<<<<<<<<<<<<</|b��Fv}���o���yl��>=�t�pR*�$-�{�N���|X(��p��û�+�{4=�v}�kQ����ASC���K����k�H�   �Ѳ)���miw�
        //�6!u����->Վ�>qHQЕ(�B1�ՍUZ_��z�K��
        //��`��u:|����)}��G����żs��,���oympm
        //��/����4����3W����hrl��kpGƽ����t��%��7��D<���y���Ȯ��C�|w�ɖ�u���ޣ��9�?�y=�9�J��������(yg������ۮl���m��!
        //���7� �m�Q�i��N����0���;"@��-Ps�T�FFF���}T���  |iehĦl�U�̆m!�hd    l��q�
        //DD# �l��h�xqa������jS���@�G=�N[�����CIݮ���ʕ�"�@�2)�L91E����9���ZnV<H�i��@�d__[������H���ѱ��i��'-W$�΅��鎏;����\�;O�L�)� }�IR���"��ܦ�+�Ҏ���ٍ�&�/l��lp�� d_n���2�V[��z���{���w��%�WW.ߠՕ6�� �E{��H�C�419&�H�.�72"+4�?�.�?��:t��E{�E�Y��Y�15y�F�fX۰���(�ʕ+��_�J"���E��[�pV<q9�/U��4�x�rΞ��ח�v��m۔.��V:*h�&����o�z��u�T�N�C�<�_�_EH�ڷ_�l�|�_E
     

    }

}
```

对流的处理：

导包：

![image-20220214134805002](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220214134805002.png)

```xml
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.3.1</version>
</dependency>
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.4</version>
</dependency>
```

![image-20220214135141449](D:\学习\截图\image-20220214135141449.png)

![image-20220214135305519](D:\学习\截图\image-20220214135305519.png)

```java
import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileItemFactory;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.util.List;
@WebServlet(name = "UploadServlet", value = "/uploadServlet")
public class UploadServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1 判断上传的数据是否是多段的，多段的应该是上传文件的
        if (ServletFileUpload.isMultipartContent(req)){
            //创建fileItemFactory工厂实现类
            FileItemFactory fileItemFactory =new DiskFileItemFactory();
            //创建用于解析上传数据的工具类servletFileUpload
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);

            try {
                //解析上传的数据，得到每一个表单项FileItem
                List<FileItem> fileItems = servletFileUpload.parseRequest(req);
                //循环判断，每一个表单项，是普通类型，还是上传的文件
                for (FileItem fileItem : fileItems) {
                    if (fileItem.isFormField()){
                        //普通
                        System.out.println("表单项的name属性值:"+fileItem.getFieldName());
                        //参数防止乱码
                        System.out.println("表单项的value属性值:"+fileItem.getString("utf-8"));
                    }else {
                        System.out.println("表单项的name属性值:"+fileItem.getFieldName());
                        System.out.println("上传的文件名:"+fileItem.getName());
                        fileItem.write(new File("e:\\"+fileItem.getName()));
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }

        }
    }

}
```

## 



## 文件下载

![image-20220214150017071](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220214150017071.png)

![image-20220214154644527](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220214154644527.png)

```java
@WebServlet(name = "DownloadServlet", value = "/downloadServlet")
public class DownloadServlet extends HelloServlet{
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1、获取要下载的文件名
        String downloadFileName = "abc.png";
        //2、 读取要下载的文件内容
        ServletContext servletContext = getServletContext();
        InputStream resourceAsStream = servletContext.getResourceAsStream("/file/"+downloadFileName);

        //获取要下载文件的类型
        String mimeType = servletContext.getMimeType("/file/" + downloadFileName);

        //4、 在回传前，通过响应头告诉客户端返回的数据类型
        resp.setContentType(mimeType);
        //5、 还要告诉客户端收到的数据是用于下载使用(还是使用响应头)
        //filename表示指定下载的文件名  下载的文件名是在响应头里设置的
        resp.setHeader("Content-Disposition","attachment;filename= "+downloadFileName);
        //3、 把下载的文件内容回传给客户端
        OutputStream outputStream = resp.getOutputStream();
        IOUtils.copy(resourceAsStream,outputStream);
    }
}
```

url编码 解决中文下载名乱码：

http协议没有支持中文

![image-20220224110210639](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220224110210639.png)

![image-20220224110408753](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220224110408753.png)

以前火狐需要使用下面的方法（现在URLEncoder也行）

![image-20220224113859341](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220224113859341.png)

## 练习—书城项目

### 阶段1

### 用户登入注册

知识准备：

javaEE三层架构

![image-20220207105008034](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207105008034.png)



#### 1.创建需要的数据库和表

```mysql
USE book_donggei;
CREATE TABLE t_user(
	`id` INT PRIMARY KEY AUTO_INCREMENT,
	`username` VARCHAR(20) NOT NULL UNIQUE,
	`password` VARCHAR(32)NOT NULL,
	`email` VARCHAR(200)
);
INSERT INTO t_user(`username`,`password`,`email`)VALUES('admin','admin','admin@donggei.com');
SELECT * FROM t_user;
```

#### 2.编写数据库表对应的JavaBean对象

```Java
public class User {
    private Integer id;
    private String username;
    private String password;
    private String email;
```

#### 3.编写工具类JDBCUtils

写dao前需要编写工具类

```java
import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

/**
 * @className: JdbcUtils
 * @description: TODO 类描述
 * @author: Dong
 * @date: 2022/2/7
 **/
public class JdbcUtils {
    private static DruidDataSource dataSource;
    static {
        try {
            Properties properties =new Properties();
            InputStream inputStream = JdbcUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
            properties.load(inputStream);
            dataSource = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取数据库连接池中的连接
     * @return 如果返回null,说明获取连接失败<br/>有值就是获取连接成功
     */
    public static Connection getConnection(){
        Connection connection =null;
        try {
            connection =dataSource.getConnection();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return connection;

    }

    /**
     * 关闭连接
     * @param connection
     */
    public static void close(Connection connection){
        if (connection != null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```



#### 4.编写Dao持久层

[JDBC框架——DBUtils](https://www.jianshu.com/p/10241754cdd7)

**先写BaseDao**

```java
import com.donggei.utils.JdbcUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

/**
 * @className: BaseDao
 * @description: TODO 类描述
 * @author: Dong
 * @date: 2022/2/7
 **/
public abstract class BaseDao {
    private QueryRunner queryRunner =new QueryRunner();

    /**
     * update() 方法用来执行：Insert\Update\Delete语句
     * @return 如果返回-1,说明执行失败<br/>返回其他表示影响的行数
     */
    public int update(String sql,Object ...args){
        Connection conn = JdbcUtils.getConnection();

        try {
            return queryRunner.update(conn,sql,args);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.close(conn);
        }
        return -1;
    }
    /**
     * 查询返回一个javaBean的sql语句
     *
     * @param type 返回的对象类型
     * @param sql  执行的sql语句
     * @param args sql对应的参数值
     * @param <T>  返回的类型的泛型
     * @return
     */
    public<T> T  queryForOne(Class<T> type,String sql,Object ...args){
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn,sql,new BeanHandler<T>(type),args);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.close(conn);
        }
        return null;
    }
    /**
     * 查询返回多个javaBean的sql语句
     *
     * @param type 返回的对象类型
     * @param sql  执行的sql语句
     * @param args sql对应的参数值
     * @param <T>  返回的类型的泛型
     * @return
     */
    public <T> List<T> queryForList(Class<T> type,String sql,Object ...args){
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn,sql,new BeanListHandler<T>(type),args);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.close(conn);
        }
        return null;
    }
    /**
     * 执行返回一行一列的sql语句
     * @param sql   执行的sql语句
     * @param args  sql对应的参数值
     * @return
     */
    public Object queryForSingleValue(String sql,Object... args){
        Connection conn = JdbcUtils.getConnection();
        try {
          return   queryRunner.query(conn,sql,new ScalarHandler(),args);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.close(conn);
        }
        return null;

    }
}
```

写具体Dao

```java
public interface UserDao {

    /**
     * 根据用户名查询用户信息
     * @param username 用户名
     * @return 如果返回null,说明没有这个用户。没有这个用户才可以注册。
     */
    public abstract User queryUserByUsername(String username);
    /**
     * 保存用户信息
     * @param user
     * @return 返回-1表示操作失败，其他是sql语句影响的行数
     */
    public int saveUser(User user);
    /**
     * 根据 用户名和密码查询用户信息
     * @param username
     * @param password
     * @return 如果返回null,说明用户名或密码错误,反之亦然
     */
    public User queryUserByUsernameAndPassword(String username,String password);
}



public class UserDaoImpl extends BaseDao implements UserDao {

    @Override
    public User queryUserByUsername(String username) {
        String sql = "select `id`,`username`,`password`,`email`from t_user where username= ?";
        return queryForOne(User.class,sql,username);
    }

    @Override
    public int saveUser(User user) {
        String sql ="insert into t_user(`username`,`password`,`email`)values(?,?,?,?)";
        return update(sql,user.getUsername(),user.getPassword(),user.getEmail());
    }

    @Override
    public User queryUserByUsernameAndPassword(String username, String password) {
        String sql = "select `id`,`username`,`password`,`email`from t_user where username= ? and password = ?";
        return queryForOne(User.class,sql,username,password);
    }
}
```



在接口中按 ctrl+shift+T 快速生成测试

![image-20220207202353162](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207202353162.png)

```java
public class UserDaoTest {

    UserDao userDao = new UserDaoImpl();

    @Test
    public void queryUserByUsername() {

        if (userDao.queryUserByUsername("admin1234") == null ){
            System.out.println("用户名可用！");
        } else {
            System.out.println("用户名已存在！");
        }
    }

    @Test
    public void queryUserByUsernameAndPassword() {
        if ( userDao.queryUserByUsernameAndPassword("admin","admin1234") == null) {
            System.out.println("用户名或密码错误，登录失败");
        } else {
            System.out.println("查询成功");
        }
    }

    @Test
    public void saveUser() {
        System.out.println( userDao.saveUser(new User(null,"admin2", "123456", "abc@qq.com")) );
    }
}
```

#### 5.编写UserService和测试

```java
public interface UserService {
      public void registUser(User user);
      public User login(User user);
      public boolean existsUsername(String username);
}
```

```java
/**
 * @className: UserService
 * @description: TODO 类描述
 * @author: Dong
 * @date: 2022/2/7
 **/
public class UserServiceImpl implements UserService {
    private UserDao userDao =new UserDaoImpl();
    /**
     * 注册用户
     * @param user
     */
    @Override
    public void registUser(User user) {
//        if (userDao.saveUser(user) == -1){
//  这里老师没讲 直接忽略掉错误的情况了 可能是因为下面 有检查 用户名是否可用的方法啦
//        }
        userDao.saveUser(user);
    }
    /**
     * 登录
     * @param user
     * @return 如果返回null，说明登录失败，返回有值，是登录成功
     */
    @Override
    public User login(User user) {
        return userDao.queryUserByUsernameAndPassword(user.getUsername(),user.getPassword());
    }
    /**
     * 检查 用户名是否可用
     * @param username
     * @return 返回true表示用户名已存在，返回false表示用户名可用
     */
    @Override
    public boolean existsUsername(String username) {
        if(userDao.queryUserByUsername(username) ==null){
            return false;
        }
        return true;
    }
}
```

```java
public class UserServiceTest {

    UserService userService = new UserServiceImpl();

    @Test
    public void registUser() {
        userService.registUser(new User(null, "bbj168", "666666", "bbj168@qq.com"));
        userService.registUser(new User(null, "abc168", "666666", "abc168@qq.com"));
    }

    @Test
    public void login() {
        System.out.println( userService.login(new User(null, "wzg168", "123456", null)) );
    }

    @Test
    public void existsUsername() {
        if (userService.existsUsername("wzg16888")) {
            System.out.println("用户名已存在！");
        } else {
            System.out.println("用户名可用！");
        }
    }
}
```

#### 6.实现用户注册功能 ，编写Servlet

实际开发 一般使用这两种。这两种都行，这里使用了第一种，

![image-20220207211748333](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207211748333.png)

![image-20220207211801246](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207211801246.png)

base 一般写在这里：

使用base 可能原来的路径都不对了 要再修改，可以在浏览器控制台调试。

![image-20220207213344523](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207213344523.png)

**这里只是学习base的使用 我感觉直接写在表单写绝对路径更简便**

没有name属性发不到服务器。

![image-20220207214332774](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220207214332774.png)

下面这一堆可以不看，就是js做了大部分的输入格式限制，如果不符合要求，表单是不会提交到Servlet的。

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>尚硅谷会员注册页面</title>
	<base href="http://localhost:8080/book/">
	<link type="text/css" rel="stylesheet" href="static/css/style.css" >
	<script src="static/script/jquery.js"></script>
	<script>
		$(function (){
			$("#sub_btn").click(function (){
				//验证用户名：必须由字母，数字下划线组成，并且长度为 5 到 12 位

				//获取 正则表达式验证 提示
				var username = $("#username").val();
				var usernamepatt =/^\w{5,12}$/;
				if (!usernamepatt.test(username)){
					$(".errorMsg").text("用户名不合法");
					return false;
				}

				// 验证密码：必须由字母，数字下划线组成，并且长度为 5 到 12 位
				var password = $("#password").val();
				var passwordpatt =/^\w{5,12}$/;
				if (!passwordpatt.test(password)){
					$(".errorMsg").text("密码不合法");
					return false;
				}
				// 验证确认密码：和密码相同
				var repassword = $("#repwd").val();
				if (repassword != password){
					$(".errorMsg").text("两次输入密码不一致");
					return false;
				}

				// 邮箱验证：xxxxx@xxx.com
				var email = $("#email").val();
				var emailpatt =/^[a-z\d]+(\.[a-z\d]+)*@([\da-z](-[\da-z])?)+(\.{1,2}[a-z]+)+$/;
				if (!emailpatt.test(email)){
					$(".errorMsg").text("邮箱格式不合法");
					return false;
				}

				// 验证码：现在只需要验证用户已输入。因为还没服务器。验证码生成。
				var code = $("#code").val();
				//去除前后空格
				code = $.trim(code);

				if (code == null || code == ""){
					$(".errorMsg").text("验证码不可为空");
					return false;
				}
			})
		})
		$("span.errorMsg").text("");

	</script>
<style type="text/css">
	.login_form{
		height:420px;
		margin-top: 25px;
	}
	
</style>
</head>
<body>
		<div id="login_header">
			<img class="logo_img" alt="" src="static/img/logo.gif" >
		</div>
		
			<div class="login_banner">
			
				<div id="l_content">
					<span class="login_word">欢迎注册</span>
				</div>
				
				<div id="content">
					<div class="login_form">
						<div class="login_box">
							<div class="tit">
								<h1>注册尚硅谷会员</h1>
								<span class="errorMsg"></span>
							</div>
							<div class="form">
								<form action="registServlet" method="post">
									<label>用户名称：</label>
									<input class="itxt" type="text" placeholder="请输入用户名" autocomplete="off" tabindex="1" name="username" id="username" />
									<br />
									<br />
									<label>用户密码：</label>
									<input class="itxt" type="password" placeholder="请输入密码" autocomplete="off" tabindex="1" name="password" id="password" />
									<br />
									<br />
									<label>确认密码：</label>
									<input class="itxt" type="password" placeholder="确认密码" autocomplete="off" tabindex="1" name="repwd" id="repwd" />
									<br />
									<br />
									<label>电子邮件：</label>
									<input class="itxt" type="text" placeholder="请输入邮箱地址" autocomplete="off" tabindex="1" name="email" id="email" />
									<br />
									<br />
									<label>验证码：</label>
									<input class="itxt" type="text" style="width: 150px;" name="code" id="code"/>
									<img alt="" src=" static/img/code.bmp" style="float: right; margin-right: 40px">
									<br />
									<br />
									<input type="submit" value="注册" id="sub_btn" />
									
								</form>
							</div>
							
						</div>
					</div>
				</div>
			</div>
		<div id="bottom">
			<span>
				尚硅谷书城.Copyright &copy;2015
			</span>
		</div>
</body>
</html>
```

RegistServlet 处理表单。

```java
public class RegistServlet extends HttpServlet {
    private UserService userService = new UserServiceImpl();

    //有密码 使用post
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //  1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String email = req.getParameter("email");
        String code = req.getParameter("code");

        // 2、检查 验证码是否正确  === 写死,要求验证码为:abcde
        if ("abcde".equalsIgnoreCase(code)) { //忽略大小写
        //3、检查 用户名是否可用
            if (userService.existsUsername(username)) {
                System.out.println("用户名[" + username + "]已存在!");
                //跳回注册页面
                req.getRequestDispatcher("/pages/user/regist.html").forward(req, resp);
            } else {
                //      可用
                //调用Sservice保存到数据库
                userService.registUser(new User(null, username, password, email));

                // 跳到注册成功页面 regist_success.html
                req.getRequestDispatcher("/pages/user/regist_success.html").forward(req, resp);
            }
        } else {
            System.out.println("验证码[" + code + "]错误");
            req.getRequestDispatcher("/pages/user/regist.html").forward(req, resp);
        }
    }
}
```

#### 7.实现用户注册功能 

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        UserService userService = new UserServiceImpl();
        User loginUser = userService.login(new User(null, username, password, null));
        if (loginUser == null){
            System.out.println("登入失败");
            req.getRequestDispatcher("/pages/user/login.html").forward(req,resp);
        }else {
            req.getRequestDispatcher("/pages/user/login_success.html").forward(req,resp);
        }

    }
}
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>尚硅谷会员登录页面</title>
	<base href="http://localhost:8080/book/">
<link type="text/css" rel="stylesheet" href="static/css/style.css" >
</head>
<body>
		<div id="login_header">
			<img class="logo_img" alt="" src="static/img/logo.gif" >
		</div>
		
			<div class="login_banner">
			
				<div id="l_content">
					<span class="login_word">欢迎登录</span>
				</div>
				
				<div id="content">
					<div class="login_form">
						<div class="login_box">
							<div class="tit">
								<h1>尚硅谷会员</h1>
								<a href="regist.html">立即注册</a>
							</div>
							<div class="msg_cont">
								<b></b>
								<span class="errorMsg">请输入用户名和密码</span>
							</div>
							<div class="form">
								<form action="loginServlet" method="post">
									<label>用户名称：</label>
									<input class="itxt" type="text" placeholder="请输入用户名" autocomplete="off" tabindex="1" name="username" />
									<br />
									<br />
									<label>用户密码：</label>
									<input class="itxt" type="password" placeholder="请输入密码" autocomplete="off" tabindex="1" name="password" />
									<br />
									<br />
									<input type="submit" value="登录" id="sub_btn" />
								</form>
							</div>
							
						</div>
					</div>
				</div>
			</div>
		<div id="bottom">
			<span>
				尚硅谷书城.Copyright &copy;2015
			</span>
		</div>
</body>
</html>
```

### 阶段2

#### 1.页面改jsp

![image-20220224192957284](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220224192957284.png)

![image-20220224192915591](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220224192915591.png)

#### 2.相同部分合并

![image-20220224194856184](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220224194856184.png)

保证基本的维护

![image-20220226102131077](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226102131077.png)

#### 3.动态化BASE便签

tip:写base标签,永远固定相对路径跳转的结果

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    String basePath = request.getScheme()
            + "://"
            + request.getServerName()
            + ":"
            + request.getServerPort()
            + request.getContextPath()
            + "/";
%>

<!--写base标签，永远固定相对路径跳转的结果-->
<base href="<%=basePath%>">
<link type="text/css" rel="stylesheet" href="static/css/style.css" >
<script type="text/javascript" src="static/script/jquery-1.7.2.js"></script>
```



#### 4.表单提交错误后的回显

这个其实在 阶段一 已经解决过一次了，是在前端解决的。

![image-20220226104208874](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226104208874.png)

登录回传

![image-20220226105814535](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226105814535.png)

![image-20220226110136957](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226110136957.png)

![image-20220226110008645](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226110008645.png)

注册也类似

#### 4.代码优化 合并登入注册

![image-20220226120349156](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226120349156.png)

使用隐藏域区分功能

![image-20220226121322723](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226121322723.png)

![image-20220226152007266](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226152007266.png)

#### 5.使用反射优化大量else if 代码

![image-20220226152344879](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226152344879.png)

如果功能增加的多，那么4点中的if判断有些繁琐。

![image-20220226153152388](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226153152388.png)

![image-20220226153533976](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226153533976.png)

#### 6.抽取BaseServlet程序

用户模块和图书模块 有相同的部分 

![image-20220226160806377](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226160806377.png)

```java
public class UserServlet extends BaseServlet {

    private UserService userService = new UserServiceImpl();

    /**
     * 处理登录的功能
     *
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void login(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //  1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        // 调用 userService.login()登录处理业务
        User loginUser = userService.login(new User(null, username, password, null));
        // 如果等于null,说明登录 失败!
        if (loginUser == null) {
            // 把错误信息，和回显的表单项信息，保存到Request域中
            req.setAttribute("msg", "用户或密码错误！");
            req.setAttribute("username", username);
            //   跳回登录页面
            req.getRequestDispatcher("/pages/user/login.jsp").forward(req, resp);
        } else {
            // 登录 成功
            //跳到成功页面login_success.html
            req.getRequestDispatcher("/pages/user/login_success.jsp").forward(req, resp);
        }

    }

    /**
     * 处理注册的功能
     *
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void regist(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //  1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String email = req.getParameter("email");
        String code = req.getParameter("code");

        User user = WebUtils.copyParamToBean(req.getParameterMap(), new User());

//        2、检查 验证码是否正确  === 写死,要求验证码为:abcde
        if ("abcde".equalsIgnoreCase(code)) {
//        3、检查 用户名是否可用
            if (userService.existsUsername(username)) {
                System.out.println("用户名[" + username + "]已存在!");

                // 把回显信息，保存到Request域中
                req.setAttribute("msg", "用户名已存在！！");
                req.setAttribute("username", username);
                req.setAttribute("email", email);

//        跳回注册页面
                req.getRequestDispatcher("/pages/user/regist.jsp").forward(req, resp);
            } else {
                //      可用
//                调用Sservice保存到数据库
                userService.registUser(new User(null, username, password, email));
//
//        跳到注册成功页面 regist_success.jsp
                req.getRequestDispatcher("/pages/user/regist_success.jsp").forward(req, resp);
            }
        } else {
            // 把回显信息，保存到Request域中
            req.setAttribute("msg", "验证码错误！！");
            req.setAttribute("username", username);
            req.setAttribute("email", email);

            System.out.println("验证码[" + code + "]错误");
            req.getRequestDispatcher("/pages/user/regist.jsp").forward(req, resp);
        }

    }


}
```

```java
public abstract class BaseServlet extends HttpServlet {

    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String action = req.getParameter("action");
        try {
            // 获取action业务鉴别字符串，获取相应的业务 方法反射对象
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class, HttpServletResponse.class);
//            System.out.println(method);
            // 调用目标业务 方法
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}
```

dubug:可以知道请求直接到了baseSevrlet方法中，而且this可以正常的使用反射

这个因该是跳转时是post方法，但是子类中没有post，就像上找post，然后父类有post，调用父类post方法。

这里还是子类访问，只是向上调用了父类的dopost方法。而且可以理解的是没有改web.xml，所以访问的还是原来的

UserServlet。

补充：

![image-20220226164417761](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226164417761.png)

#### 7.数据的封装和抽取BeanUtils的使用

BeanUtils工具类作用之一，它可以一次性的把所有请求的参数注入到JavaBean中。

**BeanUtils它不是Jdk的类。而是第三方的工具类。所以需要导包。**

这两个：![image-20220226165133033](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20220226165133033.png)

测试代码：

参数名和bean的名字必须对应，通过set方法注入的，一定要写标准的set方法命名

![image-20220226165348758](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226165348758.png)

现在中又一直会用到 所以又写成了一个工具类，

![image-20220226165750513](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226165750513.png)



![image-20220226165836629](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226165836629.png)

改进：参数使用Map,可以保证其他层也可以调用，耦合性：模块之间的关联程度，如果上面的这种，耦合度高。

![image-20220226171531307](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226171531307.png)

改进2：省去原来的先new 再传进去

![image-20220226172147832](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226172147832.png)

![image-20220226172341364](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220226172341364.png)

改进3：使用泛型省去类型转换 (**泛型的本质就是为了参数化类型**)

最后版：

```java
public class WebUtils {
    /**
     * 把Map中的值注入到对应的JavaBean属性中。
     * @param value
     * @param bean
     */
    public static <T> T copyParamToBean( Map value , T bean ){
        try {
            System.out.println("注入之前：" + bean);
            /**
             * 把所有请求的参数都注入到user对象中
             */
            BeanUtils.populate(bean, value);
            System.out.println("注入之后：" + bean);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return bean;
    }
}
```

#### 8.使用EL表达式代替前面的<%=     >    ，表单错误回显



```jsp
<h1>注册会员</h1>
<span class="errorMsg">
   ${ requestScope.msg }
</span>
```

EL表达式中如果msg 是 null  就是直接输出空串

```jsp
<h1>会员登入</h1>
<span class="errorMsg">
   ${ empty requestScope.msg ? "请输入用户名和密码":requestScope.msg }
</span>
```

### 图书管理

建表->bean->dao->service->web

#### 9.MVC概念

![image-20220227100233325](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220227100233325.png)

![image-20220227100352989](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220227100352989.png)

#### 10.建表

```sql
CREATE TABLE t_book(
	`id` INT PRIMARY KEY AUTO_INCREMENT,
	`name` VARCHAR(100),
	`price` DECIMAL(11,2),
	`author` VARCHAR(100),
	`sales` INT,
	`stock` INT,
	`img_path` VARCHAR(200)	
)
INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'java从入门到放弃' , '国哥' , 80 , 9999 , 9 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '数据结构与算法' , '严敏君' , 78.5 , 6 , 13 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '怎样拐跑别人的媳妇' , '龙伍' , 68, 99999 , 52 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '木虚肉盖饭' , '小胖' , 16, 1000 , 50 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'C++编程思想' , '刚哥' , 45.5 , 14 , 95 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '蛋炒饭' , '周星星' , 9.9, 12 , 53 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '赌神' , '龙伍' , 66.5, 125 , 535 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'Java编程思想' , '阳哥' , 99.5 , 47 , 36 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'JavaScript从入门到精通' , '婷姐' , 9.9 , 85 , 95 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'cocos2d-x游戏编程入门' , '国哥' , 49, 52 , 62 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'C语言程序设计' , '谭浩强' , 28 , 52 , 74 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'Lua语言程序设计' , '雷丰阳' , 51.5 , 48 , 82 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '西游记' , '罗贯中' , 12, 19 , 9999 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '水浒传' , '华仔' , 33.05 , 22 , 88 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '操作系统原理' , '刘优' , 133.05 , 122 , 188 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '数据结构 java版' , '封大神' , 173.15 , 21 , 81 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'UNIX高级环境编程' , '乐天' , 99.15 , 210 , 810 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , 'javaScript高级编程' , '国哥' , 69.15 , 210 , 810 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '大话设计模式' , '国哥' , 89.15 , 20 , 10 , 'static/img/default.jpg');

INSERT INTO t_book(`id` , `name` , `author` , `price` , `sales` , `stock` , `img_path`)
VALUES(NULL , '人月神话' , '刚哥' , 88.15 , 20 , 80 , 'static/img/default.jpg');
```

#### 11.编写javaBean

 ```java
 public class book {
     private Integer id;
     private String name;
     private String author;
     private BigDecimal price;
     private Integer sales;
     private Integer stock;
     private String imgPath="static/img/default.jpg";
 
     public book() {
     }
 
     public book(Integer id, String name, String author, BigDecimal price, Integer sales, Integer stock, String imgPath) {
         this.id = id;
         this.name = name;
         this.author = author;
         this.price = price;
         this.sales = sales;
         this.stock = stock;
         //对路径基本判断
         if (imgPath != null && !imgPath.equals("")){
             this.imgPath = imgPath;
         }
     }
 ```

get set toString 省略 

#### 12.编写DAO

```java
public class BookDaoImpl extends BaseDao implements BookDao {

    @Override
    public int addBook(Book book) {
        String sql = "INSERT INTO t_book( `name` , `author` , `price` , `sales` , `stock` , `img_path`) VALUES(? , ? , ? , ? , ? , ? );";
        return update(sql,book.getName(),book.getAuthor(),book.getPrice(),book.getSales(),book.getStock(),book.getImgPath());
    }

    @Override
    public int deleteBookById(Integer id) {
        String sql="delete from t_book where id = ?";

        return update(sql,id);
    }

    @Override
    public int updateBook(Book book) {
        String sql ="update t_book set  `name`=? , `author`=? , `price`=? , `sales`=? , `stock`=? , `img_path`=? where id =?";
        return update(sql,book.getName(),book.getAuthor(),book.getPrice(),book.getSales(),book.getStock(),book.getImgPath(),book.getId());
    }

    @Override
    public Book queryBookById(Integer id) {
        //imgPath目的是解决pojo中的字段属性跟数据库中的不一致
        String sql ="select `name` , `author` , `price` , `sales` , `stock` , `img_path` imgPath from t_book where id =?";
        return queryForOne(Book.class,sql,id);
    }

    @Override
    public List<Book> queryBooks() {
        String sql ="select `name` , `author` , `price` , `sales` , `stock` , `img_path` imgPath from t_book";
        return queryForList(Book.class,sql);
    }
}
```

#### 13.编写Service

```java
public class BookServiceImpl implements BookService {

    private BookDao bookDao = new BookDaoImpl();

    @Override
    public void addBook(Book book) {
        bookDao.addBook(book);
    }

    @Override
    public void deleteBookById(Integer id) {
        bookDao.deleteBookById(id);
    }

    @Override
    public void updateBook(Book book) {
        bookDao.updateBook(book);
    }

    @Override
    public Book queryBookById(Integer id) {
        return bookDao.queryBookById(id);
    }

    @Override
    public List<Book> queryBooks() {
        return bookDao.queryBooks();
    }
}
```

#### 14.编写web

##### 展示页面：

![image-20220227165231468](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220227165231468.png)

```java
protected void list(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1 通过BookService查询全部图书
        List<Book> books = bookService.queryBooks();
        //2 把全部图书保存到Request域中
        req.setAttribute("books", books);
        //3、请求转发到/pages/manager/book_manager.jsp页面
        req.getRequestDispatcher("/pages/manager/book_manager.jsp").forward(req,resp);
    }
```

前后端网页一般位置：

![image-20220227195605583](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220227195605583.png)

##### 添加页面：

```Java
    protected void add(HttpServletRequest req, HttpServletResponse resp) throws  IOException {

        //        1、获取请求的参数==封装成为Book对象
        Book book = WebUtils.copyParamToBean(req.getParameterMap(),new Book());
//        2、调用BookService.addBook()保存图书
        bookService.addBook(book);
//        3、跳到图书列表页面
//                /manager/bookServlet?action=list
//        req.getRequestDispatcher("/manager/bookServlet?action=list").forward(req, resp);
//  	不可以使用请求转发，如果浏览器按f5 那就会重新发起最后一次请求，会导致重新重复提交表单 
      
        resp.sendRedirect(req.getContextPath() + "/manager/bookServlet?action=list");

    }
```

不可以使用请求转发，如果浏览器f5 那就会重新发起最后一次请求，会导致重新重复提交表单 。

本质的理解是请求转发是一次请求，重定向是两次请求，

重定向，如果浏览器按f5，重新发起最后一次请求，就是/manager/bookServlet?action=list，而不是下面的这个重新提交

```
<form action="manager/bookServlet" method="get">
   <input type="hidden" name="action" value="add">
```

![image-20220227204740330](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220227204740330.png)

##### 删除页面:

![image-20220227212546929](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220227212546929.png)

```java
    protected void delete(HttpServletRequest req, HttpServletResponse resp) throws  IOException {
//        1、获取请求的参数id，图书编程
        int id = WebUtils.parseInt(req.getParameter("id"), 0);
//        2、调用bookService.deleteBookById();删除图书
        bookService.deleteBookById(id);
//        3、重定向回图书列表管理页面
//                /book/manager/bookServlet?action=list
        resp.sendRedirect(req.getContextPath() + "/manager/bookServlet?action=list");
    }
```

```jsp
<c:forEach items="${requestScope.books}" var="book">
   <tr>
      <td>${book.name}</td>
      <td>${book.price}</td>
      <td>${book.author}</td>
      <td>${book.sales}</td>
      <td>${book.stock}</td>
      <td><a href="book_edit.jsp">修改</a></td>
      <td><a href=manager/bookServlet?action=delete&&id=${book.id}>删除</a></td>
   </tr>
</c:forEach>
```

添加删除时友情提示：

```jsp
<script type="text/javascript">
		$(function (){
			$("a.deleteClass").click(function (){
				/**
				 confirm 是确认提示框函数
				 参数是它的提示内容
				 它有两个按钮，-个确认，一个是取消。
				 返回true表示点击了，确认，返回false 表示点击取消。
				 */
			//在事件的function函数中， 有一 个this对象。 这个this对象，是当前正在响应事件的dom对象。
			//函数return false 表单则不会提交
				return confirm("你确定要删除"+$(this).parent().parent().find("td:first").text()+"?")
			})
		})

	</script>
```

##### 修改页面：

修改图书第一步，回显修改的信息

```jsp
<td><a href="manager/bookServlet?action=getBook&id=${book.id})">修改</a></td>
```

不从book_manager的数据直接过去   因为要经过数据库保证数据一致性

![image-20220228113433573](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228113433573.png)

```java
protected void getBook(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //1 获取请求的参数图书编号
    int id = WebUtils.parseInt(req.getParameter("id"), 0);
    //2 调用bookService.queryBookById查询图书
    Book book = bookService.queryBookById(id);
    //3 保存到图书到Request域中
    req.setAttribute("book", book) ;
    //4 请求转发到。pages/manager/book_edit.jsp页面
    req.getRequestDispatcher("/pages/manager/book_edit.jsp").forward(req,resp);
}
```

修改图书第二步，提交给服务器保存修改

![image-20220228115149230](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228115149230.png)

```java
    protected void update(HttpServletRequest req, HttpServletResponse resp) throws  IOException {
//        1、获取请求的参数==封装成为Book对象
        Book book = WebUtils.copyParamToBean(req.getParameterMap(),new Book());
//        2、调用BookService.updateBook( book );修改图书
        bookService.updateBook(book);
//        3、重定向回图书列表管理页面
//        地址：/工程名/manager/bookServlet?action=list
        resp.sendRedirect(req.getContextPath() + "/manager/bookServlet?action=list");
    }
```

解决使用到相同页面的问题：

+ 方法一：

动态改变隐藏域的值

![image-20220228121531142](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228121531142.png)

![image-20220228120228615](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228120228615.png)

![image-20220228120253427](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228120253427.png)

理解：点击两个不同的按钮时,发出有各自参数的请求，在book_edit.jsp提交表单时，动态改变自己的隐藏域，这样可以走bookServlet中不同的方法

补充：

${param.name} 等价于 request.getParamter("name")，这两种方法一般用于服务器从页面或者客户端获取的内容。

getParamter是指的地址栏的参数

${requestScope.name} 等价于 request.getAttribute("name")，一般是从服务器传递结果到页面，在页面中取出服务器保存的值。

getAttribute 是请求域中的数据

+ 方法二：

观察页面地址：

![image-20220228145156984](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228145156984.png)

测试：

![image-20220228145047089](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228145047089.png)

应用：

![image-20220228145127194](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228145127194.png)

+ 方案三：

![image-20220228150330142](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228150330142.png)

![image-20220228150354252](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228150354252.png)

解决了上面的问题 还要添加一个隐藏域保存id信息，否则修改时表单没有提交id信息导致数据库不能根据id修改。

我理解到了 表单提交 只会发送表单的所有信息，以前请求转发过来的参数不会发送

![image-20220228162735455](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228162735455.png)

或者：

![image-20220228162938458](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220228162938458.png)

#### 15.图书的分页

分析：

![image-20220228171439591](https://s2.loli.net/2022/05/11/ecyqIS27dotwY1b.png)

![image-20220228195051436](https://s2.loli.net/2022/05/11/CFkTs7MYKNBg5JR.png)

![image-20220228205618313](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20220228205618313.png)

编写pojo

```java
public class Page {
    //常量名全大写
    public static final Integer PAGE_SIZE = 4;
    //当前页码
    private Integer pageNo;
    //总页数
    private Integer pageTotal;
    //当前显示数量
    private Integer pageSize = PAGE_SIZE;
    //总记录数
    private Integer pageTotalCount;
    //当前页数据
    private List<Book> items;
```

##### 分页初步实现

```java
protected void page(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1、获取请求的参数pageNo和pageSize
        int pageNO = WebUtils.parseInt(req.getParameter("pageNO"), 1);
        int pageSize = WebUtils.parseInt(req.getParameter("pageSize"), Page.PAGE_SIZE);
        //2、调用BookService. page(pageNo, pageSize) :Page对象
        Page<Book> bookPage=bookService.page(pageNO,pageSize);
        //3、保存到Request域中
        req.setAttribute("page",bookPage);
        //4、请求转发到/pages/manager/book_ manager. jsp页面
        req.getRequestDispatcher("/pages/manager/book_manager.jsp").forward(req,resp);
    }
```

```java
public Page<Book> page(int pagesNo, int pageSize) {
    Page<Book> bookPage = new Page<>();
    bookPage.setPageNo(pagesNo);
    bookPage.setPageSize(pageSize);
    //求记录总数
    Integer pageTotalCount=bookDao.queryForPageTotalCount();
    bookPage.setPageTotalCount(pageTotalCount);
    Integer pageTotal=pageTotalCount/pageSize;
    if (pageTotalCount%pageSize>0){
        pageTotal++;
    }
    bookPage.setPageTotal(pageTotal);
    //求开始索引
    Integer begin=(pagesNo-1)*pageSize;
    //求当前页数据
    List<Book> items =bookDao.queryForPageItem(begin,pageSize);
    bookPage.setItems(items);
    return bookPage;
}
```

```java
@Override
public Integer queryForPageTotalCount() {
    String sql="select count(*) from t_book";
    //虽然Scalar这个方法返回值是Object，但是运行类型是Long，所以用Integer强转不行
    //scalarHandler的返回值只能用long或者父类number类来接受
    Number number = (Number) queryForSingleValue(sql);
    return number.intValue();
}

@Override
public List<Book> queryForPageItem(Integer begin, Integer pageSize) {
    String sql="select `id`,`name` , `author` , `price` , `sales` , `stock` , `img_path` imgPath from t_book limit ?,?";
    return queryForList(Book.class,sql,begin,pageSize);
}
```

```jsp
<div id="page_nav">
   <a href="#">首页</a>
   <a href="#">上一页</a>
   <a href="#">3</a>
   【${requestScope.page.pageNo}】
   <a href="#">5</a>
   <a href="#">下一页</a>
   <a href="#">末页</a>
   共${requestScope.page.pageTotal}页，${requestScope.page.pageTotalCount}条记录
   到第<input value="4" name="pn" id="pn_input"/>页
   <input type="button" value="确定">
</div>
```

##### 首页  下一页  上一页 末页

```java
<div id="page_nav">
			<c:if test="${requestScope.page.pageNo > 1}">
				<!-- 大于首页才显示 -->
				<a href="manager/bookServlet?action=page&pageNo=1">首页</a>
				<a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo-1}">上一页</a>
				<a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo-1}">${requestScope.page.pageNo-1}</a>

			</c:if>

			【${requestScope.page.pageNo}】
			<c:if test="${requestScope.page.pageNo <requestScope.page.pageTotal}">
				<a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo+1}">${requestScope.page.pageNo+1}</a>
				<a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageNo+1}">下一页</a>
				<a href="manager/bookServlet?action=page&pageNo=${requestScope.page.pageTotal}">末页</a>
			</c:if>


			共${requestScope.page.pageTotal}页，${requestScope.page.pageTotalCount}条记录
			到第<input value="4" name="pn" id="pn_input"/>页
			<input type="button" value="确定">
		</div>
```

##### 跳到指定页数

绑定事件

```jsp
到第<input value="${requestScope.page.pageNo}" name="pn" id="pn_input"/>页
<input id="searchPageBtn" type="button" value="确定">
<script type="text/javascript">
   $(function (){
      $("#searchPageBtn").click(function (){
         var pageNo=$("#pn_input").val();
         // javascript语言中提供了一个location地址栏对象
         //它有一个属性叫href 它可以获取浏览器地址栏中的地址
         //可读，可写 （可赋值），赋值会导致跳转
         //location.href="http://localhost:8080/book/manager/bookServlet?action=page&pageNo="+pageNo;
         location.href="${pageScope.basePath}manager/bookServlet?action=page&pageNo="+pageNo;

      })
   })
</script>
```

引用的head.jsp:

```jsp
<%
    String basePath = request.getScheme()
            + "://"
            + request.getServerName()
            + ":"
            + request.getServerPort()
            + request.getContextPath()
            + "/";
    pageContext.setAttribute("basePath",basePath);
%>
```

##### 数据的有效边界检查

方法一：

```jsp
共${requestScope.page.pageTotal}页，${requestScope.page.pageTotalCount}条记录
   到第<input value="${requestScope.page.pageNo}" name="pn" id="pn_input"/>页
   <input id="searchPageBtn" type="button" value="确定">
   <script type="text/javascript">
      $(function (){
         $("#searchPageBtn").click(function (){
            var pageNo=$("#pn_input").val();
            var pageTotal=${requestScope.page.pageTotal};
            if (pageNo<1 ||pageNo>pageTotal){
               alert("不能跳转");
               //关键return
               return;
            }
            // javascript语言中提供了一个location地址栏对象
            //它有一个属性叫href 它可以获取浏览器地址栏中的地址
            //可读，可写 （可赋值），赋值会导致跳转
            //location.href="http://localhost:8080/book/manager/bookServlet?action=page&pageNo="+pageNo;
            location.href="${pageScope.basePath}manager/bookServlet?action=page&pageNo="+pageNo;

         })
      })
   </script>
</div>
```

上面的方法 如果有的人直接在地址栏输入超出的页码，也会出错。跳过了js的校验。

方法二：

在BookServiceImpl   page方法实现

```java
public Page<Book> page(int pageNo, int pageSize) {

    //if (pageSize<)
    Page<Book> bookPage = new Page<>();

    bookPage.setPageSize(pageSize);
    //求记录总数
    Integer pageTotalCount=bookDao.queryForPageTotalCount();
    bookPage.setPageTotalCount(pageTotalCount);
    Integer pageTotal=pageTotalCount/pageSize;
    if (pageTotalCount%pageSize>0){
        pageTotal++;
    }
    bookPage.setPageTotal(pageTotal);
    //判断合理性 数据边界有效检查
    
    if (pageNo<1){
        pageNo=1;
    }
    if (pageNo>pageTotal){
        pageNo=pageTotal;
    }
    
    bookPage.setPageNo(pageNo);

    //求开始索引
    Integer begin=(pageNo-1)*pageSize;
    //求当前页数据
    List<Book> items =bookDao.queryForPageItem(begin,pageSize);
    bookPage.setItems(items);
    return bookPage;
}
```

上面的代码  注意顺序！！！

代码优化：

写在Page对象的setPageNo，注意可以保证以后的人员分页等等其他模块的使用。前提是先bookPage.setPageTotal(pageTotal);再 bookPage.setPageNo(pageNo);

```java
public void setPageNo(Integer pageNo) {
        if (pageNo<1){
            pageNo=1;
        }
        if (pageNo>pageTotal){
            pageNo=pageTotal;
        }
        this.pageNo = pageNo;
    }
```

##### 分页条 页码 输出

（以后这个貌似 使用组件解决）

![image-20220302112550382](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302112550382.png)

![image-20220302105546718](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302105546718.png)

这个是每一个情况的 重复部分 是优化过后的：

![image-20220302113211929](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302113211929.png)



##### 修改分页对原来，添加、删除、修改的影响

比如 添加后 跳转的页面中应该包含 新添加的，而不是第一页

解决：点击添加按钮后 把最后一页传过去 给了book_edit.jsp

![image-20220302132325169](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302132325169.png)

book_edit.jsp 增加隐藏域 最后传给Servlet

![image-20220302134759912](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302134759912.png)

![image-20220302134819841](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302134819841.png)

删除：

不用担心 是最后一页,最后一个，在page方法中 如果超出了页码，会转到最后一页

![image-20220302150654871](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302150654871.png)

![image-20220302150451402](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302150451402.png)

修改类似

##### 前台分页的初步实现

这种的首页地址不合适  不能直接显式的去访问红色

![image-20220302191230869](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302191230869.png)

解决方法：

![image-20220302192201598](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220302192201598.png)

```java
public class ClientBookServlet extends BaseServlet{
    private BookService bookService = new BookServiceImpl();


    protected void page(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1、获取请求的参数pageNo和pageSize
        int pageNo = WebUtils.parseInt(req.getParameter("pageNo"), 1);
        int pageSize = WebUtils.parseInt(req.getParameter("pageSize"), Page.PAGE_SIZE);

        //2、调用BookService. page(pageNo, pageSize) :Page对象
        Page<Book> bookPage=bookService.page(pageNo,pageSize);
        //3、保存到Request域中
        req.setAttribute("page",bookPage);
        //4、请求转发到/pages/manager/book_ manager. jsp页面
        req.getRequestDispatcher("/pages/client/index.jsp").forward(req,resp);
    }
}
```

```java
public class ClientBookServlet extends BaseServlet{
    private BookService bookService = new BookServiceImpl();


    protected void page(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1、获取请求的参数pageNo和pageSize
        int pageNo = WebUtils.parseInt(req.getParameter("pageNo"), 1);
        int pageSize = WebUtils.parseInt(req.getParameter("pageSize"), Page.PAGE_SIZE);

        //2、调用BookService. page(pageNo, pageSize) :Page对象
        Page<Book> bookPage=bookService.page(pageNo,pageSize);
        //3、保存到Request域中
        req.setAttribute("page",bookPage);
        //4、请求转发到/pages/manager/book_ manager. jsp页面
        req.getRequestDispatcher("/pages/client/index.jsp").forward(req,resp);
    }
}
```

配置地址:

```xml
<servlet>
    <servlet-name>ClientBookServlet</servlet-name>
    <servlet-class>com.donggei.web.ClientBookServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ClientBookServlet</servlet-name>
    <url-pattern>/client/bookServlet</url-pattern>
</servlet-mapping>
```

修改前端：

```jsp
<c:forEach items="${requestScope.page.items}" var="book">
   <div class="b_list">
      <div class="img_div">
         <img class="book_img" alt="" src="${book.imgPath}" />
      </div>
      <div class="book_info">
         <div class="book_name">
            <span class="sp1">书名:</span>
            <span class="sp2">${book.name}</span>
         </div>
         <div class="book_author">
            <span class="sp1">作者:</span>
            <span class="sp2">${book.author}</span>
         </div>
         <div class="book_price">
            <span class="sp1">价格:</span>
            <span class="sp2">${book.price}</span>
         </div>
         <div class="book_sales">
            <span class="sp1">销量:</span>
            <span class="sp2">${book.sales}</span>
         </div>
         <div class="book_amount">
            <span class="sp1">库存:</span>
            <span class="sp2">${book.stock}</span>
         </div>
         <div class="book_add">
            <button>加入购物车</button>
         </div>
      </div>
   </div>
</c:forEach>
```

弹幕：终于了解到client和manager同级的好处了，改起来方便

```jsp
<div id="page_nav">
   <c:if test="${requestScope.page.pageNo > 1}">
      <!-- 大于首页才显示 -->
      <a href="client/bookServlet?action=page&pageNo=1">首页</a>
      <a href="client/bookServlet?action=page&pageNo=${requestScope.page.pageNo-1}">上一页</a>
      <a href="client/bookServlet?action=page&pageNo=${requestScope.page.pageNo-1}">${requestScope.page.pageNo-1}</a>

   </c:if>

   【${requestScope.page.pageNo}】
   <c:if test="${requestScope.page.pageNo <requestScope.page.pageTotal}">
      <a href="client/bookServlet?action=page&pageNo=${requestScope.page.pageNo+1}">${requestScope.page.pageNo+1}</a>
      <a href="client/bookServlet?action=page&pageNo=${requestScope.page.pageNo+1}">下一页</a>
      <a href="client/bookServlet?action=page&pageNo=${requestScope.page.pageTotal}">末页</a>
   </c:if>
```

##### 优化代码—分页条抽取

因为分页的请求地址大致相同，抽取出来,在page对象中添加一个属性，保存请求地址。

在前端jsp中把所有该替换的全部替换

![image-20220303115726406](https://s2.loli.net/2022/05/11/sVnSGhdmgFvuXpE.png)

然后在对应的Sevrlet中set对应的url  :

![image-20220303132501143](https://s2.loli.net/2022/05/11/s9M2Qf5IYPit7ak.png)

![image-20220303132515358](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220303132515358.png)

然后又发现 分页内容基本一致 ，可以单独抽出来：

![image-20220303134221084](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220303134221084.png)

![image-20220303134256592](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220303134256592.png)

##### 价格区间图书搜索

![image-20220303142626619](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220303142626619.png)

```java
protected void pageByPrice(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1、获取请求的参数pageNo和pageSize
        int pageNo = WebUtils.parseInt(req.getParameter("pageNo"), 1);
        int pageSize = WebUtils.parseInt(req.getParameter("pageSize"), Page.PAGE_SIZE);
        int min = WebUtils.parseInt(req.getParameter("min"), 0);
        int max = WebUtils.parseInt(req.getParameter("max"), Integer.MAX_VALUE);

        //2、调用BookService. page(pageNo, pageSize) :Page对象
        Page<Book> bookPage=bookService.pageByPrice(pageNo,pageSize,min,max);
        bookPage.setUrl("client/bookServlet?action=pageByPrice");
        //3、保存到Request域中
        req.setAttribute("page",bookPage);
        //4、请求转发到/pages/manager/book_ manager. jsp页面
        req.getRequestDispatcher("/pages/client/index.jsp").forward(req,resp);
    }
```

```jsp
<form action="client/bookServlet" method="get">
   <input type="hidden" name="action" value="pageByPrice">
   价格：<input id="min" type="text" name="min" value="${param.min}"> 元 -
      <input id="max" type="text" name="max" value="${param.max}"> 元
      <input type="submit" value="查询" />
</form>
```

```
value="${param.max}"   value="${param.min}"  数据的回显
```

##### 区间查询后 点击下一页出现Bug

页码的地址中    发现没有min和max参数，导致数据库查出的是全部数据

![image-20220303181123359](https://s2.loli.net/2022/05/11/QqlyYaBcDCRx5pt.png)

![image-20220303181047816](https://s2.loli.net/2022/05/11/JqKuL1kpmTAzdty.png)

解决：在ClientBookServlet 的pageByPrice的bookPage.setUrl中添加参数

#### 阶段三

##### 登录 

```java
// 登录 成功
//保存用户信息到session域中
req.getSession().setAttribute("user",loginUser);
```

```jsp
<c:if test="${empty sessionScope.user}">
   <a href="pages/user/login.jsp">登录</a> |
   <a href="pages/user/regist.jsp">注册</a> &nbsp;&nbsp;
</c:if>
<c:if test="${not empty sessionScope.user}">
   <span>欢迎<span class="um_span">${sessionScope.user.username}</span>光临尚硅谷书城</span>
   <a href="pages/order/order.jsp">我的订单</a>
   <a href="../../index.jsp">注销</a>&nbsp;&nbsp;
```

##### 注销

![image-20220304220053424](https://s2.loli.net/2022/05/11/iXNsqaM25PkIWQr.png)

##### 验证码——表单重复提交

![image-20220305101254893](https://s2.loli.net/2022/05/11/5MgIidxurAU4TRc.png)

![image-20220305102326954](https://s2.loli.net/2022/05/11/f3Q6HSObCz2n5wN.png)

##### 谷歌kaptcha图片验证码的使用

1.添加依赖

```xml
<dependency>
  <groupId>com.github.penggle</groupId>
  <artifactId>kaptcha</artifactId>
  <version>2.3.2</version>
</dependency>
```

2.在web.xml中添加用于生成验证码的Servlet程序

![image-20220305124426928](https://s2.loli.net/2022/05/11/o7SvyQz9a4CU1nc.png)

```xml
<servlet>
    <servlet-name>KaptchaServlet</servlet-name>
    <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>KaptchaServlet</servlet-name>
    <url-pattern>/kaptcha.jpg</url-pattern>
</servlet-mapping>
```

访问这个/kaptcha.jpg 后 会生成一个图片，自动把验证码保存在Session中

KaptchaServlet 路径改写为了.jpg 这样看起来像一个验证码

在表单中使用img标签去显示验证码图片并使用它

3.在表单中使用img标签去显示验证码图片并便用它

4.在服务器获取谷歌生成的验证码和客户端发送过来的猃证码比较

```java
//获取Session 中的验证码
String token = (String)req.getSession().getAttribute(KAPTCHA_SESSION_KEY);
//删除Session中的验证码
req.getSession().removeAttribute(KAPTCHA_SESSION_KEY);
 if (token.equalsIgnoreCase(code)){
     
 }
```

5.点击验证码切换

浏览器缓存对验证码的影响（谷歌不影响。火狐和IE 确实影响到了）

![image-20220305153702999](https://s2.loli.net/2022/05/11/ZbwBe9nK17J6AWj.png)

解决方法:

请求加问号 随便给一个每次不同的参数，那样浏览器就找不到对应的缓存了

```jsp
$(function (){
				$("#code_img").click(function (){
					//在事件响应的function函数中有一个this对象。 这个this对象，是当前正在响应事件的dom对象
					this.src="${basePath}kaptcha.jpg?d="+new Date();
					//Random()不是很严谨，用new Date().getTime();  Chrome 、Firefox、IE 都可以使用
				})
			})
```

30-32 thyme leaf

40-49 mvc

52-55 源码分析

56-68 QQZone项目

69-76 书城项目（不知道和旧版的有什么区别，还没学到）

84-86 vue、axios

