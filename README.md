# 🔄 Auto-Update Plugin for Claude Code

Never manually update Claude Code again! This plugin automatically checks for and installs updates whenever you start a new session.

## Features

- **Automatic updates** - Updates install silently on session start
- **Cross-platform** - Works on macOS, Windows, and Linux
- **Smart detection** - Auto-detects your package manager (brew, winget, apk, or npm)
- **Non-blocking** - Updates run in the background, just restart to apply

## Supported Package Managers

| Platform | Package Manager | Install Command |
|----------|-----------------|-----------------|
| macOS    | Homebrew        | `brew install claude-code` |
| Windows  | winget          | `winget install Anthropic.ClaudeCode` |
| Linux    | apk (Alpine)    | `apk add claude-code` |
| All      | npm             | `npm install -g @anthropic-ai/claude-code` |

## Installation

### Step 1: Clone this repo

```bash
git clone https://github.com/yusufsaber/claude-code-auto-update.git
```

### Step 2: Add the hook to your Claude Code settings

Edit your settings file:
- **macOS/Linux:** `~/.claude/settings.json`
- **Windows:** `%USERPROFILE%\.claude\settings.json`

Add the `SessionStart` hook (merge with existing hooks if you have any):

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "node /path/to/claude-code-auto-update/hooks/auto-update.js"
          }
        ]
      }
    ]
  }
}
```

> **Note:** Replace `/path/to/claude-code-auto-update` with the actual path where you cloned this repo.

### Step 3 (Optional): Hide the built-in update notification

Since this plugin handles updates automatically, you can hide Claude's "Update available!" message.

**macOS/Linux** - Add to your `~/.zshrc` or `~/.bashrc`:
```bash
export DISABLE_AUTOUPDATER=1
```

**Windows** - Add to your PowerShell profile:
```powershell
$env:DISABLE_AUTOUPDATER = "1"
```

Then restart your terminal.

## How It Works

```
┌─────────────────────────────────────────────────────────┐
│                   Claude Code Starts                     │
└─────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│          SessionStart hook triggers plugin               │
└─────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│     Detect package manager (brew/winget/apk/npm)         │
└─────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│           Check if update is available                   │
└─────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                │                       │
                ▼                       ▼
        ┌───────────┐           ┌───────────────┐
        │ No update │           │ Update found! │
        │  Continue │           │ Run upgrade   │
        └───────────┘           └───────────────┘
                                        │
                                        ▼
                                ┌───────────────┐
                                │ Restart Claude│
                                │ to apply      │
                                └───────────────┘
```

## FAQ

### How often does it check for updates?
Every time you start a new Claude Code session.

### Will this slow down Claude Code startup?
The check is fast (< 1 second). Updates only download when available.

### What if the update fails?
The plugin fails silently and lets Claude Code continue normally. You can always update manually.

### Why am I still one version behind?
Package managers (brew, winget) may take up to 24 hours to publish new releases after they're available on npm. This is normal.

## License

MIT © Yusuf Saber
