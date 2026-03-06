---
title: "GraphQL"
aliases:
  - "Graph Query Language"
tags:
  - type/concept
  - topic/api
  - topic/system-design
  - topic/web-development
created: 2024-12-07
---

# GraphQL

## 定义

GraphQL 是由 Facebook 于 2012 年开发、2015 年开源的 API 查询语言和运行时。它允许客户端精确指定所需的数据结构，解决了 REST API 中常见的过度获取（over-fetching）和不足获取（under-fetching）问题。

## 核心特性

1. **精确查询**：客户端定义返回数据的结构
2. **单一端点**：所有请求通过一个端点处理
3. **强类型 Schema**：使用类型系统定义 API 能力
4. **内省 (Introspection)**：可查询 API 自身的 Schema
5. **实时订阅**：支持 Subscription 实现实时更新

## 操作类型

```graphql
# Query - 读取数据
query {
  user(id: "1") {
    name
    email
    posts {
      title
    }
  }
}

# Mutation - 修改数据
mutation {
  createUser(name: "John", email: "john@example.com") {
    id
    name
  }
}

# Subscription - 实时订阅
subscription {
  newMessage {
    content
    sender
  }
}
```

## 优势

- **减少网络请求**：一次请求获取所有需要的数据
- **避免过度获取**：只返回请求的字段
- **强类型系统**：编译时类型检查，更好的开发体验
- **API 演进**：可以添加字段而不影响现有查询
- **开发工具**：GraphiQL、Apollo DevTools 等强大工具支持

## 局限性

- **复杂度**：比 REST 更复杂，学习曲线较陡
- **缓存困难**：无法直接使用 HTTP 缓存
- **N+1 问题**：嵌套查询可能导致性能问题
- **文件上传**：需要额外处理
- **服务端复杂**：需要实现 resolver 逻辑

## 适用场景

- 移动应用（网络带宽敏感）
- 复杂数据关系的应用
- 多客户端需要不同数据的场景
- 快速迭代的产品开发

## 主流实现

- **Apollo** - 最流行的 GraphQL 客户端和服务端实现
- **Relay** - Facebook 官方的 React GraphQL 客户端
- **Hasura** - 自动生成 GraphQL API 的引擎
- **Prisma** - 现代数据库工具链

## 相关概念

- [[REST API]] - 传统的 API 架构风格
- [[gRPC]] - 另一种高效的 API 协议
- [[WebSocket]] - GraphQL Subscription 底层可能使用

## 来源

- [[📺-The Hidden Languages Powering Your Favorite Apps  Top API Protocols]]
