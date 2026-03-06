# Windows Docker Desktop 安装指南

[← 返回主页](../README.md)

---

## 前置要求

- Windows 10/11（64 位）
- 已启用 **WSL 2** 或 **Hyper-V**
- 至少 4GB 内存
- 至少 2GB 可用磁盘空间（镜像本身）

---

## 第一步：安装 Docker Desktop

1. 前往 [Docker 官网](https://www.docker.com/products/docker-desktop/) 下载 **Docker Desktop for Windows**
2. 运行安装程序，按提示完成安装
3. 安装完成后重启电脑
4. 启动 Docker Desktop，等待引擎初始化完成

<!-- 后续添加截图 -->
<!-- ![Docker Desktop 安装](../images/install/docker-desktop-install.png) -->

确认 Docker 正在运行：

```powershell
docker --version
# 应输出类似: Docker version 24.x.x, build xxxxxxx
```

---

## 第二步：拉取 MediaSync 镜像

打开 **PowerShell** 或 **Windows Terminal**：

```powershell
docker pull mediasync:latest
```

<!-- 后续添加截图 -->
<!-- ![拉取镜像](../images/install/docker-pull.png) -->

---

## 第三步：创建数据目录

为 MediaSync 的下载文件和配置数据创建本地存储目录：

```powershell
# 创建下载目录
mkdir D:\MediaSync\downloads

# 创建配置数据目录
mkdir D:\MediaSync\data

# 创建直播录制目录（可选）
mkdir D:\MediaSync\live_recordings
```

> 💡 路径可以自定义，但建议放在非系统盘，确保有足够空间存放下载的媒体文件。

---

## 第四步：启动容器

### 基础启动命令

```powershell
docker run -d `
  --name mediasync `
  --restart unless-stopped `
  -p 4399:3000 `
  -v D:\MediaSync\downloads:/downloads `
  -v D:\MediaSync\data:/app/data `
  -e MEDIASYNC_DATA_DIR="/app/data" `
  -e BACKEND_URL="http://localhost:9000" `
  mediasync:latest
```

### 包含代理和直播录制的完整启动命令

```powershell
docker run -d `
  --name mediasync `
  --restart unless-stopped `
  -p 4399:3000 `
  -v D:\MediaSync\downloads:/downloads `
  -v D:\MediaSync\data:/app/data `
  -v D:\MediaSync\live_recordings:/live_recordings `
  -e MEDIASYNC_DATA_DIR="/app/data" `
  -e BACKEND_URL="http://localhost:9000" `
  -e ALL_PROXY="http://你的代理地址:端口" `
  mediasync:latest
```

<!-- 后续添加截图 -->
<!-- ![启动容器](../images/install/docker-run.png) -->

---

## 第五步：验证部署

### 检查容器状态

```powershell
docker ps | Select-String mediasync
```

应看到容器状态为 `Up` 且端口映射为 `0.0.0.0:4399->3000/tcp`。

### 访问 Web UI

打开浏览器，访问：

```
http://localhost:4399
```

<!-- 后续添加截图 -->
<!-- ![Web UI 首页](../images/install/web-ui-home.png) -->

---

## 通过 Docker Desktop 图形界面管理

你也可以直接在 Docker Desktop 的图形界面中管理容器：

1. 打开 Docker Desktop
2. 左侧点击 **Containers**
3. 找到 `mediasync` 容器
4. 可以进行启动、停止、重启、查看日志等操作

<!-- 后续添加截图 -->
<!-- ![Docker Desktop 管理](../images/install/docker-desktop-manage.png) -->

---

## 常用管理命令

```powershell
# 查看容器日志
docker logs mediasync

# 实时查看日志
docker logs -f mediasync

# 停止容器
docker stop mediasync

# 启动容器
docker start mediasync

# 重启容器
docker restart mediasync

# 删除容器（数据不会丢失）
docker stop mediasync
docker rm mediasync
```

---

## 升级 MediaSync

```powershell
# 1. 拉取最新镜像
docker pull mediasync:latest

# 2. 停止并删除旧容器
docker stop mediasync
docker rm mediasync

# 3. 用同样的参数重新启动
docker run -d `
  --name mediasync `
  --restart unless-stopped `
  -p 4399:3000 `
  -v D:\MediaSync\downloads:/downloads `
  -v D:\MediaSync\data:/app/data `
  -e MEDIASYNC_DATA_DIR="/app/data" `
  -e BACKEND_URL="http://localhost:9000" `
  mediasync:latest
```

> ✅ 数据文件存储在本地挂载目录中，升级不会丢失任何数据。

---

## 故障排查

| 问题 | 解决方案 |
|:---|:---|
| Docker Desktop 启动失败 | 确认已启用 WSL 2 或 Hyper-V，尝试重启电脑 |
| 拉取镜像超时 | 检查网络连接，或配置 Docker 镜像加速 |
| 端口 4399 被占用 | 更换映射端口，如 `-p 8080:3000` |
| 无法访问 Web UI | 检查防火墙设置，确认容器正在运行 |
| 下载文件找不到 | 确认 `-v` 挂载路径正确，文件在本地挂载目录中 |

---

[← 返回主页](../README.md)
