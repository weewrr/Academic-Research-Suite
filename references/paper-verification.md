# Paper Verification

Consolidated guide for verifying that a paper accurately reflects its code, data, and cited literature, and for critically evaluating the methodology and evidence behind its claims. Merges the paper-verification methodology (from phd-skills) with the scientific critical-thinking frameworks (from claude-scientific-writer) and the claim-audit verification tooling (from academic-research-skills).

## When to Use

- Verifying paper claims against code or data: "verify claims", "check numbers", "do the numbers match", "audit the paper", "cross-check results".
- Auditing numerical accuracy, terminology consistency, formula–code alignment, or code–paper alignment.
- Validating citation accuracy (BibTeX metadata, cited numbers, retraction status).
- Evaluating research methodology, experimental design validity, statistical validity, biases, confounders, or evidence quality (including GRADE and Cochrane Risk of Bias assessments).
- Assessing whether scientific claims and conclusions are supported before submission, or as a quality-control step during writing.

This is the most critical quality-control step in academic writing: assume nothing in the draft is correct until traced to a source.

## Workflow

1. **Read the full paper** (or the specified sections) and build an inventory of claims, numbers, terms, equations, and citations.
2. **Run each verification dimension** below, building a verification table per dimension.
3. **For each entry**, read the actual source (code output, result file, log, cited paper) and verify.
4. **Optionally run automated claim-audit tooling** (see Scripts & Resources) for claim–reference alignment, uncited-assertion detection, retraction status, and tortured-phrase screening.
5. **Produce a prioritized issue list** and a structured verification report.

### Dimension 1: Numerical accuracy audit (from phd-skills)

For every number in the paper (dataset sizes, metric values, percentages, counts):

1. **Extract** the number and its context from the source file (e.g., the `.tex`).
2. **Trace** it to its source: code output, result file, log, or tracking system.
3. **Verify** the value matches exactly — watch for rounding and percentage-vs-decimal confusion.
4. **Flag** any number that cannot be traced to a source.

Template:

```
| Paper claim | Location (.tex) | Source file/code | Source value | Match? |
|-------------|-----------------|------------------|--------------|--------|
| "13,999 frames" | abstract L3 | len(glob(labels/*.json)) | ? | ? |
| "4.2% improvement" | Table 2 | eval_results.json | ? | ? |
```

Common numerical errors: rounding inconsistencies (3.14 in text vs 3.1415 in table); stale numbers from earlier experiments not updated after re-runs; percentage vs absolute confusion; off-by-one in dataset counts (headers counted or not).

### Dimension 2: Terminology consistency audit (from phd-skills)

1. Extract all defined terms from the methods section.
2. Search for each term across ALL sections.
3. Flag inconsistent usage: same concept with different names ("tag head" vs "classification head"); same name with different meanings across sections; defined but never used, or used but never defined.

### Dimension 3: Code–paper alignment (from phd-skills)

For each method described in the paper: find the corresponding code (function, class, module), compare the description with the implementation, and check that algorithm steps match code flow, hyperparameters in text match config/code defaults, architecture descriptions match model code, loss functions in equations match loss code, and training procedures match training scripts.

Common mismatches: the paper describes an idealized version while the code has unmentioned edge cases; hyperparameters changed during development but the paper was not updated; the paper describes a method that was later modified or removed from the code.

### Dimension 4: Formula–code verification (from phd-skills)

For each equation: identify the equation and its variables, find the implementing code, map each mathematical operation to its code equivalent, and verify that summation bounds match loop bounds, division operations handle edge cases, normalization factors match, gradient flow matches (`detach`, `no_grad`), and reduction operations (mean vs sum) match.

### Dimension 5: Citation fact-checking protocol (from phd-skills)

For each citation:

1. Extract the claim and the cited paper.
2. Verify BibTeX metadata against DBLP: author names (exact spelling, correct order), paper title (exact, from the published version, not the preprint), venue and year.
3. For cited claims with specific numbers: locate the exact table/figure in the cited paper and verify the number matches; if it cannot be confirmed, suggest qualitative language instead.
4. Check for common errors: citing a preprint when a published version exists; wrong year (submission vs publication); author-name misspellings; citing a paper for a claim it does not actually make.

### Dimension 6: Critical evaluation of claims and evidence (from claude-scientific-writer)

Assess the seven capability areas — for each, ask the questions and record what the answers imply:

1. **Methodology critique** — design, controls, confounding; can the method answer the question asked?
2. **Bias detection** — selection, measurement, publication, and cognitive biases.
3. **Statistical analysis evaluation** — power, multiplicity, p-value misuse, effect sizes.
4. **Evidence quality assessment** — study hierarchy, replication, strength of inference (GRADE, Cochrane Risk of Bias).
5. **Logical fallacy identification** — the fallacies that recur in scientific argument.
6. **Research design guidance** — how to strengthen the design before data collection.
7. **Claim evaluation** — separating what was shown from what is being asserted.

### Output: prioritized issue list and report

- **HIGH**: incorrect numbers, wrong claims, missing citations.
- **MEDIUM**: terminology inconsistencies, stale but close numbers.
- **LOW**: minor formatting, optional improvements.

Structured verification report: (1) Summary — X issues found (Y high, Z medium, W low); (2) Numerical audit table; (3) Terminology issues with locations; (4) Code–paper mismatches; (5) Citation issues; (6) Suggested fixes — specific text replacements for each issue.

## Techniques

### Critique principles (from claude-scientific-writer)

- **Be constructive**: identify strengths as well as weaknesses; distinguish fatal flaws from minor limitations; all research has limitations.
- **Be specific**: point to instances ("Table 2 shows..."); quote problematic statements; reference the principle or standard violated.
- **Be proportionate**: match criticism severity to issue importance; consider whether issues affect primary conclusions.
- **Apply consistent standards**: same criteria across all studies; judge methodology, not results; acknowledge your own potential biases.
- **Consider context**: practical and ethical constraints; field-specific norms; exploratory vs confirmatory contexts.

### Structured feedback format (from claude-scientific-writer)

1. **Summary** — brief overview of what was evaluated.
2. **Strengths** — what was done well (important for credibility and learning).
3. **Concerns** — organized by severity: critical (threaten main conclusions), important (affect interpretation but not fatally), minor (worth noting, don't change conclusions).
4. **Specific recommendations** — actionable suggestions.
5. **Overall assessment** — balanced conclusion about evidence quality and what can be concluded.

When uncertain: acknowledge uncertainty ("This could be X or Y; additional information needed is Z"); ask clarifying questions ("Was [detail] done? This affects interpretation."); provide conditional assessments ("If X was done, then Y follows; if not, Z is a concern"); note what additional information would resolve the uncertainty.

### Core distinctions to maintain (from claude-scientific-writer)

- Data (what was observed) vs interpretation (what it means).
- Correlation vs causation.
- Statistical significance vs practical importance.
- Exploratory vs confirmatory findings.
- What is known vs what is uncertain.
- Evidence against a claim vs evidence for the null.

### Verification confidence calibration

Proportional confidence to evidence strength: a number verified against a result file is confirmed; a number with no traceable source is flagged, not assumed correct; a cited claim whose supporting table cannot be located is downgraded to qualitative language. Never mark something "verified" because it looks plausible or was checked in an earlier draft — verify against the current source of truth.

## Scripts & Resources

### ARS claim-audit and verification tooling (from academic-research-skills)

Available at `scripts/academic-research-skills/scripts/`:

| Script | What it does | How to invoke |
|---|---|---|
| `claim_audit_pipeline.py` | The §4 Step 1–6 claim–reference alignment audit pipeline. Library module (no CLI): `run_audit_pipeline(...)` audits each claim/citation pair with dependency-injected `retrieve_fn` / `judge_fn` and returns per-claim verdicts — SUPPORTED / UNSUPPORTED (with defect-stage hints: source_description, metadata, citation_anchor, synthesis_overclaim) / AMBIGUOUS / VIOLATED — plus anchorless, retrieval-failure, uncited-assertion, constraint-violation, and claim-drift entries. Production callers wire real retrieval/judge clients; rationale text is length-bounded for schema conformance. | Import and call `run_audit_pipeline()`; drive from the orchestrating layer. |
| `claim_audit_finalizer.py` | Claim-faithfulness finalizer: projects audit rows onto the 8-row matrix with annotation literals and severity tiers (`none` / `low_warn` / `med_warn` / `high_warn`), e.g. `[HIGH-WARN-CLAIM-NOT-SUPPORTED]`, `[HIGH-WARN-FABRICATED-REFERENCE]`, `[CLAIM-AUDIT-AMBIGUOUS]`, `[UNCITED-ASSERTION]`, `[LOW-WARN-CLAIM-DRIFT]`. `apply_finalizer()` reduces over the passport to a gate decision + reason list; `render_stage6_histogram()` renders the Stage 6 reflection histogram (≥5 completed entries). Does no file I/O — pure dict-in/dict-out. | Import `classify_claim_audit_result` / `apply_finalizer` / `render_stage6_histogram`. |
| `retraction_status.py` | Deterministic retraction-status resolver (issue #651). Consumes already-returned OpenAlex/Crossref metadata — performs **no network I/O** and never judges whether citing a retracted work is legitimate. Normalizes DOIs, classifies events (retraction / reinstatement / expression of concern / correction), and maintains a 30-day revalidation cache. | `python retraction_status.py input.json --output signal.json` (input is a JSON payload of the fetched metadata). |
| `tortured_phrase_screening.py` | Hermetic tortured-phrase risk-marker screening (issue #660). Consumes only explicitly named local inputs; never downloads a phrase list, invokes a model, judges authorship, or rewrites text — a deterministic match is a heuristic advisory only. Exit codes: 0 success, 1 fail-closed contract/replay error, 2 invocation error; snapshot failures still write an explicit degraded/not-checked artifact. | `python tortured_phrase_screening.py <subcommand>` with subcommands `validate-snapshot`, `scan-draft` (`--input`, `--artifact-id`, `--format markdown|latex`, `--checked-at`, `--recorded-at`, `--output`, optional `--snapshot`/`--snapshot-manifest`), `validate-draft`, `render-draft`, `enrich-passport`. |
| `uncited_assertion_detector.py` | D4-c uncited-assertion detector implementing a three-condition token rule: a sentence is an `uncited_assertion` candidate iff (1) it contains a quantifier or empirical verb (numbers, percentages, `most`/`several`/`two-thirds`, `showed`/`demonstrated`/`observed`/`proved`/`confirmed` — with a guard pass rejecting years, version triples, and section/figure numbers), (2) it has no `<!--ref:slug-->` marker, and (3) it is not definitional (`refers to`, `is defined as`, ...). The `detect_uncited_assertions()` wrapper supports an `adjacent_text` cross-sentence check so a marker on a neighboring clause filters the candidate. | Import `detect_uncited` / `detect_uncited_assertions`; output feeds `claim_audit_pipeline.run_audit_pipeline`'s `uncited_sentences` parameter. |
| `verification_cache.py` | Persistent SQLite-backed cache for the bibliographic resolvers so the same paper cited across drafts does not re-hit Crossref/OpenAlex/Semantic Scholar/arXiv each run. Keyed on `(citation_key, resolver_name, query_form)`; 90-day TTL (older = miss); WAL mode. | Import as a library. Cache path env: `ARS_VERIFICATION_CACHE_PATH` (default `~/.cache/ars/verification.db`); staleness advisory env: `ARS_CACHE_STALE_ADVISORY_DAYS` (default 30; entries older than this carry an advisory flag but remain hits). |

Shared constants, schemas, and contracts live under `scripts/academic-research-skills/shared/` (e.g., audit and passport JSON schemas referenced by the screening and finalizer modules).

### phd-skills paper-verification methodology

Source skill: `scripts/phd-skills/skills/paper-verification/` — the five verification dimensions and the audit table templates above.

### claude-scientific-writer scientific-critical-thinking references

Located at `scripts/claude-scientific-writer/scientific-critical-thinking/references/`:

- `core_capabilities.md` — the seven capability areas with questions and implications.
- `scientific_method.md` — scientific methodology, red flags in claims, causal inference standards.
- `common_biases.md` — taxonomy of cognitive, experimental, methodological, statistical, and analysis biases with detection/mitigation strategies.
- `statistical_pitfalls.md` — p-value misunderstandings, multiple comparisons, sample-size issues, effect-size mistakes, correlation/causation confusion, regression pitfalls, meta-analysis issues.
- `evidence_hierarchy.md` — evidence hierarchy, GRADE system, study quality criteria, decision frameworks.
- `logical_fallacies.md` — fallacies by type (causation, generalization, authority, relevance, structure, statistical) with detection strategies.
- `experimental_design.md` — comprehensive design checklist: questions, hypotheses, variables, sampling, blinding, randomization, controls, measurement, validity threats, reporting standards.

Load these references when detailed frameworks are needed; use grep to search them for specific topics.

For citation-specific verification tooling (Crossref/OpenAlex/Semantic Scholar/arXiv/Chinese-literature resolvers), see `references/citations.md`.
