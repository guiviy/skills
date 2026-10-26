# T3 Code

Source: github.com/pingdotgg/t3code（MIT）。Agent harness 控制台：用手机 App、网页 App 或桌面 App 控制本机上跑着的编码 Agent。GitHub 本周趋势榜第 14 位，2026-10-26 一周。

## 这是什么

- 一个“控制面”，不是 Agent 本身：它不替你写代码，而是让你在手机或另一台机器上查看、指挥本机上的 Claude Code、Codex、Cursor、Grok Build、OpenCode、Google Antigravity 等 Agent。
- 目标是远程就绪、性能好、真正开源：作者称如果项目走偏，用户手里有全部代码可以 fork。
- 与已学的 Orca 的区别：Orca 侧重多 Agent 编排与并行工作区，T3 Code 侧重“随时随地控制”——手机端是其主打体验。

## 什么时候用

- 人不在电脑前、想用手机看 Agent 跑到哪一步、回一句话继续指挥时。
- 同时管多个 Agent 会话、想在网页端统一查看时。
- 想把 Agent 会话变成后台服务长期跑着、随时连上去时。

## 安装

- 命令行：`curl -fsSL https://t3.codes/install.sh | sh`（Windows 用 PowerShell 脚本），装完跑 `t3` 启动服务器并打开本地网页 App；`t3 service install` 可设为后台服务，`t3 update` 更新。
- 免安装试用：`npx t3@latest`。
- 桌面 App：从 GitHub Releases 下载，或用 winget、scoop、Homebrew、.deb、AUR 安装。
- 前置条件：先装好并登录至少一个 Agent 提供方（Codex、Claude、Cursor、Grok Build、OpenCode 需各自登录；Antigravity 在设置里启用并用 Google 登录）。
- 本技能只存用法说明，不替用户安装、不花钱。

## 核心用法

- 先在提供方登录，再开 T3 Code：没登录的 Agent 在 T3 里是不可用的。
- 会话管理：查看各 Agent 会话状态、切换会话、发后续指令，相当于把终端会话搬到手机上。
- 远程访问：按文档里的远程访问章节配置后，可从手机或另一台机器连回本机服务。
- 文档在仓库 `docs/` 目录：安装与首跑、权限模式、快捷键、项目设置、外观、MCP 连接、源码控制集成等。

## 注意事项

- 作者自述项目非常早期、预期有 bug：不要把它当成生产级稳定工具，重要会话仍以本机终端为准。
- 远程访问等于把本机 Agent 控制权暴露到网络：务必按文档做好鉴权与网络隔离，不要裸奔到公网。
- 目前作者基本不接受大功能贡献，提需求走 Ideas 讨论区即可。
