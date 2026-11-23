# My-Panel (BastionSSH 网关与面板服务)

专为现代运维打造的极轻量 **SSH / 纯真 PTY WebTTY / Docker 容器与应用商店管理面板** 后端服务。

## 🌟 核心特性

- 🖥️ **全双工 WebTTY 终端**：基于 xterm.js 与 PTY，支持 `top`、`htop`、`vim`、`nano`、快捷指令库与手机端长按选词复制。
- 🐳 **完整 Docker 管理控制台**：多主机容器启停重启、日志查看、实时 stats 性能监控、文件浏览器与镜像管理。
- 🛒 **Docker Compose 应用商店**：支持一键从官方/自定义 Git 仓库拉取模板、可视化配置环境变量并自动化部署。
- 📊 **多主机实时状态大盘**：CPU / 内存 / 磁盘 / 网络流量实时仪表盘与进程监控。
- 📁 **内置 SFTP 管理器**：支持远程文件树浏览、在线编辑、上传下载与权限管理。
- 🔒 **Cloudflare Tunnel 一体化集成**：容器内置 `cloudflared`，仅需填入 Token 即可一键拉起安全免公网 IP 的 HTTPS 穿透。

---

## 🚀 快速启动

### 1. 启动服务

```bash
cd my-panel
docker compose up -d
```

### 2. 访问面板

打开浏览器访问：
```text
http://<宿主机IP>:3000
```

---

## ⚙️ 网络模式选择

在 `docker-compose.yml` 中支持两种网络模式：

### 模式 A：端口映射模式（默认）
- 使用标准 Docker Bridge 网络，将容器 3000 端口映射到宿主机：
  ```yaml
  ports:
    - "3000:3000"
  ```
- 访问宿主机服务：可通过 `host.docker.internal` 访问宿主机。

### 模式 B：主机网络模式（Host 模式，推荐自建/单机使用）
- 与宿主机共享网络栈，解除容器网络隔离，能够**直接免密/直连访问宿主机 `127.0.0.1:22` 的 SSH 服务**：
  ```yaml
  network_mode: host
  ```
  *(注：启用 Host 模式时需注释掉 `ports` 字段)*

---

## 🌐 配置 Cloudflare 穿透 (可选)

若需要从外网免公网 IP 安全访问面板：

1. 在 [Cloudflare Zero Trust 控制台](https://one.dash.cloudflare.com/) 创建一个 Tunnel。
2. 配置 Public Hostname 指向服务 `http://localhost:3000`。
3. 在 `docker-compose.yml` 中或 `.env` 填入 Token：
   ```yaml
   environment:
     - TUNNEL_TOKEN=你的Cloudflare_Tunnel_Token
     - TUNNEL_PROTOCOL=http2
   ```
4. 执行 `docker compose up -d` 重新加载容器，服务启动时将自动建立加密穿透通道。

---

## 📦 目录挂载与数据持久化

| 宿主机路径 | 容器内路径 | 说明 |
| :--- | :--- | :--- |
| `/var/run/docker.sock` | `/var/run/docker.sock` | 允许面板管理宿主机的 Docker 守护进程 |
| `./data` | `/root/.config/bastionssh` | 应用商店仓库缓存与持久化配置目录 |
