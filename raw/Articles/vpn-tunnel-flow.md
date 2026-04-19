
# macOS 通过 Parallels Desktop 虚拟机 + SSH 隧道访问公司内网

> 在虚拟机中运行 FortiClient，通过 SSH 隧道将代理暴露到宿主机，配合 Clash Verge Rev 实现精准域名路由。

## 场景

公司要求安装 FortiClient VPN 才能访问内部服务（如 `uatstg-passport.tcpro.io`），
但不想在全局安装 FortiClient 导致所有流量被监控。

## 方案概述

在 Parallels Desktop 虚拟机中安装 FortiClient，通过 SSH 隧道将 SOCKS 代理暴露到宿主机，
配合 Clash Verge Rev 的路由规则，将公司域名精准路由到隧道。

```
你的流量 ──→ Clash Verge (TUN) ──→ 规则匹配 tcpro.io ──→ SOCKS5 (localhost:1080)
                                                                        │
SSH 隧道 ────────────────────────────────────────────────────────────────┘
  │
  ▼
Parallels VM (macOS + FortiClient) ──→ 公司内网 DNS ──→ uatstg-passport.tcpro.io
```

---

## 一、虚拟机中的设置

### 1.1 安装 Parallels Desktop 并创建 macOS 虚拟机

- 在 Parallels Desktop 中安装一台 macOS 虚拟机
- 虚拟机的网络模式使用**共享网络**（Shared Network）
### 1.2 在虚拟机中安装 FortiClient

- 下载并安装公司要求的 FortiClient
- 登录并连接公司 VPN
- 确认在虚拟机中能正常访问 `uatstg-passport.tcpro.io`

### 1.3 在虚拟机中开启 SSH 服务

macOS 默认不开启 SSH 远程登录，需要手动开启：

1. **系统设置** → **通用** → **共享** → 勾选 **远程登录**
2. 记录下虚拟机的 IP 地址（`ifconfig | grep "inet "`）
3. 或者在虚拟机中执行：
   ```bash
   sudo systemsetup -setremotelogin on
   ```

### 1.4 配置 Parallels 端口转发

在 Parallels Desktop 中配置端口转发，将虚拟机的 22 端口映射到宿主机：

1. 右键虚拟机 → **配置** → **硬件** → **网络**
2. 点击 **端口转发** → 添加规则：
   - **名称**：SSH
   - **协议**：TCP
   - **主机端口**：2222
   - **虚拟机端口**：22
   - **虚拟机 IP**：虚拟机内网 IP（如 10.211.55.3）

> 配置完成后，在宿主机执行 `ssh -p 2222 dingsheng@127.0.0.1` 应该能登录到虚拟机。

---

## 二、宿主机中的设置

### 2.1 建立 SSH 隧道

通过 SSH 的 SOCKS 代理功能，在宿主机建立通往 VM 的隧道：

```bash
ssh -f -D 1080 -N -p 2222 dingsheng@127.0.0.1
```

**参数说明：**

| 参数                       | 含义                               |
| ------------------------ | -------------------------------- |
| `-f`                     | 后台运行（fork 到后台）                   |
| `-D 1080`                | 在宿主机 localhost:1080 启动 SOCKS5 代理 |
| `-N`                     | 不执行远程命令，仅做端口转发                   |
| `-p 2222`                | 连接 Parallels 转发的端口               |

**验证隧道是否建立成功：**

```bash
# 看到 SSH 进程在 1080 端口监听即为成功
lsof -i :1080
# 输出示例：
# ssh  81731  dingsheng  5u  IPv6  0t0  TCP localhost:socks (LISTEN)
# ssh  81731  dingsheng  6u  IPv4  0t0  TCP localhost:socks (LISTEN)
```

**测试隧道连通性：**

```bash
curl -I -x socks5h://localhost:1080 "https://uatstg-passport.tcpro.io"
# 正常应返回 HTTP 200 或 302
```

### 2.2 配置便捷的 shell alias（可选）

在 `~/.zshrc` 中添加：

```bash
# 建立隧道
alias vpn-bridge="ssh -f -D 1080 -N -p 2222 dingsheng@127.0.0.1"

# 开启终端代理（仅对当前终端有效）
alias vpn-on="export ALL_PROXY=socks5://127.0.0.1:1080"
alias vpn-off="unset ALL_PROXY"
```

> **注意：** `ALL_PROXY` 只对终端命令生效，浏览器流量由 Clash 管理。

---

## 三、Clash Verge Rev 的设置

### 3.1 为什么需要 Clash？

SSH 隧道建立后，还需要让浏览器的流量**自动**走这个隧道。Clash Verge Rev 开启了 TUN 模式，接管了系统所有网络流量。如果不做配置，浏览器流量会走 Clash 默认的代理规则，无法到达公司内网。

### 3.2 需要解决的问题

1. **添加 SOCKS5 代理节点**：让 Clash 知道 SSH 隧道是一个可用的出站代理
2. **添加路由规则**：将 `tcpro.io` 域名匹配到这个代理
3. **关闭 fake-ip 解析**：Clash 默认使用 fake-ip，会导致 `tcpro.io` 解析到 `198.18.x.x` 假地址

### 3.3 使用 Profile Enhancement 模板

Clash Verge Rev 支持 Profile Enhancement 模板，可以在不修改订阅源的情况下注入自定义配置。模板文件位于：

```sh
~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/profiles/
```

#### 步骤 1：添加 SOCKS5 代理节点

编辑 `profiles/pEz36f4X6jeG.yaml`（Proxies 模板）：

```yaml
prepend:
  - name: "VPN-Tunnel"
    type: socks5
    server: "127.0.0.1"
    port: 1080
    udp: true
```

`prepend` 表示将节点排在最前面，在 proxy-groups 中可引用。

#### 步骤 2：添加路由规则

编辑 `profiles/r0l7esGD3SKM.yaml`（Rules 模板）：

```yaml
prepend:
  - 'DOMAIN-SUFFIX,tcpro.io,VPN-Tunnel'
```

`prepend` 确保该规则优先级最高，在所有预定义规则之前匹配。

#### 步骤 3：关闭 tcpro.io 的 fake-ip

编辑 `profiles/m2D0V3T17YH0.yaml`（Merge 模板）：

```yaml
dns:
  fake-ip-filter:
    - '*.tcpro.io'
    - 'tcpro.io'
```

**为什么需要这一步？**

Clash 使用 `enhanced-mode: fake-ip` 时，DNS 查询会返回 `198.18.x.x` 范围内的假 IP。这个假 IP 只在 Clash 内部有映射关系，通过 SOCKS5 代理发送时，目标地址变成了假 IP 而非真实域名，导致代理无法正确处理。

加入 `fake-ip-filter` 后，`tcpro.io` 的 DNS 查询走真实解析，拿到真实 IP 后再由 Clash 规则路由到 VPN-Tunnel。

### 3.4 重新加载配置

在 Clash Verge Rev 中：

1. 打开 **Dashboard** 页面
2. 点击**重载配置**按钮（刷新图标）
3. 或关闭再重新打开 TUN 模式

验证方法：
- **Proxies** 页面应出现 `VPN-Tunnel` 节点
- **Connections** 页面访问 `uatstg-passport.tcpro.io`，日志应显示 `match DomainSuffix(tcpro.io) using VPN-Tunnel`

---

## 四、原理详解

### 4.1 整体架构

```
┌─────────────────────────────────────────────────────────────────┐
│  宿主机 (macOS)                                                 │
│                                                                 │
│  ┌──────────────┐    ┌─────────────────┐    ┌───────────────┐  │
│  │   浏览器      │───→│  Clash (TUN)    │───→│  SOCKS5       │  │
│  │  访问域名     │    │  规则引擎       │    │  localhost:1080│  │
│  └──────────────┘    └─────────────────┘    └───────┬───────┘  │
│                                                      │          │
│                                      SSH 隧道 (加密)  │          │
│                                                      ▼          │
│                              ┌───────────────────────────────┐  │
│                              │  Parallels 端口转发            │  │
│                              │  127.0.0.1:2222 → VM:22       │  │
│                              └───────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                │
                                │ SSH 连接
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│  虚拟机 (macOS on Parallels)                                    │
│                                                                 │
│  ┌──────────┐    ┌──────────────┐    ┌───────────────────────┐  │
│  │  SSHD     │───→│  FortiClient │───→│  公司内网 DNS + 服务   │  │
│  │  (port 22)│    │  VPN 隧道    │    │  uatstg-passport...   │  │
│  └──────────┘    └──────────────┘    └───────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 流量路径详解

**Step 1：浏览器发起 HTTPS 请求**

```
浏览器 → https://uatstg-passport.tcpro.io
```

**Step 2：Clash TUN 模式拦截流量**

```
TUN 网卡 (clash-verge-tun) 捕获出站流量
  → DNS 查询 uatstg-passport.tcpro.io
  → fake-ip-filter 匹配 → 走真实 DNS 解析
  → 拿到 Cloudflare IP (104.26.9.44)
```

**Step 3：Clash 规则匹配**

```
规则引擎从上到下匹配：
  1. DOMAIN-SUFFIX,tcpro.io,VPN-Tunnel  ← 匹配成功！
  2. ...（后续规则不再执行）
```

**Step 4：流量转发到 SOCKS5 代理**

```
Clash → SOCKS5 代理 (127.0.0.1:1080)
  → SSH 进程收到 SOCKS5 请求
  → 通过 SSH 加密隧道转发到 VM
```

**Step 5：VM 中 FortiClient 处理**

```
VM SSH 进程 → 解密 → 发出原始请求
  → FortiClient VPN 隧道
    → 公司内网 DNS 解析（内网 IP）
      → 访问 uatstg-passport.tcpro.io
        → 返回响应沿原路返回浏览器
```

### 4.3 关键点

| 问题 | 解决方案 |
|------|----------|
| SSH 隧道空闲断开 | 加 `ServerAliveInterval=30` 心跳 |
| Clash fake-ip 返回假地址 | 加入 `fake-ip-filter` |
| 订阅更新覆盖配置 | 使用 Profile Enhancement 模板 |
| 终端命令走代理 | 用 `vpn-on` 设置 `ALL_PROXY` |

### 4.4 安全性

- **宿主机流量不受影响**：只有 `tcpro.io` 域名走隧道，其他流量按 Clash 默认规则
- **FortiClient 只在 VM 中**：不会拦截宿主机的全局流量
- **SSH 隧道加密传输**：宿主机到 VM 的通信通过 SSH 加密
- **端口仅监听 localhost**：`-D 1080` 只绑定 localhost，外部无法访问

---

## 五、常用命令

```bash
# 建立 SSH 隧道
vpn-bridge

# 开启终端代理（临时）
vpn-on

# 关闭终端代理
vpn-off

# 检查隧道状态
lsof -i :1080

# 测试隧道连通性
curl -I -x socks5h://localhost:1080 "https://uatstg-passport.tcpro.io"

# 查看 Clash 日志中 tcpro.io 的匹配情况
# 在 Clash Verge → Logs 页面搜索 "tcpro.io"
```

## 六、故障排查

| 症状 | 可能原因 | 解决方法 |
|------|----------|----------|
| `lsof -i :1080` 无输出 | SSH 隧道断开 | 重新执行 `vpn-bridge` |
| Clash 日志匹配到规则但无法访问 | 隧道已断开 | 检查 SSH 隧道，重新建立 |
| DNS 解析到 `198.18.x.x` | fake-ip-filter 未生效 | 重载 Clash 配置 |
| 虚拟机 SSH 无法连接 | Parallels 端口转发未配置 | 检查 Parallels 网络设置 |
| FortiClient 断连 | VM 网络变化 | 检查 VM 网络模式和 VPN 状态 |
