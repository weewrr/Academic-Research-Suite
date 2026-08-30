---
name: "academic-research-suite"
description: "All-in-one academic research suite: paper discovery, deep reading, literature review, idea generation, paper writing, peer review, rebuttal, verification, experiments, paper reproduction, publishing, figures, office documents, plus 148 scientific tool guides. Also produces complete papers/theses end-to-end via the 12-phase full pipeline (检索→精读→综述→写作→评审→修订→成稿) or the 23-stage autonomous research pipeline (23 阶段全自主管线/AutoResearchClaw, 原创实验+PIVOT/REFINE). Invoke for any academic research or paper task (论文检索/精读/综述/写作/审稿/Rebuttal/发表/图表/复现论文/从头写一篇完整论文)."
---

# Academic Research Suite

This skill consolidates 7 skill repositories (186 distinct skills after dedup — 59 verbatim under `scripts/`, 152 under `tools/`, 25 shared copies) into one suite: Research-Paper-Writing-Skills, ResearchClaw, academic-research-skills, claude-scientific-writer, phd-skills, research-superpower, and k-dense scientific-skills.

## How to use this skill

1. Identify the user's intent from the routing table below.
2. **Read the matching reference guide** under `references/` in this skill's directory — it contains the full merged workflow, checklists, and templates.
3. Follow that guide. Guides may point to deeper resources (verbatim source skills, scripts, templates, command files) — read those on demand.

## Routing table

| User intent (examples) | Read this guide |
|---|---|
| **Write a COMPLETE paper / thesis from scratch** — "写一篇论文", "写一篇本科/硕士/博士毕业论文", "帮我写博士论文", "produce a full paper/thesis", "use Academic Research Suite to write a paper" — anything asking for an end-to-end deliverable rather than one stage | `references/full-pipeline.md` |
| **Full autonomous 23-stage research pipeline** — "23 阶段", "AutoResearchClaw", "全自主研究管线", "autonomous research run", research papers with original experiments + PIVOT/REFINE logic | `references/autoresearch-pipeline.md` |
| Find / search papers, arXiv-PubMed-Scholar queries, open-access versions | `references/paper-discovery.md` |
| Deep-read a paper, evaluate relevance, citation traversal, screening rubrics, answer research questions | `references/paper-reading.md` |
| Literature review, survey, systematic review, PRISMA, gap analysis | `references/literature-review.md` |
| Reading list management, research profile (ResearchClaw DNL) | `references/reading-list-profile.md` |
| Research ideas, gap-driven proposals, hypothesis generation | `references/idea-generation.md` |
| Draft / write a paper, scientific writing workflow | `references/paper-writing.md` |
| Write a specific section (intro / abstract / method / experiments / related work / conclusion) | `references/paper-sections.md` |
| Peer review a manuscript, act as reviewer, reviewer panel | `references/peer-review.md` |
| Verify paper claims, numbers, code-paper alignment, retractions, tortured phrases | `references/paper-verification.md` |
| Rebuttal, respond to reviewers, revise manuscript | `references/rebuttal-revision.md` |
| Citation management, BibTeX, metadata enrichment, reference checking | `references/citations.md` |
| Experiment design, dataset curation, launch, debugging, comparison, paper reproduction | `references/experiment-research.md` |
| Publishing, journal/SCI submission ("写 SCI 论文", "投期刊/会议"), venue templates, LaTeX setup, submission package, full ARS pipeline | `references/publishing.md` |
| Grant proposals, clinical reports, market research reports, scholar evaluation | `references/grants-clinical.md` |
| Figures, scientific schematics, slides, posters, infographics, image generation | `references/figures-slides.md` |
| Create/edit Word, PDF, PPT, Excel documents; format conversion; parallel web research | `references/office-documents.md` |
| ARS command workflows (ars-full, ars-reviewer, ars-rebuttal-audit, ...) | `references/ars-commands.md` |
| Working with a specific scientific tool/database/library (chemistry, bioinformatics, ML, data APIs...) | `references/tools-index.md` |

## Layout of this skill

- `references/` — merged domain guides (the routing targets above)
- `commands/` — ARS (`ars-*.md`) and PhD skill command definitions
- `templates/` — ResearchClaw HTML templates (reading list, research profile, paper note)
- `scripts/` — verbatim source skills with their scripts (**source repo only**, see Conventions):
  - `scripts/claude-scientific-writer/<skill>/` — 26 skills incl. office documents, figures, clinical, grants (Python scripts)
  - `scripts/academic-research-skills/` — scripts (verification/claim-audit, literature API clients), shared contracts, agents, plus the ARS skills (academic-paper, academic-paper-reviewer, academic-pipeline, deep-research) and `MODE_REGISTRY.md`
  - `scripts/phd-skills/` — skills, shell scripts, agents (bash required)
  - `scripts/research-superpower/`, `scripts/ResearchClaw/`, `scripts/research-paper-writing/` — verbatim skills and section-writing references with examples
- `tools/` — 148 k-dense scientific tool guides (`tools/<tool>/SKILL.md`), indexed in `references/tools-index.md` (**source repo only**)

## Conventions

- All paths in guides are relative to this skill's root directory.
- **Slim install note**: the installed skill bundles only `SKILL.md` + `references/` + `commands/` + `templates/` (~0.5MB). Any path starting with the `scripts/` or `tools/` prefix resolves against the **source repository** at `D:\github\lunwen\Academic-Research-Suite` — read those files on demand from there, never assume they exist inside the install directory.
- Python scripts require Python 3.10+ (several need 3.11+ — grants-clinical, peer-review, publishing; xlsx scripts work on 3.8+); `.sh` scripts require bash.
- When a task matches multiple guides, load the most specific one first (e.g. writing an introduction → `paper-sections.md`, not `paper-writing.md`).
- **Whole-paper requests beat single-guide routing**: if the user wants a complete paper produced (not one stage of it), always route to `full-pipeline.md` — it orchestrates the other guides in order. Multi-stage requests that are NOT whole-paper (e.g. "search then review these") chain the individual guides directly.
- Prefer the merged guides over the verbatim copies in `scripts/`; the copies exist for scripts and full-detail deep dives.
