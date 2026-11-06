# Superpowers Agent 开发方法论

Source: github.com/obra/superpowers（MIT）。一套完整的编码 Agent 开发方法论：不是单个命令，而是一组可组合的技能加启动指令，让 Agent 在动手写代码前先把需求问清楚、把设计写下来、把计划拆到可执行，最后用子 Agent 驱动开发并逐项评审。2026-11-02 一周 GitHub 趋势榜在列，仓库星数超 31 万。

## 这是什么

- 核心理念：技能是强制工作流，不是建议。Agent 在任务前必须检查相关技能，触发后按流程走，不能跳过。
- 基本工作流（按顺序触发）：
  1. brainstorming——动手前先问：把用户真正想做的事、为什么做问出来，再把理解分段复述给用户纠正；小改动在聊天里定，大项目写成设计文档。
  2. using-git-worktrees——设计批准后建隔离工作区（新分支），跑项目初始化，确认干净的测试基线。
  3. writing-plans——把批准的设计拆成 2–5 分钟一个的小任务，每个任务含精确文件路径、完整代码与验证步骤，写得要让“热情但没品位、没判断力、没项目上下文、讨厌测试的初级工程师”也能照做。
  4. subagent-driven-development 或 executing-plans——按计划执行：前者每个任务派一个全新子 Agent 并逐项评审（最彻底），后者当前会话内联执行、最后对整个分支做一次全新评审（最省）。
  5. test-driven-development——执行中强制红绿重构：先写失败测试、看着它失败、写最少代码、看着它通过、提交；测试之前写好的代码要删掉。
  6. requesting-code-review——任务之间对照计划评审，按严重度报告问题，关键问题阻塞进度。
  7. finishing-a-development-branch——任务完成时验测试、给出选项（合并/PR/保留/丢弃）、清理工作区。
- 贯穿原则：真正的红绿 TDD、YAGNI（用不上的不做）、DRY（不重复）。
- 出问题时有 diagnosing-superpowers 技能：读会话记录、按行举证哪里跑偏，需要时打包一份脱敏材料用于报 bug。

## 什么时候用

- 做 aivorka 站点这类“从零到上线”的多步骤开发时用：先 brainstorming 定设计，再按计划执行，而不是一上来就写代码。
- 做较复杂的功能、重构或修 bug 时用：计划拆分 + 逐项评审能防止 Agent 跑偏、烧掉大量 token。
- 小改动（如改一行文案、修一个拼写）不必全套走：流程本身说小改动在聊天里定即可，不要为小事启动完整流水线。
- 想长期复用同一套开发纪律时用：装一次后技能自动触发，不用每会话重新交代。

## 安装

- 按所用工具分别安装（用多个工具时每个都要单独装一次）：
  - Claude Code：官方插件市场 `/plugin install superpowers@claude-plugins-official`，或先 `/plugin marketplace add obra/superpowers-marketplace` 再 `/plugin install superpowers@superpowers-marketplace`。
  - Muse：本地检出后 `muse plugins install ./` 再 `muse plugins approve superpowers`；或克隆后 `muse plugins install ./superpowers` 再 approve；更新用 `muse plugins update superpowers`。装完重启会话让 SessionStart 钩子生效。
  - Codex App/CLI、Cursor、Gemini CLI、Devin、Factory Droid、Kimi Code、Qwen Code、Hermes Agent、Pi、OpenCode 等均有对应插件/扩展安装方式，统一入口是仓库 README 的 Installation 一节；OpenCode 是让 Agent 去读 `https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md` 并照做。
- 更新：按各工具的插件更新命令重装即可；装完重启当前会话，否则钩子不生效。
- 本技能只存用法说明，不替用户安装；是否安装、装到哪个工具，由用户决定。

## 核心用法

- 装好后不用特殊操作：新会话一说“做个某某功能”，正常安装会自动先触发 brainstorming 问需求，而不是直接写代码。验证方法：新会话说一句“做个 React 待办清单”，看它是否先问需求再动手。
- 流程节点上用户要给明确信号：设计分段复述后要纠正或确认；设计批准后才进入写计划；说“go”后才启动子 Agent 执行。不要在设计没定型时催它“直接写”。
- 评审是阻塞项：任务间评审报出的关键问题要先修再继续，不要让 Agent 带着关键问题往下走。
- 会话跑偏时直接说：“查一下这会话里 superpowers 哪里出了问题”，它会触发 diagnosing-superpowers；要查旧会话时带上会话 id。
- 很长的会话发生压缩（compaction）后可能丢失启动指令：Hermes 等无压缩后钩子的工具尤其如此，技能不触发时开新会话是最快的恢复办法。

## 注意事项

- Superpowers 是方法论，不是代码库：它不替你写具体业务逻辑，价值在“先问清楚、先写计划、先写测试、逐项评审”这套纪律。
- 子 Agent 驱动模式可能连续自主工作数小时：适合“定好设计就放手”的任务，不适合需要每步人工确认的任务；后者选 executing-plans（内联执行 + 最后总评审）。
- 商业支持走 sales@primeradiant.com，与免费开源使用无关；本技能不涉及任何付费订阅。
- 与已学的 andrej-karpathy-skills 可互补：Karpathy 四原则约束“怎么写代码”（先想再写、简单优先、精准改动、目标驱动），Superpowers 约束“怎么做项目”（需求、设计、计划、测试、评审全流程）。
