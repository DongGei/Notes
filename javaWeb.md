# java Web

## 一.基本概念

### 1.1介绍

web：网页； 在互联网上拿到一点的资源

+ 静态web

  + html CSS
  + 提供给所有人看的数据始终不会发生变化

+ 动态web

  + 淘宝..几乎所有的网站

  + 每个人在不同的时间 地点 看的不同
  + 技术栈： Servlet/JSP,ASp,PHP

在Java中 动态web资源开发的技术统称javaweb

### 1.2web程序

## 注意点

### 1.maven设置

与JavaWeb无关

![image-20210928223359089](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20210928223359089.png)

### 2.Servlet容器（web容器）

比如Tomcat（Servlet和JSP容器）

利用容器提供的方法，你能轻松的让servlet与web服务器对话，而不用自己建立serversocket、监听某个端口、创建流等

### 3.转发和重点向

转发是服务器内部完成的 重点向是服务器返回新路径再由浏览器请求

重定向，路径变，页面也变

**A找Ｂ借钱，Ｂ没有，B找Ｃ借，然后给Ａ，就是转发！**

**B让A直接去找C，就是重定向**

```java
//转发  
request.getRequestDispatcher("/welcome").forward(request, response);
//重定向
response.sendRedirect("welcome");
```

**转发是服务器行为，重定向是客户端行为。转发耗时比重定向少。**

转发——>客户浏览器发送HTTP请求——>web服务器接受请求——>调用内部一个方法在容器内部完成请求处理和转发动作——>再将转发跳转到的那个网页资源返回给客户；  转发只能在同一个容器内完成 转发的时候浏览器地址是不会变的，在客户浏览器里只会显示第一次进入的那个网址或者路径，客户看不到这个过程，只是得到了想要的目标资源。转发行为浏览器只做了一次请求。（转发只能跳转一次）

重定向——>客户浏览器发送HTTP请求——>web服务器接受请求后发送302状态码以及新的位置给客户浏览器——>客户浏览器发现是302响应，则自动再发送一个新的HTTP请求，请求指向新的地址（302：Found  临时移动，但资源只是临时被移动。即你访问网址A，但是网址A因为服务器端的拦截器或者其他后端代码处理的原因，会被重定向到网址B。）——>服务器根据此请求寻找资源发个客户；再客户浏览器中显示的是重定向之后的路径，客户可以看到地址的变化。重定向行为浏览器做了至少两次请求。（重定向可以跳转多次）
![image-20211225223710010](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211225223710010.png)



使用maven 新建项目

![image-20211219214826815](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211219214826815.png)

先把包(文件夹)补齐

ieda 自己带的web.xml版本低，记得改一下配置，替换为webapp4.0版本和tomcat一致![image-20211225195333213](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211225195333213.png)

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

## 二.Cookie Session

### 2.1 会话

**会话**：打开浏览器，点击链接，访问了多个web资源，关闭浏览器 这个过程

**有状态会话**： 下次你来的时候，服务器会知道你曾经来过

​	一个网站怎么证明 你来过？

​	客户端         服务端

​	1. 服务端给客户端一个cookie,客户端下次带上cookie就可以了;（带着红领巾应该就是小学生）

​	2. 服务器登记 你来过了 下次你来的时候我匹配你；seesion

### 2.2 保存会话的两种技术

**cookie** 

+ 客户端技术（服务端通过响应发给客户端，客户端通过请求带给服务器）

**session**

+ 服务器技术（可以保存用户的会话信息，我们可以把信息和数据放在session中）

场景：第二次访问不需要登入

cookie在开发中用的不多，一般用session

### 2.3cookie 

1. 从请求中拿到cookie
2.  服务器响应给客户端Cookie

```java
public class CookieDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //服务器告诉你，你来的时间，把这个时间封装成一个信件cookie，下次你来的时候，我将知道你来了

        //解决中文乱码
        req.setCharacterEncoding("utf-16");
        resp.setCharacterEncoding("utf-16");

        PrintWriter out = resp.getWriter();
        //cookie 从客户端拿过来
        //数组 说明cookie可能存在多个
        Cookie[] cookies = req.getCookies();
        //判断是否存在cookie
        if (cookies != null){
            out.write("你上一次访问的时间是");

            for (int i =0; i<cookies.length;i++){
                Cookie cookie = cookies[i];
                //获取cookie的名字
                if (cookie.getName().equals("lastLoginTime")){
                    //获取cookie的值
                    String cookieValue = cookie.getValue();
                    long l = Long.parseLong(cookieValue);
                    Date date = new Date(l);
                    out.write(date.toLocaleString());
                }
            }
        }else {
            out.write("这是你第一次访问本站");
        }
        //服务器给客户端响应一个cookie
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis()+"");
        resp.addCookie(cookie);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```java
//设置有效期时间 为 1 天
// 效果就是设置后 浏览器关了之后 再打开你的上一次cookie还可以在请求走 
cookie.setMaxAge(24*60*60);
```

![image-20211225212109791](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211225212109791.png)



cookie 会保存在本地文件 一般在用户目录下Appdata；

一个网站cookie 细节问题

+ 一个cookie只保存一个信息
+ 一个Web站点可以给浏览器发送多个cookie，站点最多存放20个
+ cookie 大小限制4kb
+ 浏览器最多300个

删除cookie

+ 不设置有效期，关闭浏览器，自动失效
+ 设置有效期为0

注意：

1. 

![image-20211225210349165](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211225210349165.png)

2. 

解决cookie值的乱码问题  demo没有设置也没有错误，这个方法可以用在其他写代码的过程中  解码与编码

![image-20211225220117528](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211225220117528.png)

## 其他总结

#### 1.req与resp

req 请求：**接收数据 **  HTTP请求中的所有信息会被封装到HttpServletRequest

resp 响应：**数据发送**  

[req和resp的作用及常用方法](https://blog.csdn.net/m0_51649818/article/details/119616840)

#### 2.



#### [学习视频](https://www.bilibili.com/video/BV12J411M7Sj?p=16)
#### [参考原文](https://blog.csdn.net/bell_love/article/details/105667638?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163263921816780255218049%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163263921816780255218049&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-4-105667638.pc_search_insert_js_new&utm_term=%E7%8B%82%E7%A5%9Eweb&spm=1018.2226.3001.4187)

