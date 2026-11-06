# Hallmark 反 AI 味设计技能

Source: github.com/Nutlope/hallmark（MIT）。给 Claude Code、Codex、Cursor、OpenCode 用的设计技能，目标是让 Agent 做出的页面不再一眼“AI 生成”。2026-11-02 一周 GitHub 趋势榜第 1 位。

## 这是什么

- 一套“先定底线、再选材质、最后真机检查”的建页流程，而不是一套现成模板。
- 内含 50 个主题（theme），每个主题是一种材质（纸张、字体、色彩、表面、组件与质感），附一张成品页图片；一次构建只读一个主题文件（不超过 8 KB），主题只定材质、不定页面结构，所以同一主题、不同提示会做出不同页面。
- 内置 sloplint 检查：在真实浏览器里按 8 个宽度渲染页面，抓“AI 味”特征——每个标题上都有的小眉标（eyebrow）、全大写追踪字母标签、标题里单字变色、整页空框或只剩照片、首屏撑不满的 Hero、每页结尾都有的订阅框、点了只说谢谢却不发送的表单。
- 三档门禁：Floor 永不放行（对比度、焦点态、减动效、编造事实、手机端布局 broken）；Reflex 是模型默认坏习惯，有理由可在记录里推翻；Finish 是工艺细节，不阻塞上线。
- 另有 755 张开放授权真照片库（Wikimedia Commons，CC0/公有领域/CC BY/CC BY-SA），按主题挑选并在页面上署名；不使用 Unsplash、Pexels、Pixabay（其条款禁止 AI 工具选图）。

## 什么时候用

- 给 aivorka 站点、落地页、作品页等做新页面时用，尤其适合“不要 AI 味、要像人做的”这类要求。
- 已有页面想翻新时用 redesign，代替从零重做；只想知道问题在哪时用 audit，不改动原页。
- 不适合只改一处小样式的场景：它的流程是整页构建（含 Hero 单独先做、先冷审再做其余部分），小修小补用它反而重。

## 安装

- 全局安装（所有项目可用）：`npx skills add nutlope/hallmark -g`，需 Node 18+；只装当前项目则去掉 `-g`。
- 只装某一个 Agent：`npx skills add nutlope/hallmark -g --agent claude-code`（可选 codex、opencode、cursor、github-copilot）。
- 作为 Claude Code 插件：`/plugin marketplace add nutlope/hallmark`，再 `/plugin install hallmark@hallmark`（调用为 `/hallmark:hallmark`）。
- 浏览器检查（`sloplint --render`、截图）另需 `npm i -g puppeteer-core` 与 Chrome；没有它们时静态检查仍可运行。
- 从克隆安装：`node skills/hallmark/scripts/install.mjs` 会先校验全部主题与引用再复制；`install-hook.mjs` 可给 Claude Code 加编辑时 Floor 检查。
- 本技能只存用法说明，不替用户安装；是否安装、装到哪个 Agent，由用户决定。

## 核心用法

- 构建新页面：在 Agent 里说“用 hallmark 做……”或 `/hallmark <提示>`。流程是：预检（读项目字体、配色、框架与锁定的 design.md，并一次性问清素材：照片、截图、真实数字，绝不编造）→ `scripts/theme.mjs deal` 发三张主题牌供挑选 → `draw` 按页面任务定结构 → Hero 单独先做、渲染、冷审 → 其余部分 → `scripts/sloplint.mjs --render` 在 8 个宽度下检查 → 新上下文的子 Agent 最后复审。
- 自定义页面：说 custom，Hallmark 放弃主题，只保留底线、门禁与一份简短方向（`direction.md`），从“只有这个客户才有的点子”出发设计。
- 翻新已有页面：`/hallmark redesign ./src/app` 保留内容与接线、只换设计系统与布局；说“keep the layout”则只换风格，说“polish”则只修 AI 味特征。翻新前 `sloplint --before` 会记录全部链接、字段、处理器与标题，`sloplint --kept` 在丢了任何一项时直接失败，最后交付新旧对照。
- 只审计不改动：`/hallmark audit ./index.html`；只 lint 任意页面：`node skills/hallmark/scripts/sloplint.mjs <文件或目录> --render`。
- 每个成品页是自包含 HTML+CSS，首个 CSS 注释里盖章记录它做了哪些决策，方便复查。

## 注意事项

- 预检阶段给不出真实素材（照片、截图、真实数字）时不要让它编：编造事实属于 Floor 门禁，本应被拦下。
- 主题是材质不是方案：不要指望选了主题页面结构就定型，结构由页面任务决定。
- 如果技能没有自动触发，直接点名：“用 hallmark 来做……”。
- 与 impeccable 的分工：Hallmark 擅长从零建新页；已有页面想逐项打分、逐条修，用 impeccable 的 critique/audit 更合适。两个都装时不要让它们在同一页面上重复 lint，选其一做终审。
