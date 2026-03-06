---
title: "REST API"
aliases:
  - "RESTful API"
  - "Representational State Transfer"
tags:
  - type/concept
  - topic/api
  - topic/system-design
  - topic/web-development
created: 2024-12-07
---

# REST API

## 定义

REST (Representational State Transfer) 是一种软件架构风格，用于设计网络应用程序的 API。它使用标准的 HTTP 方法（GET、POST、PUT、DELETE）来操作通过 URL 标识的资源，数据通常以 JSON 或 XML 格式交换。

## 核心原则

1. **无状态 (Stateless)**：每个请求包含处理所需的全部信息，服务器不保存客户端状态
2. **统一接口 (Uniform Interface)**：使用标准 HTTP 方法和状态码
3. **资源导向 (Resource-Based)**：一切皆资源，通过 URI 标识
4. **可缓存 (Cacheable)**：响应可以被缓存以提高性能
5. **分层系统 (Layered System)**：客户端无法判断是否直接连接到终端服务器

## HTTP 方法映射

| HTTP 方法 | CRUD 操作 | 描述 |
|-----------|-----------|------|
| GET | Read | 获取资源 |
| POST | Create | 创建资源 |
| PUT | Update | 更新资源（完整替换） |
| PATCH | Update | 部分更新资源 |
| DELETE | Delete | 删除资源 |

## 优势

- **简单易用**：基于 HTTP，学习曲线低
- **广泛支持**：几乎所有编程语言和框架都支持
- **可扩展性**：无状态设计便于水平扩展
- **缓存友好**：可利用 HTTP 缓存机制
- **可读性强**：URL 结构清晰，易于理解

## 局限性

- **过度获取/不足获取**：客户端无法指定需要的字段
- **多次往返**：获取关联数据可能需要多次请求
- **版本管理**：API 版本管理相对复杂
- **不适合实时**：需要轮询实现实时更新

## 适用场景

- Web 应用和移动应用的后端 API
- 公开的第三方 API
- 需要高可扩展性的系统
- CRUD 操作为主的应用

## 相关概念

- [[GraphQL]] - 解决 REST 过度获取问题的替代方案
- [[gRPC]] - 高性能的 RPC 替代方案
- [[WebSocket]] - 实时通信的补充方案

## 来源

- [[📺-The Hidden Languages Powering Your Favorite Apps  Top API Protocols]]
- [[📺-9 Must-Know REST API Design Principles for Developers]]
