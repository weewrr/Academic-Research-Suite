<div align="center">

# Academic Research Suite · 学术研究全家桶

**一个技能 = 7 个仓库 · 186 个技能 · 一次安装全拥有**

[![skills](https://img.shields.io/badge/%E6%95%B4%E5%90%88%E6%8A%80%E8%83%BD-186-8b5cf6)](#内容一览)
[![guides](https://img.shields.io/badge/%E5%90%88%E5%B9%B6%E6%8C%87%E5%8D%97-20-3b82f6)](#内容一览)
[![tools](https://img.shields.io/badge/%E5%B7%A5%E5%85%B7%E6%8C%87%E5%8D%97-148-10b981)](references/tools-index.md)
[![license](https://img.shields.io/badge/%E8%AE%B8%E5%8F%AF%E8%AF%81-MIT%20%2F%20CC--BY%204.0-f59e0b)](#致谢)

[English](README.md) · [中文](README_zh.md)

<img src="assets/banner.svg" alt="Academic Research Suite 横幅：一个技能、7 个仓库、186 个技能、20 份合并指南、148 个工具指南，覆盖检索、写作、评审、管线四大领域" width="880">

</div>

---

## 为什么做这个

安装 180+ 个独立技能会撑爆上下文窗口、触发词互相冲突（三个 `paper-writing`、两个 `literature-review`……），维护更是噩梦。**Academic Research Suite** 把它们整合为**一个**路由式技能：

> 你的请求 → `SKILL.md` 意图路由 → 一份 150-400 行的聚焦指南 → 深层资源按需加载

只安装**一个文件夹**，即获得完整能力栈：论文检索、精读、文献综述、选题灵感、分章节写作、同行评审、声明核验、Rebuttal、实验研究、发表投稿、图表制作、办公文档，外加 148 个领域工具指南。

<img src="assets/before-after.svg" alt="整合前后对比：180+ 零散技能导致上下文膨胀与触发冲突，对比一个带路由、按需加载的整合套件" width="880">

## 架构

<img src="assets/architecture.svg" alt="Academic Research Suite 架构图：用户请求经 SKILL.md 路由到 20 份领域指南，底层由 scripts、tools、commands、templates 支撑" width="880">

## 覆盖研究全生命周期

<img src="assets/lifecycle.svg" alt="研究生命周期与指南映射：发现、精读、综述、选题、写作、核验、答辩、发表" width="880">

## 内容一览

<img src="assets/capabilities.svg" alt="能力全景图：17 项能力分为检索阅读、写作产出、评审质量、管线领域四组" width="880">

| 领域 | 指南 | 亮点 |
|---|---|---|
| 检索与阅读 | `paper-discovery`、`paper-reading`、`literature-review`、`reading-list-profile`、`idea-generation` | arXiv/PubMed/Scholar 检索式构建、开放获取回退链、DNL 七段精读笔记、PRISMA 筛选、缺口驱动选题 |
| 写作与产出 | `paper-writing`、`paper-sections`、`figures-slides`、`office-documents` | 9 步写作工作流、分章节 playbook（引言/摘要/方法…）、色盲安全配图、docx/pdf/pptx/xlsx 管线 |
| 评审与质量 | `peer-review`、`paper-verification`、`rebuttal-revision`、`citations` | 5 席评审团、claim 审计工具链、撤稿与"折磨短语"筛查、审稿回复手册 |
| 管线与领域 | `experiment-research`、`publishing`、`grants-clinical`、`ars-commands`、`tools-index` | 实验设计→论文复现（七阶段）、venue 合规、LaTeX 环境、基金/临床报告、148 个工具索引 |
| 端到端管线 | `full-pipeline`、`autoresearch-pipeline` | 12 阶段论文流水线（需求→交付，AUTO/STEP 双模式、反伪造哨兵）；完整保留 AutoResearchClaw 的 23 阶段全自主研究管线（5/9/20 门控、PIVOT/REFINE 决策、自愈实验、4 层引文核验） |

## 目录结构

```text
academic-research-suite/
├── SKILL.md                  # 意图路由入口 — 常驻加载，约 70 行
├── references/               # 20 份合并后的领域指南（日常主力）
│   ├── paper-discovery.md    ├── paper-writing.md      ├── peer-review.md
│   ├── paper-reading.md      ├── paper-sections.md    ├── paper-verification.md
│   ├── literature-review.md  ├── figures-slides.md    ├── rebuttal-revision.md
│   ├── reading-list-profile.md ├── office-documents.md ├── citations.md
│   ├── idea-generation.md    ├── experiment-research.md ├── publishing.md
│   ├── grants-clinical.md    ├── ars-commands.md      ├── tools-index.md
│   ├── full-pipeline.md      └── autoresearch-pipeline.md
├── tools/                    # 148 个 k-dense 工具指南（tools/<名称>/SKILL.md）
├── scripts/                  # 6 个源仓库：技能原文 + 脚本（含 ARS academic-paper /
│                             #   -reviewer / -pipeline 与 deep-research 四个技能）
├── commands/                 # ARS（16 个）+ PhD（6 个）命令定义
├── templates/                # ResearchClaw HTML 模板
└── assets/                   # 本 README 配图
```

## 路由示例

直接用自然语言说需求即可：

| 你说 | 加载的指南 |
|---|---|
| "帮我找 LLM 量化的最新论文" | `paper-discovery.md` |
| "用全家桶帮我写一篇本科毕业论文" | `full-pipeline.md` — 12 阶段流水线：需求 → 选题 → 检索 → 精读 → 提纲 → 自愈实验 → 起草 → 图表 → 自评审 → 核验 → 修订 → DOCX 交付 |
| "用全家桶帮我写一篇硕士毕业论文" | `full-pipeline.md` — 12 阶段流水线，master-thesis 档：更深的文献综述章 + 多章方法/实验 |
| "帮我写博士论文" | `full-pipeline.md` — phd-thesis 档主线（系统综述章 + 3-5 章创新点）+ 原创实验核心 `experiment-research.md`；实验部分要全自动跑 → `autoresearch-pipeline.md` 23 阶段 |
| "把我的研究写成 SCI 论文并投稿" | `publishing.md` — 10 阶段学术管线（research → write → integrity → review → revise → finalize）+ venue 模板合规；已有草稿可从中途阶段进入 |
| "跑 AutoResearchClaw 的 23 阶段全自主管线" | `autoresearch-pipeline.md` — 8 阶段组 · 3 门控 · PIVOT/REFINE · 自愈实验 · 4 层引文核验 |
| "帮我精读这篇论文" | `paper-reading.md` |
| "给我的 NeurIPS 论文写引言" | `paper-sections.md` |
| "以 Reviewer 2 的身份评审这份稿件" | `peer-review.md` |
| "检查我的参考文献有没有被撤稿" | `paper-verification.md` + `citations.md` |
| "针对这份审稿意见写 rebuttal" | `rebuttal-revision.md` |
| "把我的实验结果做成 LaTeX 海报" | `figures-slides.md` |
| "用 ChEMBL 查这个化合物" | `tools-index.md` → `tools/chembl-database/` |

## 12 阶段流水线：端到端执行示例

以「基于随机森林的电力负荷预测」本科毕业论文为例，走一遍 `full-pipeline.md` 的 12 个阶段（STEP 模式，每个 ◆ 检查点等用户确认）：

| 阶段 | 加载的指南 | 具体执行示例（输入 → 输出） |
|---|---|---|
| 0 需求卡 | （本指南） | 输入「写一篇本科毕业论文：基于随机森林的电力负荷预测」→ 补全需求卡：中文、1-2 万字、学校章节结构、`experiments: empirical`、模式 STEP |
| 1 选题与研究问题 | `idea-generation.md` | 3 个候选 RQ + 工作题目（≤25 字）+ 定位段 → **Gate A：确认题目与研究问题** ✅ |
| 2 文献检索 | `paper-discovery.md` | 真实 API 检索（中文源走 CNKI 客户端）→ 15-30 条 verified source registry（题录 + DOI + 一句话相关性）→ STEP 展示 |
| 3 精读与综述 | `paper-reading.md` → `literature-review.md` | 5-8 篇核心精读 → 综合矩阵 + 缺口陈述（如"缺本地化特征工程对比"） |
| 4 提纲与证据表 | `paper-sections.md` | 绪论 / 相关技术 / 方法 / 实验 / 结论 五章提纲 + claim-evidence 对照表 |
| 5 实验（可选） | `experiment-research.md` | `electricity.csv` · RF vs 基线模型对比，固定种子 → **Gate B：确认实验方案** ✅；训练脚本报错 → 自愈修复重跑 |
| 6 起草 | `paper-writing.md` + `paper-sections.md` | 按学校章节逐章起草成完整初稿 |
| 7 图表 | `figures-slides.md` | 特征重要性图、模型对比柱状图（色盲安全配色）+ 图注 |
| 8 自评审 | `peer-review.md` | 评审报告：如「第 4 章缺基线设置说明」→ 修订 roadmap |
| 9 核验 | `paper-verification.md` + `citations.md` | 核验报告：不在 registry 里的引用一律删除；全文数字可溯源 |
| 10 修订 | `rebuttal-revision.md` | 按 roadmap 修订 → 终稿 + change log（8→9→10 最多循环 2 轮） |
| 11 格式与交付 | `office-documents.md` | Markdown → DOCX（学校模板）→ **Gate C：终稿验收** ✅ |

**本次运行的交付物**：论文 DOCX（学校模板）· verified source registry · 指标 JSON + 图表 · 核验报告 · change log

**AUTO 模式**：说「全自动跑」即连续执行，仅 3 个门控（A/B/C）与 SmartPause 会打断（自愈耗尽、二次 PIVOT、伪造引用、RQ 崩塌）。

## 23 阶段管线：端到端执行示例

以「LoRA 与全参数微调在低资源场景下的精度-效率权衡」为例，走一遍 `autoresearch-pipeline.md` 的完整执行过程。阶段编号与门控规则见管线文档，每个阶段按映射表加载对应的套件指南：

| 阶段 | 加载的指南 | 具体执行示例（输入 → 输出） |
|---|---|---|
| 1 `TOPIC_INIT` | `idea-generation.md` | 输入「研究 LoRA 与全参微调在低资源场景的精度-效率权衡，目标 NeurIPS 2027」→ 主题陈述 + 目标 venue + 验收标准 |
| 2 `PROBLEM_DECOMPOSE` | `idea-generation.md` | 拆出 3 个子问题（可训练参数-精度曲线 / 显存受限下最大 batch / 差距最大的任务）→ RQ brief |
| 3-4 `SEARCH_STRATEGY` → `LITERATURE_COLLECT` | `paper-discovery.md` | 真实 API 检索：`("LoRA" OR "low-rank") AND "full fine-tuning"` 于 arXiv/Semantic Scholar/OpenAlex → 原始 47 篇 → 去重 41 篇 → 全文 23 篇 |
| 5 `LITERATURE_SCREEN` **[GATE]** | `paper-discovery.md` | 展示按相关性排序的筛选表：剔除 6 篇无基线对比 → 17 篇进入知识提取；**等待用户确认** ✅ |
| 6-7 `KNOWLEDGE_EXTRACT` → `SYNTHESIS` | `literature-review.md` | 逐篇提取方法/基线/结论 → 按 模型规模 × 任务 × 可训练参数量 构建证据矩阵 → 定位证据缺口 |
| 8 `HYPOTHESIS_GEN` | `idea-generation.md` | 多视角辩论：正「1% 可训练参数可达全量微调 95%+」，反「代码/推理任务显著衰减」→ 定稿 H1/H2/H3 |
| 9 `EXPERIMENT_DESIGN` **[GATE]** | `experiment-research.md` | 提交实验协议：3 模型规模 × 4 任务 × {LoRA r=8/32/64, 全量}，固定种子统一评测 → **等待用户确认** ✅ |
| 10-11 `CODE_GENERATION` → `RESOURCE_PLANNING` | `experiment-research.md` | 生成固定种子、输出结构化 metrics JSON 的训练/评测脚本 → sandbox 模式，单卡 A100，预算 ~40 GPU·小时 |
| 12-13 `EXPERIMENT_RUN` → `ITERATIVE_REFINE` | `experiment-research.md` | `CUDA OOM` → 诊断：3B 全量微调超预算 → 修复：梯度累积 + 混合精度 → 重跑通过（自愈，≤5 轮） |
| 14 `RESULT_ANALYSIS` | `experiment-research.md` § 分析 | 统计显著性/效率/泛化三视角并行：分类任务差距 <3%，代码生成任务差距 11.7% |
| 15 `RESEARCH_DECISION` | `experiment-research.md` § 分析 | 判定 **REFINE** → 代码任务补 r=128 + 更长训练后重跑（若假设被推翻则 **PIVOT**，≤1 次后询问用户） |
| 16-17 `PAPER_OUTLINE` → `PAPER_DRAFT` | `paper-writing.md` + `paper-sections.md` | 提纲含 claim-evidence 对照表；按 Method → Experiments → Related Work → Intro → Conclusion → Abstract 起草成 `paper_draft.md` |
| 18-19 `PEER_REVIEW` → `PAPER_REVISION` | `peer-review.md` + `rebuttal-revision.md` | 评审团证据审计：「Fig.3 缺置信区间」「3.2 节未说明学习率搜索范围」→ 修订 roadmap |
| 20 `QUALITY_GATE` **[GATE]** | `paper-verification.md` | 全数字可溯源、无 NaN/Inf、引用相关度达标 → **通过** ✅ |
| 21-23 `ARCHIVE_RUN` → `EXPORT_WITH_CHARTS` → `CITATION_VERIFICATION` | `citations.md` + `publishing.md` | 归档实验与决策 → 导出 NeurIPS 模板 `paper.tex` + 带误差棒 `charts/` → 四层核验击杀 3 条无法证实的引用，`references.bib` 47 → 44 |

**本次运行的最终交付物**

- `paper_draft.md` — 全文（Intro / Related Work / Method / Experiments / Results / Conclusion）
- `paper.tex` — NeurIPS 模板 LaTeX
- `references.bib` — 44 条真实 BibTeX，已修剪至与行内引用严格一致
- `verification_report.json` — 4 层引文完整性与相关性报告
- `charts/` — 带误差棒与置信区间的条件对比图
- `reviews.md` — 含方法-证据一致性检查的多智能体评审
- `evolution/` — 本次运行沉淀的自学习经验
- `deliverables/` — Overleaf 就绪的汇总文件夹

**实验模式怎么选**：`simulated`（快速草稿/无环境，论文中必须声明）· `sandbox`（本机执行，默认）· `ssh_remote`（远程 GPU 重实验）。

**一句话启动**：直接说「跑 23 阶段全自主管线，主题是……」即可；管线会在阶段 5 / 9 / 20 停下等你确认（即 `gate-only` 模式）。需要完全无人工介入时，明确说「全自动跑，门控自动通过」。

## 致谢

- [Research Paper Writing](https://github.com/Master-cai/Research-Paper-Writing-Skills) — 分章节论文写作方法论（MIT）
- [ResearchClaw](https://github.com/syr-cn/ResearchClaw) — 论文检索、精读、阅读列表、研究画像、选题灵感（MIT）
- [Academic Research Skills](https://github.com/Imbad0202/academic-research-skills) — 研究到发表全流程管线、深度研究、评审团 + 400 个脚本（CC-BY-4.0）
- [Claude Scientific Writer](https://github.com/K-Dense-AI/claude-scientific-writer) · [Scientific Agent](https://github.com/K-Dense-AI/claude-scientific-skills) — 科研写作、图表、办公文档、临床技能与 148 个领域工具指南（MIT）
- [PhD Research](https://github.com/fcakyon/phd-skills) — 实验设计、论文复现、发表、审稿防御（MIT）
- [Research Superpowers](https://github.com/kthorn/research-superpower) — 系统性文献检索、筛选、引文遍历（MIT）
- [AutoResearchClaw](https://github.com/aiming-lab/AutoResearchClaw) — 实验循环、PIVOT/REFINE 决策、反伪造守卫、4 层引文核验的设计来源

## 说明

- 脚本支撑的功能（claim 审计、引文 API 客户端、文档生成）需要 Python 3.10+ 及少量 PyPI 包（`pyyaml`、`jsonschema`、`requests`、`ruamel.yaml`、`defusedxml`）；PhD 技能的 shell 守卫需要 bash。所有方法论指南零依赖。
- 精简安装：技能目录只装 `SKILL.md + references/ + commands/ + templates/`（约 0.5MB）；`scripts/` 与 `tools/` 留在本仓库按需回读（见 `SKILL.md` 的 Slim install note）。
- 平台限制：`ars-mark-read` 与 LibreOffice 校验包装器为 Unix 专属（`fcntl` / `AF_UNIX`），Windows 需走 WSL；格式转换另需安装 Pandoc。
- 合并指南是推荐入口；`scripts/` 与 `tools/` 下的原文副本用于深度阅读和脚本调用。
- 原 Claude 插件的会话钩子不可移植，已移除；斜杠命令以文档形式保留在 `commands/` 下。
- 指南内的所有路径均相对于本技能根目录。

---

<div align="center">

**[English](README.md) · [中文](README_zh.md)**

</div>
