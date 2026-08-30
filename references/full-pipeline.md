# Full Paper Pipeline — End-to-End Orchestration

Master conductor for producing a **complete paper or thesis from a single request**. When the user says "write a paper / 写一篇论文 / write my graduation thesis / use Academic Research Suite to produce a full paper", do NOT execute a single guide — run this pipeline. Each stage below loads one domain guide, executes it with defined inputs, produces defined artifacts, then hands off to the next stage.

This guide choreographs the other guides; it contains no domain methodology itself.

**Design credits**: the experiment-execution loop, PROCEED/REFINE/PIVOT decision, anti-fabrication guards, and SmartPause rules are adapted from [AutoResearchClaw](https://github.com/aiming-lab/AutoResearchClaw)'s 23-stage autonomous pipeline (MIT), simplified for a skill environment (no Docker sandbox — experiments run in the local workspace).

## When to Use

- "Write a complete paper on X" / "帮我写一篇关于X的论文"
- "Write my undergraduate/master graduation thesis" / "写一篇本科毕业论文"
- "From scratch to submission: research, write, review, revise" / "从头到尾生产一篇论文"
- "Use Academic Research Suite to produce a full paper" / "用全家桶流水线产出一篇论文"
- NOT for: single-stage requests ("find papers", "write the intro", "review this draft") — route those to the single guide directly.

## Operating Modes

Ask once at pipeline start (default = STEP if user doesn't answer):

| Mode | Behavior |
|---|---|
| **AUTO** ("全自动/不要打断我/just produce it") | Stages run consecutively; checkpoints collapse to one-line status reports; no waiting for confirmation — **except SmartPause triggers and approval gates (see below)**. Only the Intake (Phase 0) question is mandatory — never fabricate a topic the user didn't give. |
| **STEP** (default) | Pause at every ◆ checkpoint, present artifacts summary, wait for confirmation before the next stage. |

**Approval gates (both modes, never auto-skipped):**
1. **Gate A — Hypothesis/RQ approval** (end of Phase 1): the research question and scope drive everything downstream; wrong RQ = wasted pipeline.
2. **Gate B — Experiment plan approval** (Phase 5 entry, only when experiments run): what will be tested, on what data, with what success metric.
3. **Gate C — Final acceptance** (Phase 11 entry): user sees the final draft + verification report before formatting.

**SmartPause (AUTO mode only) — pause and ask despite AUTO when:**
- Experiment self-healing cap exhausted without a working run
- PIVOT decision triggered for the second time (research direction is unstable)
- Verification finds a fabricated or ungrounded citation (kill first, ask after)
- Confidence in the RQ collapses mid-pipeline (e.g. literature shows the question is already answered)

## Phase Map

| # | Phase | Guide loaded | Key artifacts out |
|---|---|---|---|
| 0 | Intake & thesis profile | (this guide) | Requirement card |
| 1 | Topic & research question | `idea-generation.md` | RQ brief, working outline skeleton |
| 2 | Literature discovery | `paper-discovery.md` | Verified source registry (15-40 refs) |
| 3 | Reading & synthesis | `paper-reading.md` → `literature-review.md` | Reading notes, synthesis matrix, gap statement |
| 4 | Outline & evidence map | `paper-sections.md` | Full outline, claim-evidence table |
| 5 | Experiments *(optional — research papers only)* | `experiment-research.md` | Code, run logs, metrics JSON, charts |
| 6 | Drafting | `paper-writing.md` + `paper-sections.md` | Complete draft, section by section |
| 7 | Figures & tables (if needed) | `figures-slides.md` | Figures, tables, captions |
| 8 | Self peer review | `peer-review.md` | Review reports, revision roadmap |
| 9 | Verification | `paper-verification.md` + `citations.md` | Verification report, corrected citations |
| 10 | Revision | `rebuttal-revision.md` | Final draft, change log |
| 11 | Formatting & delivery | `office-documents.md` / `publishing.md` §A Stage 5 | DOCX/LaTeX/PDF deliverables |

Loop rules: Phase 8→9→10 iterate until review verdict is Accept/Minor AND verification has no unresolved P0 issue (hard cap: 2 full loops, then report residual issues to the user). Phase 5 has its own internal loop (see below).

## Workflow

### Phase 0 — Intake (mandatory, both modes)

Collect a requirement card. Ask only for what's missing — never interrogate for what's already implied:

```yaml
type: undergraduate-thesis | master-thesis | phd-thesis | journal-paper | conference-paper | course-paper
topic: <user's topic, verbatim>        # MANDATORY — never invent
field: <discipline>
language: zh | en                       # thesis language (deliverable language)
length: <word count or page target>     # e.g. 本科毕设通常 8000-15000 字/1-2万字
structure: <school chapters OR standard IMRaD>  # see thesis profiles below
format_reqs: <school template / venue>  # optional
sources_preference: <must-cite refs, advisor suggestions>  # optional
experiments: none | simulation | empirical | reproduction   # none = literature-synthesis thesis
mode: AUTO | STEP
```

If the user gave only a topic sentence (typical: "写一篇本科毕业论文，题目是X"), infer sensible defaults for the rest, **state them explicitly in one short message**, and proceed — in AUTO mode do not wait for a reply.

**Thesis profiles** (default chapter structures when user doesn't specify):

- 中文本科毕业论文 (Chinese undergraduate thesis): 摘要(中英) → 第一章 绪论(背景/意义/国内外现状/组织结构) → 第二章 相关理论与技术 → 第三章 方法/设计 → 第四章 实验与分析 → 第五章 结论与展望 → 参考文献 → 致谢. Length 1-2 万字. Tone: expository, comprehensive coverage over novelty. Most such theses are `experiments: none` — literature synthesis + system design.
- English research paper: Abstract → Introduction → Related Work → Method → Experiments → Conclusion. Tone: contribution-driven, claim-evidence alignment. Usually `experiments: simulation | reproduction`.
- Master thesis: like undergraduate but deeper literature chapter, often 3+ chapters of method/experiments.
- PhD thesis (博士论文): original contribution is the bar. 绪论/综述 as a systematic chapter, 3-5 technical chapters each stating its innovation, conclusion defends the contributions as a whole. Length 5-15 万字 (field/school dependent). Experiments usually `empirical | simulation | reproduction` — for a fully autonomous experimental core with PIVOT/REFINE, hand the experiment phases to `autoresearch-pipeline.md`.

### Phase 1 — Topic & research question

Load `references/idea-generation.md`. With the user's topic, produce:
1. 1-3 candidate research questions (or thesis statements for a non-research thesis)
2. Working title (Chinese thesis: 题目 ≤ 25 字, avoid "研究与研究")
3. One-paragraph positioning: what this paper argues / contributes

◆ **Gate A** (both modes): confirm RQ + title before spending search budget.

### Phase 2 — Literature discovery

Load `references/paper-discovery.md`. Inputs: RQ brief. Targets by paper type:

- Undergraduate thesis: 15-30 references (mix: recent 3-5 years + a few classics; CNKI/中文 sources matter for Chinese theses — use the Chinese literature client noted in `citations.md`)
- Master thesis: 40-80 references; PhD thesis: 80-150, systematic coverage of the research area (field-dependent)
- Journal/conference: 25-40 references, venue-calibrated

Output: **verified source registry** (title / authors / year / venue / DOI / one-line relevance note). Every entry must come from a real search result — never a plausible-looking citation. This registry is the single source of truth: any citation appearing later in the pipeline that is not in the registry gets **deleted, not repaired**.

◆ STEP checkpoint: present the registry.

### Phase 3 — Reading & synthesis

Load `references/paper-reading.md`; for the 5-8 most relevant sources run its deep-read framework (skim the rest at abstract level via its screening rules). Then load `references/literature-review.md` and produce:
1. Reading notes (key method / finding / limitation per core source)
2. Synthesis matrix (themes × sources)
3. Gap statement — what this thesis adds, in one sentence

For a typical undergraduate thesis the bar is "organized, correctly-cited coverage", not novelty — calibrate depth accordingly, but never fabricate content of a paper you didn't read.

◆ STEP checkpoint: present the gap statement + source registry.

### Phase 4 — Outline & evidence map

Load `references/paper-sections.md`. Map the thesis profile chapters to concrete subsections. Build the claim-evidence table:

| Section | Claim it makes | Evidence (which ref / which experiment) | Figure/Table? |

Every section that asserts something must have a row. Sections with no evidence get flagged — either find evidence (back to Phase 2 for a specific source) or soften the claim in Phase 6. If experiments are planned, the experiment-derived rows stay empty until Phase 5 fills them.

### Phase 5 — Experiments (optional; skip silently when `experiments: none`)

Load `references/experiment-research.md`. Only for theses/papers claiming original results. Adapted from AutoResearchClaw's Stages 10-15:

1. **Experiment plan (Gate B, both modes)**: hypotheses to test, datasets/benchmarks, baselines, metrics, success criteria, environment constraints (what can actually run on this machine — no GPU cluster assumptions). Get user approval before writing code.
2. **Code generation**: small, self-contained scripts with fixed seeds, config-documented, outputting structured metrics (JSON/markdown), not console dumps.
3. **Execution with self-healing**: run in the local workspace. On failure, diagnose (dependency? bug? data path? OOM?) and repair — **max 5 repair cycles per experiment**. Cap exhausted → SmartPause.
4. **Metrics & charts**: collect into one results table; generate comparison charts with error bars / confidence intervals where sample size allows (see `figures-slides.md`).
5. **Decision point — PROCEED / REFINE / PIVOT** (adapted from AutoResearchClaw Stage 15):
   - **PROCEED**: results support the hypothesis → fill the Phase 4 evidence rows, continue to Phase 6.
   - **REFINE**: results ambiguous/weak → adjust parameters or experimental setup, re-run affected experiments (max 2 refine loops).
   - **PIVOT**: hypothesis contradicted → reformulate RQ or swap approach. PIVOT requires returning to Phase 4 with the new evidence map. Max 1 PIVOT; a second PIVOT trigger → SmartPause (direction is unstable, human judgment needed).

Anti-fabrication sentinel (runs continuously from here on): every number in the draft must trace to either the Phase 2 source registry or a Phase 5 run artifact (metrics JSON / logs). NaN/Inf/implausible metrics are quarantined and reported, never smoothed over. Detected fabrication = kill switch: delete the claim, log it in the verification report, SmartPause in AUTO mode.

### Phase 6 — Drafting

Load `references/paper-writing.md` for the global workflow, then `references/paper-sections.md` for per-section playbooks. Draft in this order regardless of final order: **Method/方法 → Experiments/实验 → Related Work/相关工作 → Introduction/绪论 → Conclusion → Abstract/摘要** (abstract last — it summarizes the finished paper).

Rules:
- Write section by section; after each, run the section's self-review checklist from `paper-sections.md` before moving on.
- Every citation in text must be from the Phase 2 registry — zero new uncited sources appear during writing. If a gap is found, go back to Phase 2 for that specific source.
- Every number must satisfy the sentinel rule above.
- In AUTO mode draft everything, then do one global coherence pass (terminology, notation, tense, chapter transitions) before Phase 7.

◆ STEP checkpoint (or AUTO status line): full draft complete, word count vs target.

### Phase 7 — Figures & tables

Load `references/figures-slides.md` only if the outline calls for figures. Generate figures/tables, ensure captions are self-contained and every figure is referenced from the text. Phase 5 charts land here in final form. A thesis with no figures is acceptable for many humanities topics — skip silently.

### Phase 8 — Self peer review

Load `references/peer-review.md`. Run its review workflow with yourself as the panel (for theses: 2 reviewers are enough; for papers: full 5-seat panel including Devil's Advocate). Output: review reports + a prioritized revision roadmap (P0 must-fix / P1 should-fix / P2 optional).

### Phase 9 — Verification

Load `references/paper-verification.md` + `references/citations.md`. Four layers (adapted from AutoResearchClaw's 4-layer citation verification):
1. **Number consistency**: abstract vs body vs tables vs Phase 5 metrics JSON
2. **Citation existence**: DOI spot-checks against the source registry; no retracted/phantom references; format consistency
3. **Claim grounding**: sample the strongest claims in each section — does the cited source actually say what the text claims? Flag ungrounded citations.
4. **Methodology-evidence consistency**: do the conclusions match what the evidence (refs + experiments) supports — no overclaiming.

### Phase 10 — Revision

Load `references/rebuttal-revision.md`. Apply the revision roadmap: P0 all, P1 most, P2 judgment. Produce a change log (issue → fix → section). Loop 8→9→10 if review verdict was Major.

◆ **Gate C** (both modes): present final draft + change log + verification report + residual issues before formatting.

### Phase 11 — Formatting & delivery

Load `references/office-documents.md`. Deliver:
1. Markdown master → DOCX (thesis default; school template if provided)
2. LaTeX/PDF only if the user asked or venue requires (see `publishing.md` §A Stage 5 rules)
3. A delivery note: word count, reference count, what was verified, residual risks, and — honestly — which sections are literature-synthesis vs. original argument/experiment, so the user knows where to add their own work.

## Anti-Patterns

- **Fabricated citations** — the single worst failure mode. A citation that looks right but doesn't exist is academic misconduct; only Phase 2 registry entries may be cited. Detected fabrications are deleted, never "repaired" into plausibility.
- **Inventing experiment results** when code fails — report the failure instead; a thesis with an honest negative result beats one with fictional numbers.
- Silently skipping Phase 8-10 in AUTO mode because "the draft looks fine" — review and verification always run.
- Writing the abstract first and never revising it.
- One giant dump at the end in STEP mode — checkpoints exist so the user can redirect cheaply.
- Treating an undergraduate thesis like a NeurIPS submission — calibrate depth, tone, and review intensity to the thesis profile.

## Academic Integrity

This pipeline drafts and polishes; the user owns authorship and must verify content before submission, especially for a degree thesis. The Phase 11 delivery note must state this. If the user's institution prohibits AI drafting for theses, flag it at Phase 0 and offer literature-synthesis-only assistance instead.
