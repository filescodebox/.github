# FileCodeBox 生态

高性能文件/文本匿名分享平台(Go + Hertz + GORM 后端,Vue3 前端),按角色拆分的多仓库结构。

## 仓库导航

| 仓库 | 角色 | 说明 |
|------|------|------|
| [contracts](https://github.com/filescodebox/contracts) | 契约层 | 统一错误码 + Thrift 生成类型,纯类型零业务依赖,所有实现方的单一真相源 |
| [core](https://github.com/filescodebox/core) | 业务核心库 | 10 个域服务 + repo/storage + bootstrap 库入口,`Bootstrap()` 一次调用拉起全部业务 |
| [server](https://github.com/filescodebox/server) | 独立部署应用 | 薄壳入口 + 前端静态资源 + Dockerfile,自托管标准形态 |
| [frontend](https://github.com/filescodebox/frontend) | Web 前端 | Vue3 + TS + Vite + Element Plus,API 类型由 openapi.json 自动生成 |
| [filecodebox-fnos](https://github.com/filescodebox/filecodebox-fnos) | 飞牛 fnOS 应用 | 单容器库式复用 core,接入飞牛 SSO/共享目录/通知/内网穿透 |
| [FileCodeBox](https://github.com/filescodebox/FileCodeBox) | 历史存档(镜像) | 拆分前的单仓库版本,保留完整历史 |

## 依赖方向

```
frontend ─┐
server ───┼──► core ──► contracts
fnos ─────┘
```

单向依赖,CI 强制守护(contracts 零项目内依赖;core 不依赖任何下游)。

## 快速开始

```bash
docker run -d -p 12345:12345 \
  -e FCB_JWT_SECRET=$(openssl rand -hex 32) \
  ghcr.io/filescodebox/server:latest
```

默认管理员 `admin / admin123`(生产务必修改并注入 `FCB_ADMIN_PASSWORD`)。

## 版本

各仓库独立语义化版本,镜像经 GitHub Actions 打 tag 自动发布至 [ghcr.io/filescodebox](https://github.com/orgs/filescodebox/packages)。
