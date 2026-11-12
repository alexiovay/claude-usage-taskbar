# Claude Usage Widget for the Windows Taskbar

**See your Claude usage limit live in the Windows 11 taskbar.** The 5-hour limit and the weekly limit of your Claude plan, always visible, for Claude Code and every Claude plan. No browser tab, no `/usage` command.

![Claude 5-hour and weekly usage limit in the Windows 11 taskbar](docs/screenshot-5h-and-weekly.png)

- Native Windows app in Rust, about 400 KB, no runtime, no browser, no Electron
- Live numbers: percent used or remaining, reset time, plan name in the tooltip
- Adapts to your plan: shows the limits Claude reports and hides the ones your plan does not have
- Several Claude Code accounts side by side
- No telemetry, no auto-update, talks only to Anthropic

## Adapts to every Claude plan

Claude plans differ: some have a 5-hour limit plus a weekly limit, others only a weekly limit. The widget does not hard-code plans. It shows exactly the limits your account reports and nothing else:

![Claude weekly usage limit only in the Windows taskbar, for plans without a 5-hour limit](docs/screenshot-weekly-only.png)

| Your plan reports | Widget shows |
|---|---|
| 5-hour and weekly limit | `5h 38%` and `W 32%` |
| Weekly limit only | `W 32%` |
| 5-hour limit only | `5h 38%` |

The plan name (for example "Max 20x") is read from the Claude Code login and shown in the tooltip.

## Download

Grab `ClaudeUsageTaskbar.exe` from the [latest release](https://github.com/alexiovay/claude-usage-taskbar/releases/latest). It is built by GitHub Actions from this source; the SHA-256 sits next to it. Or build it yourself (see below).

Requirement: you are signed in to Claude Code (`claude`). The widget reads the login from `%USERPROFILE%\.claude\.credentials.json` (or `CLAUDE_CONFIG_DIR`) and refreshes the token the same way Claude Code does.

Put the `.exe` in a permanent folder, run it, then right-click the widget and enable **Start with Windows**.

## Features

- Sits in the taskbar left of the tray, dark and light theme, per-monitor DPI
- Hover tooltip: used and remaining percent, reset countdown and time, plan, last update
- Left click opens the Claude usage page, right click opens the menu
- Refresh interval from 30 seconds to 30 minutes, backs off on rate limits
- Updates right after a limit resets and when the PC wakes from sleep
- Font size, labels and icon are adjustable
- Optional [claude-swap](https://github.com/realiti4/claude-swap) support: switch to the next saved account from the menu

## FAQ

**How do I see my Claude usage limit on Windows without opening the browser?**
Run this widget. It shows the same numbers as the Claude usage page, in the taskbar.

**Does it work with Claude Max, Pro and Team?**
It reads whatever limits your account reports and shows those, so it is not tied to one plan.

**My plan has no 5-hour limit. What does the widget show?**
Only the weekly number. The 5-hour part is hidden automatically.

**Is it safe?**
The code is small and open. It sends your Claude Code token only to `api.anthropic.com` and `platform.claude.com` to read usage and refresh the login. There is no telemetry and no self-update, so nothing replaces the program behind your back.

**Does it work with the Claude desktop or web app?**
It reads the login of Claude Code, so you need Claude Code signed in on that PC.

## Build

```
rustup default stable-x86_64-pc-windows-msvc
cargo build --release
```

Needs Rust and the Visual Studio Build Tools (C++ workload). Result: `target\release\claude-usage-widget.exe`.

## Privacy

- Reads the Claude Code login file; the access token goes only to Anthropic, to fetch usage and to refresh the token.
- Settings live in `HKCU\Software\ClaudeUsageWidget`.
- Network: `api.anthropic.com` and `platform.claude.com`, nothing else.

## License

MIT, see `LICENSE`.

"Claude" and the Claude symbol are trademarks of Anthropic. This is an unofficial tool and not affiliated with Anthropic.
