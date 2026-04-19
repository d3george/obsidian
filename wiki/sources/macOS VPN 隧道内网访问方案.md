---
created: 2026-04-16
updated: 2026-04-19
title: macOS 通过 Parallels Desktop 虚拟机 + SSH 隧道访问公司内网
author: dingsheng
raw: "[[raw/articles/vpn-tunnel-flow.md]]"
---

# macOS VPN 隧道内网访问方案

## 摘要

通过 Parallels Desktop 虚拟机运行 FortiClient VPN，利用 SSH 动态端口转发将 SOCKS5 代理暴露到宿主机，配合 Clash Verge Rev 的 TUN 模式 + Profile Enhancement 模板实现精准域名路由。避免在宿主机全局安装 FortiClient 导致流量被监控。

## 核心要点

- **架构三层隔离**：VM（FortiClient）→ SSH 隧道（加密）→ 宿主机（Clash TUN + 规则引擎）
- **SSH SOCKS5 代理**：`ssh -f -D 1080 -N -p 2222 user@127.0.0.1` 建立隧道
- **Parallels 端口转发**：宿主机 2222 → VM 22，共享网络模式
- **Clash Profile Enhancement**：通过 prepend 模板注入代理节点、路由规则、DNS 过滤，不被订阅更新覆盖
- **Fake-IP Filter**：必须排除目标域名，否则 Clash 返回 `198.18.x.x` 假 IP 导致代理失效
- **精准路由**：只有 `tcpro.io` 走隧道，其余流量走 Clash 默认规则

## 流量路径

1. 浏览器发起 HTTPS 请求 → Clash TUN 拦截
2. Clash DNS 查询（fake-ip-filter 生效 → 真实解析）
3. Clash 规则匹配 `DOMAIN-SUFFIX,tcpro.io` → 转发至 VPN-Tunnel
4. SOCKS5 代理 → SSH 加密隧道
5. VM SSHD 解密 → FortiClient VPN 隧道 → 公司内网

## 关键配置

| 组件 | 作用 |
|------|------|
| Parallels 端口转发 | 127.0.0.1:2222 → VM:22 |
| SSH `-D 1080` | 宿主机 SOCKS5 代理 |
| Clash Proxies 模板 | `prepend` 注入 SOCKS5 节点 |
| Clash Rules 模板 | `prepend` 注入域名路由 |
| Clash Merge 模板 | `fake-ip-filter` 排除目标域名 |

## 故障排查

| 症状 | 原因 | 解决 |
|------|------|------|
| 链路 DIRECT 而非 VPN-Tunnel | Clash 未重新加载配置 | Dashboard → 重载配置，或切换 TUN 模式 |
| Raycast/CLI 切换 mode 后规则失效 | GUI 不同步，运行时缓存未刷新 | 手动重载配置 |

## 提取的概念

- [[SSH 隧道]]
- [[Clash Verge Rev]]
- [[Fake-IP DNS]]
- [[FortiClient VPN]]
- [[Parallels 端口转发]]
- [[SOCKS5 代理]]

## 关联图表

- 流量流转 Excalidraw 图：[x/vpn-tunnel-flow.excalidraw](../../x/vpn-tunnel-flow.excalidraw)
