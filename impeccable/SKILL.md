# Impeccable 设计词汇与 AI 味检测

Source: github.com/pbakaus/impeccable（Apache-2.0）。给编码 Agent 的设计词汇：用 24 个动词命令指挥改设计，用 critique/audit 做评审，用 62 条确定性规则的检测器在上线前抓 AI 味。2026-11-02 一周 GitHub 趋势榜第 2 位。

## 这是什么

- 一套“动词 + 目标”的指挥语言：不说“让它好看点”，而说“把定价卡片 bolder 一点”“把落地页 quieter 一点”，每个动词背后是一份只针对这一个动作的 playbook。
- 一个评审器：`critique` 按可用性启发式打分并排定先修顺序；`audit` 查无障碍、性能、主题与响应式，结果按 P0（阻塞）到 P3（打磨）分级。
- 一个无需 LLM、无需 API Key 的 AI 味检测器：62 条确定性规则，抓 Inter 字体滥用、紫蓝渐变、卡片套卡片、彩色底上的灰字、每个标题上的圆角方形图标块等“模型默认款”，也抓内边距过紧、标题层级跳号等质量问题。
- 一份项目上下文机制：`/impeccable init` 读代码、只问推断不出的事（产品给谁用、要做什么、有何约束），写出 `PRODUCT.md`；`/impeccable document` 把现有视觉系统记录为 `DESIGN.md`，后续改动保持在品牌内。

## 什么时候用

- 已有页面或组件想迭代时最强：指着一个具体目标评审、改动，比整页重做效果好。
- 新页面也可以做，但官方提示这是较难的模式：先从一个组件起手，例如 `/impeccable critique the pricing cards`。
- 想在 Agent 写完 UI 文件后自动拦截 AI 味时用设计钩子（hook）：钩子在 Agent 宣称完成前扫描改过的文件并把结果交回。
- 想在终端或 CI 里不依赖 Agent 做检查时用 CLI 检测器。

## 安装

- 从项目根目录：`npx impeccable install`，安装器会识别已装的编码工具，问装到本项目还是全局，并提供设计钩子——建议接受钩子。装完重载编码工具，再在 Agent 里跑 `/impeccable init`（Codex 上是 `$impeccable init`）。
- 更新：`npx impeccable update`。脚本化安装：`npx impeccable install --providers=claude,codex,cursor --scope=project|global`。
- 插件路线：Claude Code 用 `/plugin marketplace add pbakaus/impeccable` 再安装；VS Code Copilot 用扩展 `renaissance-geek.impeccable`（需 VS Code 1.109.3+、Copilot Chat、可信工作区，装技能但不装钩子）；Grok Build 用 `grok plugin install pbakaus/impeccable#plugin --trust`。
- 团队锁定版本：`git submodule add https://github.com/pbakaus/impeccable vendor/impeccable`，再 `npx impeccable link --source=vendor/impeccable --providers=claude,cursor` 并提交；升级时先 `git submodule update --remote` 再重跑 link。注意 link 只装技能不装钩子。
- 手动复制（无需 Node）：从仓库根按工具取对应文件夹（如 Claude Code 是 `.claude/skills/impeccable`），拷到项目或全局技能目录；但手动复制会跳过钩子与子 Agent，想补全仍需跑安装器。
- 本技能只存用法说明，不替用户安装；是否安装、是否接受钩子，由用户决定。

## 核心用法

- 24 个命令（在 Agent 里 `/impeccable <命令> <目标>`）：
  - 指挥方向：`bolder` 放大安全设计、`quieter` 收敛过 loud 的设计、`distill` 删页面不需要的东西、`colorize` 给灰界面上色、`typeset` 修字体层级与尺寸、`layout` 修间距节奏、`animate` 加过渡与交互反馈、`delight` 加个性、`overdrive` 上着色器、弹簧物理与滚动驱动效果。
  - 评审：`critique` 打分式 UX 评审、`audit` 技术质量报告。
  - 收尾：`polish` 上线前最后一遍、`harden` 覆盖错误、国际化、溢出与边界情况、`clarify` 重写含糊文案、`adapt` 处理屏幕尺寸与设备、`optimize` 修加载与渲染性能、`onboard` 设计首次使用流程与空状态。
  - 实时迭代：`live` 在运行中的开发服务器上打开覆盖层，点中元素、循环看变体，接受后写回源码（支持 Vite、Next、SvelteKit、Astro、Nuxt、纯 HTML）；`generate` 不用手动点选，直接为指定元素生成变体。
  - 规划与建制：`shape` 写代码前先规划 UX、`init` 写 `PRODUCT.md`、`document` 写 `DESIGN.md`、`extract` 把重复模式抽成设计系统。
  - 常用命令可固定为快捷方式，如 `/impeccable pin audit` 后可直接用 `/audit`。
- 目标越窄效果越好：审一个组件好过一眼扫整页；想改什么就点名目标，不要只说“优化这个页面”。
- CLI 检测器（无需 Agent）：
  - `npx impeccable detect src/` 扫目录；`npx impeccable detect https://example.com` 扫渲染后页面（用本机 Chrome/Edge）；`npx impeccable detect --json .` 出机器可读结果；`npx impeccable detect --github src/` 出 GitHub Actions 标注；`npx impeccable detect --list-rules` 列全部规则。
  - 退出码 2 表示有发现，1 表示有目标扫不了。JSX/TSX 等源码只跑模式类规则；需渲染的规则（低对比度、字太小）要扫 URL，所以也要指向开发服务器，如 `npx impeccable detect http://localhost:5173`。
  - 忽略规则：品牌字体可 `npx impeccable ignores add-value overused-font Inter --reason "Brand font"`；单文件关闭某规则用注释 `<!-- impeccable-disable overused-font: exported brand doc -->`。
- 浏览器扩展：Chrome 扩展可在任意线上页面上叠加同样的检测结果，适合评审别人的页面。

## 注意事项

- 钩子有三处需另行确认：Codex 里在 `/hooks` 批准项目钩子（改动钩子的更新后需再批一次）；Grok Build 跑 `/hooks-trust` 或以 `--trust` 启动；Hermes Agent 需 `hermes plugins enable impeccable-design-hook` 一次。
- Live 模式只改本地检出：不要把它注入已部署站点，也不要为它放宽生产 CSP。应用文案编辑时会跑 `package.json` 里可选的 `impeccable:manual-edit-validate` 脚本，不熟悉的仓库先审该脚本再用。
- `craft` 是已废弃的别名，等同于普通构建请求，不要再用。
- 与 hallmark 的分工：Hallmark 擅长从零建新页；Impeccable 擅长评审、打分与迭代已有界面。同一页面只选其一做终审，避免重复 lint。
