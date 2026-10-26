# Claude Plugins Official 目录

Source: github.com/anthropics/claude-plugins-official（Apache-2.0）。Anthropic 官方维护的 Claude Code 插件目录，收录高质量的内部插件与通过审核的第三方插件。GitHub 本周趋势榜第 11 位，2026-10-26 一周。

## 这是什么

- 一个插件市场（marketplace），不是单个插件：`/plugins` 是 Anthropic 内部插件，`/external_plugins` 是第三方合作方插件。
- 每个插件可包含斜杠命令、Agent 定义、技能（SKILL.md）和可选的 MCP 服务器配置。
- 目录本身会演变：插件名一旦发布即不可改名，改名走顶层 `renames` 映射做自动迁移，安装时认准旧名也能迁移。

## 什么时候用

- 想给 Claude Code 扩能力时先来这里查官方目录，而不是随手从不明仓库装插件。
- 按场景找插件，例如：开发类（agent-sdk-dev）、设计类（Adobe 创意工具）、效率类（Airtable）、安全类（API 安全审计、SAST 与密钥扫描）。
- 想了解某个插件是否官方背书、由谁维护、主页在哪时，用目录里的作者与主页字段核对。

## 安装

- 在 Claude Code 内安装指定插件：`/plugin install {plugin-name}@claude-plugins-official`
- 浏览安装：在 `/plugin > Discover` 里搜索浏览。
- 插件结构（供判断质量时参考）：`plugin-name/` 下含 `.claude-plugin/plugin.json`（必需）、`.mcp.json`（可选）、`commands/`、`agents/`、`skills/`、`README.md`。

## 核心用法

- 先定任务再选插件：例如“做 API 安全审计”找安全类插件，“连 Airtable 做运营表”找效率类插件，不要一次装一堆。
- 装完先读该插件自己的 README，确认它要接哪些 MCP 服务器、需要什么账号或密钥。
- 只装当前任务需要的插件；用完可卸载，保持 Agent 的可用工具集干净。

## 注意事项

- 官方目录原文明确提醒：安装前确认信任该插件——Anthropic 不控制插件内含的 MCP 服务器、文件或软件，也无法保证其行为与日后变化。
- 第三方插件即使进了目录，也要按“不可信代码”对待：先看主页与源码，再给账号与写权限。
- 本技能只存用法说明，不替用户安装任何插件、不花钱、不订阅付费服务。
