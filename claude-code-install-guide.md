# Claude Code Installation Guide

> **English | [中文](claude-code-install-guide.zh-CN.md)**

> **MSEM Module — 20 March 2026**
> Estimated time: 10–15 minutes.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [macOS Installation](#macos-installation)
- [Windows Installation](#windows-installation)
- [Troubleshooting](#troubleshooting)
- [Quick Reference](#quick-reference)
- [Need Help?](#need-help)

---

## Prerequisites

Before you begin, make sure you have:

- A **Mac** (macOS 12 or later) or **Windows PC** (Windows 10 or later)
- A stable **internet connection**
- **4 GB+ RAM** on your machine
- At least **500 MB** of free disk space

> **What is a "terminal"?** A terminal (called **Terminal** on macOS, **PowerShell** on Windows) is a text-based interface where you type commands to interact with your computer. You'll copy and paste commands from this guide — no memorization needed.

Follow the section that matches your operating system:
- **Mac users** → [macOS Installation](#macos-installation)
- **Windows users** → [Windows Installation](#windows-installation)

---

## macOS Installation

### Step 1: Open Terminal

1. Press **Cmd + Space** to open Spotlight Search
2. Type **Terminal** and press **Enter**

You should see a window with a command prompt — something like:

```
yourname@MacBook ~ %
```

> **Tip:** You can also find Terminal in **Applications → Utilities → Terminal**.

### Step 2: Install Claude Code

Copy and paste the following command into Terminal, then press **Enter**:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

> **What does this do?** This command downloads the Claude Code installer from the internet (`curl`) and runs it (`bash`). It's a standard way to install developer tools on macOS.

Wait for the installation to complete. You should see output ending with a message like:

```
Claude Code has been installed successfully!
```

> **If the command hangs or fails:** Check that your internet connection is working. If you're on a corporate or university network, a firewall may be blocking the download — try switching to a personal hotspot temporarily.

### Step 3: Verify the Installation

Run the following command to confirm Claude Code was installed correctly:

```bash
claude --version
```

**Expected output:** A version number such as `2.x.x`.

If you see `claude: command not found`, try these steps in order:

1. **Close and reopen Terminal**, then run `claude --version` again.
2. If it still doesn't work, you need to add Claude Code to your PATH (the list of places your computer looks for programs). Run the following command to detect your shell and apply the fix:

   **If your prompt ends with `%`** (zsh — the default on modern Macs):

   ```bash
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
   ```

   **If your prompt ends with `$`** (bash — common on older Macs):

   ```bash
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bash_profile && source ~/.bash_profile
   ```

3. Run `claude --version` again to confirm it works.

### Step 4: Configure API Settings

Now you need to tell Claude Code which API server to connect to. You'll create a settings file in your **home directory** (the folder you start in when you open Terminal, usually `/Users/<your-username>/`).

Run these commands to create the settings folder and open the settings file in a text editor:

```bash
mkdir -p ~/.claude
nano ~/.claude/settings.json
```

> **What is `nano`?** It's a simple text editor that runs inside Terminal. You'll type or paste text, then use keyboard shortcuts to save and exit.

Paste the following into the editor, replacing `YOUR_API_KEY` with the API key provided during class:

```json
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
        "ANTHROPIC_BASE_URL": "https://coding-intl.dashscope.aliyuncs.com/apps/anthropic",
        "ANTHROPIC_MODEL": "qwen3.5-plus"
    }
}
```

Save and exit nano:
1. Press **Ctrl + O** (the letter O, not zero) to write the file
2. Press **Enter** to confirm the filename
3. Press **Ctrl + X** to exit nano

> **How do I verify?** Run `cat ~/.claude/settings.json` to print the file contents in Terminal. Check that your API key appears correctly and the JSON looks exactly like the example above.

### Step 5: Skip Onboarding

Create one more configuration file. This is a **different** file from the one above — notice it's called `.claude.json` and lives directly in your home directory, **not** inside the `.claude` folder.

```bash
nano ~/.claude.json
```

Paste the following:

```json
{
  "hasCompletedOnboarding": true
}
```

Save and exit (**Ctrl + O**, **Enter**, **Ctrl + X**).

> **Warning:** `hasCompletedOnboarding` must be at the top level of the JSON (not nested inside another object). If this is missing or incorrect, you'll see an **"Unable to connect to Anthropic services"** error when you start Claude Code.

**Close your Terminal window** and open a **new** one to apply all settings.

### Step 6: Test It

In your new Terminal window, run:

```bash
claude --version
```

If you see a version number, you're all set! You can now start Claude Code by simply typing `claude` and pressing Enter.

---

## Windows Installation

### Step 1: Install Git for Windows

Claude Code on Windows requires **Git for Windows** (a set of tools that Claude Code uses behind the scenes).

1. Open your web browser and go to [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win)
2. Download the installer — choose the **64-bit** version
3. Run the downloaded installer
4. **Use all the default settings** — just keep clicking **Next** until installation completes
5. Click **Finish**

> **Important:** Do not change the installation path unless you have a specific reason to. If you do install to a custom location, see [Windows: Custom Git Installation Path](#windows-custom-git-installation-path) in Troubleshooting.

**Verify Git installed correctly:** Open a new PowerShell window (see Step 2 below) and run:

```powershell
git --version
```

You should see something like `git version 2.x.x.windows.1`.

### Step 2: Open PowerShell

1. Press the **Windows key** on your keyboard
2. Type **PowerShell**
3. Right-click **Windows PowerShell** and select **Run as Administrator**

You should see a window with a prompt like:

```
PS C:\Users\YourName>
```

### Step 3: Install Claude Code

Copy and paste the following command into PowerShell, then press **Enter**:

```powershell
irm https://claude.ai/install.ps1 | iex
```

> **What does this do?** `irm` downloads the installer script from the internet, and `iex` runs it. This is the standard PowerShell equivalent of downloading and running an installer.

Wait for the installation to complete. You should see a success message.

> **If you get an "execution policy" error**, PowerShell is blocking scripts for security. Run this command first to allow it, then retry the install command above:
>
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
> ```
>
> This only changes the policy for your user account and is safe to do.

### Step 4: Verify the Installation

**Close PowerShell** and open a **new** PowerShell window, then run:

```powershell
claude --version
```

**Expected output:** A version number such as `2.x.x`.

> **If `claude` is not recognized:** Close all PowerShell windows, open a fresh one, and try again. Windows sometimes needs a new window to detect newly installed programs.

### Step 5: Configure API Settings

Now you need to tell Claude Code which API server to connect to. You'll create a settings file in your **home directory** (usually `C:\Users\<your-username>\`).

Open PowerShell and run:

```powershell
New-Item -ItemType Directory -Force -Path $HOME\.claude
notepad $HOME\.claude\settings.json
```

> **What does `$HOME` mean?** It's a shortcut that refers to your home folder (e.g., `C:\Users\YourName`). Using `$HOME` means the command works regardless of your actual username.

Notepad will open (it may ask if you want to create a new file — click **Yes**). Paste the following, replacing `YOUR_API_KEY` with the API key provided during class:

```json
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
        "ANTHROPIC_BASE_URL": "https://coding-intl.dashscope.aliyuncs.com/apps/anthropic",
        "ANTHROPIC_MODEL": "qwen3.5-plus"
    }
}
```

Save the file (**Ctrl + S**) and close Notepad.

> **How do I verify?** In PowerShell, run `Get-Content $HOME\.claude\settings.json` to print the file contents. Check that your API key appears and the JSON matches the example above.

### Step 6: Skip Onboarding

This is a **different** file from the one above — notice it's called `.claude.json` and lives directly in your home directory, **not** inside the `.claude` folder.

In PowerShell, run:

```powershell
notepad $HOME\.claude.json
```

Notepad will open (click **Yes** if asked to create the file). Paste the following:

```json
{
  "hasCompletedOnboarding": true
}
```

Save (**Ctrl + S**) and close Notepad.

> **Warning:** `hasCompletedOnboarding` must be at the top level of the JSON (not nested inside another object). If this is missing or incorrect, you'll see an **"Unable to connect to Anthropic services"** error when you start Claude Code.

**Close PowerShell** and open a **new** PowerShell window to apply all settings.

### Step 7: Test It

In your new PowerShell window, run:

```powershell
claude --version
```

If you see a version number, you're all set! You can now start Claude Code by simply typing `claude` and pressing Enter.

---

## Troubleshooting

### Common Issues

| Problem | Solution |
|---------|----------|
| `claude: command not found` (macOS) | Close and reopen Terminal. If it persists, see [Step 3 in macOS Installation](#step-3-verify-the-installation) for the PATH fix. |
| `claude` not recognized (Windows) | Close **all** PowerShell windows and open a fresh one. |
| "Git Bash not found" error (Windows) | Make sure you installed Git for Windows ([Step 1](#step-1-install-git-for-windows)). If you used a custom install path, see below. |
| "Unable to connect to Anthropic services" | Check that `.claude.json` contains `"hasCompletedOnboarding": true` at the top level (not nested). Also verify your API key in `.claude/settings.json`. |
| Installation script hangs | Check your internet connection. If on a corporate/university network, try a personal hotspot. |
| PowerShell execution policy error | Run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`, then retry. |
| `Permission denied` errors (macOS) | Do **not** use `sudo`. Instead, check that your user has write access to `~/.local/bin/`. |
| JSON parse error on startup | Open the relevant `.json` file and check for missing commas, extra commas, or mismatched braces. JSON requires exact syntax. |

### Windows: Custom Git Installation Path

If you installed Git for Windows to a non-default location and Claude Code can't find it, you need to add the path to your settings file.

Open `$HOME\.claude\settings.json` in Notepad and **merge** the Git path with your existing API settings:

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

> **Important:** Don't create a second `"env"` block — add the `CLAUDE_CODE_GIT_BASH_PATH` line inside the existing one, and make sure to add a comma after the line above it.

---

## Quick Reference

Once installed, here are the commands you'll use most:

| Command | What It Does |
|---------|-------------|
| `claude` | Start an interactive Claude Code session |
| `claude --version` | Check your installed version |
| `claude doctor` | Run a health check on your installation |
| `claude update` | Manually update to the latest version |

> **Auto-updates:** Claude Code automatically updates itself in the background. You don't need to do anything to stay current.

---

## Need Help?

- **In class:** Raise your hand — we'll sort it out together
- **Official docs:** [code.claude.com/docs](https://code.claude.com/docs)
