---
title: WebSocket
aliases:
  - WS
  - WebSockets
tags:
  - type/concept
  - topic/api
  - topic/system-design
  - topic/websocket
created: 2024-12-07
---

# WebSocket

## 定义

WebSocket 是一种在单个 TCP 连接上提供全双工通信通道的协议。它允许服务器主动向客户端推送数据，解决了 HTTP 请求-响应模型的限制，是构建实时应用的基础技术。

## 工作原理

1. **握手阶段**：通过 HTTP Upgrade 请求建立连接
2. **数据传输**：建立后通过帧传输数据
3. **关闭连接**：任一方可发起关闭

```
客户端 → 服务器: HTTP Upgrade 请求
GET /chat HTTP/1.1
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==

服务器 → 客户端: HTTP 101 Switching Protocols
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=

[双向数据传输开始]
```

## 核心特性

1. **全双工**：客户端和服务器可同时发送数据
2. **低延迟**：无需为每条消息建立新连接
3. **轻量级**：握手后帧头开销很小（2-14 字节）
4. **持久连接**：连接保持打开直到显式关闭
5. **跨域支持**：原生支持跨域通信

## 客户端 API

```javascript
// 创建连接
const socket = new WebSocket('wss://example.com/socket');

// 连接打开
socket.onopen = (event) => {
  console.log('Connected');
  socket.send('Hello Server!');
};

// 接收消息
socket.onmessage = (event) => {
  console.log('Received:', event.data);
};

// 连接关闭
socket.onclose = (event) => {
  console.log('Disconnected:', event.code, event.reason);
};

// 错误处理
socket.onerror = (error) => {
  console.error('Error:', error);
};

// 发送消息
socket.send(JSON.stringify({ type: 'message', content: 'Hello' }));

// 关闭连接
socket.close(1000, 'Normal closure');
```

## 优势

- **实时性**：毫秒级消息传递
- **效率高**：避免 HTTP 轮询的开销
- **双向通信**：服务器可主动推送
- **广泛支持**：所有现代浏览器支持
- **协议简单**：易于理解和实现

## 局限性

- **连接管理**：需要处理重连、心跳
- **资源消耗**：长连接占用服务器资源
- **负载均衡**：需要支持 WebSocket 的负载均衡器
- **防火墙问题**：某些企业环境可能阻止
- **无内置重连**：需要自行实现断线重连

## 适用场景

- **即时通讯**：聊天应用、消息推送
- **实时协作**：在线文档、白板
- **实时数据**：股票行情、体育比分
- **在线游戏**：多人游戏同步
- **IoT 设备**：设备状态监控
- **通知系统**：实时告警推送

## WebSocket vs 其他技术

| 技术 | 方向 | 延迟 | 适用场景 |
|------|------|------|----------|
| WebSocket | 双向 | 低 | 实时应用 |
| SSE (Server-Sent Events) | 单向（服务器→客户端） | 低 | 实时通知 |
| HTTP 轮询 | 单向 | 高 | 简单场景 |
| HTTP 长轮询 | 单向 | 中 | 兼容性要求高 |

## 相关技术

- **Socket.IO**：WebSocket 的增强库，自动降级
- **SignalR**：.NET 的实时通信库
- **Pusher**：实时通信 SaaS 服务

## 相关概念

- [[REST API]] - 请求-响应模式的 API
- [[gRPC]] - 支持双向流的 RPC 框架
- [[GraphQL]] - Subscription 可基于 WebSocket

## 来源

- [[📺-The Hidden Languages Powering Your Favorite Apps  Top API Protocols]]
- [[📺-How Web Sockets work  System Design Interview Basics]]