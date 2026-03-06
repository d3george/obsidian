---
title: "gRPC"
aliases:
  - "gRPC Remote Procedure Call"
  - "Google RPC"
tags:
  - type/concept
  - topic/api
  - topic/system-design
  - topic/microservices
created: 2024-12-07
---

# gRPC

## 定义

gRPC 是 Google 开发的高性能、开源的远程过程调用（RPC）框架。它使用 HTTP/2 作为传输协议，Protocol Buffers（protobuf）作为接口定义语言和消息序列化格式，支持多种编程语言的代码自动生成。

## 核心特性

1. **高性能**：二进制协议，比 JSON 更小更快
2. **HTTP/2**：支持多路复用、头部压缩、流式传输
3. **双向流**：支持客户端流、服务端流、双向流
4. **代码生成**：从 .proto 文件自动生成多语言代码
5. **强类型**：Protocol Buffers 提供严格的类型定义

## 通信模式

```protobuf
service Greeter {
  // 一元 RPC（Unary）
  rpc SayHello (HelloRequest) returns (HelloReply);

  // 服务端流式 RPC
  rpc ListFeatures (Rectangle) returns (stream Feature);

  // 客户端流式 RPC
  rpc RecordRoute (stream Point) returns (RouteSummary);

  // 双向流式 RPC
  rpc RouteChat (stream RouteNote) returns (stream RouteNote);
}
```

## 优势

- **极高性能**：序列化/反序列化速度快，消息体积小
- **强契约**：.proto 文件定义明确的接口契约
- **多语言支持**：支持 10+ 主流编程语言
- **流式传输**：原生支持双向流
- **负载均衡**：内置客户端负载均衡支持
- **超时和取消**：内置 deadline 和 cancellation 机制

## 局限性

- **浏览器支持有限**：需要 gRPC-Web 代理
- **学习曲线**：需要学习 Protocol Buffers
- **调试困难**：二进制格式不如 JSON 直观
- **生态系统**：工具链相对 REST 不够成熟

## 适用场景

- **微服务间通信**：内部服务调用的理想选择
- **实时通信**：流式 RPC 适合实时场景
- **多语言系统**：统一的接口定义，多语言实现
- **性能敏感场景**：如高频交易、游戏服务器
- **移动端通信**：带宽敏感的移动应用

## Protocol Buffers 示例

```protobuf
syntax = "proto3";

message Person {
  string name = 1;
  int32 id = 2;
  string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    string number = 1;
    PhoneType type = 2;
  }

  repeated PhoneNumber phones = 4;
}
```

## 相关概念

- [[REST API]] - 更简单但性能较低的替代方案
- [[GraphQL]] - 灵活查询的替代方案
- [[WebSocket]] - 另一种实时通信方案

## 来源

- [[📺-The Hidden Languages Powering Your Favorite Apps  Top API Protocols]]
