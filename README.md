# DroidRun MCP Server

A text-based MCP (Model Context Protocol) server for controlling Android devices from Claude Code. Pure accessibility tree automation - no screenshots needed.

## Features

- **Text-based UI interaction** - Read and interact with Android UI elements using accessibility tree
- **No screenshots required** - Faster and more efficient than vision-based approaches
- **Simple indexed tapping** - Elements are numbered for easy interaction
- **Full device control** - Navigate, type, swipe, and launch apps

## Prerequisites

1. **Android device** connected via ADB (USB debugging enabled)
2. **DroidRun Portal app** installed on the device with accessibility service enabled
3. **Python 3.10+**
4. **Claude Code CLI** installed

### Install DroidRun Portal

```bash
pip install droidrun
droidrun setup
```

This will install the DroidRun Portal app on your connected Android device and guide you through enabling the accessibility service.

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/chukfinley/droidrun-mcp-server.git
cd droidrun-mcp-server
```

### 2. Install dependencies

```bash
pip install droidrun mcp
```

### 3. Configure Claude Code

Add to your `~/.claude.json` (or project-specific `.claude.json`):

```json
{
  "mcpServers": {
    "droidrun": {
      "type": "stdio",
      "command": "python3",
      "args": ["/path/to/droidrun-mcp-server/server.py"],
      "env": {}
    }
  }
}
```

### 4. Restart Claude Code

```bash
claude
```

## Available Tools

| Tool | Description |
|------|-------------|
| `device_info()` | Get device model, Android version, and serial |
| `ui()` | Get current screen UI elements as indexed list |
| `tap(index)` | Tap element by index number from `ui()` output |
| `tap_xy(x, y)` | Tap screen at specific coordinates |
| `swipe(direction)` | Swipe up/down/left/right |
| `text(content)` | Input text (optionally tap element first) |
| `back()` | Press Android back button |
| `home()` | Press Android home button |
| `enter()` | Press Enter key |
| `app(package)` | Open app by package name |
| `apps()` | List installed apps (non-system) |

## Usage Example

Once configured, you can ask Claude Code to control your Android device:

```
> Open the Settings app and go to About Phone

Claude will:
1. Call app("com.android.settings")
2. Call ui() to see the screen
3. Call tap() on the appropriate elements
4. Navigate to About Phone
```

Example `ui()` output:
```
APP: Settings (com.android.settings)
KEYBOARD: hidden
FOCUS: none

1. TextView: "Network & internet" - (0,200,1080,300)
2. TextView: "Connected devices" - (0,300,1080,400)
3. TextView: "Apps" - (0,400,1080,500)
4. TextView: "About phone" - (0,900,1080,1000)
```

Then tap element 4: `tap(4)`

## How It Works

This MCP server wraps the [DroidRun](https://github.com/droidrun/droidrun) library to provide Android device control through Claude Code. It uses the Android accessibility tree (not screenshots) for UI detection, making it fast and reliable.

The DroidRun Portal app runs on your Android device and provides:
- Accessibility service for UI tree extraction
- Numbered overlay for visual feedback (optional)
- Input method for text entry

## Troubleshooting

### "No Android device connected"
- Ensure USB debugging is enabled on your device
- Run `adb devices` to verify connection
- Try `adb kill-server && adb start-server`

### "Portal is not installed"
- Run `droidrun setup` to install the Portal app

### "Accessibility service not enabled"
- Go to Settings > Accessibility > DroidRun Portal
- Enable the service

### MCP server not showing in Claude Code
- Check your `~/.claude.json` configuration
- Ensure the path to `server.py` is absolute and correct
- Restart Claude Code after config changes

## License

MIT

## Credits

- [DroidRun](https://github.com/droidrun/droidrun) - Android automation framework
- [MCP](https://modelcontextprotocol.io/) - Model Context Protocol by Anthropic
