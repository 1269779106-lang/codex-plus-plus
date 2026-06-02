# Codex++ 安装包与使用指南

> OpenAI Codex 桌面应用的**外部增强启动器**，不修改原始安装文件，通过 Chromium DevTools Protocol (CDP) 注入增强脚本。

## 📥 下载安装

从 [Releases 页面](https://github.com/1269779106-lang/codex-plus-plus/releases) 下载最新版：

- **Windows：** `CodexPlusPlus-*-windows-x64-setup.exe`

> 源码及更多版本请见上游仓库：[BigPizzaV3/CodexPlusPlus](https://github.com/BigPizzaV3/CodexPlusPlus)

## 🚀 快速开始

### 第一步：安装

双击运行安装包，一路下一步。

安装后桌面上出现**两个快捷方式**：

| 快捷方式 | 用途 |
|----------|------|
| **`Codex++`** | 🔇 静默启动，直接启动 Codex 并注入增强功能 |
| **`Codex++ 管理工具`** | ⚙️ 图形化控制面板，配置中转、管理脚本、日志、更新 |

### 第二步：选择模式

#### 🔵 模式 A：增强注入（不改中转，纯增强）

适合已经能用 Codex、只是想要更多功能的用户。

1. 打开 **`Codex++ 管理工具`**
2. 「增强功能」→ 确保**增强注入已开启**（默认开启）
3. 关闭管理工具，双击 **`Codex++`** 启动

启动后你将获得：

| 功能 | 说明 |
|------|------|
| 🔓 插件入口解锁 | API Key 登录模式也能使用插件 |
| 🗑️ 会话删除按钮 | 悬停出现，支持确认和撤销 |
| 📋 Markdown 导出 | 对话导出为 Markdown |
| ⏱️ Timeline 时间线 | 时间线方式浏览会话 |
| 📦 项目/会话移动 | 在项目之间移动会话 |
| 📜 用户脚本 | 注入自定义 JS 脚本 |

#### 🟢 模式 B：中转注入（对接国产模型 / 中转 API）

适合想用第三方 API 中转站的用户。

1. 打开 **`Codex++ 管理工具`**
2. 「中转注入」→ 添加中转配置：
   - **Base URL：** 中转站提供的地址（如 `https://example.com/v1`）
   - **Key：** 中转站提供的 API Key
3. 选择配置 → 点击 **「应用中转注入」**
4. 用 **`Codex++`** 启动

Codex++ 会自动修改 `~/.codex/config.toml`，写入 `CodexPlusPlus` provider。

> 如需切回官方模式：在管理工具中点击「清除 API 模式」即可。

### 第三步：验证

启动 Codex 后，看**顶部菜单栏**是否出现 `Codex++` 字样：

- ✅ 出现了 → 注入成功，功能已生效
- ❌ 没出现 → 确认是从 `Codex++` 入口启动的（不是原版 Codex），查看管理工具「诊断」页面

---

## 🔧 工作原理

```
Codex++ Launcher
    │
    ├─ 启动 Codex.app + CDP 调试参数 (--remote-debugging-port)
    │
    ├─ 通过 CDP 连接到 Codex 渲染进程
    │
    └─ 注入 renderer-inject.js → Codex 页面中出现 Codex++ 菜单
```

- **不修改** Codex App 原始文件（`app.asar` 不动）
- **不写入** DLL 到 Codex 安装目录
- **外部注入**，Codex 更新后可能需要等待 Codex++ 同步更新

---

## 📁 相关路径

| 用途 | 路径 |
|------|------|
| Codex 配置 | `~/.codex/config.toml` |
| Codex 登录状态 | `~/.codex/auth.json` |
| Codex 本地数据库 | `~/.codex/state_5.sqlite` |
| Codex++ 日志 | `~/.codex-session-delete/` |
| Provider 同步备份 | `~/.codex/backups_state/provider-sync` |

---

## ❓ 常见问题

### Codex++ 菜单没出现

1. 确认是从 `Codex++` 快捷方式启动，不是原版 Codex
2. 打开 `Codex++ 管理工具` → 「诊断」和「日志」页面查看注入状态

### 插件显示后端连不上

在 PowerShell 测试后端是否正常：

```powershell
Invoke-RestMethod -Method Post -Uri http://127.0.0.1:57321/backend/status -Body "{}" -ContentType "application/json"
```

如果接口正常但插件超时 → 重启 Codex++，或在管理工具日志中查看 `renderer.script_loaded`、`bridge.request`、`bridge.response`。

### macOS 提示已损坏

安装包未签名，执行以下命令解除隔离：

```bash
sudo xattr -rd com.apple.quarantine /Applications/Codex++\ 管理工具.app
sudo xattr -rd com.apple.quarantine /Applications/Codex++.app
```

---

## 📢 交流与更新

- **上游仓库：** [BigPizzaV3/CodexPlusPlus](https://github.com/BigPizzaV3/CodexPlusPlus)
- **QQ 交流群：** 1103050832
- **Telegram：** [@CodexPlusPlus](https://t.me/CodexPlusPlus)
- **Release 更新：** 管理工具「关于」页可检查更新

---

## ⚠️ 免责声明

Codex++ 是第三方增强工具，与 OpenAI 无关。使用前请确认你已拥有合法的 Codex 使用权。本项目仅提供安装包镜像和使用指南，版权归原作者 [BigPizzaV3](https://github.com/BigPizzaV3) 所有。
