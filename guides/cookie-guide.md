# Cookie 与登录指南

[← 返回主页](../README.md)

---

## 概述

为了访问各平台的私有内容（收藏夹、关注列表、高画质等），MediaSync 需要你的平台登录状态。不同平台支持不同的登录方式。

---

## 各平台登录方式

| 平台 | 扫码登录 | Cookie 导入 | 浏览器登录 |
|:---:|:---:|:---:|:---:|
| Bilibili | ✅ | ✅ | — |
| YouTube | — | ✅ | ✅ |
| 抖音 | — | ✅ | — |
| 小红书 | — | ✅ | — |
| Twitter/X | — | ✅ | — |
| Instagram | — | ✅ | — |
| Pinterest | — | ✅ | — |

---

## Bilibili 登录

### 方式一：扫码登录（推荐）

1. 进入 **设置 → Cookies → Bilibili**
2. 点击 **扫码登录**
3. 使用手机 B 站 APP 扫描显示的二维码
4. 在手机上确认登录
5. 页面自动显示登录成功

<!-- 后续添加截图 -->
<!-- ![B站扫码登录](../images/guides/bilibili-qrcode.png) -->

### 方式二：Cookie 导入

1. 在浏览器中登录 B 站
2. 使用浏览器扩展（如 EditThisCookie、Cookie-Editor）导出 Cookie
3. 在 **设置 → Cookies → Bilibili** 中粘贴 Cookie 内容
4. 点击保存

---

## YouTube 登录

### 方式一：浏览器登录

1. 进入 **设置 → Cookies → YouTube**
2. 点击 **浏览器登录**
3. 系统会打开一个内置浏览器窗口
4. 在浏览器中登录你的 Google 账号
5. 登录成功后，系统自动获取 Cookie

### 方式二：Cookie 文件导入

1. 使用浏览器插件导出 YouTube 的 Cookie（Netscape 格式）
2. 在 **设置 → Cookies → YouTube** 中上传 Cookie 文件
3. 点击保存

---

## 抖音登录

1. 在浏览器中登录 [抖音网页版](https://www.douyin.com)
2. 使用浏览器开发者工具（F12）→ Application → Cookies
3. 复制所有 Cookie
4. 在 **设置 → Cookies → 抖音** 中粘贴
5. 点击保存

---

## 小红书登录

1. 在浏览器中登录 [小红书网页版](https://www.xiaohongshu.com)
2. 使用浏览器开发者工具获取 Cookie
3. 在 **设置 → Cookies → 小红书** 中粘贴
4. 点击保存

---

## Twitter/X 登录

1. 在浏览器中登录 [Twitter/X](https://x.com)
2. 使用浏览器开发者工具获取 Cookie
3. 在 **设置 → Cookies → Twitter** 中粘贴
4. 点击保存

---

## Instagram 登录

1. 在浏览器中登录 [Instagram](https://www.instagram.com)
2. 使用浏览器开发者工具获取 Cookie
3. 在 **设置 → Cookies → Instagram** 中粘贴
4. 点击保存

---

## Pinterest 登录

1. 在浏览器中登录 [Pinterest](https://www.pinterest.com)
2. 使用浏览器开发者工具获取 Cookie
3. 在 **设置 → Cookies → Pinterest** 中粘贴
4. 点击保存

---

## Cookie 获取通用方法

### 使用浏览器开发者工具

1. 在浏览器中登录对应平台
2. 按 `F12` 打开开发者工具
3. 切换到 **Application**（应用程序）标签
4. 在左侧选择 **Cookies** → 对应网站域名
5. 复制所有 Cookie 条目

### 使用浏览器扩展

推荐使用以下浏览器扩展快速导出 Cookie：

- **Cookie-Editor** — Chrome / Firefox / Edge
- **EditThisCookie** — Chrome

---

## 登录状态检查

- 在 **设置 → Cookies** 页面，每个平台旁边会显示当前登录状态
- ✅ 绿色 = 已登录
- ❌ 红色 = 未登录或 Cookie 已过期

> ⚠️ Cookie 可能会过期，如果出现获取数据失败的情况，请重新登录或更新 Cookie。

---

[← 返回主页](../README.md)
