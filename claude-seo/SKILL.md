# Claude SEO 网站 SEO 与 AI 搜索审计

Source: github.com/AgriciDaniel/claude-seo（MIT）。免费开源的 Claude Code SEO 插件：给它任意网站，10–15 分钟跑完一次完整 SEO 审计，交一份按重要性排好序的行动计划。2026-11-02 一周 GitHub 趋势榜第 3 位。

## 这是什么

- 由 26 个子技能与 19 个专长 Agent 组成：一次完整审计最多 17 个 Agent 并行，分别查技术、内容、结构化数据、速度、AI 搜索等；每条建议都说明依据、依赖与“怎么知道它起效了”。
- 查的范围（一个好 SEO 顾问会查的全部）：
  - 技术 SEO：可爬性、索引、站点地图、robots.txt、重定向、页面速度与核心 Web 指标（Core Web Vitals）。
  - 内容质量：E-E-A-T（经验、专业、权威、可信）、单薄页面与主题覆盖。
  - 结构化数据：发现、校验并生成 Schema.org 标记，争取富结果展示。
  - AI 搜索（GEO）：Google AI Overviews、AI Mode、ChatGPT、Perplexity、Gemini 引用你页面的可能性。
  - 本地与电商 SEO：Google 商家档案、评价、地图包、产品页与国际站点。
  - Agent 就绪度：Lighthouse Agentic Browsing、AI Agent 访问、llms.txt、Markdown、WebMCP。
- 报告存为 Markdown，`/seo google report` 可转成排版好的 PDF；漂移监控（drift）用 SQLite 快照记录变化，可做基线、对比与历史查看。

## 什么时候用

- aivorka 站点上线前后做一次完整审计时用：先跑 `/seo audit` 拿总清单，再按优先级逐项修。
- 单页有问题（如某页不被收录、富结果出不来）时用 `/seo page` 做深挖，不必每次跑全站。
- 做内容规划时用 `/seo content-brief`（目标关键词、大纲、内链）与 `/seo cluster`（基于搜索结果的语义聚类）。
- 想知道 AI 搜索是否引用自己时用 `/seo geo`；想知道 Agent（AI 助手）能否顺利读自己站点时用 `/seo agentic`。
- 不适合替代真实的搜索控制台数据：审计给的是检查与建议，最终排名与点击仍以 Google Search Console 为准。

## 安装

- 需要 Claude Code 1.0.33 或更新版。在 Claude Code 内依次跑：
  - `/plugin marketplace add AgriciDaniel/claude-seo`
  - `/plugin install claude-seo@agricidaniel-claude-seo`
  - `/seo setup`（准备它自己的 Python 环境与无头浏览器，不动系统其余部分）；随时用 `/seo doctor` 检查运行环境是否就绪。
- 手动安装（macOS/Linux）：`git clone --depth 1 https://github.com/AgriciDaniel/claude-seo.git` 后 `bash claude-seo/install.sh`；Windows 用 `install.ps1`。单行 curl 安装前务必先下载、通读脚本再执行，装完删掉脚本文件。
- 扩展（可选，9 个，用于实时数据）：如图片生成、SE Ranking 的 AI 份额、Profound 的 LLM 引用追踪、Bing 站长工具与 IndexNow 提交、多页 Lighthouse 运行器等；核心审计不需要扩展。
- 本技能只存用法说明，不替用户安装；是否安装、是否接 Google API（GSC、PageSpeed、GA4 等），由用户决定。

## 核心用法

- 在 Claude Code 里输入 `/seo` 看全部命令；常用命令分六组：
  - 审计与规划：`/seo audit <url>` 全站审计；`/seo page <url>` 单页深挖；`/seo plan <saas|local|ecommerce|publisher|agency>` 按站点类型做战略规划；`/seo setup`、`/seo doctor` 管运行环境。
  - 技术 SEO：`/seo technical <url>`（9 大类）；`/seo sitemap <url|generate>` 分析或生成 XML 站点地图；`/seo images <url>` 图片优化；`/seo hreflang <url>` 多语言；`/seo programmatic <url>` 程序化 SEO；`/seo google [命令]` 接 Google 各 API；`/seo drift baseline|compare|history <url>` 漂移监控。
  - 内容与 E-E-A-T：`/seo content <url>`；`/seo content-brief <主题>`；`/seo cluster <关键词>`；`/seo sxo <url>`（搜索体验优化：页面类型、用户故事、画像）；`/seo flow [阶段]`（FLOW 框架提示）；`/seo competitor-pages <url>` 竞品对比页。
  - Schema 与 AI 搜索：`/seo schema <url>`；`/seo geo <url>`；`/seo agentic <url>`。
  - 本地、链接、电商：`/seo local <url>`；`/seo maps [命令]`；`/seo backlinks <url>`；`/seo ecommerce <url>`。
  - 扩展：`/seo image-gen`、`/seo seranking`、`/seo profound`、`/seo bing`、`/seo unlighthouse`。
- 工作方式：先让它识别站点类型（SaaS、本地、电商、出版、代理），它会据此调整检查项；审计完成后按行动计划的优先级修，每项修完用它给出的验证方法确认是否起效，再跑 drift 对比。
- 报告管理：报告默认是 Markdown 文件，自己留存；需要对外交付时再转 PDF，不要把审计过程的中间文件当成最终报告发出。

## 注意事项

- 插件本身免费，但它跑在 Claude Code 计划内；扩展里涉及第三方服务（如 SE Ranking、Profound）时，按对方条款可能需要账号或付费——用之前先确认，不在本技能里默认开通。
- 审计会并发访问目标站点：只审自己的站点或经授权的站点，不要拿它去扫别人的站做施压式爬取。
- 它的“真实效果”案例（自家站点 12 周 10.8K 点击、移动端 PageSpeed 99 分）是项目方自述，仅供参考，不保证任何站点能复现。
- 不要把审计建议当成排名保证：搜索引擎算法与 AI 引用规则会变，建议的有效期以审计时的官方指南为准，定期重跑 drift 对比。
