---
created: 2026-04-16
updated: 2026-04-16
sources: 1
---

# FortiClient VPN

## 定义

FortiClient 是 Fortinet 开发的企业级 VPN 客户端。连接后可访问公司内网资源，但通常会拦截所有流量并可能进行监控。

## 核心要点

- 企业常要求安装 FortiClient 才能访问内部服务
- 全局安装会导致所有宿主机的流量经过 FortiClient
- **隔离方案**：在虚拟机中安装 FortiClient，只将公司域名流量通过隧道路由到 VM

## 应用场景

- 企业内网访问
- 配合 SSH 隧道实现精准域名路由（见 [[macOS VPN 隧道内网访问方案]]）

## 相关概念

- [[SSH 隧道]]
- [[Parallels 端口转发]]
- [[macOS VPN 隧道内网访问方案]]

## 来源

- [[macOS VPN 隧道内网访问方案]]
