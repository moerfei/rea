# rea — ZCode 本地插件

把 [REA（Reverse Engineer Anything）](https://github.com/morluto/rea) 接入 ZCode 的本地插件：

- **MCP server**：stdio 方式启动 `npx -y rea-agents@6.8.0 mcp`，把 REA 的逆向分析工具挂进 ZCode（工具名形如 `mcp__rea__<tool>`）。
- **Skill**：`reverse-engineer-anything` —— REA 官方的调查工作流指令（含 7 篇 references：原生二进制/JS 应用/Android/证据工作流/连接恢复/运行时观察/AgentEnv 环境构建）。

## 来源与许可

- Skill 内容与 MCP server 均来自 [morluto/rea](https://github.com/morluto/rea)（MIT License），本插件只做打包封装。
- 本仓库以 MIT 分发，`LICENSE` 保留上游 `Copyright (c) 2026 morluto` 声明；MCP 运行时仍从 npm 拉取 `rea-agents`，不改上游代码。
- REA 官方 `npx rea-agents setup` 的客户端名单不含 ZCode，故采用手动注册：stdio MCP 走插件顶层 `.mcp.json`（与 grafana-mcp 插件同一机制），skill 拷自仓库 `.agents/skills/reverse-engineer-anything/`。
- 仅用于合法授权的逆向研究。

## 依赖

- Node.js `^22.23.4 || ^24.22.1 || >=26.12.1`（6.8.0 的 engines 要求；低于此版本 npm 只告警、未强制阻断，但可能触发运行时问题。首次调用时 npx 会拉取 `rea-agents` 包）
- 按分析目标另需：Ghidra / Hopper / IDA（原生二进制）、JADX + JDK（APK）、Binwalk/Unblob（固件）等 —— REA 只桥接，不代装。

## 升级 REA 版本

1. 改 `.mcp.json` 中 `rea-agents@6.8.0` 的版本号（建议固定版本，勿用 `@latest`）。
2. 同步 `.zcode-plugin/plugin.json` 的 `version`。
3. 如 skill 有更新，从仓库 `.agents/skills/reverse-engineer-anything/` 重新拷贝。
4. 在 ZCode 插件管理里重新安装/刷新本插件。

## 注意

- REA v6.0.0 起 MCP 的文件输入要求**绝对路径**。
- Windows 上命令用 `cmd /d /c npx` 包裹（与 REA 官方 setup 的做法一致，规避 npx.cmd 直接 spawn 的问题）。
