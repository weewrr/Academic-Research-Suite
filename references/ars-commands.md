# ARS Commands — Index

Compact index of the 16 `ars-*` slash commands (bundled in this suite at `commands/ars/`).
Most commands trigger a mode of an ARS skill via `scripts/academic-research-skills/MODE_REGISTRY.md`; three
dispatch a Python script directly. "Fidelity spectrum" and "oversight" tags are the
upstream ARS pipeline's output-trust classification (how strictly output follows sources,
and how much human review the mode assumes).

## When to Use

- End-to-end paper project → `ars-full`.
- Early-stage planning (dialogue, outline, abstract) → `ars-plan`, `ars-outline`, `ars-abstract`.
- Literature work → `ars-3w` (quick comparison scan) or `ars-lit-review` (paper-format section).
- Compliance and citation hygiene → `ars-citation-check`, `ars-disclosure`, `ars-cache-invalidate`.
- Review and revision cycle → `ars-reviewer`, `ars-revision`, `ars-revision-coach`, `ars-rebuttal-audit`.
- Reading-state provenance → `ars-mark-read`, `ars-unmark-read`.
- Output format conversion → `ars-format-convert`.

## Workflow

Typical pipeline ordering (mirrors the 10-stage `academic-pipeline` orchestration that
`ars-full` runs): deep-research (`ars-3w` / deep-research `lit-review`) → planning
(`ars-plan` → `ars-outline` → `ars-abstract`) → writing (academic-paper) → integrity
(`ars-citation-check`, `ars-disclosure`) → review (`ars-reviewer`) → revision
(`ars-revision` or `ars-revision-coach`) → re-review → final integrity → finalize
(`ars-format-convert`). Reading attestations (`ars-mark-read` / `ars-unmark-read`) and
cache invalidation (`ars-cache-invalidate`) are maintenance commands usable at any gate.
Standalone invocations (e.g. `ars-rebuttal-audit`) run outside the pipeline and do not
emit Schema 11 / Material Passport / verified status.

## Techniques — Per-Command Reference

### ars-full
- **Purpose:** Run the complete academic research workflow — research → write → review → revise → finalize.
- **When to use:** A full paper project from scratch through the whole pipeline.
- **Flags/args:** None; passes through to the orchestrator. Triggers the `academic-pipeline` skill (10-stage orchestration; the orchestrator has no named mode of its own).
- **Scripts:** None (delegates to `scripts/academic-research-skills/academic-pipeline/SKILL.md`; mode ref `scripts/academic-research-skills/MODE_REGISTRY.md` § academic-pipeline).

### ars-plan
- **Purpose:** Socratic chapter-by-chapter planning producing a Chapter Plan + INSIGHT collection.
- **When to use:** Earliest stage; interactive dialogue with the user before any outline exists.
- **Tags:** Originality-spectrum, very-high oversight.
- **Scripts:** None (academic-paper `plan` mode).

### ars-outline
- **Purpose:** Detailed paper outline with evidence map, no full draft.
- **When to use:** After planning, before drafting; need structure plus evidence mapping only.
- **Tags:** Balanced spectrum, high oversight.
- **Scripts:** None (academic-paper `outline-only` mode).

### ars-abstract
- **Purpose:** Bilingual (zh-TW + EN) abstract plus keywords.
- **When to use:** Abstract-only deliverable; carries the v3.6.7 `report_compiler_agent` PATTERN PROTECTION layer when invoked through the pipeline.
- **Tags:** Fidelity spectrum, medium oversight.
- **Scripts:** None (academic-paper `abstract-only` mode).

### ars-3w
- **Purpose:** WHY / HOW / WHAT paper comparison — compact shortlist plus cross-paper synthesis (common WHY, divergent HOW, strongest WHAT, unresolved gap).
- **When to use:** Quick deep-research scan; lighter than a full literature review. Escalate to `lit-review` / `systematic-review` for full coverage.
- **Tags:** Fidelity spectrum, low oversight.
- **Scripts:** None (deep-research `three-way-scan` mode).

### ars-lit-review
- **Purpose:** Annotated bibliography rendered as a literature-review section (paper format).
- **When to use:** The writing-side lit review inside the paper. For the upstream research-side review (annotated bibliography + synthesis report) prefer deep-research `lit-review` mode instead.
- **Tags:** Fidelity spectrum, medium oversight.
- **Scripts:** None (academic-paper `lit-review` mode).

### ars-citation-check
- **Purpose:** Citation error report — missing references, mismatched in-text citations, format errors.
- **When to use:** Pre-submission citation hygiene; no content rewriting.
- **Tags:** Fidelity spectrum, low oversight.
- **Scripts:** None (academic-paper `citation-check` mode).

### ars-format-convert
- **Purpose:** Convert a paper between LaTeX, DOCX (via Pandoc), PDF, or Markdown; convert citation styles between major formats.
- **When to use:** Deliverable needs a different format or citation style.
- **Tags:** Fidelity spectrum, low oversight.
- **Scripts:** None directly (academic-paper `format-convert` mode; conversion uses Pandoc upstream).

### ars-disclosure
- **Purpose:** Venue applicability/status bundle (or `--policy-anchor` render) for AI/disclosure policies.
- **When to use:** Checking whether the target venue requires an AI-use disclosure. Agent 9 must load `academic-paper/references/disclosure_mode_protocol.md` before rendering — the generic formatter disclosure is not a fallback. Default venue path returns `REQUIRED`, `ACTION_ONLY`, `NOT_REQUIRED`, or `UNKNOWN` plus an explicit typed halt status. Supports 15 policy targets: ICLR / NeurIPS / Nature / Science / ACL / EMNLP plus medical-publishing targets (ICMJE / NEJM / The Lancet / JAMA / BMJ / PLOS / Frontiers / publisher-wide 中华护理杂志社 / journal-level 国际眼科杂志).
- **Key flags:** `--policy-anchor` selects the separate anchor-specific renderer.
- **Tags:** Fidelity spectrum, low oversight.
- **Scripts:** None (academic-paper `disclosure` mode).

### ars-reviewer
- **Purpose:** Simulated peer-review panel on the current draft.
- **When to use:** Review before/after revision; honor explicit alternate modes when present: `quick`, `methodology-focus`, `re-review`, `guided`, `calibration`.
- **Notes:** Runs on the inherited session model — the v3.7.0 `opus` frontmatter floor was retired in the 2026-06 harness pass (a stronger session model is never silently downgraded).
- **Scripts:** None (academic-paper-reviewer `full` mode).

### ars-revision
- **Purpose:** Revised draft plus point-by-point response-to-reviewers.
- **When to use:** Reviewer comments exist and the actual revision should be written.
- **Tags:** Fidelity spectrum, high oversight.
- **Scripts:** None (academic-paper `revision` mode).

### ars-revision-coach
- **Purpose:** Revision Roadmap + Response Letter skeleton (or the #668 committee-correspondence variant) without writing the revision.
- **When to use:** Reviewer comments present but revision not yet written. Ordinary comments produce the roadmap + response skeleton. The committee-correspondence variant applies **if and only if** the user explicitly identifies a real committee or institutional review office: preserve the UTF-8 source, emit the separate concern tracker and placeholder response skeleton, and run its deterministic completeness checker. Never infer committee authority from tone; never emit priority, severity, determination, or Schema 11 on that branch.
- **Notes:** Runs on the inherited session model (opus floor retired, as above).
- **Scripts:** None (academic-paper `revision-coach` mode).

### ars-rebuttal-audit
- **Purpose:** Advisory QA of an **existing** rebuttal/response draft against reviewer comments (per-comment coverage + gaps + risk flags).
- **When to use:** Both reviewer comments AND a rebuttal draft exist. If only reviewer comments are present, use `ars-revision-coach` instead. Does NOT generate a new response and does NOT emit Schema 11 / Material Passport / verified status (standalone invocation).
- **Tags:** Fidelity spectrum, low oversight.
- **Scripts:** None (academic-paper `rebuttal-audit` mode).

### ars-mark-read
- **Purpose:** Record the user's `USER_ATTESTED_READ` declaration for the source(s) backing one or more citation keys. A user statement, not independent evidence of reading. A finalizer may promote `<!--ref:slug LOW-WARN-->` to `<!--ref:slug ok-->` only when the declared scope covers that citation's anchor.
- **When to use:** The user attests having read specific sources; needed to lift reading warnings at finalization.
- **Key flags:** `--scope {full_text,sections,abstract_only,toc_only,unknown}` (required for every new mark; declaration-only — pass through what the user states, never infer); `--locator "<text>"` (repeatable, requires `--scope sections`; page coverage requires an explicit `page`, `p.`, or `pp.` locator — bare numbers and `section <n>` never count); `--note "<text>"` (requires `--scope`). The dispatching agent substitutes the active Material Passport path for `--passport-path "<path>"` (quoting preserved for paths with spaces). Validation: citation_key must exist in `literature_corpus[]` (else `[ARS-MARK-READ ERROR: ...]` and refuse to write) plus 4 fail-fast environment checks; append-only write per §3.6 firm rule 3 into the session-scoped peer file `<passport-stem>_human_read_log.yaml`.
- **Scripts:** `python3 scripts/academic-research-skills/scripts/ars_mark_read.py $ARGUMENTS --passport-path "<path>"`; finalizer-side resolution by `scripts/academic-research-skills/scripts/human_read_attestation_resolver.py` (deterministic, transient routing decision, not a persisted audit receipt).

### ars-unmark-read
- **Purpose:** Rescind a previously recorded `USER_ATTESTED_READ` declaration.
- **When to use:** The user withdraws a reading claim. The read-log is append-only: rescind writes `rescinded_at: <ISO 8601>` on the matching entry (never deletes), so audit replay can reconstruct the signal trajectory. The next finalizer pass demotes coverage-dependent `<!--ref:slug ok-->` back to `<!--ref:slug LOW-WARN-->`.
- **Key flags:** `--passport-path "<path>"` (substituted as above); requires the citation key to exist in `literature_corpus[]` AND have an unrescinded prior mark — hard-fails otherwise.
- **Scripts:** `python3 scripts/academic-research-skills/scripts/ars_mark_read.py $ARGUMENTS --passport-path "<path>" --unmark`.

### ars-cache-invalidate
- **Purpose:** Drop all cached verification entries for one citation key so the next pipeline run re-verifies it live against Crossref / OpenAlex / Semantic Scholar / arXiv.
- **When to use:** A citation's metadata changed (e.g. a preprint gained a published DOI) or a prior verification looks wrong.
- **Key flags/args:** The citation key as the argument. Cache (spec v3.11 #182 Delta 2) is a local SQLite store at `~/.cache/ars/verification.db` (override: `ARS_VERIFICATION_CACHE_PATH`), keyed by `(citation_key, resolver_name, query_form)` with a 90-day TTL. Removes every cached entry for the key (all resolvers, all query forms); other citations untouched; idempotent no-op on empty. Invalidation cascade (#541, unconditional): next gate regenerates the verification summary and re-runs Phase E audit verdicts for claims citing it. Stale-entry advisory at gates: `ARS_CACHE_STALE_ADVISORY_DAYS` (default 30); opt-in live re-verification via `ARS_CACHE_REVALIDATE=1`. To wipe the entire cache (e.g. after a systemic resolver bug): `rm ~/.cache/ars/verification.db` — recreated empty on the next run.
- **Scripts:** `python3 scripts/academic-research-skills/scripts/ars_cache_invalidate.py $ARGUMENTS`.

## Scripts & Resources

Direct-dispatch scripts (Python 3, run with `python3`; bundled in this suite
under `scripts/academic-research-skills/scripts/`):

| Script | Invoked by |
|---|---|
| `scripts/academic-research-skills/scripts/ars_mark_read.py` | `ars-mark-read` (and `ars-unmark-read` with `--unmark`) |
| `scripts/academic-research-skills/scripts/ars_cache_invalidate.py` | `ars-cache-invalidate` |
| `scripts/academic-research-skills/scripts/human_read_attestation_resolver.py` | Finalizer passes resolving mark-read attestations |

Mode/skill references used by the command files (bundled in this suite under
`scripts/academic-research-skills/`):
`MODE_REGISTRY.md` (§ academic-paper, § academic-paper-reviewer, § deep-research,
§ academic-pipeline); skill entries `academic-paper/SKILL.md`,
`academic-paper-reviewer/SKILL.md`, `deep-research/SKILL.md`,
`academic-pipeline/SKILL.md`; design specs
`docs/design/2026-05-21-v3.10-182-promote-citation-gate-spec.md` §2 Delta 2
(cache) and `docs/design/2026-04-30-ars-v3.6.8-trust-provenance-and-drift-transparency-spec.md`
§3.6 + Step 7 (reading attestations); protocol
`academic-paper/references/disclosure_mode_protocol.md` (disclosure rendering).
