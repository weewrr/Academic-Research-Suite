# AutoResearchClaw 23-Stage Autonomous Pipeline — Full Preservation

Complete preservation of the fully autonomous 23-stage research pipeline from [AutoResearchClaw](https://github.com/aiming-lab/AutoResearchClaw) (aiming-lab, MIT, v0.5.0). One research topic in → one conference-grade paper out. Users may never trigger it — but it is here, complete, for when maximum rigor or full autonomy is wanted.

**Two ways to use this guide:**
1. **As a blueprint executed stage-by-stage here** — each stage below maps to a suite guide (mapping table at the end); the agent executes the stages in order with the gates and loops intact.
2. **As the manual for the real tool** — the original Python project runs standalone with its own sandbox, agents, and CLI; see "Running the original tool" at the end.

## When to Use

- "Run the full autonomous 23-stage pipeline" / "跑 23 阶段全自主管线"
- "AutoResearchClaw-style research" / "像 AutoResearchClaw 那样全自动出论文"
- Research-paper tasks needing **original experiments with self-healing and PIVOT logic**, not just literature synthesis
- NOT for: theses satisfied by literature synthesis → use `full-pipeline.md` (12-phase, thesis-calibrated). NOT for single-stage tasks → single guides.

## The 23 Stages, 8 Phase Groups

```
Group A: Research Scoping            Group E: Experiment Execution
  1. TOPIC_INIT                        12. EXPERIMENT_RUN
  2. PROBLEM_DECOMPOSE                 13. ITERATIVE_REFINE    ← self-healing
Group B: Literature Discovery        Group F: Analysis & Decision
  3. SEARCH_STRATEGY                   14. RESULT_ANALYSIS     ← multi-agent
  4. LITERATURE_COLLECT  ← real APIs   15. RESEARCH_DECISION   ← PIVOT/REFINE
  5. LITERATURE_SCREEN   [GATE]
  6. KNOWLEDGE_EXTRACT               Group G: Paper Writing
                                        16. PAPER_OUTLINE
Group C: Knowledge Synthesis           17. PAPER_DRAFT
  7. SYNTHESIS                          18. PEER_REVIEW         ← evidence audit
  8. HYPOTHESIS_GEN     ← debate        19. PAPER_REVISION
                                      Group H: Finalization
Group D: Experiment Design             20. QUALITY_GATE        [GATE]
  9. EXPERIMENT_DESIGN [GATE]           21. ARCHIVE_RUN
 10. CODE_GENERATION                    22. EXPORT_WITH_CHARTS
 11. RESOURCE_PLANNING                  23. CITATION_VERIFICATION (4-layer)
```

**Gate stages (require approval; `--auto-approve` bypasses in the original tool — in this suite, ask the user):**
- **Stage 5** — Literature Screen: validates collected literature quality before knowledge extraction
- **Stage 9** — Experiment Design: validates the experiment protocol before any code generation
- **Stage 20** — Quality Gate: validates overall paper quality before final export

## Stage Behaviors

| Mechanism | Behavior |
|---|---|
| **Self-healing execution** (13) | Failed experiment code is diagnosed and repaired iteratively (validator checks AST/security/imports; up to 10 repair cycles in the original). Still failing → fall back to simulated mode or stop honestly. |
| **Multi-agent debate** (8, 14, 18) | Hypothesis generation, result analysis, and peer review each run as structured multi-perspective debate, not a single pass. |
| **PIVOT / REFINE decision** (15) | After analysis: PROCEED (hypothesis supported) / REFINE (ambiguous — tweak params, re-run) / PIVOT (contradicted — new direction, artifacts auto-versioned). |
| **Evidence-audited review** (18) | Peer review checks methodology-evidence consistency, not just prose quality. |
| **4-layer citation verification** (23) | arXiv + CrossRef + DataCite + LLM relevance cross-check; fabricated citations are killed, not repaired. Auto-prune `references.bib` to match inline citations exactly. |

## Experiment Modes (Stage 12-13)

| Mode | What runs | Use when |
|---|---|---|
| `simulated` | LLM generates synthetic results — no code execution | Fast drafts, no environment available; **must be disclosed in the paper** |
| `sandbox` | Generated code executes locally (Python env, GPU/MPS/CPU auto-detected) | Default for real results |
| `ssh_remote` | Executes on a remote GPU server via SSH | Heavy experiments |

**Domain-specialist executors (v0.5.0)** — the pipeline auto-selects by research domain: high-energy physics (ColliderAgent: Lagrangian → FeynRules → MadGraph5 → Delphes), biology (COBRApy genome-scale metabolic modelling), statistics (simulation-study agent), generic Docker executor for chemistry/materials.

## Human-in-the-Loop System (v0.4.0)

Six intervention modes, from full autonomy to step-by-step:

| Mode | Behavior |
|---|---|
| `full-auto` | No human intervention |
| `gate-only` | Pause at gates 5/9/20 only |
| `checkpoint` | Pause at every stage boundary |
| `step-by-step` | Human approves every action |
| `co-pilot` | Deep collaboration: **Idea Workshop** (hypothesis co-creation), **Baseline Navigator** (experiment design review), **Paper Co-Writer** (collaborative drafting) |
| `custom` | Per-stage policy configuration |

Plus: **SmartPause** (confidence-driven dynamic intervention), **ALHF** intervention learning, cost budget guardrails, pipeline branching for parallel hypothesis exploration.

## Quality Infrastructure

- **Sentinel watchdog** (background): NaN/Inf detection, paper-evidence consistency, citation relevance scoring, anti-fabrication guard
- **Claim verification**: inline fact-checking — extracts claims from AI text, cross-references collected literature, flags ungrounded citations and fabricated numbers
- **Anti-fabrication system**: VerifiedRegistry — every number must trace to a registered source or experiment artifact
- **Self-learning**: lessons extracted per run (decision rationale, runtime warnings, metric anomalies), 30-day time-decay; future runs learn from past mistakes (MetaClaw cross-run bridge, opt-in)
- **Knowledge base**: each run builds structured KB across 6 categories (decisions, experiments, findings, literature, questions, reviews)
- **ARC-Bench**: 55-topic open-ended autonomous-research benchmark (ML 25, HEP 10, quantum 10, biology 7, statistics 3) — manifests + rubrics for graded scoring

## Deliverables (per run)

| Artifact | Content |
|---|---|
| `paper_draft.md` | Full paper (Introduction, Related Work, Method, Experiments, Results, Conclusion) |
| `paper.tex` | Conference-ready LaTeX (NeurIPS / ICLR / ICML templates) |
| `references.bib` | Real BibTeX from OpenAlex / Semantic Scholar / arXiv, pruned to inline citations |
| `verification_report.json` | 4-layer citation integrity + relevance verification |
| `experiment runs/` | Generated code + sandbox results + structured JSON metrics |
| `charts/` | Condition comparison charts with error bars and confidence intervals |
| `reviews.md` | Multi-agent peer review with methodology-evidence consistency checks |
| `evolution/` | Self-learning lessons extracted from the run |
| `deliverables/` | All final outputs in one Overleaf-ready folder |

## Executing the 23 Stages in This Suite — Stage-to-Guide Mapping

Run stages in order; load the mapped guide at each stage; honor gates and loops.

| Stage(s) | Suite guide | Notes |
|---|---|---|
| 1-2 | `idea-generation.md` | Topic → sub-problems → RQ brief |
| 3-6 | `paper-discovery.md` → `paper-reading.md` | Real searches only (PubMed/Semantic Scholar/arXiv/OpenAlex); GATE@5: present the screened list |
| 7-8 | `literature-review.md` + `idea-generation.md` | Synthesis matrix; hypothesis debate (argue for/against before committing) |
| 9-11 | `experiment-research.md` | GATE@9: experiment protocol approval; code with fixed seeds, structured metrics output |
| 12-13 | `experiment-research.md` | Local execution + self-healing (max 5 repair cycles in this suite, then honest stop or simulated fallback) |
| 14-15 | `experiment-research.md` § analysis | Multi-perspective analysis; PROCEED/REFINE/PIVOT (max 1 PIVOT, then ask the user) |
| 16-17 | `paper-writing.md` + `paper-sections.md` | Outline with claim-evidence table; draft Method → Experiments → Related Work → Intro → Conclusion → Abstract |
| 18-19 | `peer-review.md` + `rebuttal-revision.md` | Panel review with evidence audit; revise per roadmap |
| 20-23 | `paper-verification.md` + `citations.md` + `publishing.md` | GATE@20: quality verdict; archive artifacts; LaTeX + charts; 4-layer citation verification |

Anti-fabrication sentinel and claim verification run continuously from stage 9 onward (see `full-pipeline.md` Phase 5 for the suite-adapted rules — same policy applies here).

## Running the Original Tool (external)

When the user wants the real thing — its own sandbox, domain agents, CLI, resume support:

```bash
git clone https://github.com/aiming-lab/AutoResearchClaw.git
cd AutoResearchClaw
python3 -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -e .
researchclaw setup    # interactive: checks Docker/LaTeX
researchclaw init     # creates config.arc.yaml
researchclaw run --topic "Your research idea" --auto-approve        # fully autonomous
researchclaw run --topic "Your research idea" --mode co-pilot       # collaborative
```

Requires Python 3.11+, an LLM API key (or an ACP-compatible agent: Claude Code, Codex CLI, Copilot CLI, Gemini CLI, Kimi CLI), optionally Docker and LaTeX. Output lands in `artifacts/rc-<timestamp>-<hash>/deliverables/`. This suite does not bundle the tool — this section is the pointer.

## Differences from `full-pipeline.md`

| | `full-pipeline.md` (12 phases) | This guide (23 stages) |
|---|---|---|
| Calibrated for | Theses (incl. 本科毕设), course papers | Research papers with original experiments |
| Literature synthesis | Core deliverable | Input to hypothesis generation |
| Experiments | Optional phase | Central phases (9-15) with domain executors |
| Decision loop | Fixed 8→9→10 review loop | PIVOT/REFINE + branching exploration |
| Default output | DOCX (school template) | Conference LaTeX (NeurIPS/ICML/ICLR) |

Both pipelines share the same anti-fabrication, verification, and citation-kill policies. When in doubt for a degree thesis, use `full-pipeline.md`; for an autonomous research run targeting a conference submission, use this one.

## Source

- Project: https://github.com/aiming-lab/AutoResearchClaw (MIT © aiming-lab)
- Paper: *AutoResearchClaw: Self-Reinforcing Autonomous Research with Human-AI Collaboration* — https://arxiv.org/abs/2605.20025
- Benchmark: ARC-Bench — https://huggingface.co/datasets/AIMING-Lab-UNC/ARC-Bench
- This guide preserves the v0.5.0 pipeline design (stages, gates, modes, watchdog, self-learning) in suite-executable form; the original project is not bundled — see "Running the Original Tool".
