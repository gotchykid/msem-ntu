# Claude Code Installation Guide

> **MSEM Module — 20 March 2026**
> Follow the section for your operating system (macOS or Windows).
> Estimated time: 10–15 minutes.

---

## What You Need Before Starting

- A **Mac** or **Windows PC**
- A stable **internet connection**
- **4 GB+ RAM** on your machine

---

## macOS Installation

### Step 1: Open Terminal

1. Press **Cmd + Space** to open Spotlight Search
2. Type **Terminal** and press **Enter**

You should see a window with a command prompt — something like:

```
yourname@MacBook ~ %
```

### Step 2: Install Claude Code

Copy and paste the following command into Terminal, then press **Enter**:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Wait for the installation to complete. You should see a success message.

### Step 3: Add Claude Code to Your PATH

The installer usually does this automatically. If you get `claude: command not found` after installing, you need to add it to your PATH manually.

1. Run the following command in Terminal:

   ```bash
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
   ```

2. Reload your profile:

   ```bash
   source ~/.zshrc
   ```

### Step 4: Verify the Installation

Run:

```bash
claude --version
```

You should see a version number printed (e.g., `2.x.x`). If you still see an error, close and reopen Terminal, then try again.

### Step 5: Configure API Settings

Create the `.claude` folder in your home directory (if it doesn't already exist) and open the settings file:

```bash
mkdir -p ~/.claude
nano ~/.claude/settings.json
```

Paste the following into the file, replacing `YOUR_API_KEY` with the API key we will provide during class:

```json
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
        "ANTHROPIC_BASE_URL": "https://coding-intl.dashscope.aliyuncs.com/apps/anthropic",
        "ANTHROPIC_MODEL": "qwen3.5-plus"
    }
}
```

Save the file (**Ctrl + O**, then **Enter**, then **Ctrl + X** to exit nano) and close your terminal.

### Step 6: Skip Onboarding

Create or edit the file `~/.claude.json` (note: this is in your home directory, **not** inside the `.claude` folder):

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

> **Important:** `hasCompletedOnboarding` must be a top-level field (not nested inside another object). This prevents the "Unable to connect to Anthropic services" error on startup.

Open a **new** Terminal window to apply all settings.

---

## Windows Installation

### Step 1: Install Git for Windows

Claude Code on Windows requires **Git for Windows**. Install it first:

1. Go to [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win)
2. Download the installer (the **64-bit** version)
3. Run the installer
4. **Use all the default settings** — just keep clicking **Next** until installation completes
5. Click **Finish**

### Step 2: Open PowerShell

1. Press the **Windows key** on your keyboard
2. Type **PowerShell**
3. Click **Windows PowerShell** (you do **not** need to run as Administrator)

You should see a window with a prompt like:

```
PS C:\Users\YourName>
```

### Step 3: Install Claude Code

Copy and paste the following command into PowerShell, then press **Enter**:

```powershell
irm https://claude.ai/install.ps1 | iex
```

Wait for the installation to complete. You should see a success message.

> **Note:** If you get an error about execution policies, run this command first, then retry:
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
> ```

### Step 4: Verify the Installation

Run:

```powershell
claude --version
```

You should see a version number printed (e.g., `2.x.x`).

### Step 5: Configure API Settings

Open **PowerShell** (see Step 2 if you need a reminder how). Create the `.claude` folder and open the settings file:

```powershell
mkdir -Force $HOME\.claude
notepad $HOME\.claude\settings.json
```

Paste the following into Notepad, replacing `YOUR_API_KEY` with the API key we will provide during class:

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

### Step 6: Skip Onboarding

Open the file `C:\Users\YourUsername\.claude.json` (note: this is in your home directory, **not** inside the `.claude` folder):

```powershell
notepad $HOME\.claude.json
```

Paste the following:

```json
{
  "hasCompletedOnboarding": true
}
```

Save (**Ctrl + S**) and close Notepad.

> **Important:** `hasCompletedOnboarding` must be a top-level field (not nested inside another object). This prevents the "Unable to connect to Anthropic services" error on startup.

**Close PowerShell** and open a **new** PowerShell window to apply all settings.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `claude: command not found` (macOS) | Close and reopen Terminal, then try again |
| `claude` not recognized (Windows) | Close and reopen PowerShell, then try again |
| "Git Bash not found" error (Windows) | Make sure you installed Git for Windows (Step 1). If you installed it to a custom location, see note below |
| Installation script hangs | Check your internet connection and try again |
| PowerShell execution policy error | Run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` then retry |

### Windows: Custom Git Installation Path

If you installed Git for Windows to a non-default location and Claude Code can't find it, create or edit the file `%USERPROFILE%\.claude\settings.json` and add:

```json
{
  "env": {
    "CLAUDE_CODE_GIT_BASH_PATH": "C:\\Your\\Custom\\Path\\Git\\bin\\bash.exe"
  }
}
```

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

- Official docs: [code.claude.com/docs](https://code.claude.com/docs)
- Raise your hand in class — we'll sort it out together
