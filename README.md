# CodexStatusbar Windows

English | [简体中文](README.zh-CN.md)

A Windows status overlay for the official Codex Desktop app. This is an independent derivative of [dongyue-19/codex-statusbar](https://github.com/dongyue-19/codex-statusbar), based on its v1.0.0-rc3 release.

## Changes in this derivative

- Prefer a consistent modern usage source when modern and legacy Codex usage records coexist.
- Deduplicate modern response usage by response ID and avoid reporting throughput when data is ambiguous.
- Disable start-with-Windows by default. An existing startup entry may be removed during startup reconciliation when startup is disabled.
- Add regression coverage for mixed sources, repeated responses and invalid or incomplete usage.

The displayed token total is cumulative input plus output for the conversation, not current context-window usage. Cache percentage is cached input divided by input, not account quota. TPS is a log-derived estimate, not a server-provided instantaneous measurement; incomplete or ambiguous data may be shown as unknown.

## Build and start

Requires Windows x64 and the .NET 10 SDK. From the repository root:

```powershell
dotnet publish src/CodexStatusbar/CodexStatusbar.csproj -c Release -r win-x64 --self-contained true -o dist
.\start-monitor.bat
```

Start the tool manually. No Codex configuration changes are required. The tray menu exposes a manual start-with-Windows option.

Run the built-in tests, including the repository fixtures:

```powershell
.\dist\CodexStatusbar.exe --self-test fixtures
```

A run against the current local Windows environment passed 309 checks. Client updates, different data formats and window layouts can change behavior; verify on your own system. This third-party program is not an official OpenAI product and reads local Codex session usage data.

## Attribution and license

Retains upstream MIT notices and third-party attribution. See [LICENSE](LICENSE), [LICENSE-codex-token-overlay](LICENSE-codex-token-overlay) and [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).
