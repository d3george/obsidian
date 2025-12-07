---
title: "SOAP"
aliases:
  - "Simple Object Access Protocol"
tags:
  - "type/concept"
  - "topic/api"
  - "topic/system-design"
  - "topic/enterprise"
created: 2024-12-07
---

# SOAP

## 定义

SOAP（Simple Object Access Protocol）是一种基于 XML 的消息传递协议，用于在分布式环境中交换结构化信息。它具有严格的标准和规范，内置安全性、事务处理和错误处理机制，是企业级 Web 服务的传统选择。

## 核心组件

1. **WSDL (Web Services Description Language)**：描述服务接口
2. **UDDI (Universal Description, Discovery and Integration)**：服务注册与发现
3. **XML Schema**：定义消息结构
4. **WS-Security**：安全标准

## 消息结构

```xml
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
    <!-- 可选的头部信息 -->
    <wsse:Security>
      <!-- 安全令牌 -->
    </wsse:Security>
  </soap:Header>
  <soap:Body>
    <!-- 实际的请求/响应数据 -->
    <m:GetPrice xmlns:m="http://example.com/prices">
      <m:Item>Apple</m:Item>
    </m:GetPrice>
  </soap:Body>
</soap:Envelope>
```

## 优势

- **标准化**：严格的规范确保互操作性
- **安全性**：WS-Security 提供企业级安全
- **事务支持**：WS-AtomicTransaction 支持分布式事务
- **可靠消息**：WS-ReliableMessaging 确保消息传递
- **平台无关**：语言和平台独立
- **错误处理**：内置标准化的错误处理（SOAP Fault）

## 局限性

- **复杂度高**：XML 解析和处理复杂
- **性能开销**：XML 消息体积大，解析慢
- **学习曲线**：需要理解大量 WS-* 规范
- **开发效率**：相比 REST 需要更多样板代码
- **不适合移动端**：带宽和处理能力要求高

## WS-* 规范族

| 规范 | 用途 |
|------|------|
| WS-Security | 消息安全、认证、加密 |
| WS-ReliableMessaging | 可靠消息传递 |
| WS-AtomicTransaction | 分布式事务 |
| WS-Addressing | 消息路由 |
| WS-Policy | 服务策略描述 |

## 适用场景

- **金融服务**：银行、支付系统等高安全要求场景
- **医疗系统**：HL7、HIPAA 合规要求
- **政府系统**：需要严格标准和审计
- **企业集成**：遗留系统集成
- **B2B 通信**：企业间正式接口

## SOAP vs REST

| 特性 | SOAP | REST |
|------|------|------|
| 协议 | 协议 | 架构风格 |
| 格式 | 仅 XML | JSON、XML 等 |
| 性能 | 较低 | 较高 |
| 安全 | 内置 WS-Security | 依赖 HTTPS/OAuth |
| 事务 | 内置支持 | 需自行实现 |
| 学习曲线 | 陡峭 | 平缓 |

## 相关概念

- [[REST API]] - 更轻量的现代替代方案
- [[gRPC]] - 高性能的现代替代方案
- [[GraphQL]] - 灵活查询的现代方案

## 来源

- [[📺-The Hidden Languages Powering Your Favorite Apps  Top API Protocols]]
