

# AI 视频作品集 (Feishu Video Portfolio)

基于 [飞书多维表格 (Feishu Bitable)](https://www.feishu.cn/) 和 GitHub Actions 全自动化构建的极致响应式视频作品集网站。

## 🌟 项目亮点 (Features)
- **真·无服务器架构 (Serverless)**：利用 GitHub Pages 免费搭建，摆脱任何云服务器租用费用。
- **全自动化数据流 (Fully Automated)**：以“飞书多维表格”作为后台 CMS，只要在表格新增视频、修改标签，网站就会借助 GitHub Actions 云端按需抓取和同步，网站永远保持最新。
- **动态防过期机制 (Anti-Expiration)**：飞书云空间的文件安全链接默认包含 24 小时防御期。本项目的定制 Python 刷新脚本能够无缝穿透获取最新视频直链（`tmp_download_url`），让你的视频永不断流。
- **极简高级感 (Aesthetic UI)**：自带类 Pinterest 瀑布流响应式排版（PC端四列，手机端单列），采用高级暖色调护眼设计，原生内置第一帧智能封面截取逻辑 (`#t=0.001`)。

## 🛠 原理 & 架构
1. **CMS 后端**：你在飞书上搭建的多维表格（Bitable）。其中核心字段包含：`内容`（标题）、`样片`（视频附件）、`类型`（多选标签）、`时长`、`AI工具`。
2. **中间件同步栈**：托管在 GitHub Actions 上的 `refresh.py` 脚本，每次定点通过 Python Natively 与飞书 OpenAPI 通信，生成映射后的 `api/videos.json` 数据字典文件。
3. **前端渲染库**：轻量级 vanilla `index.html`，无需任何 Vue/React 底座，拉取数据后瞬间通过原生 DOM 动态铺展。

## 🚀 部署指南 (Deployment Setup)

### 1. 建立飞书机器人的鉴权通道
此项目脱离开本地，通过你的飞书企业自建应用 (`App ID` 和 `App Secret`) 发起 API 请求。
1. 前往 [飞书开发者后台](https://open.feishu.cn/app/) 创建一个“企业自建应用”获取对应的 **App ID** 和 **App Secret**。
2. 确保此自建应用申请了正确的权限范围，例如多维表格的阅读权限。
3. **关键步骤**：去到你存储视频的飞书多维表格页面，点击右上角的 **分享（Share）** -> 搜索你的应用名称 -> 添加为**协作者 (可阅读)**，否则云端 API 无权抓取你个人空间下的任何图片。

### 2. 配置 GitHub 云端机密环境变量 (Secrets)
前往当前 GitHub 仓库的 **Settings (设置)** -> **Secrets and variables** -> **Actions**，新增**四个** Repository secrets：
*   **`LARK_APP_ID`**：你的飞书自建应用 ID
*   **`LARK_APP_SECRET`**：你的飞书自建应用 Secret
*   **`LARK_BASE_TOKEN`**：多维表格所在的 App Token（通常位于浏览器网址栏 `/base/` 后面的那一串代码）
*   **`LARK_TABLE_ID`**：你需要抓取的那张子表的 Table ID（网址栏 `?table=` 后面的代码）

### 3. 上线与自动化刷新
1. 在 Github 的 **Settings** -> **Pages** 将托管源设为 `Deploy from a branch`，并选择 `main`，至此网站正式全网公开。
2. (推荐) 单击顶部导航栏的 **Actions**，选中 **Auto Refresh Feishu Videos**，点击右侧的 `Run workflow` 进行一次首次测试同步。
3. 此后系统将在预设时刻或者手动点击下全自动工作！

---
**技术栈：** HTML5, CSS3, ES6+, Python 3 (urllib), GitHub Actions CI/CD
