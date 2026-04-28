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


为了确保你的隐私万无一失，这里是关于 **GitHub Secrets** 配置的详细图文级操作指南。

通过这个设置，即便你的 GitHub 仓库是**公开（Public）**的，别人也只能看到你的代码逻辑，而永远无法获取你的 Key 和 Webhook 地址。

---

### 第一步：在 GitHub 仓库中创建 Secret

1.  打开你的 GitHub 仓库页面。
2.  点击顶部菜单栏最右侧的 **Settings**（设置）。
3.  在左侧侧边栏中，找到 **Security** 部分，点击 **Secrets and variables** 展开，然后选择 **Actions**。
4.  点击页面右侧的大按钮：**New repository secret**（新建仓库机密）。

#### 5. 添加第一个 Secret (API Key)：
*   **Name**: 输入 `WEGAME_API_KEY`（必须全大写，建议和代码保持一致）。
*   **Secret**: 粘贴你的 Key，即 `sk-ba042e079cf9ccb30e72b3d5af458f45`。
*   点击 **Add secret**。

#### 6. 添加第二个 Secret (Webhook)：
*   再次点击 **New repository secret**。
*   **Name**: 输入 `WECHAT_WEBHOOK_URL`。
*   **Secret**: 粘贴你企业微信群机器人的完整 Webhook 链接。
*   点击 **Add secret**。

**完成后，你应该能在页面上看到两个变量，但它们的内容是加密不可见的。**

---

### 第二步：代码是如何“偷”到这些 Key 的？（原理解析）

你可能会担心：代码里没写 Key，它是怎么运行的？这需要一个“桥梁”。

#### 1. 桥梁的一端：YAML 配置文件
在 `.github/workflows/rocom_check.yml` 文件中，有一段关键配置：

```yaml
env:
  MY_API_KEY: ${{ secrets.WEGAME_API_KEY }}
  MY_WEBHOOK: ${{ secrets.WECHAT_WEBHOOK_URL }}
```
*   **意思是**：GitHub 运行脚本时，会从加密保险箱（Secrets）里取出值，临时存放在这台虚拟电脑的“系统环境变量”里，起名叫 `MY_API_KEY`。

#### 2. 桥梁的另一端：Python 脚本
在 `main.py` 中，我们使用了 `os.getenv`：

```python
import os

class RocomTargetBot:
    def __init__(self):
        # 从虚拟电脑的“系统环境变量”里读取那个叫 MY_API_KEY 的值
        self.api_key = os.getenv("MY_API_KEY")
        self.webhook_url = os.getenv("MY_WEBHOOK")
```
*   **意思是**：Python 会去系统里找这个变量。因为 GitHub 已经提前塞进去了，所以 Python 就能顺利拿到真正的 Key。

---

### 第三步：如何验证配置是否成功？

1.  代码上传后，点击仓库顶部的 **Actions** 选项卡。
2.  在左侧选择你的 Workflow 名称（例如 **Rocom Merchant Bot**）。
3.  点击右侧的 **Run workflow** 按钮手动执行。
4.  点击进入正在运行的任务（显示为黄色圆圈或绿色对勾）。
5.  查看 **Run script** 这一步的日志：
    *   **如果成功**：你会看到“未发现目标物品”或者推送成功的提示。
    *   **隐私保护**：即使代码报错，GitHub 也会非常聪明地自动将日志里的 Key 替换为 `***`，确保万无一失。

---

### ⚠️ 隐私保护的最后提醒
*   **本地测试不要上传**：如果你在电脑本地测试时，不小心把 Key 直接写在了代码里（如 `self.api_key = "sk-ba04..."`），**绝对不要 `git commit` 推送到 GitHub**。
*   **清理历史记录**：如果不小心上传了，即使你后来删掉代码再重新上传，Key 仍然会留在 `Git History`（历史版本）里。这种情况下，你必须**立即重置（Reset）**你的 Wegame API Key，让旧的 Key 失效。
