# Grants, Clinical Documentation & Applied Research Reports

Consolidated guide for funding proposals and safety-bounded applied-research documents: research grant writing (NSF, NIH, DOE, DARPA, Taiwan NSTC), treatment-plan documentation, clinical report structures, clinical decision-support research artifacts, evidence-traceable market research reports, and developmental scholar evaluation. Merged from six claude-scientific-writer skills.

## When to Use

- **Research grants** — writing proposals for NSF, NIH, DOE, DARPA, or NSTC programs; preparing project descriptions, specific aims, or technical narratives; developing broader-impacts or significance statements; timelines and milestones; budget justifications; resubmissions responding to reviewer comments; biosketches, CVs, facilities descriptions.
- **Treatment plans** — formatting and structurally validating clinician-authored treatment-plan documentation: source traceability, intervention records, goals and checkpoints, shared-decision records, reconciliation handoffs, release gates. Only after clinical decisions have been supplied and verified by authorized licensed professionals — never for clinical decision-making.
- **Clinical reports** — draft structures and deterministic checks for clinical case reports, diagnostic-report scaffolds (radiology/pathology/laboratory), trial results and protocol reports, CSRs, safety reports, and aggregate research summaries; only with synthetic, de-identified, or aggregate inputs.
- **Clinical decision support** — research-only CDS evaluation artifacts: intended-use statements, GRADE evidence profiles, aggregate cohort tables, survival analysis plans, model/biomarker evaluation, decision-logic traceability, de-identification process checklists. Not patient care or live clinical operation.
- **Market research reports** — market definition, industry and customer evidence, competitive landscapes, TAM/SAM/SOM reconciliation, forecast sensitivity, auditable report scaffolds.
- **Scholar evaluation** — qualitative-first, evidence-traceable developmental review of scholarly works (papers, drafts, protocols, syntheses, research ideas) and audits of low-stakes assessment rubrics. Never for ranking people or consequential decisions.

## Workflow

### Step 0: Shared safety-first pattern (clinical family)

The treatment-plans, clinical-reports, clinical-decision-support, and scholar-evaluation skills all follow the same shape. Apply it before any substantive work:

1. **Check the hard boundary** (see Techniques for each skill's specific prohibitions). If a request crosses a boundary, stop the unsafe portion; offer a blank structured template, a source-fact manifest, or a deterministic structural check, and route clinical/regulatory/personnel decisions to the responsible qualified professional. Do not redirect to another skill to obtain a prohibited output.
2. **Apply the data gate**: synthetic or qualified de-identified inputs only; no direct identifiers in examples, assets, tests, prompts, or logs; local-only processing (no external models, APIs, telemetry); minimum-necessary data; documented authority and purpose. If conditions are not documented, do not read or process the content.
3. **Mark every artifact** with the required draft notice (e.g., "DRAFT — NOT MEDICAL ADVICE — DOCUMENTATION-ONLY — AUTHORIZED CLINICIAN SIGN-OFF REQUIRED"). Structural success never removes the notice.
4. **Populate from verified sources only**: every field or claim maps to verified source-fact IDs with provenance (locator, verifier role, verification date). Unsupported content stays `null`/`missing` — never replace it with plausible text, and never infer missing clinical content.
5. **Run deterministic local checks** (structure and internal consistency only — they never assess clinical appropriateness).
6. **Require qualified human review** proportionate to the artifact before any release. A script pass is never authorization to use the package for care, filing, or submission.

### Step 1: Grant development (research-grants)

Five phases, working back from the deadline:

- **Phase 1 — Planning (2–6 months out)**: identify funding opportunities, review program announcements, consult program officers, assemble the team, develop preliminary data, outline specific aims, review successful proposals. Output: selected opportunity, team with roles, aims outline, preliminary-data gap analysis.
- **Phase 2 — Drafting (2–3 months out)**: write specific aims or objectives first, then project description / research strategy; create figures; draft timeline and milestones; prepare preliminary budget; write broader-impacts or significance sections; request letters of support.
- **Phase 3 — Internal review (1–2 months out)**: circulate to co-investigators, seek mentor feedback, run a mock review if possible, revise, refine budget.
- **Phase 4 — Finalization (2–4 weeks out)**: final narrative revisions, all required forms, budget justification, biosketches/CVs/current-and-pending, letters, data management plan, project summary, proofreading.
- **Phase 5 — Submission (1 week out)**: institutional review and approval, portal upload, verification, submit 24–48 hours before the deadline, archive everything. Never wait until the deadline — portals crash and files corrupt.

### Step 2: Clinical report production (clinical-reports)

1. **Confirm the input gate**: explicit purpose (publication draft, diagnostic scaffold, trial-results manuscript, CSR draft, aggregate safety table, aggregate research summary); allowed data class (`synthetic`, `deidentified`, `aggregate`); documented authority; local-only handling feasible; minimum-necessary defined; provenance for every populated field; identified review owner. Do not accept raw free-text patient records when a structured source-fact manifest can be supplied.
2. **Route before drafting** using the artifact → guidance table in Techniques (CARE, ACR, CAP, CLIA, CONSORT, SPIRIT, ICH E3/E2A/E2D...). Check the dated primary-source ledger and the live official source when requirements could have changed.
3. **Create a source-fact manifest** recording only local record locators, field paths, verification state, verifier role, verification date, and a SHA-256 value hash.
4. **Generate the correct template** (fail-closed JSON; no clinical content, no overwrites), then populate verified fields only — preserve uncertainty and "not assessed" exactly as recorded; keep source record and draft separate.
5. **Run the deterministic checks** (structure validators for CARE / trial manifests, adverse-event table formatter, terminology schema validator, de-identification process checker, provenance and consistency checkers).
6. **Apply the right review**: clinician for clinical facts, statistician for results/populations/estimands, safety professional for coding/seriousness/causality/reportability, privacy/legal for HIPAA and consent, sponsor regulatory/medical for CSRs, all accountable authors plus journal checks for publication.

### Step 3: CDS research artifacts (clinical-decision-support)

1. **Frame the research question**: define the estimand before viewing results; distinguish descriptive, prognostic, predictive, diagnostic-accuracy, and causal questions; pre-specify outcomes, time origin, horizon, subgroups, cut points, missing-data handling, multiplicity, and sensitivity analyses; separate exploratory from confirmatory.
2. **Select the artifact** from the asset/script matrix (intended-use, GRADE evidence profile, model/biomarker evaluation, aggregate cohort table, survival plan, logic traceability, de-identification review).
3. **Run locally**, writing outputs only to a reviewed local directory — never into an EHR, alerting system, clinical portal, or device workflow.
4. **Human review**: methodologist/statistician, domain expert, privacy officer, regulatory/legal counsel, human-factors specialist, and an authorized governance owner for release and change control.

### Step 4: Market research reports (market-research-reports)

1. **Establish the research contract**: decision, audience, deadline, materiality threshold; formal market definition and exclusions; geography and channels; historical/forecast periods and retrieval cutoff; measure (revenue, units, users, capacity...) and denominator; currency, base year, nominal/real basis; taxonomy with version; permitted sources and confidentiality.
2. **Build the evidence plan**: route each question to the source closest to the underlying event — primary law/regulator filings/official statistics first, then company filings, transparent surveys, peer-reviewed research, association data, secondary synthesis, paid estimates with inspectable methods, and news only for leads.
3. **Maintain a source ledger** (stable IDs `S-001...`, publication + retrieval dates, original producer behind aggregators, geography/period/vintage, currency and measure, revision status, method and limitations) and a **claims ledger** (`C-001...` with statement type, source IDs, as-of date, confidence, calculation and assumption IDs).
4. **Size the market as scenarios**: compute top-down and bottom-up independently, then apply scenario-specific serviceability and capture assumptions (formulas under Techniques). Use at least two genuinely different scenarios; SOM is not a guaranteed revenue forecast.
5. **Forecast with explicit uncertainty**: separate observed/estimated/forecast periods; state driver assumptions and the conditions that would invalidate each scenario; show sensitivity and switching values.
6. **Analyze customers and competitors ethically**: disclose full survey/interview methodology; define competitor scope from the customer perspective; validate feature matrices against the source ledger.
7. **Draft and review**: lead with findings and uncertainty, not frameworks; keep recommendations separate from evidence; pass the release gate (every claim mapped, methods reconciled, no fabricated figures).

### Step 5: Scholar evaluation (scholar-evaluation)

1. **Confirm allowed use**: developmental purpose, unit of assessment `scholarly_work`, work type/stage/discipline, authorized source location and classification, accountable owner, conflicts and recusals, accessibility process, appeal route. Stop on a prohibited decision context.
2. **Define the construct before criteria**: what quality is being examined, excluded constructs, intended interpretation, where it does not travel, evidence requirements, known limitations.
3. **Adapt and validate the rubric** starting from the bundled template; obtain qualified disciplinary, methods, stakeholder, accessibility, privacy, and fairness review. The template records content validity as `not_established` — do not change that without documented evidence.
4. **Build traceable evidence records**: distinguish observed evidence from interpretation, supporting from contrary, `missing` from `not_applicable`, and uncertainty from absence. Failure to find prior work does not prove novelty.
5. **Rate independently**: each criterion is `rated` (anchor score, bounded uncertainty, evidence IDs, rationale reference), `missing`, or `not_applicable` — never encode missing as zero.
6. **Run local quality checks**: bounded scoring, traceability, inter-rater agreement, weight sensitivity, and the fail-closed process checklist.
7. **Synthesize qualitatively**: lead with criterion-level evidence, not the composite; cite evidence, state anchor interpretation, note disagreements, offer non-prescriptive improvement options.
8. **Human review and release**: a qualified accountable committee verifies construct and rubric provenance, validity evidence, rater training and drift, traceability, sensitivity, bias review, privacy, and appeal information. Document dissent.

## Techniques

### Agency profiles (from claude-scientific-writer: research-grants)

- **NSF**: PAPPG 24-1 governs unless a solicitation overrides; Intellectual Merit + Broader Impacts equally weighted; 15-page project description (prior-NSF-support results max 5 pages); education/diversity/societal-benefit emphasis; panel + ad hoc merit review.
- **NIH**: Specific Aims (1 page) + Research Strategy (12 pages for R01); Significance/Innovation/Approach review criteria; preliminary data typically required; modular budgets ($250K increments); multiple resubmission opportunities.
- **DOE**: energy/climate/computational-science focus; often cost sharing or industry partnerships; national-laboratory collaboration; commercialization pathways; varies by office (ARPA-E, Office of Science, EERE).
- **DARPA**: high-risk/high-reward "DARPA-hard" problems (what if true, who cares); prototypes, demonstrations, transition paths; multi-phase structure; strong milestone tracking; varies dramatically by program manager and BAA.
- **NSTC (Taiwan)**: CM03 form as the core technical format; bilingual (Chinese + English) abstract; innovation and feasibility as primary review focus; preliminary data highly critical; research architecture diagram mandatory.
- Figures help when they show methodology/workflow, timelines (Gantt), conceptual architecture, experimental flow, or broader-impacts activity. 1–3 figures support review but never substitute for clear aims and methods.
- Mistake taxonomy to check before submission — conceptual (unaddressed review criteria, mission mismatch, vague objectives), writing (poor organization, jargon, verbosity, inconsistent terminology), technical (inadequate methods, over-ambition, no preliminary data, unrealistic timeline, misaligned budget), formatting (page-limit overruns are automatic rejection; wrong font/margins; missing sections), and strategic (wrong program/mechanism, weak team, missing broader impacts, late submission).

### Treatment-plan boundaries and templates (from claude-scientific-writer: treatment-plans)

- Hard boundary — never use to: diagnose or classify a person; select/rank/recommend/compare therapies; choose medication, dose, route, frequency, duration, or monitoring; start/stop/titrate/taper anything; check interactions, allergies, contraindications, or eligibility; infer missing clinical content; triage or determine urgency; predict outcomes; replace medication reconciliation, pharmacist review, informed consent, or clinician review; claim FDA approval, HIPAA compliance, or standard-of-care conformity.
- Six bounded JSON input templates: source-fact manifest, clinician-authored intervention, goals/monitoring/checkpoint, informed-preference/shared-decision, transition/reconciliation, intended-use handoff. Empty template arrays and pending attestations are intentional release blockers.
- Transcription rules: copy only clinician-authored facts; preserve source locators, versions/dates, author and verification roles; record goals, checkpoints, and transition dates exactly as supplied; leave missing fields unresolved; for medication content record the clinician-authored text and references without interpreting or validating it.
- The timeline generator schedules only dates already supplied — it never derives recurrence or clinical intervals.
- Source boundaries: FDA labeling/Medication Guides/REMS are authoritative records only when an authorized clinician or pharmacist verifies applicability; WHO/Joint Commission guidance only for process structure; AHRQ/NICE guidance only to document that shared decision-making occurred; CMS documentation requirements only when program, provider type, jurisdiction, and local policy are confirmed.

### Clinical report routing table (from claude-scientific-writer: clinical-reports)

| Artifact | Primary route | Key boundary |
|---|---|---|
| Case report for publication | CARE 2013 checklist + 2017 explanation | Consent, privacy, journal policy need human verification |
| Radiology draft scaffold | ACR 2025 communication practice parameter | Qualified radiologist authors findings/impression |
| Pathology draft scaffold | Specimen-specific CAP Cancer Protocol | Qualified pathologist selects protocol/version and authors diagnosis |
| Laboratory draft scaffold | 42 CFR 493.1291 + laboratory policy | Performing laboratory controls results and release |
| Randomized-trial results | CONSORT 2025 + applicable extensions | Reporting guidance, not a conduct/submission standard |
| Randomized-trial protocol | SPIRIT 2025 + extensions | For protocols, not results or CSRs |
| Clinical Study Report | ICH E3 + Q&A; consider E6(R3) | Adaptable guidance, not a rigid template |
| Pre-approval safety report | ICH E2A; E2B(R3) for ICSR data | Sponsor/investigator controls reportability |
| Post-approval individual safety | ICH E2D(R1) + E2B(R3) | Never automate case assessment or submission |
| Aggregate safety presentation | Protocol/SAP, ICH E3, CONSORT Harms | Aggregate tables never determine individual reportability |

Key distinctions to preserve: seriousness vs severity; adverse event vs suspected adverse reaction; CONSORT 2025 has 30 minimum items and SPIRIT 2025 has 34 (superseding SPIRIT 2013). SOAP/H&P/consultation/discharge-summary interfaces were removed — do not recreate patient-care notes, medication plans, triage instructions, or disposition advice.

### Privacy rules (from claude-scientific-writer: clinical-reports + clinical-decision-support)

- HHS recognizes Safe Harbor and Expert Determination under 45 CFR 164.514(b). Safe Harbor also requires no actual knowledge that remaining information can identify an individual; Expert Determination must be performed and documented by a qualified expert.
- A checklist or pattern scan cannot establish de-identification or HIPAA compliance. Rare conditions, small cells, dates, free text, images, metadata, and quasi-identifier combinations retain re-identification risk.
- Cohort tables: choose the minimum cell threshold under an approved disclosure policy, apply primary and complementary suppression, report denominators and missingness, avoid baseline significance testing as a balance diagnostic, and label adjusted/unadjusted/pre-specified/exploratory results.

### CDS artifact requirements (from claude-scientific-writer: clinical-decision-support)

- Every artifact header must include: type/title/version/status/owner/date; intended purpose, users, aggregate population scope, decision role; all prohibited uses; data level and no-PHI confirmation; limitations and failure modes; external-validation and subgroup applicability status; human-review roles and approval boundary; source citations with versions; monitoring, change-control, retirement, and audit expectations; and "Not for patient care or live clinical use."
- GRADE: never infer certainty from article text, study design alone, p-values, or keywords; never use legacy `1A/2B` shorthand as universal output. A human panel documents risk of bias, inconsistency, indirectness, imprecision, publication bias, upgrading considerations, effect estimate and uncertainty, rationale and source IDs, and the final certainty judgment with a named review role.
- Model/biomarker evaluation: accept only aggregate confusion counts and calibration bins; require locked model/assay/version with pre-specified threshold provenance, internal + independent external validation, calibration and discrimination, subgroup performance with uncertainty, missingness and bias assessment, human-factors and prospective evaluation where relevant, and monitoring/rollback/retirement criteria.
- Survival plans: define time zero, event, competing events, censoring, intercurrent events, estimand, horizon, effect measure, and analysis population together; assess proportional hazards before treating a hazard ratio as constant; pre-specify alternatives (time-varying effects, restricted mean survival time); use cumulative-incidence methods when competing events matter; address immortal-time, informative-censoring, delayed-entry, missing-data, and multiplicity risks.
- Reporting-guideline selection: STROBE (cohort/case-control/cross-sectional, + RECORD for routinely collected data); TRIPOD+AI and PROBAST+AI (prediction models); REMARK (tumor prognostic markers); STARD-AI with STARD (AI diagnostic accuracy); SPIRIT-AI (AI trial protocols); CONSORT-AI (AI trial reports); DECIDE-AI (early live evaluation — outside execution scope).
- Regulatory context: FDA device status turns on intended use and function, not a document label; FDA's January 2026 CDS guidance examples are not a self-certification checklist; ONC HTI-1 applies within its certification scope.

### Release gates and final handoffs (from claude-scientific-writer)

- **Market-research release gate**: market boundary, taxonomy, denominator, geography, and period explicit; every factual/quantitative claim maps to exact source IDs; publication/retrieval dates, revisions, methods, and limitations recorded; currency/base year, nominal/real basis, stock/flow, and units consistent; top-down and bottom-up methods use disjoint coverage and are reconciled; TAM/SAM/SOM and forecasts are conditional scenarios with sensitivity; survey/interview evidence carries method, privacy, and inference limits; competitor evidence is lawful, dated, and scoped; source conflicts and revisions remain visible; no fabricated/unsupported paid figures, PII, trade secrets, deceptive collection, brand impersonation, or investment-advice framing.
- **Clinical-reports final handoff** states six things: (1) artifact type and exact guidance/version used; (2) allowed data class and local-only handling; (3) unresolved `null`, `missing`, conflicts, and unsupported claims; (4) provenance and deterministic-check results; (5) required qualified reviewers; (6) the draft/non-submission warning. Never say "compliant," "HIPAA-safe," "validated clinically," "approved," "ready to file," or "ready to submit."
- **Treatment-plan human review** requires the accountable team to: compare every transcribed item with its signed source; perform medication reconciliation in approved systems; verify current FDA labeling, Medication Guide, REMS materials, and local formulary/policy when applicable; resolve every discrepancy and missing item; review shared-decision documentation and transition recipients; complete privacy/security/legal/records review; and sign, date, and release through the authorized record system. The final handoff retains provenance and unresolved-item routing — a script pass is not authorization to use the package for care.

### Market sizing formulas and claim rules (from claude-scientific-writer: market-research-reports)

```text
TAM_top    = sum(disjoint in-scope component values)
TAM_bottom = sum(customer_count × addressable_fraction
                 × annual_quantity_per_customer × price_per_unit)
SAM_s      = TAM × serviceable_fraction_s
SOM_s      = SAM_s × obtainable_share_s
```

- Never add: manufacturer revenue to distributor/end-customer spend; production + imports + sales without trade/inventory reconciliation; parent and subsidiary revenue; bundles and their components; gross output and value added; installed-base stock and annual flow; overlapping segments. Give every component a disjoint `coverage_key` and one shared `denominator_id`; preserve an unknown/residual category instead of forcing totals.
- Claims-ledger rules: one end-of-paragraph citation does not support unrelated sentences; split compound claims relying on different evidence; a calculation cites its inputs, not a source that never published the result; an aggregator and its original source are not independent corroboration; an interview theme is not population prevalence; absence of public evidence means `unknown`, not `no`.
- Forecast discipline: do not call scenario bounds confidence or prediction intervals; do not assign probabilities without a validated probabilistic model and diagnostics; report both sizing methods, the midpoint-relative gap, scope differences, and unresolved reconciliation — never average incompatible methods.
- Ethics: no brand impersonation, no invented citations or market shares, no PII or trade secrets, no disguised selling, no investment/legal/antitrust advice. HHI/CRn are descriptive screens, not legal conclusions.

### Evaluation interpretation rules (from claude-scientific-writer: scholar-evaluation)

- Hard boundary — never automate, recommend, materially influence, or score: hiring, promotion, tenure, admissions, grants, prizes, discipline/dismissal, or any high-impact personnel decision. Never rank people, never reduce a person to a composite score, never infer ability, character, protected traits, or future performance. A nominal human-in-the-loop does not remove this boundary. Do not issue publication-readiness, accept/reject, or "top-tier" judgments.
- Metric and prestige policy — do not score or infer quality from journal impact factors, h-index, citation or publication counts, altmetrics, venue/institution prestige, or author affiliation/reputation/network. The rubric validator rejects common proxy-measure criteria. If a qualified reviewer mentions an indicator descriptively, record its purpose, coverage, biases, and gaming risk — never hide it inside an opaque composite.
- ScholarEval status: the referenced framework (Moussa et al., arXiv:2510.16234v2) is an experimental literature-grounded research-idea evaluation framework, not validated psychometrics. Do not generalize its results to person assessment, consequential decisions, all disciplines, or this rubric.
- Interpretation: a score is an ordinal rubric summary, not a natural measurement; normalization does not repair incomplete evidence; the bundled uncertainty range is not a confidence interval; agreement does not establish reliability or validity; the overall score never overrides criterion evidence or qualified judgment; no output is a decision recommendation.
- Data boundary: bundled scripts accept only strict local JSON/CSV with pseudonymous IDs, bounded ratings, statuses, uncertainty, and local references. Allowed classifications are `synthetic`, `public_scholarly_work`, and `deidentified_low_stakes`. Never put raw applications, CVs, letters, reviewer identities, contact details, protected attributes, or source-document text in inputs, outputs, logs, or examples — keep source content in the authorized records system behind opaque local references.

## Scripts & Resources

All paths relative to the skill root; all bundled scripts are Python 3.11+ standard-library, local-only (no network, credentials, external models, or image services) unless noted.

**Research grants** — `scripts/claude-scientific-writer/research-grants/`:

- `SKILL.md` — full agency-specific methodology and five-phase workflow.
- `references/` — `nsf_guidelines.md`, `nih_guidelines.md`, `doe_guidelines.md`, `darpa_guidelines.md`, `nstc_guidelines.md`, `core_components.md`, `review_criteria.md`, `writing_principles.md`, `proposal_types_and_resubmission.md`, `broader_impacts.md`, `specific_aims_guide.md`.
- `assets/` — `nsf_project_summary_template.md`, `nih_specific_aims_template.md`, `budget_justification_template.md`.
- Optional figures: the scientific-schematics skill (`--doc-type grant`) requires `OPENROUTER_API_KEY` and outbound API access; AI generation sends prompts to a third-party API — do not include unpublished sensitive details.

**Treatment plans** — `scripts/claude-scientific-writer/treatment-plans/`:

- `SKILL.md` — hard safety boundary, data gate, and five-step workflow.
- `scripts/` — `generate_template.py`, `validate_treatment_plan.py`, `validate_traceability.py`, `check_completeness.py`, `privacy_process_check.py`, `check_consistency.py`, `timeline_generator.py` (run from the skill directory; reports contain rule codes and field paths only).
- `assets/` — the six bounded JSON input templates.
- `references/` — `safety_scope.md`, `privacy_governance.md`, `documentation_workflow.md`, `source_boundaries.md`, `shared_decision_handoff.md`, `source_ledger.md`.
- `tests/treatment-plans/` — unittest suite (`python3 -m unittest discover -s tests/treatment-plans -p 'test_*.py'`).

**Clinical reports** — `scripts/claude-scientific-writer/clinical-reports/`:

- `SKILL.md` — input gate, routing table, and safe drafting workflow.
- `scripts/` — `generate_report_template.py`, `validate_case_report.py`, `validate_trial_report.py`, `format_adverse_events.py`, `terminology_validator.py`, `check_deidentification.py`, `provenance_validator.py`, `consistency_checker.py`.
- `assets/` — fail-closed JSON/CSV templates (case report, radiology, pathology, lab, CSR, trial results, protocol checklist, safety aggregate, AE aggregate input, research summary, de-identification checklist, quality review checklist, provenance/terminology/consistency manifests).
- `references/` — `report_type_routing.md`, `case_report_guidelines.md`, `diagnostic_reports_standards.md`, `clinical_trial_reporting.md`, `safety_reporting.md`, `privacy_and_deidentification.md`, `medical_terminology.md`, `data_presentation.md`, `professional_review.md`, `sources.md` (ledger checked 2026-07-23).

**Clinical decision support** — `scripts/claude-scientific-writer/clinical-decision-support/`:

- `SKILL.md` — hard boundary, artifact header contract, and four-step workflow.
- `scripts/` — `validate_cds_artifact.py`, `evidence_profile_check.py`, `model_biomarker_evaluation.py`, `cohort_table_generator.py`, `survival_plan_validator.py`, `decision_logic_traceability.py`, `deidentification_checklist.py`.
- `assets/` — matching JSON templates for each artifact type (intended use, evidence profile, aggregate model evaluation, cohort table, survival plan, decision-logic traceability, de-identification checklist).
- `references/` — `safety_and_scope.md`, `regulatory_and_governance.md`, `evidence_profiles.md`, `study_reporting.md`, `cohort_evaluation.md`, `survival_analysis.md`, `model_biomarker_evaluation.md`, `privacy_and_disclosure.md`, `decision_logic_traceability.md`, `sources.md`.
- `tests/clinical-decision-support/` — unittest suite: `python3 -m unittest discover -s tests/clinical-decision-support -p 'test_*.py'`.

**Market research reports** — `scripts/claude-scientific-writer/market-research-reports/`:

- `SKILL.md` — operating principles and ten-step workflow with the release gate.
- `scripts/` — `validate_evidence_ledger.py`, `audit_claim_citations.py`, `calculate_market_sizing.py`, `forecast_sensitivity.py`, `validate_competitor_matrix.py`, `check_unit_consistency.py`, `generate_report_scaffold.py`.
- `assets/` — source-ledger CSV, claims and consistency templates, market-sizing scenarios, forecast sensitivity, competitor matrix, report manifest, plus optional LaTeX assets (`market_report_template.tex`, `market_research.sty`, `FORMATTING_GUIDE.md` — the LaTeX template requires XeLaTeX or LuaLaTeX).
- `references/` — `report_structure_guide.md`, `evidence_model.md`, `data_analysis_patterns.md`, `official_data_sources.md`, `methods_and_ethics.md`, `visual_generation_guide.md`, `sources.md`. Online research requires user-approved network access and source-specific terms; bundled scripts make no network, LLM, or image calls.

**Scholar evaluation** — `scripts/claude-scientific-writer/scholar-evaluation/`:

- `SKILL.md` — hard boundary, metric policy, and eight-step workflow.
- `scripts/` — `validate_rubric.py`, `calculate_scores.py`, `check_traceability.py`, `summarize_agreement.py`, `weight_sensitivity.py`, `check_process.py`, `generate_report_scaffold.py` (run with `PYTHONDONTWRITEBYTECODE=1`; weight sensitivity requires two or more distinct evaluation files).
- `assets/` — `rubric_template.json`, `evaluation_template.json`, `evidence_manifest_template.json`, `process_checklist_template.json` (intentionally unconfirmed, fails closed), `ratings_template.csv`.
- `references/` — `responsible_assessment.md`, `evaluation_framework.md`, `local_tooling.md`, `source_ledger.md`, `security_validation.md`.
