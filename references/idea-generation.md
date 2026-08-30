# Idea Generation & Hypothesis Formation — Consolidated Guide

Merged from two complementary methodologies: cross-paper research idea
generation (ResearchClaw idea-generator) and evidence-bounded scientific
hypothesis formation (claude-scientific-writer hypothesis-generation). The
first turns a read-paper corpus into actionable research proposals; the
second turns an observation into transparent, testable, rival-aware research
plans. Use them together or independently.

## When to Use

- "给我一些研究灵感" / "idea generator" / "跨论文分析" / "research ideas" —
  generate 3-5 actionable research ideas from the papers the user has read.
- Turning an observation, anomaly, or preliminary finding into candidate
  hypotheses, rival explanations, and a testable analysis plan.
- Framing a research question (PICO/PECO-style), declaring claim types
  (descriptive / associational / predictive / causal / mechanistic), or
  planning discriminating predictions before data analysis.
- Preparing a preregistration-ready plan or checking a draft plan for HARKing
  (hypothesizing after results are known).

Do NOT use for: paper writing (see `paper-writing.md`), literature discovery
or reading-list management (see `reading-list-profile.md`).

## Workflow

### Stage 0 — Load the research profile (always first, from ResearchClaw)

Load the user's research profile before running any capability. If it does
not exist, use defaults silently and mention at the end that the user can
customize by updating their research profile. Fields: `research_direction`
(free-text focus), `seed_papers` (gold-standard arXiv IDs), `keywords`
(interest topics), `whitelist_authors` (researchers to prioritize),
`learned_preferences.accept` / `.reject` (liked / disliked topics). See
`reading-list-profile.md` in this skill for the full profile schema.

### Stage 1 — Gather the corpus (from ResearchClaw)

1. Load the reading list data file; filter papers with status `done` or
   `reading` (papers the user has actually engaged with).
2. For each paper, load its note (from the `note_link` field) or re-fetch the
   abstract.
3. Extract: title, method summary, results, takeaway, tags.
4. If fewer than 3 papers, inform the user that deep-reading a few papers
   first will improve results, then proceed with what exists.

### Stage 2 — Freeze the observation (from claude-scientific-writer)

Before interpreting, record: the measurement or source; population, system,
place, and time; unit of observation and unit of analysis; uncertainty,
missingness, exclusions, preprocessing; and whether the pattern was expected,
exploratory, or selected after viewing results. Use "reported", "observed", or
"associated" — not causal language — unless a causal design justifies it.

### Stage 3 — Identify gaps / generate rivals

Two complementary gap analyses:

- **Research-gap taxonomy (from ResearchClaw)**:
  - Recurring unsolved problems — limitations mentioned across multiple
    papers.
  - Methodology mismatches — technique A works in domain X but is untried in
    domain Y.
  - Evaluation gaps — common benchmarks none of the papers tackle.
  - Combination opportunities — two methods from different papers that could
    combine synergistically.
  - Scaling questions — results that hold at small scale but are unvalidated
    at large scale.
- **Rival-explanation classes (from claude-scientific-writer)** — generate
  candidates from genuinely different explanatory classes: proposed
  mechanism; measurement or processing artifact; confounding or common cause;
  selection or attrition; conditioning on a collider; reverse causation;
  temporal/contextual/boundary-condition differences; stochastic variation;
  competing mechanisms at another scale. Generate an initial rival set
  independently before AI-assisted expansion to reduce anchoring, keep every
  candidate labeled `candidate`, and do not force false symmetry.

### Stage 4 — Frame the research question (from claude-scientific-writer)

Choose a framework only when it fits: PICO/PICOT for intervention questions
(population, intervention, comparator, outcome, time); PECO for exposure
questions; population–index test–reference standard–target condition for
diagnostic accuracy; population–prognostic factor–outcome–time for prognosis;
a domain-specific construct–context–outcome frame for qualitative or
theoretical work. PICO is not universal. Refine with FINER (Feasible,
Interesting, Novel, Ethical, Relevant) as a mnemonic, not a scoring system;
treat "Novel" as unresolved until a documented search and expert review
support it.

### Stage 5 — Establish a dated evidence boundary (from claude-scientific-writer)

Search before making literature-dependent statements. Record: search date and
cutoff; databases, queries, filters, screening boundary; included/excluded
source types; sources supporting, challenging, or contextualizing each claim;
known access, language, and time limitations. A search establishes what was
searched, not universal absence — say "not located within the documented
search boundary", never "no prior work exists". Never claim novelty because a
quick search found nothing.

### Stage 6 — Declare claim type and estimand (from claude-scientific-writer)

Classify each target as descriptive, associational, predictive, causal, or
mechanistic. For a causal target, define before analysis: target population
or system; intervention/exposure and comparator; outcome and time horizon;
population-level summary; treatment versions and intercurrent-event handling;
identification assumptions and target-trial analogue. Document confounding,
selection, collider, measurement, and reverse-causation risks separately.

### Stage 7 — Generate the output

- **Research ideas (from ResearchClaw)** — produce 3-5 ideas, each with the
  proposal template below.
- **Hypothesis record (from claude-scientific-writer)** — derive
  discriminating predictions for every candidate: (1) conditions and boundary
  conditions; (2) observable and measurement; (3) expected pattern and
  uncertainty; (4) a result incompatible with the candidate under declared
  assumptions; (5) contrast with at least one rival; (6) indeterminate
  outcomes and what would be learned. Prefer tests where rivals predict
  meaningfully different outcomes; add positive, procedural, and negative
  controls when scientifically appropriate (a negative control must be
  incapable of operating through the target mechanism while sharing relevant
  bias pathways).

### Stage 8 — Prevent HARKing and plan updating (from claude-scientific-writer)

Before accessing target outcomes, timestamp the question, candidates,
predictions, outcomes, exclusions, transformations, analysis, multiplicity,
missing-data plan, and stopping rule. Afterwards: label data-dependent ideas
exploratory; preserve and report planned analyses; list deviations with date,
rationale, decider, and expected impact; never rewrite an observed pattern as
an a priori prediction. Distinguish reproducibility (same data/code ->
consistent results) from replicability (new data for the same question ->
consistent results). Update candidate status when contrary, null, or
replication evidence arrives; do not hide negative results.

## Techniques

### Keep the objects distinct (from claude-scientific-writer)

| Object | Meaning |
|--------|---------|
| Observation | What was measured, with provenance and uncertainty |
| Research question | The answerable question that defines scope |
| Hypothesis | A candidate explanatory or relational proposition |
| Mechanism | The proposed process connecting conditions to outcome |
| Causal estimand | The precisely defined causal contrast to estimate |
| Prediction | An observable implication derived before checking the result |
| Alternative explanation | A rival account, including bias or non-causal accounts |
| Null hypothesis | A specified no-effect model used by an analysis |
| Negative control | A control expected not to operate through the proposed mechanism |
| Operationalization | How a construct becomes a variable or measurement |
| Analysis plan | Prespecified transformations, models, contrasts, decision rules |
| Evidence | Observations bearing on a claim; never the claim itself |

Do not collapse these labels: a mechanistic story is not a prediction; a
prediction is not evidence; rejecting one null does not prove a mechanism;
supporting one candidate does not eliminate unconsidered rivals.

### Research idea proposal template (from ResearchClaw)

```
💡 Research Idea {N}: **{CATCHY_TITLE}**

📋 Problem Statement — 1-2 sentences: what specific gap or challenge?
🔧 Proposed Approach — 2-3 sentences: how to tackle it; the key insight
📚 Related Papers — paper title + how it connects (2+)
🎯 Feasibility Assessment:
   - Dataset: what data is needed / available
   - Compute: estimated requirements
   - Timeline: rough estimate for a first experiment
   - Risk: the main technical risk
⭐ Why Now? — 1 sentence on timeliness / tractability today
```

### Safety and integrity boundaries (from claude-scientific-writer)

- Never present a hypothesis, mechanism, causal effect, citation, or apparent
  pattern as established evidence.
- Never infer causation from association, temporal order, predictive
  accuracy, or model output.
- Never fabricate sources, identifiers, search coverage, data, results, or
  preregistrations; never automatically score, rank, select, accept, or reject
  scientific hypotheses.
- Before using unpublished, sensitive, personal, or proprietary material,
  confirm authorization and applicable policies; keep it local unless an
  authorized human approves a named external destination; stop at the
  appropriate ethics / biosafety / data-governance / regulatory gate.
- The accountable human must verify every citation and source-to-claim link,
  domain plausibility, causal assumptions, and all AI-assisted ideas — AI can
  confabulate citations, anchor reasoning, and homogenize candidate sets.

### Error handling (from ResearchClaw)

- Corpus too small (<3 engaged papers): proceed but note the limitation.
- Paper notes unavailable: fall back to fetching abstracts from arXiv.
- User provides DOI/PDF instead of arXiv ID: try extracting the arXiv ID from
  the DOI or searching arXiv by title.

## Scripts & Resources

- ResearchClaw idea-generator source (profile loading, gap taxonomy, proposal
  template): `scripts/ResearchClaw/skills/idea-generator/`.
- claude-scientific-writer hypothesis-generation source with local
  deterministic CLIs and asset templates:
  `scripts/claude-scientific-writer/hypothesis-generation/` —
  `scripts/claude-scientific-writer/hypothesis-generation/scripts/validate_hypothesis_schema.py` (hypothesis-record schema),
  `scripts/claude-scientific-writer/hypothesis-generation/scripts/check_operationalization.py` (measurement checklist),
  `scripts/claude-scientific-writer/hypothesis-generation/scripts/validate_prediction_matrix.py` (prediction/rival matrix),
  `scripts/claude-scientific-writer/hypothesis-generation/scripts/lint_causal_claims.py` (claim-language lint),
  `scripts/claude-scientific-writer/hypothesis-generation/scripts/check_falsification_controls.py` (falsification/controls),
  `scripts/claude-scientific-writer/hypothesis-generation/scripts/audit_evidence_ledger.py` (evidence/source audit),
  `scripts/claude-scientific-writer/hypothesis-generation/scripts/generate_preregistration_scaffold.py` (preregistration scaffold);
  asset templates under `scripts/claude-scientific-writer/hypothesis-generation/assets/`
  (hypothesis_record, operationalization,
  prediction_rival_matrix, falsification_controls, evidence_ledger,
  search_boundary, preregistration_scaffold).
- Reference docs under `scripts/claude-scientific-writer/hypothesis-generation/references/`:
  `concepts_and_workflow.md`,
  `hypothesis_quality_criteria.md`,
  `literature_search_strategies.md`,
  `causal_inference_and_claims.md`,
  `experimental_design_patterns.md`,
  `preregistration_and_open_science.md`,
  `ethics_safety_and_ai.md`, `tool_reference.md`.
- Companion guides in this skill: `reading-list-profile.md` (profile and
  corpus data), `paper-writing.md` (turning a selected idea into a paper).
