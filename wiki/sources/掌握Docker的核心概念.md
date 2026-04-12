---
created: 2026-04-12
updated: 2026-04-12
title: 掌握Docker的核心概念而非死记命令
author:
  - ByteMonk
url:
  - https://www.youtube.com/watch?v=ObhdD49AEYw
  - https://www.youtube.com/watch?v=T1nFYnonON8
raw: "[[掌握Docker的核心概念而非死记命令]]"
---

# 掌握Docker的核心概念而非死记命令

## 摘要
通过两个 ByteMonk 视频系统讲解 Docker 核心概念：容器 vs 虚拟机、Image vs Container、Dockerfile 层级缓存、数据持久化（Volumes vs Bind Mounts）、网络通信、Docker Compose 编排。强调理解底层模型比死记命令更重要。

## 核心要点
- 容器本质是共享宿主内核的隔离进程（Namespaces + Cgroups）
- 镜像是分层只读文件系统，容器是镜像的可写实例
- Dockerfile 指令顺序决定缓存命中：改动少的指令放前面
- 自定义网络提供内置 DNS，容器通过服务名通信
- Docker Compose 是声明式的 docker run 集合

## 提取的概念
- [[掌握Docker的核心概念而非死记命令]]（已有页面，已覆盖核心内容）
