---
categories: websocket
tags: websocket群聊
title: websocket群聊
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
let groupUser = []
//监听connection（用户连接）事件，socket为用户连接的实例
socketIo.on('connection',(socket)=>{
  socket.on('disconnect',()=>{
    Object.keys(groupUser).forEach(item=>{
      groupUser[item] = groupUser[item].filter(ele=>ele.socketId !== socket.id)
    })
    console.log("用户"+socket.id+"断开连接");
  });
  
  // 监听客户端加入群聊连接事件
  socket.on('joinRoom', data=>{
    const { nickName, id, roomId } = data
    socket.join(roomId)
    if(!groupUser[roomId]) groupUser[roomId] = []
    groupUser[roomId].push({
      nickName,
      socketId:socket.id,
      id
    })
    socket.emit('roomUser', groupUser)
    socket.broadcast.emit('roomUser', groupUser)
  })
 
 socket.on('client_msg',(data)=>{
    //监听msg事件（这个是自定义的事件）
    // type 1代表左侧消息，2代表右侧消息
    const { msg, nickName, roomId, userId } = data
    const params = {
      msg,
      nickName,
      roomId,
      times:moment(new Date().getTime()).format('YYYY-MM-DD HH:mm:ss'),
      userId,
    }
    
    const leftMessage = { ...params, type:1 }
    const rightMessage = { ...params, type:2 }
    
    // 向该房间其他人发送消息
    socket.in([data.roomId]).emit('server_msg', leftMessage);
    // 向当前发送者返回消息
    socket.emit('server_msg',rightMessage);
  })
})
```

socket.id自动生成，每次连接都是新的id，这里通过监听client\_online事件，收集客户端传入的数据data,
通过client\_message

# 2、客户端

### 2.1、安装

socket.io v4.x版本 至少需要 Nodev10

### 2.2、下载

```javascript
npm install socket.io-client
```

### 2.3、使用

```javascript
import io from 'socket.io-client'
const socket = io('ws://localhost:9999')

// 根据当前用户的group，加入不同聊天群组
const group = [
    { name:"聊天群1", value:1 },
    { name:"聊天群2", value:2 },
]
// userId 为当前用户id， nickName:昵称
group.forEach(item => {
  socket.emit('joinRoom', {
    nickName,
    id:userId,
    roomId:`room${ item.value }`,
  })
})


```

通过joinRoom自定义事件，加入不同得房间。

```javascript
socket.emit('client_msg', { 
  msg,
  nickName,
  roomId: `room${ currentChat.roomId }`,
  userId
})
```

通过client\_msg自定义事件，向服务端发送聊天信息,发送消息需要传当前的roomId,服务端根据当前roomId,群发消息到对应群内

```javascript
const msgArr = new Map()
socket.on('server_msg', (data) => {
  const { roomId } = data
  if(!msgArr.get(roomId)) msgArr.set(roomId, [])
  const roomData = msgArr.get(roomId).concat(data)
  msgArr.set(roomId, roomData)
});
```

通过监听server\_msg，获取服务端返回的消息，并保存

