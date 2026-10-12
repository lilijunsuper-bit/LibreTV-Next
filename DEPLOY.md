# LibreTV 部署指南

本仓库为 LibreTV（Next.js 重构版）的部署分支，仓库：`lilijunsuper-bit/LibreTV-Next`。

LibreTV 是**免费在线视频聚合搜索与观看平台**：服务端聚合搜索、HLS/HTTP-FLV 播放、直播/IPTV（M3U 订阅）、首页推荐等。**需要设置访问密码 `PASSWORD`**（未设置时站点会提示管理员配置）。

## 部署方式（任选其一）

推荐 **Render**（免费 Web Service、无需信用卡、跑完整 Node 进程、支持直播长连接）。

### 方式一：Render（推荐）
- 登录 https://render.com → **New +** → **Blueprint**
- 连接本仓库 `lilijunsuper-bit/LibreTV-Next`，Render 会读取根目录 `render.yaml` 自动创建服务
- 服务创建后：**Dashboard → 该服务 → Environment** 添加 `PASSWORD`（访问密码）
- 访问地址：`https://<服务名>.onrender.com`

> Free 版闲置约 15 分钟自动休眠，再次访问会重新唤醒（首次唤醒稍慢属正常）。

### 方式二：Fly.io
- 安装 flyctl：`powershell -c "irm https://fly.io/install.ps1 | iex"`
- `fly auth login`
- 进入仓库目录：`fly launch`（自动读取 `fly.toml`）
- `fly secrets set PASSWORD=你的密码`
- `fly deploy`
- 访问地址：`https://libretv.fly.dev`

> Fly 免费额度约 3 台 256MB 共享实例，超量按量计费。

### 方式三：Railway
- 登录 https://railway.app → **New Project** → **Deploy from GitHub repo**
- 选择 `lilijunsuper-bit/LibreTV-Next`（自动识别根目录 `railway.json` / Dockerfile）
- 在 Variables 中添加 `PASSWORD`
- 使用分配的域名访问

> Railway 新账户通常需要绑定信用卡才能创建项目。

## 关键环境变量
| 变量 | 必填 | 说明 |
| --- | --- | --- |
| `PASSWORD` | 是 | 访问密码 |
| `DEFAULT_SOURCES` | 否 | 预置采集站，JSON 数组 |
| `DEFAULT_LIVE_SOURCES` | 否 | 预置直播源（M3U），JSON 数组 |
| `DEFAULT_SUBSCRIPTIONS` | 否 | 预置数据源订阅（JSON 链接或 TVBOX 配置地址） |

完整变量表见上游文档：https://libretv.is-an.org/wiki/Configuration.html

## 使用
部署成功后，在站点「设置 → 源管理」添加数据源（Apple CMS 采集站 API 地址），即可聚合搜索并观看。

> 本项目仅聚合/解析第三方公开播放源，不内置任何内容，内容的合法性由对应数据源负责。
