# Docker Compose 集合仓库

收录个人常用的各种服务的 `docker-compose` 部署模板，可直接配合 Docker 或 **My-Panel 应用商店** 一键部署。

## 📂 项目列表

| 目录 | 服务名称 | 简介 | 默认端口 |
| :--- | :--- | :--- | :--- |
| [openwrt-cfnat](./openwrt-cfnat/) | OpenWrt + CFNAT | OpenWrt 软路由及 Cloudflare NAT 优选 IP 路由加速 | 1234 |
| [my-panel](./my-panel/) | My-Panel (BastionSSH) | 极轻量现代 SSH / WebTTY / Docker 运维网关与管理面板后端 | 3000 |

## 🚀 使用方法

选择对应目录，根据 `README.md` 与 `.env.example` 修改配置后启动：

```bash
cd <项目目录>
cp .env.example .env # 若有环境变量需求
docker compose up -d
```
