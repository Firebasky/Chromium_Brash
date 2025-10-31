# Brash 漏洞概念验证 (PoC)

## 项目简介

这是一个用于演示 **Brash 漏洞**的概念验证（Proof of Concept）项目。该漏洞会影响基于 Chromium 的浏览器，通过快速更新 `document.title` 属性可能导致浏览器崩溃。

## ⚠️ 警告

**此 PoC 仅用于安全研究和测试目的。请务必在受控环境中使用！**

- ⚠️ 此 PoC 会尝试使 Chromium 浏览器崩溃
- ⚠️ 请确保已保存所有重要工作
- ⚠️ 仅在测试环境中使用
- ⚠️ 不要在生产环境或重要系统上运行

## 使用方法

1. 在浏览器中打开 `brash_poc.html` 文件
2. 阅读警告信息并确认在安全环境中
3. 点击"开始攻击"按钮
4. 观察浏览器行为（可能会崩溃）
5. 如需停止，点击"停止攻击"按钮（如果浏览器仍然响应）

## 技术细节

### 攻击机制

该 PoC 采用三个阶段的方式实施攻击：

#### 阶段 1: Hash 生成/准备阶段
- 生成 100 个 512 字符的十六进制字符串作为种子
- 这些种子用于后续的 title 更新操作

#### 阶段 2: 突发注入阶段
- 每次突发执行 8000 次 `document.title` 更新
- 每次突发中，对每个种子执行 3 次连续的 title 更新
- 每次更新使用种子的不同子串

#### 阶段 3: UI 线程饱和阶段
- 使用 `requestAnimationFrame` 和 `setInterval` 组合来最大化 CPU 使用
- 同时启动多个更新循环来增加压力
- 以 1ms 的间隔持续执行突发更新

### 攻击参数

- **Hash 种子数量**: 100 个（每个 512 字符的十六进制字符串）
- **突发大小**: 8000 次更新/突发
- **更新间隔**: 1ms
- **预期速率**: 约 2400 万次 `document.title` 更新/秒

## 受影响范围

### 受影响的浏览器

- ✅ Google Chrome (所有基于 Chromium 的版本)
- ✅ Microsoft Edge (Chromium 版本)
- ✅ Brave Browser
- ✅ Opera
- ✅ 其他基于 Chromium 的浏览器

### 不受影响的浏览器

- ❌ Mozilla Firefox
- ❌ Safari
- ❌ 其他非 Chromium 内核的浏览器

## 文件说明

- `index.html` - GitHub Pages 入口文件（与 brash_poc.html 内容相同）
- `brash_poc.html` - 主要的 PoC HTML 文件，包含完整的攻击代码和 UI 界面
- `README.md` - 本说明文件
- `.github/workflows/pages.yml` - GitHub Actions 自动部署工作流

## GitHub Pages 部署

### 在线访问

项目已配置 GitHub Pages，可以通过以下链接访问：

**Demo 链接**: https://ygcaicn.github.io/Brash/

### 启用步骤

1. **推送代码到 GitHub**（如果还没推送）:
   ```bash
   git add .
   git commit -m "配置 GitHub Pages"
   git push origin main
   ```

2. **在 GitHub 仓库中启用 Pages**:
   - 进入仓库页面，点击 **Settings**
   - 在左侧菜单中找到 **Pages**
   - 在 **Source** 部分，选择 **GitHub Actions**
   - 保存设置

3. **自动部署**:
   - GitHub Actions 会在每次推送到 `main` 分支时自动部署
   - 首次部署可能需要几分钟时间
   - 部署完成后即可通过上述链接访问

### 手动触发部署

如果需要手动触发部署：
- 进入仓库的 **Actions** 标签页
- 选择 **Deploy to GitHub Pages** 工作流
- 点击 **Run workflow** 按钮

## 研究目的

本项目旨在：

1. 帮助安全研究人员理解该漏洞的机制
2. 为浏览器厂商提供漏洞报告和修复参考
3. 提高对浏览器安全性的认识

## 免责声明

本 PoC 仅用于合法的安全研究和教育目的。使用者需自行承担使用风险，作者不对使用本 PoC 造成的任何损害负责。

请负责任地使用，仅在自己的系统或获得明确授权的测试环境中使用。

## 注意事项

- 运行此 PoC 前请确保已保存所有工作
- 建议使用虚拟机或隔离的测试环境
- 如果浏览器崩溃，重启浏览器即可恢复正常
- 该 PoC 不会造成永久性损害

