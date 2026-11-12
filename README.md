# Claude Usage Widget for the Windows taskbar

Your Claude usage, always visible in the Windows 11 taskbar. No browser tab, no `/usage` command.

![5-hour and weekly limit](docs/screenshot-5h-and-weekly.png)

## Works with every plan

The widget shows exactly the limits your plan reports and nothing else. If a plan has no 5-hour limit, the 5-hour part disappears and only the weekly number stays:

![Weekly limit only](docs/screenshot-weekly-only.png)

| Your plan reports | Widget shows |
|---|---|
| 5-hour and weekly limit (typical for Pro, Max, Team) | `5h 38%` and `W 32%` |
| Weekly limit only (plans without a 5-hour limit) | `W 32%` |
| 5-hour limit only | `5h 38%` |

The plan name (for example "Max 20x") is read from the Claude Code login and shown in the tooltip. Claude's plans and limits change over time, so nothing here is hard-coded per plan: the widget follows what Anthropic's usage endpoint returns for your account.

Tested so far on one Max 20x account. Other plans should work by the same logic, but they have not been tried yet.

## Features

- Always on top of the taskbar, left of the tray, dark and light theme.
- Hover tooltip: used and remaining percent, reset time, plan, last update.
- Several Claude Code accounts side by side.
- Adjustable refresh interval, font size, labels, icon.
- Starts with Windows (menu toggle).
- No telemetry. Talks only to `api.anthropic.com` and `platform.claude.com`.
- No auto-update. You build it yourself, nothing replaces the program behind your back.

## Requirements

You must be signed in to Claude Code (`claude`). The widget reads the login from `%USERPROFILE%\.claude\.credentials.json` (or `CLAUDE_CONFIG_DIR`) and refreshes the token the same way Claude Code does.

## Build

```
rustup default stable-x86_64-pc-windows-msvc
cargo build --release
```

Needs Rust and the Visual Studio Build Tools (C++ workload). Result: `target\release\claude-usage-widget.exe`. Put a copy somewhere permanent and run it; the menu has "Start with Windows".

## Privacy

- Reads the Claude Code login file and sends the access token only to Anthropic, to fetch usage and to refresh the token.
- Stores its settings in `HKCU\Software\ClaudeUsageWidget`.
- Optional: if [claude-swap](https://github.com/realiti4/claude-swap) is installed, the menu can run `cswap switch`.

## Credits

Based on [AbdoSarmini/claude-usage-widget-for-windows-taskbar](https://github.com/AbdoSarmini/claude-usage-widget-for-windows-taskbar) v1.1.0 (MIT, see `LICENSE`). Changes: self-update code removed, official Claude symbol, plan detection, widget adapts to plans without a 5-hour or weekly limit.

"Claude" and the Claude symbol are trademarks of Anthropic. This is an unofficial tool and not affiliated with Anthropic.
