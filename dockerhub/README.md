# easymosdns-x Docker

这是一个基于 `mosdns-x` 的 EasyMosdns Docker 镜像。

该镜像面向自建 DNS 场景设计。在第一次启动时，它会自动初始化官方 `easymosdns` 配置到持久化的 `/etc/mosdns` 目录中；后续重启时则保留用户自己修改过的配置，不会重复覆盖。

## 功能特性

- 集成 `mosdns-x` 与官方 `easymosdns`
- 首次启动自动初始化 `easymosdns` 配置
- 使用持久化配置目录，并通过初始化标记文件防止误覆盖
- 支持启动时更新规则，并按 cron 表达式定时更新规则
- 内置本地 DNS 健康检查
- 支持更安全的重新初始化，并在覆盖前自动备份现有配置
- 支持通过 GitHub Actions 自动构建并推送多架构镜像到 Docker Hub

## 适用场景

- 家庭网络 DNS
- 局域网或路由器侧 DNS 转发
- 需要持久化配置的自建 easymosdns 部署

## 主要行为

- `easymosdns` 只在首次启动时初始化
- 后续重启时会保留 `/etc/mosdns` 下的用户修改
- 可以通过 `0 3 * * *` 这样的 cron 表达式安排规则更新时间
- 健康检查会同时验证 `mosdns` 进程和 `127.0.0.1:53` 的本地 DNS 解析

## 运行示例

```bash
docker run -d \
  --name easymosdns-x \
  -p 53:53/udp \
  -p 53:53/tcp \
  -p 9080:9080/tcp \
  -v easymosdns-x-data:/etc/mosdns \
  -e TZ=Asia/Shanghai \
  -e RULES_UPDATE_MODE=cdn \
  -e RULES_UPDATE_CRON="0 3 * * *" \
  olorz/easymosdns-x-docker:latest
```

## 项目地址

- GitHub: https://github.com/OLOrz996/easymosdns-x-docker
