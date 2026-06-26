<h1 align="center">MediaSync</h1>

<p align="center">
  <strong>多平台媒体解析、下载、订阅追踪与文件管理工具</strong>
</p>

<p align="center">
  <a href="#功能亮点">功能亮点</a> ·
  <a href="#支持平台">支持平台</a> ·
  <a href="#快速开始">快速开始</a> ·
  <a href="#安装部署">安装部署</a> ·
  <a href="#使用文档">使用文档</a> ·
  <a href="#常见问题">常见问题</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License" />
  <img src="https://img.shields.io/badge/docker-ready-brightgreen.svg" alt="Docker Ready" />
  <img src="https://img.shields.io/badge/platform-Windows%20%7C%20NAS%20%7C%20Linux-lightgrey.svg" alt="Platform" />
</p>

---

## 项目简介

MediaSync 是一款面向个人媒体归档场景的 Web UI 工具，支持从 YouTube、Bilibili、抖音、小红书、Twitter/X、Instagram、Pinterest 等平台解析内容、选择格式、批量下载，并可通过订阅自动追踪新内容。

你可以把它部署在 NAS / Linux 服务器上长期运行，也可以使用 Windows 桌面版在本机管理下载任务。配置、Cookie、下载历史和缓存数据都可以持久化保存，适合日常收藏整理、素材备份和直播录制。

<p align="center">
  <img src="./image/单个解析.png" alt="MediaSync 媒体解析界面" width="100%" />
</p>

---

## 功能亮点

- **多平台解析下载**：支持单个链接、批量链接、播放列表、合集、收藏夹、关注列表等内容类型。
- **格式与画质选择**：可按平台返回的格式选择分辨率、封装格式、音频格式和下载策略。
- **批量任务管理**：支持批量解析、批量下载、跳过已下载内容、暂停、恢复、取消和进度查看。
- **订阅自动追踪**：定时扫描 UP 主、频道、收藏夹或合集，新内容可自动加入下载队列。
- **Cookie 与登录管理**：内置多平台 Cookie 导入、浏览器登录、B 站扫码登录等能力。
- **通知与远程命令**：支持 Telegram Bot 和企业微信通知，可接收下载结果、订阅摘要并远程触发任务。
- **直播录制与文件管理**：支持直播监控录制、下载文件浏览、在线播放、图片预览和文件夹打包。
- **桌面端与 Docker 部署**：Windows 桌面安装包、Docker、Docker Compose / NAS 均可使用。

---

## 支持平台

| 平台 | 主要能力 |
|:---|:---|
| Bilibili | 视频解析、收藏夹、稍后再看、关注 UP、合集 / 系列、订阅、直播录制、防风控配置 |
| YouTube | 视频解析、播放列表、频道、喜欢的视频、订阅、直播录制 |
| 抖音 | 视频解析、无水印下载、收藏、关注、合集 / 系列 |
| 小红书 | 笔记解析、视频 / 图片下载、收藏笔记 |
| Twitter/X | 视频与图片解析下载 |
| Instagram | 视频与图片解析下载 |
| Pinterest | Pin / 画板解析下载 |

> 不同平台能力会受登录状态、Cookie 有效期、地区网络和平台接口变化影响。遇到私密内容、高清格式或风控提示时，请先在设置中补充 Cookie 或代理。

---

## 快速开始

### Windows 桌面版

1. 前往 GitHub Releases 下载最新的 `MediaSync-Setup-*.exe`。
2. 运行安装包，安装完成后从桌面或开始菜单启动 MediaSync。
3. 首次进入后建议先设置下载目录、代理和常用平台 Cookie。

### Docker 一键运行

```bash
docker run -d \
  --name mediasync \
  --restart unless-stopped \
  -p 4399:3000 \
  -v /path/to/downloads:/downloads \
  -v /path/to/data:/app/data \
  -e MEDIASYNC_DATA_DIR="/app/data" \
  -e BACKEND_URL="http://localhost:9000" \
  lincolnpark2000/mediasync:latest
```

启动后访问：

```text
http://你的IP:4399
```

---

## 安装部署

### Windows Docker Desktop

适合在 Windows 上通过 Docker 长期运行。

```powershell
docker pull lincolnpark2000/mediasync:latest

mkdir D:\MediaSync\downloads
mkdir D:\MediaSync\data

docker run -d `
  --name mediasync `
  --restart unless-stopped `
  -p 4399:3000 `
  -v D:\MediaSync\downloads:/downloads `
  -v D:\MediaSync\data:/app/data `
  -e MEDIASYNC_DATA_DIR="/app/data" `
  -e BACKEND_URL="http://localhost:9000" `
  lincolnpark2000/mediasync:latest
```

访问 `http://localhost:4399` 即可打开 Web UI。

### NAS Docker Compose

适合群晖、威联通、极空间、绿联等支持 Docker 的 NAS。

```yaml
services:
  mediasync:
    image: lincolnpark2000/mediasync:latest
    container_name: mediasync
    restart: unless-stopped
    ports:
      - "4399:3000"
    volumes:
      - /volume1/docker/mediasync/downloads:/downloads
      - /volume1/docker/mediasync/data:/app/data
      - /volume1/docker/mediasync/live_recordings:/live_recordings
    environment:
      MEDIASYNC_DATA_DIR: "/app/data"
      BACKEND_URL: "http://localhost:9000"
      # ALL_PROXY: "http://你的代理地址:端口"
```

启动：

```bash
docker compose up -d
```

访问：

```text
http://NAS的IP地址:4399
```

### 环境变量

| 变量名 | 说明 | 默认值 |
|:---|:---|:---|
| `MEDIASYNC_DATA_DIR` | 配置、Cookie、缓存等数据目录 | `/app/data` |
| `BACKEND_URL` | 后端 API 地址 | `http://localhost:9000` |
| `ALL_PROXY` | 全局代理地址，可用于 YouTube 等平台 | 空 |
| `HTTP_PROXY` | HTTP 代理 | 空 |
| `HTTPS_PROXY` | HTTPS 代理 | 空 |
| `MEDIASYNC_BILIBILI_COOKIE_FILE` | 可选的 B 站 Cookie 文件覆盖路径 | 空 |

### 更新升级

Docker：

```bash
docker pull lincolnpark2000/mediasync:latest
docker stop mediasync
docker rm mediasync
# 使用原来的参数重新 docker run
```

Docker Compose：

```bash
docker compose pull
docker compose up -d
```

只要 `/downloads` 和 `/app/data` 已正确挂载到宿主机，升级不会丢失 Cookie、配置、缓存和下载历史。

---

## 常用入口

| 目标 | 入口 |
|:---|:---|
| 导入 Cookie / 登录平台 | 设置中心 -> Cookie 管理 |
| 设置代理 | 设置中心 -> 下载设置 |
| 设置保存路径 | 设置中心 -> 路径设置 |
| 创建订阅 | 订阅同步 |
| 查看下载文件 | 文件浏览 |
| 配置通知 | 设置中心 -> 通知系统 |
| 处理 B 站风控 | 设置中心 -> B 站防风控 |

---

## 使用文档

| 文档 | 说明 |
|:---|:---|
| [Windows Docker Desktop 安装指南](./guides/install-windows.md) | Windows Docker 图文安装教程 |
| [NAS Docker Compose 安装指南](./guides/install-nas.md) | 群晖 / 威联通等 NAS 部署教程 |
| [下载功能详解](./guides/download-guide.md) | 单个 / 批量下载、格式选择、任务管理 |
| [收藏与关注功能详解](./guides/favorites-guide.md) | 浏览和管理各平台收藏夹与关注列表 |
| [订阅功能详解](./guides/subscription-guide.md) | 创建订阅、自动扫描与自动下载 |
| [Cookie 与登录指南](./guides/cookie-guide.md) | 各平台登录方式与 Cookie 导入教程 |
| [通知与机器人详解](./guides/notification-guide.md) | Telegram / 企业微信通知与远程命令 |
| [搜索功能详解](./guides/search-guide.md) | 跨平台聚合搜索使用方法 |
| [直播录制详解](./guides/live-recorder-guide.md) | 直播监控、录制与文件管理 |
| [文件管理详解](./guides/file-browser-guide.md) | 内置文件浏览器与在线播放 |
| [防风控配置指南](./guides/anti-risk-guide.md) | B 站防风控策略配置 |
| [系统设置详解](./guides/settings-guide.md) | 下载、路径、通知、主题等设置 |
| [常见问题解答](./guides/faq.md) | 常见问题与排错指南 |

---

## 开发与打包

### 前端开发

```bash
cd frontend
npm install
npm run dev
```

默认开发地址为 `http://localhost:3100`。

### Windows 桌面安装包

```powershell
.\build-installer.bat
```

常用参数：

```powershell
.\build-installer.bat --quick
.\build-installer.bat -ForceClean
.\build-installer.bat -Release
```

- 默认使用快速压缩，构建更快，安装包体积略大。
- 发布正式小体积安装包时使用 `-Release`。
- 仅后端或启动器变更时可使用 `--quick` 跳过前端构建。

---

## 常见问题

<details>
<summary><b>容器启动后无法访问 Web UI？</b></summary>

1. 确认容器正在运行：`docker ps`
2. 检查端口映射是否为 `4399:3000`
3. 检查防火墙是否放行 `4399`
4. 查看日志：`docker logs mediasync`
</details>

<details>
<summary><b>B 站提示 403 / 412 / 需要登录怎么办？</b></summary>

进入设置中心的 Cookie 管理，重新导入或登录 B 站 Cookie。桌面端也可以通过 `MEDIASYNC_BILIBILI_COOKIE_FILE` 指定额外 Cookie 文件路径。
</details>

<details>
<summary><b>YouTube、Instagram、Pinterest 解析失败怎么办？</b></summary>

先确认网络和代理可用，再根据内容类型导入对应平台 Cookie。私密列表、登录可见内容和高清格式通常需要完整登录 Cookie。
</details>

<details>
<summary><b>升级会丢失数据吗？</b></summary>

不会。前提是你已经把 `/downloads` 和 `/app/data` 挂载到宿主机目录。Cookie、配置、缓存和下载历史都保存在这些目录中。
</details>

更多问题请查看 [完整 FAQ](./guides/faq.md)。

---

## 免责声明

本项目仅供个人学习、研究与技术交流使用。

1. 请遵守所在地法律法规，以及各平台的用户协议和版权政策。
2. 下载的视频、音频、图片等内容版权归原作者及内容平台所有。
3. 去水印能力仅用于对自己创作或合法持有授权的内容进行备份存档。
4. 各平台接口策略可能随时变化，使用本工具产生的账号限制、下载失败或其他风险由使用者自行承担。
5. 本项目按现状提供，不附带任何明示或暗示担保。

使用本项目即表示你已阅读并同意上述声明。如不同意，请停止使用。

<p align="center">
  <sub>如果觉得好用，欢迎给个 Star 支持一下。</sub>
</p>
