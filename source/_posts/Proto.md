---
title: Proto和thrift
Date: 2021-12-06
tags: [Network]
categories: [Sharing]
---

工作之后，接触了一个概念叫做RPC（remote procedure call），远程服务调用。对于大型工程来说，很多功能的实现都是通过RPC来实现的，从某种角度RPC可以类比为函数调用，但是从本地变成了远程。这就涉及到通信的技术。先写个帖子分享我对于Thrift和Protobuf的理解和使用。

# RPC

事情要从RPC通信说起。

一个正常的RPC过程可以分为以下几个步骤：

![RPC](./Proto/RPC.jpg)

1. 本地的调用者（caller）client调用本地的client stub
2. client stub将参数打包成一个消息，然后发送这个消息。打包过程叫做marshalling
3. client所在的系统将消息通过网络传输发送给服务端server
4. server将收到的包传给server stub
5. server stub解析包，得到参数。解包过程也被叫做unmarshalling
6. server stub调用服务过程。返回的结果按照相反的步骤传给client。



这里涉及到的重要组成：

- client客户端：服务的调用方。
- server服务端：服务的真正提供者。这个Server并不是提供RPC服务器IP、端口监听的模块，而是远程服务方法的具体实现，其中的代码是最普通和业务相关的代码，甚至其接口实现类本身都不知道将被某一个RPC远程客户端调用。
- Stub/Proxy：RPC框架中的“代理层”：负责处理打包/解析消息格式，网络传输协议，服务地址信息等。
- Network Service：底层传输：可以是TCP或HTTP。

为了实现RPC：有几个问题需要解决：

- 函数调用时，数据结构的约定问题：客户端和服务端实现调用的接口须一致。
- 数据传输时，序列化和反序列化问题。
- 网络通信问题。

后两者是网络传输信息中的问题。



# 序列化和反序列化

Protobuf就是专门解决序列化和反序列化的问题的。Thrift也提供了这个功能，但它同时兼具RPC远程通信的功能，这一点是Protobuf不具备的。这里集中介绍序列化和反序列化。

序列化：就是将对象转化成字节序列的过程。

反序列化：就是将字节序列转化成对象的过程。

因为网络传输数据时，无法直接传输对象。所以，所有可在网络上传输的对象都必须是可序列化的。



# Protobuf

Protocol Buffers时Google开发的一种轻便高效的结构化数据存储格式，可以用于序列化。

优点：

- 性能好：Protobuf以高效的二进制方式存储，比XML小3到10倍，速度快20到100倍
- 代码生成机制：根据数据文件自动生成结构体定义和相关方法的文件。比如A系统去调用B系统，因为A

# 参考

[Protobuf通信协议](www.cnblogs.com/xiangxiaolin/p/12712720.html)

[51CTO](https://developer.51cto.com/art/201906/597963.htm)

[长文图解Google的protobuf思考、设计、应用](www.eet-china.com/mp/a63366.html)