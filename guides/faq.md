# 常见问题解答（FAQ）

[← 返回主页](../README.md)

---

## 安装与部署

<details>
<summary><b>Q: 支持哪些操作系统？</b></summary>

MediaSync 通过 Docker 运行，理论上支持所有能运行 Docker 的操作系统：
- **Windows** 10/11（通过 Docker Desktop）
- **macOS**（通过 Docker Desktop）
- **Linux**（原生 Docker）
- **NAS 设备** — 群晖、威联通、极空间、绿联等
</details>

<details>
<summary><b>Q: 镜像有多大？需要多少存储空间？</b></summary>

- 镜像大小约 **2GB**（包含 Chromium、FFmpeg 等依赖）
- 建议预留足够的额外空间用于存放下载的媒体文件
</details>

<details>
<summary><b>Q: 容器启动后无法访问 Web UI？</b></summary>

1. 确认容器正在运行：`docker ps | grep mediasync`
2. 等待容器完全启动（约 30-60 秒），检查健康检查状态
3. 确认端口映射正确（默认 `4399:3000`）
4. 检查防火墙/安全组是否放行了端口
5. 查看日志排查错误：`docker logs mediasync`
</details>

<details>
<summary><b>Q: 如何修改访问端口？</b></summary>

修改 Docker 启动命令中端口映射的左侧数字。例如改为 `8080` 端口：

```bash
# docker run 方式
-p 8080:3000

# docker-compose 方式
ports:
  - "8080:3000"
```
</details>

---

## 下载相关

<details>
<summary><b>Q: 下载 YouTube 视频需要代理吗？</b></summary>

是的，在中国大陆访问 YouTube 需要配置代理。你可以：
1. 在 Docker 环境变量中设置 `ALL_PROXY`
2. 或在 Web UI 的 **设置 → 下载设置** 中配置代理地址
</details>

<details>
<summary><b>Q: 支持哪些视频画质？</b></summary>

取决于源平台：
- **YouTube** — 最高 4K/8K（需要 Cookie 和会员）
- **Bilibili** — 最高 4K（需要登录和大会员）
- **抖音** — 最高 1080p
- **其他平台** — 取决于平台提供的画质
</details>

<details>
<summary><b>Q: 下载速度很慢怎么办？</b></summary>

1. 检查是否设置了速度限制（设置 → 下载设置）
2. 确认代理服务器的带宽是否充足
3. 减小并发下载数，确保每个任务有足够带宽
4. B 站下载慢可能是因为未登录或画质限制
</details>

<details>
<summary><b>Q: 下载的文件在哪里？</b></summary>

文件保存在 Docker 挂载的下载目录中：
- Docker 命令中 `-v /你的路径:/downloads` 指定的本地路径
- 也可以在 Web UI 的 **文件浏览** 页面直接查看和播放
</details>

<details>
<summary><b>Q: 下载失败显示 "403 Forbidden" 怎么办？</b></summary>

通常是 Cookie 过期导致的：
1. 进入 **设置 → Cookies**，检查对应平台的登录状态
2. 重新登录或更新 Cookie
3. B 站还可能需要调整防风控配置
</details>

---

## 平台登录

<details>
<summary><b>Q: B站扫码登录后提示失效？</b></summary>

1. 确保手机 B 站 APP 是最新版本
2. 扫码后在手机上点击确认
3. 如果反复失败，尝试使用 Cookie 导入方式
</details>

<details>
<summary><b>Q: Cookie 多久需要更新一次？</b></summary>

不同平台的 Cookie 有效期不同：
- **Bilibili** — 通常较长（数月），但频繁使用可能被登出
- **YouTube** — 较长有效期
- **抖音** — 可能较短，建议定期更新
- 当功能出现异常（如获取不到收藏夹）时，优先检查 Cookie 状态
</details>

---

## B站风控

<details>
<summary><b>Q: B站提示风控/频率限制怎么办？</b></summary>

1. 切换到更安全的防风控预设（安全或极端安全）
2. 等待 5-10 分钟后再重试
3. 减少批量操作的频率
4. 确保 Cookie 有效且未过期
5. 详见 [防风控配置指南](./anti-risk-guide.md)
</details>

---

## 数据管理

<details>
<summary><b>Q: 升级会丢失数据吗？</b></summary>

**不会**。所有数据都存储在宿主机挂载的目录中（不在容器内），包括：
- 下载的媒体文件
- Cookie 和登录状态
- 下载历史和配置
- 订阅数据

升级时只需停止旧容器、拉取新镜像、用相同参数启动新容器即可。
</details>

<details>
<summary><b>Q: 如何备份数据？</b></summary>

直接备份宿主机上的挂载目录即可：
- 下载文件目录（如 `D:\MediaSync\downloads`）
- 数据目录（如 `D:\MediaSync\data`）
</details>

<details>
<summary><b>Q: 如何迁移到新设备？</b></summary>

1. 将宿主机上的挂载目录整体复制到新设备
2. 在新设备上安装 Docker
3. 使用相同的 Docker 启动命令，指向新的目录路径
</details>

---

## 通知与机器人

<details>
<summary><b>Q: MediaSync 支持哪些通知渠道？</b></summary>

当前内置支持两种渠道：
- **Telegram Bot**
- **企业微信应用消息**

你可以在 **设置 → 通知设置** 中直接配置并发送测试通知。
</details>

<details>
<summary><b>Q: 如何让通知里带上在线播放和下载链接？</b></summary>

在 **设置 → 通知设置** 中填写 **服务器公网地址**，例如：

```text
https://mediasync.example.com
```

设置后，下载完成和录制完成通知会自动生成可点击的播放 / 下载链接。
</details>

<details>
<summary><b>Q: 企业微信为什么收得到通知，但发送命令没反应？</b></summary>

这是因为企业微信的命令模式除了基础的 `CorpID / Secret / AgentId` 外，还需要配置消息回调：

1. 在企业微信应用里设置回调地址
2. 回调地址指向 `你的 MediaSync 地址/api/notifications/wechat/callback`
3. 配置回调 `Token`
4. 配置 `EncodingAESKey`
5. 在通知配置中开启 `enable_commands`

如果只配置了基础参数，系统可以发通知，但无法接收企业微信回调消息。
</details>

<details>
<summary><b>Q: Telegram 机器人支持哪些命令？</b></summary>

支持以下命令：
- `解析 / analyze / a`
- `下载 / download / d`
- `订阅 / subscribe / s`
- `状态 / status / st`
- `帮助 / help / h`

你也可以直接把链接发给机器人，系统会自动识别链接类型并给出可执行操作。
</details>

---

> 如果以上没有解答你的问题，欢迎提交 [Issue](https://github.com/your-repo/mediasync/issues)。

---

[← 返回主页](../README.md)
