# 📦 Rocom Merchant Watcher | 洛克王国商城稀有物资监测助手


这是一个基于 Python 的自动化工具，用于监测《洛克王国》游戏商城的物资更新。通过 GitHub Actions 定时触发，当商城出现特定稀有物资（如血脉秘药、棱镜等）时，会自动通过企业微信机器人推送提醒。

## ✨ 功能特点

- **自动监测**：对接 Wegame 代理接口，定时获取商城在售清单。
- **精准推送**：仅在发现包含指定“关键词”的商品时发送提醒，避免垃圾消息打扰。
- **安全可靠**：利用 GitHub Secrets 加密存储 API Key 和 Webhook 地址，确保隐私不泄露。
- **零成本运行**：完全基于 GitHub Actions，无需购买服务器。

## 🔍 监测关键词

目前脚本会自动筛选包含以下关键词的物品：
- `棱镜`、`棱彩`
- `祝福`、`炫彩`
- `国王`
- `血脉秘药`

## ⏰ 执行计划

根据游戏刷新规律，脚本设定在以下北京时间自动运行：
- **08:00** | **12:00** | **16:00** | **20:00**
*(注：GitHub Actions 的触发可能有 5-15 分钟的随机延迟)*

---

## 🚀 快速上手

### 1. 准备工作
- 获取你的 **Wegame API Key**。
- 获取企业微信群机器人的 **Webhook 地址**。

### 2. 创建仓库并上传代码
1. 在 GitHub 上新建一个公开或私有仓库。
2. 上传 `main.py` 脚本文件。
3. 创建 `.github/workflows/rocom_check.yml` 文件，并粘贴对应的 Workflow 配置。

### 3. 配置隐私变量 (Secrets)
这是最重要的一步，确保你的 Key 不会被他人看见：
1. 进入 GitHub 仓库的 **Settings**。
2. 点击左侧菜单的 **Secrets and variables** -> **Actions**。
3. 点击 **New repository secret** 分别添加以下两个变量：
   - `WEGAME_API_KEY`: 填入你的 API Key (例如 `sk-ba04...`)。
   - `WECHAT_WEBHOOK_URL`: 填入你的企业微信 Webhook 完整链接。

### 4. 手动测试
1. 点击仓库上方的 **Actions** 选项卡。
2. 在左侧选择 **Rocom Merchant Bot**。
3. 点击 **Run workflow** 手动触发一次测试。如果商城当前有匹配的物资，你的企业微信将会收到推送。

---

## 🛠️ 技术栈

- **语言**: Python 3.9+
- **网络库**: [HTTPX](https://www.python-httpx.org/) (异步 HTTP 请求)
- **自动化**: GitHub Actions

## 🛡️ 安全说明
本项目的隐私信息（API Key 等）通过 GitHub Secrets 加密存储。脚本仅在运行时通过环境变量读取，不会在运行日志或源代码中暴露任何私密信息。

## 📝 免责声明
本工具仅供学习交流使用，请勿用于商业用途。数据来源于第三方 API，不保证 100% 的准确性或及时性。


📝原作者：[查看](https://github.com/Entropy-Increase-Team/WeGame-plugin) 
---
*如果有任何问题或建议，欢迎提交 Issue。*