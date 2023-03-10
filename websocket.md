### websocket

一般网页 点击网页后，前端向后端 发送请求（使用http协议）

那么如何实现后端主动先前端发送数据，比如：网页游戏 聊天室 文件共享 扫码登入

### http定时轮询 （轮询）

在前端代码中每隔一段时间向后端发送请求。 前端不断询问后端 这个码有人扫过了没。

（伪服务器推）

消耗带宽 服务器压力大 

应用：微信扫码登入

### 长轮询

客户端发起请求后，服务端发现当前没有新的数据，这个时候服务端没有立即返回请求，而是将请求挂起，在等待一段时间后(一般为30s或者是60s)，发现还是没有数据更新的话，就返回一个空结果给客户端。客户端在收到服务端的回复后，立即再次向服务端发送新的请求。

应用：这就是百度网盘的扫码登录   rabbitMQ 消费者取消息 

避免了客户端大量的重复请求。

服务端资源大量消耗，服务端会一直hold住客户端的请求，这部分请求会占用服务器的资源。难以处理数据更新频繁的情况 如果数据更新频繁，会有大量的连接创建和重建过程，这部分消耗很大。                        

### websocket

`http1.0`：单工。因为是短连接，客户端发起请求之后，服务端处理完请求并收到客户端的响应后即断开连接。

`http1.1`：半双工。默认开启长连接`keep-alive`，开启一个连接可发送多个请求。

`http2.0`：全双工，允许服务端主动向客户端发送数据。

> WebSocket 是独立的、创建在 TCP(TCP 是基于全双工的可信传输协议的) 上的协议。
>
> Websocket 通过[HTTP](https://baike.baidu.com/item/HTTP?fromModule=lemma_inlink)/1.1 协议的101状态码进行握手。101：HTTP协议切换为WebSocket协议。

![image-20221203194314833](http://bijioss.donggei.top/image-20221203194314833.png)

![image-20221203171417367](http://bijioss.donggei.top/image-20221203171417367.png)

