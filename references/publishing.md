# Publishing, Venue Compliance & Manuscript Pipeline

Consolidated guide for producing and publishing research: the end-to-end academic paper pipeline (research → write → integrity → review → revise → finalize), venue-specific formatting compliance, LaTeX environment setup, and open-source code release. Merged from phd-skills (research-publishing, latex-setup), claude-scientific-writer (venue-templates), and academic-research-skills (academic-pipeline + ars commands).

## When to Use

- **Full paper workflow** — "academic pipeline", "research to paper", "full paper workflow", "end-to-end paper", "research-to-publication", "I want to write a research paper on X", "I already have a paper, help me review it", "I received reviewer comments, help me revise".
- **Single-stage writing tasks** — chapter planning, outline-only, abstract-only, or format conversion (see the ars command table below).
- **Venue formatting** — selecting an official template, checking current page limits or anonymity rules, adapting prose to a venue, inspecting a submission PDF's page count and fonts; preparing journal manuscripts, conference papers, research posters, and grant documents.
- **LaTeX setup** — "setup latex", "biber vs bibtex", "latex compilation error", "install latex packages", "venue template", "texlive setup".
- **Code release** — "publish code", "open source release", "reproducibility", "research repository", "code release", "prepare for publication".

## Workflow

### A. The 10-stage academic pipeline (from academic-research-skills)

An orchestrator coordinates the complete pipeline; it only detects stages, recommends modes, dispatches skills, manages transitions, and tracks state — substantive work belongs to the dispatched skills (deep-research, academic-paper, academic-paper-reviewer).

| Stage | Name | Skill / agent | Modes | Deliverables |
|-------|------|---------------|-------|--------------|
| 1 | RESEARCH | deep-research | socratic, full, quick | RQ Brief, Methodology, Bibliography, Synthesis |
| 2 | WRITE | academic-paper | plan, full | Paper Draft |
| 2.5 | INTEGRITY | integrity_verification_agent | pre-review | Integrity verification report + corrected paper |
| 3 | REVIEW | academic-paper-reviewer | full (incl. Devil's Advocate) | 5 review reports + Editorial Decision + Revision Roadmap |
| 4 | REVISE | academic-paper | revision | Revised Draft, Response to Reviewers |
| 3' | RE-REVIEW | academic-paper-reviewer | re-review | Verification review: revision checklist + residual issues |
| 4' | RE-REVISE | academic-paper | revision | Second revised draft (if needed) |
| 4.5 | FINAL INTEGRITY | integrity_verification_agent | final-check | Final verification report |
| 5 | FINALIZE | academic-paper | format-convert | Final paper (MD → DOCX via Pandoc → LaTeX → PDF) |
| 6 | PROCESS SUMMARY | orchestrator | auto | Paper creation process record (bilingual MD + PDF) |

**Entry-point detection** (mid-entry is supported from any stage):

- No materials → Stage 1; has research data → Stage 2; has paper draft → Stage 2.5; has verified paper → Stage 3; has review comments → Stage 4; has revised draft → Stage 3'; has final draft for formatting → Stage 5.
- Mid-entry cannot skip Stage 2.5 (integrity) unless the user supplies a prior verification report for unmodified content.
- If the user needs only a single function (search materials, check citations, convert format), skip the pipeline and trigger the corresponding skill directly — the pipeline is opt-in.

**State machine and gates:**

1. Stage 1 → confirm → Stage 2 → confirm → Stage 2.5.
2. Stage 2.5 PASS → Stage 3; FAIL → fix and re-verify (max 3 rounds, then a recorded user decision).
3. Stage 3 Accept → Stage 4.5; Minor/Major → Stage 4; Reject → Stage 2 or end.
4. Stage 4 → confirm → Stage 3'; Accept/Minor → Stage 4.5; Major → Stage 4' (max one re-revision round, then straight to 4.5 — no return to review).
5. Stage 4.5 must reach a recorded terminal resolution before Stage 5: PASS, or an explicit recorded user decision after the 3-round FAIL loop. Unresolved items are never silently dropped.
6. Stage 5 finalization: MD first, DOCX via Pandoc when available, then LaTeX, then PDF compiled from LaTeX (HTML-to-PDF is prohibited). Fonts: Times New Roman (English), Source Han Serif TC VF (Chinese), Courier New (monospace).
7. Stage 6 is optional (user may decline at the Stage 5 checkpoint); on delivery it requires a terminal acknowledgement before the pipeline is marked completed.

**Checkpoints (iron rule: after each stage, proactively prompt and wait for user confirmation):**

- FULL — first checkpoint, after integrity boundaries, and at final-deliverable acceptance: full deliverables list, metrics (word count vs target, references vs minimum, sections drafted), flagged issues, and options.
- SLIM — after 2+ consecutive "continue" responses on non-critical stages: one-line status + continue/pause prompt. After 4+ consecutive continues, re-insert a FULL checkpoint.
- MANDATORY — integrity FAILs, review decisions, and the Stage 5 entry gate: cannot be auto-skipped, even if the previous stage was perfect.

**Budget transparency:** at pipeline start, estimate token cost from paper length and mode, plus an interaction-count budget (revision loops, coaching rounds, integrity fix loops) — document round-trips compound corruption risk. Ask for user confirmation before Stage 1. The count is advisory; per-loop caps are the enforcement layer.

### B. Venue compliance, verification-first (from claude-scientific-writer)

Venue requirements are time-sensitive. Before giving exact page limits, deadlines, style-file names, anonymity rules, or required sections:

1. **Resolve the exact target**: venue or agency, year/cycle, track, document type (research article, short paper, R01, R21...), submission stage (initial / revision / camera-ready), and authoring format. Do not combine rules from similarly named venues or tracks.
2. **Open the official source**: author instructions, call, solicitation, NOFO, or policy guide. Record the source URL and the date checked.
3. **Capture a compliance note** in the working document, e.g.:

   ```text
   Target: ICML 2026 main track, initial submission
   Official source: https://icml.cc/Conferences/2026/AuthorInstructions
   Checked: 2026-07-20
   Main-text limit: 8 pages
   References/appendices: additional pages allowed in the same PDF
   Anonymity: required
   Official template: ICML 2026 style package linked by the author instructions
   ```

4. **Start from the official template**: download from the official source, keep class/style files unchanged, add content without overriding margins, fonts, spacing, or headers. Use a bundled scaffold only for drafting or when the official source explicitly permits it. Never infer a style-file name by changing the year in an old filename; never present a generic scaffold as an official template.
5. **Validate manually and mechanically**: page rules (main-text and total-file scope), font/margin/spacing/paper-size, anonymity and PDF metadata (no identity leaks in blind review), required statements/checklists/disclosures, figure/table placement and accessibility, reference and supplemental treatment, source-package and PDF requirements. Helper scripts can check page totals and embedded fonts, but cannot prove margin or font-size compliance — manual verification stays mandatory.

### C. LaTeX environment setup (from phd-skills)

1. **Detect current state** before installing anything: `which pdflatex/xelatex/lualatex`, `which biber && bibtex`, `which tlmgr`.
2. **Analyze the project**: document class; bibliography system (`\usepackage{biblatex}` → biber; `\usepackage{natbib}` or `\bibliographystyle{...}` → bibtex); required packages from all `\usepackage{...}` declarations; special requirements (TikZ, minted needs pygments, algorithm2e).
3. **Venue template detection**: if a venue is named, download the official template from the venue's website — not third-party mirrors — and note venue-specific compilation instructions (see the class table under Techniques).
4. **Install only what's missing**: on Ubuntu/Debian `texlive-base texlive-latex-recommended` plus targeted extras (`texlive-latex-extra`, `texlive-bibtex-extra biber`, `texlive-science`); on macOS `brew install --cask mactex` (or `basictex` + `tlmgr install <package>`). Prefer `tlmgr` for individual packages — apt packages are coarse-grained.
5. **Configure the compilation pipeline**:
   - bibtex: `pdflatex main.tex && bibtex main && pdflatex main.tex && pdflatex main.tex`
   - biber: `pdflatex main.tex && biber main && pdflatex main.tex && pdflatex main.tex`
6. **Verify**: full pipeline runs, PDF opens, bibliography entries appear, remaining warnings reviewed in the `.log`.

### D. Code release preparation (from phd-skills)

1. **Audit the repository**: scan for API keys/tokens/credentials, machine-specific hardcoded paths, internal URLs, and PII; list dependencies with pinned versions and flag proprietary/restricted/abandoned ones; identify dead code, debug artifacts, and scratch files.
2. **Apply the standard structure**: `README.md`, `LICENSE`, pinned `requirements.txt` / `pyproject.toml`, `src/`, `scripts/`, `configs/`, `data/` (sample data or download instructions), `checkpoints/` (download instructions, never actual weights), `results/`.
3. **Run the reproducibility checklist per experiment**: config matches paper hyperparameters; seeds set and documented; training command documented end-to-end; evaluation reproduces reported numbers; preprocessing scripted (not manual); hardware documented; dependencies pinned.
4. **Write the README** with: title + one-line description, paper link, visual, installation, quick start (inference on a single example, < 5 commands), training, evaluation, model zoo/checkpoints with expected metrics, BibTeX citation, license.
5. **Minimal cleanup only**: remove debugging prints and commented-out code; replace hardcoded paths with configurable ones; docstring public functions; do NOT refactor working code for style — it adds risk for no benefit.
6. **Pre-release test**: clone into a fresh directory, follow the README exactly, run quick start and evaluation, and check git history for sensitive information.

## Techniques

### ARS slash commands (from academic-research-skills)

Thin command entries that trigger specific skills/modes. Available as `commands/ars/*.md`:

| Command | Skill / mode | Produces | Oversight |
|---------|--------------|----------|-----------|
| `ars-full` | academic-pipeline (orchestrator) | Complete 10-stage workflow: research → write → integrity → review → revise → finalize | pipeline-managed |
| `ars-3w` | deep-research `three-way-scan` | Compact paper shortlist compared by WHY / HOW / WHAT plus cross-paper synthesis (common WHY, divergent HOW, strongest WHAT, unresolved gap); lighter than lit-review | low |
| `ars-plan` | academic-paper `plan` | Chapter Plan + INSIGHT collection through Socratic chapter-by-chapter dialogue | very high |
| `ars-outline` | academic-paper `outline-only` | Detailed paper outline with evidence map, no full draft | high |
| `ars-abstract` | academic-paper `abstract-only` | Bilingual (zh-TW + EN) abstract plus keywords | medium |
| `ars-format-convert` | academic-paper `format-convert` | Conversion between LaTeX, DOCX (Pandoc), PDF, Markdown; citation-style conversion | low |

### Integrity and review protocol details (from academic-research-skills)

- Stage 2.5 and Stage 4.5 run a 5-phase protocol: references → citation context → statistical data → originality → claims. Stage 4.5 performs a fresh from-scratch pass without relying on Stage 2.5 conclusions — revision may introduce new issues.
- Both integrity stages also run a 7-mode AI research failure checklist (citation hallucination, implementation bugs, hallucinated results, shortcut reliance, bug-as-insight, methodology fabrication, pipeline-level frame-lock). Suspected failures block; overrides require recorded user reasoning.
- Two-stage review: Stage 3 full review (Journal-Fit Reviewer + R1/R2/R3 + Devil's Advocate) → revision coaching → Stage 3' verification review focused on the revision response, under an evidence-before-persuasion contract (hash-bound input manifest, criteria commitment, evidence verdict, claim matching). Outcomes: Accept / Minor / Major / user-review-required deferral / fail-closed abort — never Reject.
- Early-stopping criterion for revision loops: suggest stopping only when no P0 issue remains, no unresolved decision-bearing regression remains, no criterion has a substantive status change requiring another revision, and the author has no outstanding required action. Hard cap: 2 full revision loops.
- Anti-patterns: skipping integrity checks; orchestrator doing substantive work; auto-advancing past MANDATORY checkpoints; quality degradation across stages (if Stage N output < Stage N−1, pause and reload core principles); silently dropping reviewer concerns (every concern gets an explicit status); re-verifying only known issues at Stage 4.5; inflating self-evaluation scores.
- External (human) reviewer feedback follows a 4-step workflow: Intake & Structuring → Strategic Revision Coaching → Revision & Response → Self-Verification.

### Venue class quick reference (from phd-skills)

Common venues and typical requirements — always re-verify against the current official source per the currency rule above:

| Venue | Class | Bib system | Notes |
|-------|-------|-----------|-------|
| CVPR/ECCV | Custom class file | bibtex | Usually provided in template |
| NeurIPS | neurips_20XX.sty | natbib + bibtex | Style file changes yearly |
| ICLR | iclr20XX_conference.sty | natbib + bibtex | OpenReview format |
| ACL/EMNLP | acl.cls | bibtex | ACL Anthology format |
| IEEE | IEEEtran.cls | bibtex | Column formatting specific |
| Springer | llncs.cls | bibtex or biblatex | Depends on series |

Common LaTeX compilation fixes: missing `.bib` → check `\bibliography{...}` path; undefined citations → run bibtex/biber + pdflatex twice; missing packages → install via tlmgr, not apt; font errors → `texlive-fonts-extra`; TikZ externalize errors → enable write18.

### License selection (from phd-skills)

| License | Commercial use | Attribution | Copyleft |
|---------|---------------|-------------|----------|
| MIT | Yes | Yes | No |
| Apache 2.0 | Yes | Yes | No (patent grant) |
| GPL 3.0 | Yes | Yes | Yes (derivative works) |
| CC BY 4.0 | Yes | Yes | No (for non-code) |
| CC BY-NC 4.0 | No | Yes | No (for non-code) |

Default recommendation: MIT for code, CC BY 4.0 for datasets/models.

### Bundled scaffolds and helpers (from claude-scientific-writer)

The venue-templates skill intentionally bundles only a small, explicit set — everything else requires an official external template:

- Journal/conference scaffolds: `nature_article.tex` (generic Nature-oriented, not official), `plos_one.tex`, `neurips_article.tex` (requires official `neurips_2026.sty`), three Elsevier `elsarticle` examples (numeric, num-names, author-year) with matching `.bst` files.
- Grant scaffolds: `nsf_proposal_template.tex` (planning scaffold; upload components separately), `nih_specific_aims.tex` (one-page Specific Aims). Use SciENcv and agency common forms for biosketches — do not recreate them in LaTeX.
- Poster scaffold: `beamerposter_academic.tex` (venue-agnostic; set dimensions from the event's current presenter instructions).

Helper scripts (Python 3.11+; LaTeX and Poppler optional for compilation and PDF inspection):

```bash
python scripts/query_template.py --list-all                 # list bundled templates
python scripts/query_template.py --venue NeurIPS --requirements
python scripts/customize_template.py --template nature_article.tex \
  --title "Title" --authors "A, B" --affiliations "Inst" --output my_paper.tex
python scripts/validate_format.py --file paper.pdf --venue icml-2026 \
  --content-pages 8 --check page-count,fonts
```

Review every replacement and compile before adding substantial content; user-provided text may need LaTeX escaping. `--content-pages` must be counted according to the official rule — the script does not infer where references or appendices begin.

### Final compliance checklist (from claude-scientific-writer)

- [ ] Exact venue, year/cycle, track, article type, and stage identified
- [ ] Official source URL recorded with date checked
- [ ] Official template or form used where required
- [ ] Page-limit scope understood, including excluded sections
- [ ] Required statements, checklists, and disclosures present
- [ ] Blind-review files and PDF metadata checked for identity leaks
- [ ] Figures and tables are legible and accessible
- [ ] References, appendices, and supplements follow current rules
- [ ] PDF and source package compile cleanly
- [ ] Submission portal preview reviewed before final submission

## Scripts & Resources

All paths relative to the skill root.

**phd-skills sources:**

- `scripts/phd-skills/skills/research-publishing/SKILL.md` — full repository-assessment and release methodology.
- `scripts/phd-skills/skills/latex-setup/SKILL.md` — full LaTeX environment setup guide with OS-specific installs.
- `scripts/phd-skills/scripts/latex_check.sh` — LaTeX compilation sanity checks (bash required).
- `scripts/phd-skills/scripts/citation_guard.sh` — citation/bibliography consistency guard (bash required).
- `scripts/phd-skills/scripts/jargon_scrub.sh` — prose readability pass (bash required).
- `scripts/phd-skills/scripts/visual_check.sh` — figure/visual sanity checks (bash required).

**claude-scientific-writer sources** (helper scripts require Python 3.11+):

- `scripts/claude-scientific-writer/venue-templates/SKILL.md` — verification-first workflow and mandatory currency rule.
- `scripts/claude-scientific-writer/venue-templates/references/` — per-venue formatting snapshots: `journals_formatting.md`, `conferences_formatting.md`, `posters_guidelines.md`, `grants_requirements.md`, `venue_writing_styles.md`, `nature_science_style.md`, `cell_press_style.md`, `medical_journal_styles.md`, `ml_conference_style.md`, `cs_conference_style.md`, `reviewer_expectations.md`.
- `scripts/claude-scientific-writer/venue-templates/assets/` — bundled LaTeX scaffolds (journals, grants, posters; see Techniques above).
- `scripts/claude-scientific-writer/venue-templates/scripts/` — `query_template.py`, `customize_template.py`, `validate_format.py`.

**academic-research-skills sources:**

- `scripts/academic-research-skills/academic-pipeline/SKILL.md` — full orchestrator definition (state machine, agents, integrity and review protocols).
- `scripts/academic-research-skills/academic-pipeline/references/` — deep dives: `pipeline_state_machine.md`, `integrity_review_protocol.md`, `ai_research_failure_modes.md`, `two_stage_review_protocol.md`, `external_review_protocol.md`, `process_summary_protocol.md`, `reproducibility_audit.md`, `progress_dashboard_template.md`, and more.
- `scripts/academic-research-skills/academic-pipeline/agents/` — `pipeline_orchestrator_agent.md`, `state_tracker_agent.md`, `integrity_verification_agent.md`, `collaboration_depth_agent.md`, `claim_ref_alignment_audit_agent.md`.
- `scripts/academic-research-skills/scripts/` — deterministic validators referenced by the pipeline (e.g., `check_pipeline_integrity.py`, `check_re_review_synthesis.py`); Python 3 required.
- `commands/ars/ars-full.md`, `commands/ars/ars-3w.md`, `commands/ars/ars-plan.md`, `commands/ars/ars-outline.md`, `commands/ars/ars-abstract.md`, `commands/ars/ars-format-convert.md` — slash-command entries for the modes in the table above.
