---
created: 2026-04-16
updated: 2026-04-16
sources: 1
---

# SOCKS5 代理

## 定义

SOCKS5 是 SOCKS 协议的第五个版本，提供通用的代理功能。它在传输层工作，支持 TCP 和 UDP，不关心上层协议类型。

## 核心要点

### SOCKS5 vs HTTP 代理

| 特性 | SOCKS5 | HTTP 代理 |
|------|--------|-----------|
| 工作层 | 传输层 | 应用层 |
| 协议支持 | TCP + UDP | 仅 HTTP |
| 协议感知 | 无，透传流量 | 解析 HTTP 头 |
| 域名解析 | `socks5h` 代理端解析 | 代理端解析 |

### 使用方式

```bash
# curl 使用 SOCKS5 代理（代理端做 DNS 解析）
curl -I -x socks5h://localhost:1080 "https://example.com"

# 设置环境变量
export ALL_PROXY=socks5://127.0.0.1:1080
```

### 与 SSH 的关系

SSH 的 `-D` 参数在本地创建 SOCKS5 代理端点：

```bash
ssh -f -D 1080 -N -p 2222 user@127.0.0.1
# 此时 localhost:1080 就是一个 SOCKS5 代理
```

### `socks5` vs `socks5h`

- `socks5`：本地做 DNS 解析，代理端收到 IP
- `socks5h`：代理端做 DNS 解析（h = hostname），代理端收到域名

需要远端 DNS 时（如内网解析），必须用 `socks5h`。

## 相关概念

- [[SSH 隧道]]
- [[Clash Verge Rev]]
- [[Fake-IP DNS]]
- [[macOS VPN 隧道内网访问方案]]

## 来源

- [[macOS VPN 隧道内网访问方案]]
