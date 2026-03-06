# NAS Docker Compose 安装指南

[← 返回主页](../README.md)

---

适用于群晖（Synology）、威联通（QNAP）、极空间（ZSpace）、绿联（UGREEN）等支持 Docker 的 NAS 设备。

---

## 前置要求

- NAS 设备已安装 Docker / Container Manager
- 了解基本的 SSH 操作（或使用 NAS 自带的 Docker 管理界面）
- 至少 2GB 可用空间用于镜像

---

## 方式一：通过 SSH + Docker Compose 部署（推荐）

### 第一步：创建目录结构

通过 SSH 连接 NAS，执行以下命令：

**群晖（Synology）**

```bash
mkdir -p /volume1/docker/mediasync/downloads
mkdir -p /volume1/docker/mediasync/data
mkdir -p /volume1/docker/mediasync/live_recordings
```

**威联通（QNAP）**

```bash
mkdir -p /share/Container/mediasync/downloads
mkdir -p /share/Container/mediasync/data
mkdir -p /share/Container/mediasync/live_recordings
```

> 💡 路径可根据你的 NAS 实际存储卷名称调整。

### 第二步：创建 docker-compose.yml

在上面创建的 `mediasync` 目录下创建 `docker-compose.yml`：

```bash
# 群晖示例
cd /volume1/docker/mediasync

# 威联通示例
# cd /share/Container/mediasync
```

创建文件内容如下：

```yaml
services:
  mediasync:
    image: mediasync:latest
    container_name: mediasync
    restart: unless-stopped
    ports:
      - "4399:3000"                # Web UI 访问端口（可修改左侧端口号）
    volumes:
      # ---- 必需挂载 ----
      # 下载文件存储
      - ./downloads:/downloads
      # 配置、Cookie、缓存数据持久化
      - ./data:/app/data
      # ---- 可选挂载 ----
      # 直播录制文件
      - ./live_recordings:/live_recordings
    environment:
      MEDIASYNC_DATA_DIR: "/app/data"
      BACKEND_URL: "http://localhost:9000"
      # ---- 代理设置（可选）----
      # 如需访问 YouTube 等海外平台，取消注释并填入代理地址
      # ALL_PROXY: "http://代理地址:端口"
      # HTTP_PROXY: "http://代理地址:端口"
      # HTTPS_PROXY: "http://代理地址:端口"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

### 第三步：启动服务

```bash
docker compose up -d
```

> 如果你的系统使用旧版 Docker Compose，请使用 `docker-compose up -d`（带连字符）。

### 第四步：查看运行状态

```bash
docker compose ps
```

应看到 `mediasync` 容器状态为 `running (healthy)`。

### 第五步：访问 Web UI

在浏览器中打开：

```
http://NAS的IP地址:4399
```

例如：`http://192.168.1.100:4399`

<!-- 后续添加截图 -->
<!-- ![NAS Web UI](../images/install/nas-web-ui.png) -->

---

## 方式二：通过 NAS 图形界面部署

### 群晖（Synology）Container Manager

1. 打开 **Container Manager**（旧版为 Docker 套件）
2. 进入 **注册表**，搜索 `mediasync` 并下载镜像
3. 进入 **容器** → **创建**
4. 选择 `mediasync:latest` 镜像
5. 配置端口映射：本地端口 `4399` → 容器端口 `3000`
6. 配置存储空间映射：
   - `/volume1/docker/mediasync/downloads` → `/downloads`
   - `/volume1/docker/mediasync/data` → `/app/data`
7. 配置环境变量：
   - `MEDIASYNC_DATA_DIR` = `/app/data`
   - `BACKEND_URL` = `http://localhost:9000`
8. 点击 **完成** 并启动容器

<!-- 后续添加群晖操作截图 -->
<!-- ![群晖配置](../images/install/synology-config.png) -->

### 威联通（QNAP）Container Station

1. 打开 **Container Station**
2. 点击 **创建** → **应用程序**
3. 粘贴上方的 `docker-compose.yml` 内容
4. 根据实际路径调整 volumes 挂载
5. 点击 **创建**

<!-- 后续添加威联通操作截图 -->
<!-- ![威联通配置](../images/install/qnap-config.png) -->

---

## 目录挂载说明

| 容器内路径 | 用途 | 说明 |
|:---|:---|:---|
| `/downloads` | 下载的媒体文件 | 视频、音频、图片等下载内容 |
| `/app/data` | 应用数据 | Cookie、配置文件、缓存、下载历史 |
| `/live_recordings` | 直播录制文件 | 直播录制产生的视频文件（可选） |

> ⚠️ **务必正确挂载 `/app/data`**。此目录包含登录 Cookie 和所有配置信息，未挂载将导致容器重启后丢失登录状态。

---

## 常用管理命令

```bash
# 查看日志
docker compose logs mediasync

# 实时查看日志
docker compose logs -f mediasync

# 停止服务
docker compose stop

# 启动服务
docker compose start

# 重启服务
docker compose restart
```

---

## 升级 MediaSync

```bash
cd /volume1/docker/mediasync  # 进入 docker-compose.yml 所在目录

# 拉取最新镜像并重新启动
docker compose pull
docker compose up -d
```

> ✅ 挂载目录中的数据（Cookie、配置、下载文件）会完整保留，升级无数据丢失风险。

---

## 故障排查

| 问题 | 解决方案 |
|:---|:---|
| 镜像拉取失败 | 检查 NAS 网络连接，或配置 Docker 镜像源 |
| 端口冲突 | 修改 `docker-compose.yml` 中的左侧端口号（如 `8080:3000`） |
| 无法从局域网访问 | 确认 NAS 防火墙已放行对应端口 |
| 容器反复重启 | 检查日志 `docker compose logs mediasync`，确认挂载路径存在 |
| 权限问题 | 确保挂载目录有正确的读写权限 |

---

[← 返回主页](../README.md)
