---
categories: websocket
tags: websocket聊天室
title: websocket聊天室
---

# socket.io

官网地址：<https://socket.io/docs/v4/>

# 1、服务器端

### 1.1、安装

socket.io v4.x版本 至少需要 Nodev10

### 1.2、下载

```javascript
npm install socket.io
```

### 1.3、使用

##### 1.3.1、启动端口

这里使用的是express

```javascript
var express = require('express');//引用express
var app = express();//创建express实例

var server = app.listen(9999, function () {//应用启动端口为9999
  var host = server.address().address;
  var port = server.address().port;
  console.log("访问地址为", host, port)
});

const socketIo = require('socket.io')(server, { cors: true });

```

##### 1.3.2、socketIo监听
```javascript
    let users = []
    //监听connection（用户连接）事件，socket为用户连接的实例
    socketIo.on('connection',(socket)=>{
      socket.on('disconnect',()=>{
        console.log("用户"+socket.id+"断开连接");
      });
      
      // 监听客户端连接事件
      socket.on('client_online', data => {
        const { nickName, id } = data
        const userInfo = {
          nickName,
          socketId:socket.id,
          id
        }
        users.push(userInfo)
        socketIo.emit('server_online',users);
     })
     
     socket.on('client_msg',(data)=>{
        //监听msg事件（这个是自定义的事件）
        // type 1代表左侧消息，2代表右侧消息
        const { msg, nickName, roomId, userId } = data
        const params = {
          msg,
          nickName,
          times:moment(new Date().getTime()).format('YYYY-MM-DD HH:mm:ss'),
          userId,
        }
        
        const leftMessage = { ...params, type:1 }
        const rightMessage = { ...params, type:2 }
        
        // 向其他所有人发送消息
        socket.broadcast.emit('server_msg',leftObj);
        // 向当前发送者返回消息
        socket.emit('server_msg',rightObj);
      })
    })
```
socket.id自动生成，每次连接都是新的id，这里通过监听client\_online事件，收集客户端传入的数据data,
通过client\_message
```
  data:{ 
    id:number;
    nickName:string;
  }
```

# 2、客户端

### 2.1、安装

socket.io v4.x版本 至少需要 Nodev10

### 2.2、下载

npm install socket.io-client

### 2.3、使用

```javascript
import io from 'socket.io-client'
const socket = io('ws://localhost:9999')

socket.emit('client_online', {
  nickName,
  id:userId,
})

```

通过client\_online自定义事件，向服务端传输当前用户信息，服务端保存当前连接数量及用户
```javascript
  socket.emit('client_msg', { 
    msg,
    nickName,
    userId
  })
```
通过client\_msg自定义事件，向服务端发送聊天信息
```javascript
  socket.on('server_msg', (data) => {
    const { chatMsg } = this.state
    
    const newChatMsg = chatMsg.concat(data)
    this.setState({
      chatMsg:newChatMsg
    })
  });
```
通过监听server\_msg，获取服务端返回的消息，并保存
