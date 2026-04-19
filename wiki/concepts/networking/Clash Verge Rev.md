---
created: 2026-04-16
updated: 2026-04-16
sources: 1
---

# Clash Verge Rev

## 定义

Clash Verge Rev 是基于 Clash 内核的图形化代理管理工具，支持 TUN 模式接管系统流量、Profile Enhancement 模板注入、精准域名路由。

## 核心要点

### TUN 模式

- 创建虚拟网卡（如 `clash-verge-tun`）拦截系统所有出站流量
- 在网卡层面处理，比 PAC 模式更彻底

### Profile Enhancement 模板

Clash Verge Rev 支持在不修改订阅源的情况下注入自定义配置：

- **Proxies 模板**：`prepend` 注入自定义代理节点
- **Rules 模板**：`prepend` 注入路由规则（优先级最高）
- **Merge 模板**：合并配置（如 DNS 设置）

模板位于：`~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/profiles/`

### SOCKS5 代理节点注入

```yaml
prepend:
  - name: "VPN-Tunnel"
    type: socks5
    server: "127.0.0.1"
    port: 1080
    udp: true
```

### 路由规则注入

```yaml
prepend:
  - 'DOMAIN-SUFFIX,tcpro.io,VPN-Tunnel'
```

### 规则匹配优先级

规则从上到下匹配，`prepend` 确保自定义规则优先级最高，在所有预定义规则之前执行。

## 相关概念

- [[SSH 隧道]]
- [[Fake-IP DNS]]
- [[SOCKS5 代理]]
- [[macOS VPN 隧道内网访问方案]]

## 来源

- [[macOS VPN 隧道内网访问方案]]
