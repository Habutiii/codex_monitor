# Codex Session Monitor

A compact, read-only Windows and macOS desktop monitor for active Codex sessions created by the VS Code extension.

## Run

The release is a single file: [`release/CodexMonitor.pyw`](release/CodexMonitor.pyw). It needs Python 3.10+ with Tkinter; no third-party packages or administrator rights are required.

### Windows 11

Double-click `CodexMonitor.pyw` if Python is associated with `.pyw` files, or run:

```powershell
pyw .\release\CodexMonitor.pyw
```

### macOS

Open Terminal in the downloaded folder and run:

```bash
python3 release/CodexMonitor.pyw
```

macOS does not reliably associate `.pyw` files with Python for double-click launching. The command above runs without admin rights. Gatekeeper or corporate device-management policy can still show or enforce a security warning for downloaded files; the app cannot override those operating-system policies.

The application's Tkinter UI, Codex JSONL parser, settings handling, animated status icons, and transparent cat image resources are all embedded in the one `.pyw` file. Always on Top and the 10–100% transparency control use Tk's cross-platform window attributes on both Windows and macOS.

## What it monitors

The monitor reads only local rollout logs under:

```text
%USERPROFILE%\.codex\sessions\**\rollout-*.jsonl
```

It includes rollout files whose `session_meta.payload.source` is `vscode`, names a session from its workspace/cwd basename, and displays one most-relevant row per workspace.

| Status | Persisted Codex event |
| --- | --- |
| Working | `task_started` |
| Waiting for user | `request_user_input`, approval, elicitation, or permission request |
| Done | `task_complete` |
| Failed | `error` or `turn_aborted` |
| Idle | An unfinished session with no activity for 15 minutes |

Finished sessions remain visible for five minutes. The initial read uses metadata plus a bounded file tail; after that, the app reads only appended JSONL bytes. Session status is refreshed approximately every second.

## Privacy and settings

The app never modifies Codex or VS Code files, makes no network requests, starts no web server, and does not require the Codex CLI.

Always-on-top, transparency, and window position are persisted separately in:

```text
%LOCALAPPDATA%\CodexSessionMonitor\settings.json
```
