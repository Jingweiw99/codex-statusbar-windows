# CodexStatusbar Windows

[English](README.md) | 简体中文

一个为 Windows 上的 Codex 桌面客户端提供状态栏的第三方工具，在输入框附近显示输出速度、累计 token 和缓存命中率。

本项目是 [dongyue-19/codex-statusbar](https://github.com/dongyue-19/codex-statusbar) 的独立衍生版本，基于其 `v1.0.0-rc3`；不是 OpenAI 官方产品。

## 本版本的改动

- 同时存在新旧使用量记录时，优先使用一致的现代数据来源，避免混用不同口径。
- 按响应 ID 对现代记录去重；数据不完整或有歧义时，不强行给出速度。
- 新安装默认关闭开机自启；用户仍可通过托盘菜单手动启用。启动时程序会根据自身设置协调自启项：若设置为关闭，已有的本工具自启项可能被移除。
- 为混合来源、重复响应和异常使用量增加回归测试。

## 状态栏指标

| 指标 | 含义 |
| --- | --- |
| `t/s` 或 `tok/s` | 根据本地日志计算的输出速度估算值，不是服务端提供的即时测速，也不是整个任务的完成速度。 |
| 累计 token | 当前对话累计输入和输出 token 之和，**不是当前上下文窗口的占用量**。 |
| 缓存命中率 | 累计缓存输入 token 占累计输入 token 的比例，不是账号额度或上下文占用比例。 |
| `~` | 暂时沿用最近一次有效速度，例如工具执行期间。 |
| `--` | 当前没有足够可靠的数据，不代表速度为零。 |

详情面板可显示输入、输出、缓存和上下文等字段。上下文数据同样依赖客户端日志，可能因版本变化而不准确。

## 构建和启动

需要 Windows x64 和 .NET 10 SDK。在仓库根目录的 PowerShell 中运行：

```powershell
dotnet publish src/CodexStatusbar/CodexStatusbar.csproj -c Release -r win-x64 --self-contained true -o dist
.\start-monitor.bat
```

这是自包含构建，运行生成的程序时不要求另行安装 .NET 运行时。默认手动启动；程序不需要修改 Codex 配置。运行后可从系统托盘菜单查看状态或退出。

可在 [Releases](https://github.com/Jingweiw99/codex-statusbar-windows/releases) 查看是否已有可下载的 Windows 预构建包；若没有，请使用上述源码构建方式。

## 验证和限制

在仓库根目录运行内置自测，并指定测试样例目录：

```powershell
.\dist\CodexStatusbar.exe --self-test fixtures
```

本地 Windows 环境曾通过 309 项自测。这不等同于在所有客户端版本、显示器布局或系统环境中的完整验收；Codex 更新后日志格式或界面定位可能变化。

程序会读取本机 Codex 会话使用量数据。请只在信任本工具的环境中使用，并注意保护本地日志。

## 来源和许可证

保留上游 MIT 许可和第三方来源声明，详见 [LICENSE](LICENSE)、[LICENSE-codex-token-overlay](LICENSE-codex-token-overlay) 和 [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md)。
