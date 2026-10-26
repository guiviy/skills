# Karpathy 编码指南

Source: github.com/multica-ai/andrej-karpathy-skills（MIT）。一个源自 Andrej Karpathy 对 LLM 编码毛病观察的 CLAUDE.md 文件，另含同名 Claude Code 技能与 Cursor 规则。GitHub 本周趋势榜第 13 位，2026-10-26 一周。

## 这是什么

- 针对 LLM 编码的四类通病开出的四条原则：不核对假设就乱猜、把简单事做复杂、顺手改无关代码、只说“修好它”不给可验证目标。
- 不是框架、不是工具，只是一份行为准则：把它放进项目的 CLAUDE.md 或装成插件，Agent 每次编码都按它行事。

## 四条原则

1. **先想再写（Think Before Coding）**：不确定就明说假设、给出多种解读、该反驳时反驳、真困惑时停下来问，不要静默选一个解读一路跑下去。
2. **简单优先（Simplicity First）**：只写解决问题的最小代码；不要没被要求的功能、一次性代码的抽象、没被要求的“灵活性”；200 行能写成 50 行就重写。
3. **精准改动（Surgical Changes）**：只动必须动的地方；不“顺手优化”相邻代码、注释与格式；不重构没坏的东西；只清理自己改动造成的孤儿导入与变量，无关死代码只提不删。
4. **目标驱动（Goal-Driven Execution）**：把命令式任务翻译成可验证目标再循环执行，例如“加校验”改成“先给非法输入写测试，再让测试通过”；多步骤任务先列简短计划，每步带验证方法。

## 什么时候用

- 让编码 Agent 写非平凡代码、改他人代码库、提 PR 之前，先套这份准则，能显著减少返工与无关 diff。
- 做代码评审时用它当检查表：diff 里每一行是否都直接对应用户要求。

## 安装

- Claude Code 插件（推荐）：先加市场 `/plugin marketplace add forrestchang/andrej-karpathy-skills`，再装 `/plugin install andrej-karpathy-skills@karpathy-skills`。
- 单项目 CLAUDE.md：新项目直接下载该仓库的 CLAUDE.md；已有项目把内容追加到现有 CLAUDE.md 末尾。
- Cursor：仓库自带 `.cursor/rules/karpathy-guidelines.mdc` 规则，打开项目即生效。

## 注意事项

- 这份指南偏向“谨慎压倒速度”：修 typo、一行小改这种小事不必全套流程，用判断力取舍。
- 它治的是行为毛病，不是正确性保证：测试、类型检查与人工评审仍不可少。
- 可以和项目专属规则合并使用，例如在后面追加 TypeScript 严格模式、接口测试要求等。
