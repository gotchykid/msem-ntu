# Claude Code 安装指南

> **[English](claude-code-install-guide.md) | 中文**

> **MSEM 模块 — 2026年3月20日**
> 预计用时：10–15 分钟。

---

## 目录

- [前置条件](#前置条件)
- [macOS 安装](#macos-安装)
- [Windows 安装](#windows-安装)
- [故障排除](#故障排除)
- [快速参考](#快速参考)
- [需要帮助？](#需要帮助)

---

## 前置条件

开始之前，请确保你具备以下条件：

- 一台 **Mac**（macOS 12 或更高版本）或 **Windows 电脑**（Windows 10 或更高版本）
- 稳定的**网络连接**
- **4 GB 以上内存**
- 至少 **500 MB** 的可用磁盘空间

> **什么是"终端"？** 终端（macOS 上叫 **Terminal**，Windows 上叫 **PowerShell**）是一个基于文本的界面，你可以在其中输入命令来与计算机交互。你只需从本指南中复制粘贴命令 — 无需记忆。

请按照你的操作系统对应的部分操作：
- **Mac 用户** → [macOS 安装](#macos-安装)
- **Windows 用户** → [Windows 安装](#windows-安装)

---

## macOS 安装

### 第 1 步：打开终端

1. 按 **Cmd + Space** 打开聚焦搜索
2. 输入 **Terminal** 然后按 **Enter**

你应该看到一个带有命令提示符的窗口，类似于：

```
yourname@MacBook ~ %
```

> **提示：** 你也可以在 **应用程序 → 实用工具 → 终端** 中找到 Terminal。

### 第 2 步：安装 Claude Code

将以下命令复制粘贴到终端中，然后按 **Enter**：

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

> **这个命令做什么？** 它从互联网下载 Claude Code 安装程序（`curl`）并运行它（`bash`）。这是 macOS 上安装开发者工具的标准方式。

等待安装完成。你应该看到以类似以下消息结尾的输出：

```
Claude Code has been installed successfully!
```

> **如果命令卡住或失败：** 检查你的网络连接是否正常。如果你在企业或大学网络上，防火墙可能会阻止下载 — 请尝试临时切换到个人热点。

### 第 3 步：验证安装

运行以下命令确认 Claude Code 已正确安装：

```bash
claude --version
```

**预期输出：** 版本号，如 `2.x.x`。

如果你看到 `claude: command not found`，请按顺序尝试以下步骤：

1. **关闭并重新打开终端**，然后再次运行 `claude --version`。
2. 如果仍然不行，你需要将 Claude Code 添加到 PATH（计算机查找程序的位置列表）。根据你的 shell 类型运行以下命令：

   **如果你的提示符以 `%` 结尾**（zsh — 现代 Mac 的默认设置）：

   ```bash
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
   ```

   **如果你的提示符以 `$` 结尾**（bash — 较老的 Mac 常见）：

   ```bash
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bash_profile && source ~/.bash_profile
   ```

3. 再次运行 `claude --version` 确认可以正常使用。

### 第 4 步：配置 API 设置

现在你需要告诉 Claude Code 连接哪个 API 服务器。你将在**主目录**（打开终端时的默认文件夹，通常是 `/Users/<你的用户名>/`）中创建一个设置文件。

运行以下命令创建设置文件夹并在文本编辑器中打开设置文件：

```bash
mkdir -p ~/.claude
nano ~/.claude/settings.json
```

> **什么是 `nano`？** 它是一个在终端内运行的简单文本编辑器。你可以在其中输入或粘贴文本，然后使用键盘快捷键保存和退出。

将以下内容粘贴到编辑器中，将 `YOUR_API_KEY` 替换为课堂上提供的 API 密钥：

```json
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
        "ANTHROPIC_BASE_URL": "https://coding-intl.dashscope.aliyuncs.com/apps/anthropic",
        "ANTHROPIC_MODEL": "qwen3.5-plus"
    }
}
```

保存并退出 nano：
1. 按 **Ctrl + O**（字母 O，不是数字零）写入文件
2. 按 **Enter** 确认文件名
3. 按 **Ctrl + X** 退出 nano

> **如何验证？** 运行 `cat ~/.claude/settings.json` 在终端中打印文件内容。检查你的 API 密钥是否正确显示，JSON 格式是否与上面的示例完全一致。

### 第 5 步：跳过引导流程

创建另一个配置文件。这是一个与上面**不同**的文件 — 注意它叫 `.claude.json`，位于你的主目录中，**不是**在 `.claude` 文件夹内。

```bash
nano ~/.claude.json
```

粘贴以下内容：

```json
{
  "hasCompletedOnboarding": true
}
```

保存并退出（**Ctrl + O**，**Enter**，**Ctrl + X**）。

> **警告：** `hasCompletedOnboarding` 必须位于 JSON 的顶层（不要嵌套在其他对象内）。如果缺少或不正确，启动 Claude Code 时你会看到 **"Unable to connect to Anthropic services"** 错误。

**关闭终端窗口**并打开一个**新的**终端窗口以应用所有设置。

### 第 6 步：测试

在新的终端窗口中运行：

```bash
claude --version
```

如果你看到版本号，说明一切就绪！现在你可以输入 `claude` 并按 Enter 来启动 Claude Code。

---

## Windows 安装

### 第 1 步：安装 Git for Windows

Claude Code 在 Windows 上需要 **Git for Windows**（Claude Code 在后台使用的一组工具）。

1. 打开浏览器，前往 [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win)
2. 下载安装程序 — 选择 **64 位**版本
3. 运行下载的安装程序
4. **使用所有默认设置** — 一直点击 **Next** 直到安装完成
5. 点击 **Finish**

> **重要提示：** 除非有特殊原因，否则不要更改安装路径。如果你确实安装到了自定义位置，请参阅故障排除中的 [Windows：自定义 Git 安装路径](#windows自定义-git-安装路径)。

**验证 Git 是否正确安装：** 打开一个新的 PowerShell 窗口（参见下面的第 2 步）并运行：

```powershell
git --version
```

你应该看到类似 `git version 2.x.x.windows.1` 的输出。

### 第 2 步：打开 PowerShell

1. 按键盘上的 **Windows 键**
2. 输入 **PowerShell**
3. 右键点击 **Windows PowerShell**，选择 **以管理员身份运行**

你应该看到一个类似这样的提示窗口：

```
PS C:\Users\YourName>
```

### 第 3 步：安装 Claude Code

将以下命令复制粘贴到 PowerShell 中，然后按 **Enter**：

```powershell
irm https://claude.ai/install.ps1 | iex
```

> **这个命令做什么？** `irm` 从互联网下载安装脚本，`iex` 运行它。这是 PowerShell 中下载并运行安装程序的标准方式。

等待安装完成。你应该会看到一条成功消息。

> **如果遇到"执行策略"错误**，PowerShell 出于安全考虑正在阻止脚本。先运行以下命令允许脚本执行，然后重试上面的安装命令：
>
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
> ```
>
> 这只会更改你的用户账户的策略，可以安全执行。

### 第 4 步：验证安装

**关闭 PowerShell** 并打开一个**新的** PowerShell 窗口，然后运行：

```powershell
claude --version
```

**预期输出：** 版本号，如 `2.x.x`。

> **如果 `claude` 无法识别：** 关闭所有 PowerShell 窗口，打开一个新窗口，然后再试一次。Windows 有时需要新窗口才能检测到新安装的程序。

### 第 5 步：配置 API 设置

现在你需要告诉 Claude Code 连接哪个 API 服务器。你将在**主目录**（通常是 `C:\Users\<你的用户名>\`）中创建一个设置文件。

打开 PowerShell 并运行：

```powershell
New-Item -ItemType Directory -Force -Path $HOME\.claude
notepad $HOME\.claude\settings.json
```

> **`$HOME` 是什么意思？** 它是指向你的主文件夹（例如 `C:\Users\YourName`）的快捷方式。使用 `$HOME` 意味着无论你的实际用户名是什么，命令都能正常工作。

记事本将打开（它可能会询问是否要创建新文件 — 点击**是**）。粘贴以下内容，将 `YOUR_API_KEY` 替换为课堂上提供的 API 密钥：

```json
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
        "ANTHROPIC_BASE_URL": "https://coding-intl.dashscope.aliyuncs.com/apps/anthropic",
        "ANTHROPIC_MODEL": "qwen3.5-plus"
    }
}
```

保存文件（**Ctrl + S**）并关闭记事本。

> **如何验证？** 在 PowerShell 中运行 `Get-Content $HOME\.claude\settings.json` 打印文件内容。检查你的 API 密钥是否正确显示，JSON 格式是否与上面的示例一致。

### 第 6 步：跳过引导流程

这是一个与上面**不同**的文件 — 注意它叫 `.claude.json`，位于你的主目录中，**不是**在 `.claude` 文件夹内。

在 PowerShell 中运行：

```powershell
notepad $HOME\.claude.json
```

记事本将打开（如果询问是否创建文件，点击**是**）。粘贴以下内容：

```json
{
  "hasCompletedOnboarding": true
}
```

保存（**Ctrl + S**）并关闭记事本。

> **警告：** `hasCompletedOnboarding` 必须位于 JSON 的顶层（不要嵌套在其他对象内）。如果缺少或不正确，启动 Claude Code 时你会看到 **"Unable to connect to Anthropic services"** 错误。

**关闭 PowerShell** 并打开一个**新的** PowerShell 窗口以应用所有设置。

### 第 7 步：测试

在新的 PowerShell 窗口中运行：

```powershell
claude --version
```

如果你看到版本号，说明一切就绪！现在你可以输入 `claude` 并按 Enter 来启动 Claude Code。

---

## 故障排除

### 常见问题

| 问题 | 解决方案 |
|------|----------|
| `claude: command not found`（macOS） | 关闭并重新打开终端。如果问题持续，请参阅 [macOS 安装第 3 步](#第-3-步验证安装)中的 PATH 修复方法。 |
| `claude` 无法识别（Windows） | 关闭**所有** PowerShell 窗口，然后打开一个新窗口。 |
| "Git Bash not found" 错误（Windows） | 确保你已安装 Git for Windows（[第 1 步](#第-1-步安装-git-for-windows)）。如果安装到了自定义路径，请参阅下方说明。 |
| "Unable to connect to Anthropic services" | 检查 `.claude.json` 是否在顶层包含 `"hasCompletedOnboarding": true`（不要嵌套）。同时验证 `.claude/settings.json` 中的 API 密钥。 |
| 安装脚本卡住 | 检查网络连接。如果在企业/大学网络上，请尝试个人热点。 |
| PowerShell 执行策略错误 | 运行 `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`，然后重试。 |
| `Permission denied` 错误（macOS） | **不要**使用 `sudo`。请检查你的用户是否有 `~/.local/bin/` 的写入权限。 |
| 启动时 JSON 解析错误 | 打开相关 `.json` 文件，检查是否有缺少的逗号、多余的逗号或不匹配的括号。JSON 要求语法完全正确。 |

### Windows：自定义 Git 安装路径

如果你将 Git for Windows 安装到非默认位置，且 Claude Code 无法找到它，你需要将路径添加到设置文件中。

在记事本中打开 `$HOME\.claude\settings.json`，将 Git 路径与现有 API 设置**合并**：

```json
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
        "ANTHROPIC_BASE_URL": "https://coding-intl.dashscope.aliyuncs.com/apps/anthropic",
        "ANTHROPIC_MODEL": "qwen3.5-plus",
        "CLAUDE_CODE_GIT_BASH_PATH": "C:\\Your\\Custom\\Path\\Git\\bin\\bash.exe"
    }
}
```

> **重要提示：** 不要创建第二个 `"env"` 块 — 将 `CLAUDE_CODE_GIT_BASH_PATH` 行添加到现有的块内，并确保在它上面的行末尾加上逗号。

---

## 快速参考

安装完成后，以下是你最常用的命令：

| 命令 | 功能 |
|------|------|
| `claude` | 启动 Claude Code 交互式会话 |
| `claude --version` | 检查已安装的版本 |
| `claude doctor` | 运行安装健康检查 |
| `claude update` | 手动更新到最新版本 |

> **自动更新：** Claude Code 会在后台自动更新。你无需执行任何操作即可保持最新版本。

---

## 需要帮助？

- **在课堂上：** 举手 — 我们会一起解决
- **官方文档：** [code.claude.com/docs](https://code.claude.com/docs)
