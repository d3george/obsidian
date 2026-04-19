---
created: 2026-04-16
updated: 2026-04-16
sources: 1
---

# Fake-IP DNS

## 定义

Fake-IP 是 Clash 代理工具使用的一种 DNS 增强模式。DNS 查询返回 `198.18.x.x` 范围内的假 IP 地址，而非真实解析结果。代理在内部维护域名到假 IP 的映射，从而加速连接并减少真实 DNS 查询。

## 核心要点

### 工作原理

1. 应用发起 DNS 查询 `example.com`
2. Clash 返回 `198.18.0.1`（假 IP）
3. 应用连接 `198.18.0.1`
4. Clash 拦截连接，通过内部映射查找真实域名
5. Clash 使用自己的 DNS 解析并转发流量

### 问题场景

当代理流量需要通过 SOCKS5 转发到另一台机器时（如 SSH 隧道），假 IP 会导致问题：

- SOCKS5 代理收到的目标地址是 `198.18.x.x` 而非域名
- 远端代理无法识别这个假 IP，导致连接失败

### 解决方案：Fake-IP Filter

```yaml
dns:
  fake-ip-filter:
    - '*.tcpro.io'
    - 'tcpro.io'
```

加入 filter 的域名不走假 IP 模式，直接返回真实解析结果。

### 何时使用

- 需要让代理后端做 DNS 解析的场景
- 通过 SOCKS5 代理转发域名，但远端需要真实域名而非 IP 的场景
- 内网域名需要通过远端 DNS 解析的场景

## 相关概念

- [[Clash Verge Rev]]
- [[SOCKS5 代理]]
- [[macOS VPN 隧道内网访问方案]]

## 来源

- [[macOS VPN 隧道内网访问方案]]
