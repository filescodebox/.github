<div align="center">

<img src="https://raw.githubusercontent.com/pigeonbox/pigeonbox/main/docs/brand/logo.svg" width="112" alt="PigeonBox"/>

# PigeonBox · 文件快递柜

**像取快递一样分享文件。** Anonymous passcode sharing for text & files.

一段文本、一个文件，寄件生成口令，对方凭口令取件，到期自动销毁。
无需注册、开源自托管：Docker / Kubernetes / 桌面客户端 / NAS 应用 / 路由器随你部署。

[![License](https://img.shields.io/github/license/pigeonbox/pigeonbox?color=blue)](https://github.com/pigeonbox/pigeonbox/blob/main/LICENSE)
[![Go](https://img.shields.io/badge/Go-1.26-00ADD8?logo=go&logoColor=white)](https://go.dev)
[![Vue](https://img.shields.io/badge/Vue-3-4FC08D?logo=vuedotjs&logoColor=white)](https://vuejs.org)
[![Downloads](https://img.shields.io/github/downloads/pigeonbox/pigeonbox/total?label=%E5%AE%89%E8%A3%85%E5%8C%85%E4%B8%8B%E8%BD%BD)](https://github.com/pigeonbox/pigeonbox/releases)

[快速开始](#-快速开始) · [仓库导航](#-仓库导航) · [架构图集](https://github.com/pigeonbox/pigeonbox/blob/main/docs/architecture.md) · [制品下载](https://github.com/pigeonbox/pigeonbox/releases)

</div>

## ✨ 特性一览

- 🔑 **匿名口令取件** — 寄件者拿口令、收件者凭码取件，阅后即焚，过期自动清理
- 🗄️ **多存储后端** — OpenDAL 统一抽象，共 14 种热切换：本地磁盘 / S3(MinIO) / 阿里云 OSS / 腾讯云 COS / 百度 BOS / 金山 KS3 / 华为 OBS / WebDAV / FTP / SFTP / GCS / Azure Blob / HDFS / OneDrive
- 🧩 **契约先行** — Thrift IDL 单一真相源 + 统一错误码；API 规范由后端运行时生成（`/openapi.json`）
- 🚀 **多种交付形态** — Docker / docker-compose、Kubernetes（Helm Chart）、桌面客户端（Tauri 2 托盘常驻）、NAS 应用全家桶：飞牛 fnOS 原生应用（fpk）、OpenWrt/iStoreOS ipk/apk、威联通 QTS 原生应用（QPKG，免 Docker）、群晖 DSM 套件（SPK）、绿联 UGOS Pro / 铁威马 TOS 部署包
- 📊 **开箱可观测** — Prometheus `/metrics` 默认开启，OpenTelemetry 链路追踪可选
- 🛡️ **治理内建** — API Token、上传类型/大小/频控、内容审核钩子、管理端站点配置（存库持久化，重启不丢）
- 🕸️ **P2P 联邦 & 设备直传（可选）** — 多节点联邦互认取件；桌面客户端 p2pc 端到端加密直传；MCP 端点供 AI 客户端管理

## 🗂 仓库导航

| 仓库 | 角色 | 最新版本 |
|------|------|----------|
| [**pigeonbox**](https://github.com/pigeonbox/pigeonbox) | 🗂️ 装配仓（umbrella）：`make setup` 一键拉齐全部模块，统一构建/测试/部署 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/pigeonbox)](https://github.com/pigeonbox/pigeonbox/tags) |
| [contracts](https://github.com/pigeonbox/contracts) | 📜 契约层：统一错误码 + Thrift 生成类型，纯类型零业务依赖 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/contracts)](https://github.com/pigeonbox/contracts/tags) |
| [core](https://github.com/pigeonbox/core) | 🧠 业务核心库：16 个域服务 + OpenDAL 多后端存储抽象 + `Bootstrap()` 库入口 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/core)](https://github.com/pigeonbox/core/tags) |
| [server](https://github.com/pigeonbox/server) | 🚢 独立部署壳：薄壳入口 + Dockerfile，发布多架构镜像 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/server)](https://github.com/pigeonbox/server/tags) |
| [frontend](https://github.com/pigeonbox/frontend) | 🎨 前端壳：neutral 产物 + nginx 分离镜像（平台宿主适配器归各平台仓 `web/`） | 随 server 同 `v*` |
| [frontend-core](https://github.com/pigeonbox/frontend-core) | 🖌️ 公共前端 core：平台无关应用（Vue3 + TS + Vite + Element Plus）+ 宿主适配器 SPI，Release tgz 消费 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/frontend-core)](https://github.com/pigeonbox/frontend-core/tags) |
| [desktop](https://github.com/pigeonbox/desktop) | 🖥️ 桌面客户端：Tauri 2 托盘常驻，三平台安装包 + 麒麟/统信双架构 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/desktop)](https://github.com/pigeonbox/desktop/tags) |
| [fnos](https://github.com/pigeonbox/fnos) | 🐂 飞牛 fnOS 原生应用：fpk 单进程 + 开放平台 SSO 深度融合 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/fnos)](https://github.com/pigeonbox/fnos/tags) |
| [openwrt](https://github.com/pigeonbox/openwrt) | 📡 OpenWrt/iStoreOS 原生 ipk/apk：procd 托管 + UCI 配置 + LuCI，路由器一键装 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/openwrt)](https://github.com/pigeonbox/openwrt/tags) |
| [synology](https://github.com/pigeonbox/synology) | 📦 群晖 DSM 套件（SPK）：noarch 一键装，Container Manager 编排 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/synology)](https://github.com/pigeonbox/synology/tags) |
| [qnap](https://github.com/pigeonbox/qnap) | 📦 威联通 QTS 原生应用（QPKG）：x86_64/arm_64 单进程免 Docker，QTS 账号 SSO 免登录 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/qnap)](https://github.com/pigeonbox/qnap/tags) |
| [ugreen](https://github.com/pigeonbox/ugreen) | 📦 绿联 UGOS Pro 部署包：Docker 项目一键粘贴，含国内加速编排 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/ugreen)](https://github.com/pigeonbox/ugreen/tags) |
| [terramaster](https://github.com/pigeonbox/terramaster) | 📦 铁威马 TOS 5/6/7 部署包：Docker Manager 项目导入 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/terramaster)](https://github.com/pigeonbox/terramaster/tags) |
| [p2p](https://github.com/pigeonbox/p2p) | 🕸️ P2P 联邦注册中心 + 设备直传：租约注册、口令联邦路由、WS 信令、加密中继、p2pc CLI/网页客户端 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/p2p)](https://github.com/pigeonbox/p2p/tags) |
| [kit](https://github.com/pigeonbox/kit) | 🧰 共享 Go 工具库：28 个零生态依赖通用包（retry/syncx/shutdown/workflow 等） | [![tag](https://img.shields.io/github/v/tag/pigeonbox/kit)](https://github.com/pigeonbox/kit/tags) |
| [charts](https://github.com/pigeonbox/charts) | ☸️ Kubernetes Helm Chart：Pages + OCI 双发布 | [![tag](https://img.shields.io/github/v/tag/pigeonbox/charts)](https://github.com/pigeonbox/charts/tags) |

### 依赖方向（单向，CI 强制守护）

```mermaid
graph LR
    subgraph FEW["前端(拆分双仓)"]
        frontend["🎨 frontend(壳)<br/>neutral 产物"] -->|"tgz"| fecore["🖌️ frontend-core<br/>平台无关应用 + 宿主 SPI"]
    end
    web["平台仓 web/<br/>(fnos·qnap 宿主适配器)"] -->|"tgz"| fecore
    fecore -. "HTTP API /openapi.json" .-> server["🚢 server<br/>部署壳"]
    server --> core["🧠 core<br/>业务核心库"]
    fnos["🐂 fnos<br/>fnOS 原生应用"] --> core
    openwrt["📡 openwrt<br/>OpenWrt/iStoreOS ipk/apk"] --> core
    qnap["📦 qnap<br/>QTS 原生应用"] --> core
    core --> contracts["📜 contracts<br/>错误码 + Thrift 类型 + IDL"]
    fecore -.->|"TS d.ts tgz"| contracts
    contracts --> thrift["thrift v0.13"]
    p2p["🕸️ p2p<br/>联邦注册中心"]
    core -. HTTP .-> p2p
    kit["🧰 kit<br/>共享 Go 工具库"]
    core -.->|"按需接入"| kit
```

📐 生态全景 / core 分层 / 请求流 / 数据流 / 部署形态 / 发布流水线，见 **[架构图集](https://github.com/pigeonbox/pigeonbox/blob/main/docs/architecture.md)**。

## 🚀 快速开始

**docker compose（前后端分离，推荐自托管形态）**

```bash
git clone https://github.com/pigeonbox/pigeonbox && cd pigeonbox
docker compose up -d                  # API :12345 · 前端 :80（加反代：--profile nginx）
```

**纯后端单容器（API only，无 Web UI）**

```bash
docker run -d --name pigeonbox -p 12345:12345 \
  -e PB_ADMIN_PASSWORD=change-me \
  -e PB_JWT_SECRET=$(openssl rand -hex 32) \
  ghcr.io/pigeonbox/server:latest
```

**Kubernetes（Helm）**

```bash
helm repo add pigeonbox https://pigeonbox.github.io/charts && helm repo update
helm install pigeonbox pigeonbox/pigeonbox -n pigeonbox --create-namespace
```

**桌面 & NAS & 路由器**

- 🖥️ 桌面客户端：在 [Releases](https://github.com/pigeonbox/pigeonbox/releases) 下载 `desktop-v*` 三平台安装包，连接任意 PigeonBox 服务器
- 🐂 飞牛 fnOS：下载 `fnos-v*` 应用包（fpk，原生单进程），数据落 @appdata 数据目录
- 📡 OpenWrt / iStoreOS：下载 `openwrt-v*` ipk/apk（x86_64 / aarch64），iStore 或 `opkg install` 一键装，浏览器访问 `http://<路由器IP>:12345`
- 🗄️ 威联通 QTS：下载 `qnap-v*` QPKG（x86_64 / arm_64，原生免 Docker，QTS 4.5+），App Center 手动安装
- 📦 群晖 / 绿联 / 铁威马：下载 `synology-v*` SPK（DSM 7.2+）或 `ugreen-v*`/`terramaster-v*` compose 部署包，按包内说明导入

> 默认管理员 `admin / admin123`，生产环境务必用 `PB_ADMIN_PASSWORD` 覆盖（生产模式下未注入将拒绝创建默认管理员）。

## 📦 制品出口

| 制品 | 位置 |
|------|------|
| 🐋 容器镜像（多架构） | `ghcr.io/pigeonbox/server` · `ghcr.io/pigeonbox/frontend` · `ghcr.io/pigeonbox/p2p`（`ghcr.io/pigeonbox/fnos` 已停发，冻结在 v1.14.6） |
| 📥 桌面安装包 / NAS 应用包（fpk·ipk/apk·SPK·QPKG·部署包） | 统一回挂 [hub Releases](https://github.com/pigeonbox/pigeonbox/releases) |
| ☸️ Helm Chart | [Pages 仓库](https://pigeonbox.github.io/charts/) + `oci://ghcr.io/pigeonbox/charts/pigeonbox` |

## 🤝 参与贡献

- 入口仓库 clone 后 `make setup && make test` 即可开始开发，见 [CONTRIBUTING.md](https://github.com/pigeonbox/pigeonbox/blob/main/CONTRIBUTING.md)
- 安全漏洞请走 [SECURITY.md](https://github.com/pigeonbox/pigeonbox/blob/main/SECURITY.md) 披露流程，勿直接开 issue

## 📄 许可证

全生态以 **Apache-2.0** 发布。概念灵感来自 [vastsa/FileCodeBox](https://github.com/vastsa/FileCodeBox)（LGPL-3.0），本生态为 Go 独立实现，无源码衍生关系。
