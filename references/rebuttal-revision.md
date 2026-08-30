# Rebuttal & Revision

Consolidated guide for anticipating reviewer questions, preparing rebuttals, and revising a paper in response to review comments. Merges the reviewer-defense methodology (from phd-skills), the ARS revision/rebuttal modes (from academic-research-skills), and the adversarial self-review checklist (from research-paper-writing).

## When to Use

- **Before submission**: anticipate reviewer questions, identify paper weaknesses, select the strongest ablations to present, or "strengthen the paper" / "defend the paper".
- **After receiving reviews**: prepare point-by-point responses, plan and execute revisions, or audit an existing rebuttal draft against the reviewer comments.
- **Adversarial self-review**: run a skeptical, reviewer-style checklist over the draft to detect reject risks early ("review my paper before I submit").
- **Committee correspondence**: respond to a real committee or institutional review office (explicitly identified by the user only).

Do NOT use this guide to generate a review of someone else's paper (see `peer-review.md`) or to verify claims against code/data (see `paper-verification.md`).

## Workflow

The workflow has two phases: defensive preparation before submission, and response/revision after reviews arrive.

### Phase A: Pre-submission defense (from phd-skills)

**Step 1: Vulnerability analysis.** Read the paper and identify weaknesses from a reviewer's perspective:

- *Technical*: missing baselines reviewers would expect; evaluation metrics that don't capture the contribution; unjustified assumptions; unaddressed scalability; missing error analysis or failure-case discussion.
- *Presentation*: claims stronger than the evidence supports; missing related work an area reviewer would know; unclear methodology (could someone reimplement from the paper alone?); figures that don't convey the message; inconsistencies between sections.
- *Experimental*: small dataset without justification; missing statistical significance tests; no comparison with state-of-the-art on standard benchmarks; unexplored hyperparameter sensitivity; no computational cost comparison.

**Step 2: Venue-specific anticipation.** Different venues have different review cultures: top-tier ML/CV conferences (CVPR, NeurIPS, ICLR, ECCV) expect extensive ablations, strong baselines, clearly articulated novelty, and value reproducibility; workshops are more tolerant of work-in-progress and value interesting ideas over exhaustive evaluation; journals expect thorough related work, deeper analysis and more experiments than conferences, and weight writing quality and organization more.

**Step 3: Question generation.** Generate at least 10 likely reviewer questions, ranked by probability. For each question record the question (phrased as a reviewer would write it), why they would ask it, whether existing data can answer it (point to the specific table/figure) or a new experiment is needed, and a draft 2–3 sentence response if answerable.

**Step 4: Ablation selection.** From all available experiments, select the subset that (1) proves the core contribution, (2) shows each component's value via incremental additions, (3) preemptively addresses anticipated weaknesses, and (4) tells a coherent story.

**Step 5: Frame negative results.** "We explored X but found it did not improve over Y because Z" shows thoroughness and provides insight — frame as analysis, not failure; put in supplementary if not in the main paper.

### Phase B: Adversarial self-review before submitting (from research-paper-writing)

**Critical rule — do not violate:** every major claim, especially in the Abstract and Introduction, must be (1) technically correct and (2) explicitly supported by experimental evidence. If a claim is not supported, either add evidence or weaken/remove the claim.

Apply the end-of-paper self-review checklist; answer each question with explicit evidence from the paper and mark it `pass`, `needs revision`, or `needs new experiment`:

1. **Contribution**: What new knowledge does this paper give readers? Are we solving a truly meaningful failure case, not a trivial one? Is the technical idea non-obvious beyond well-explored practice? Is the gain surprising/insightful rather than predictable? Is there at least one clear novelty type (task / pipeline / module / design finding / insight)?
2. **Writing clarity**: Can a knowledgeable reader reproduce the method from the paper? Enough technical detail per key module? Explicit motivation for every module? Consistent terms and notation? One clear message per paragraph?
3. **Experimental strength**: Are improvements over strong baselines meaningful, not just statistically tiny? Is absolute performance competitive for the venue? Are gains consistent across datasets/settings/metrics? Do we report both strengths and failure cases honestly?
4. **Evaluation completeness**: Ablations for all key design choices? All strong/recent baselines under fair settings? Standard and sufficient metrics? Datasets challenging enough? Documented comparison/ablation protocols?
5. **Method design soundness**: Realistic experimental setting? Hidden technical defects or unreasonable assumptions? Robust without heavy per-case hyperparameter retuning? Do benefits outweigh added complexity — could reviewers argue the net benefit is negative?

Revise claims, writing, experiments, or method scope accordingly; repeat until no major rejection risk remains. A paper is usually accepted for: sufficient contribution, better empirical performance under fair comparisons, and sufficient comparison experiments plus ablation studies.

### Phase C: Responding to actual reviews (from phd-skills + academic-research-skills)

**Step 1: Read ALL reviews before responding to any.** Identify common concerns across reviewers.

**Step 2: Prioritize.** Address factual errors first, then major concerns, then minor ones.

**Step 3: Plan the revision.** From ordinary reviewer comments, build a Revision Roadmap plus a response-letter skeleton — without yet writing the revision (ARS `revision-coach` mode). Every roadmap item is triaged explicitly by the author: `will_address`, `wont_address`, or `not_on_point`; the roadmap itself stays immutable.

**Step 4: Write point-by-point responses.** Be respectful (thank reviewers, acknowledge valid points), be specific (point to exact sections, tables, figures), and for new experiments promise only what can be delivered in the rebuttal period.

**Step 5: Execute the revision.** Produce the revised draft plus the point-by-point response-to-reviewers (ARS `revision` mode).

**Step 6: QA the rebuttal.** If a response draft already exists, run an advisory QA audit of it against the reviewer comments (ARS `rebuttal-audit` mode): per-comment coverage, gaps, and risk flags. This audits an existing draft — it does not generate a new response. If only reviewer comments are present (no draft yet), coach the revision instead.

**Step 7: Verify before resubmission.** Each first-round concern must be independently verified against the revised manuscript — never a blanket "all addressed". Use an R&R Traceability Matrix with Author's Claim + Verified? columns per comment.

**Committee-correspondence variant (from academic-research-skills):** if and only if the user explicitly identifies a real committee or institutional review office: preserve the UTF-8 source letter, emit a separate concern tracker and a placeholder response skeleton, and run the deterministic completeness checker. Never infer committee authority from tone; never emit priority, severity, determination, or traceability artifacts on that branch.

## Techniques

### Anticipated-question template (from phd-skills)

```
Q: [Reviewer question]
Motivation: [Why this would be asked]
Answerable: [Yes — cite Table X / No — would need experiment Y]
Draft response: [If answerable, 2-3 sentences]
```

### Ablation ranking criteria (from phd-skills)

- **Impact magnitude**: how much does it change the primary metric?
- **Narrative strength**: does it clearly support a specific claim?
- **Uniqueness**: does it show something no other ablation shows?
- **Cost**: main paper vs appendix, based on space constraints.

### Rebuttal structure per reviewer (from phd-skills)

```
We thank Reviewer X for their thoughtful feedback.

**[Major concern]**: [Direct response with evidence]

**[Specific question]**: [Concrete answer]

**[Suggestion]**: [How we will incorporate it]
```

### Common rejection dimensions (from research-paper-writing)

| Rejection dimension | Typical failure signals |
|---|---|
| 1. Insufficient contribution | Targeted failure cases are too common; technique already well explored, gains predictable/well-known |
| 2. Unclear writing | Missing technical details, not reproducible; a method module lacks clear motivation |
| 3. Weak empirical effect | Only marginal improvement over prior methods; absolute performance still not strong enough |
| 4. Incomplete evaluation | Missing ablations; missing important baselines or metrics; datasets too simple to prove the method works |
| 5. Problematic method design | Unrealistic experimental setting; technical flaws; needs per-scenario hyperparameter tuning; new limitations outweigh benefits (negative net value) |

### Revision discipline (from academic-research-skills)

- The Revision Roadmap core is immutable once issued; author intent lives in a separate explicit adjudication sidecar (`will_address` / `wont_address` / `not_on_point` per item), never by silently editing the roadmap.
- Revision must not drift claims: every change to a claim surface should be traceable to a roadmap item or an explicitly declined overlap.
- Response letters follow an R→A→C format per comment: **R**eviewer comment → **A**uthor response → **C**hange made (with location in the revised manuscript).
- When coaching rather than executing (Socratic style), the user formulates their own revision strategy; guidance asks questions (e.g., "After reading the review comments, what surprised you the most?") and never proposes, substitutes, ranks, or selects a contribution claim on the author's behalf.

### Output format for pre-submission defense (from phd-skills)

1. **Weakness table**: categorized weaknesses with severity.
2. **Top 10 anticipated questions**: with answerability and draft responses.
3. **Recommended ablation subset**: with justification for each.
4. **Suggested text edits**: specific paragraphs to strengthen before submission.

### Socratic revision coaching dialogue (from academic-research-skills)

When coaching an author through a Minor/Major Revision decision (rather than executing the revision directly), guide them through this sequence — questions only; the user answers:

1. **Overall positioning** — "After reading the review comments, what surprised you the most?"
2. **Core issue focus** — guide the user to understand the consensus issues across reviewers.
3. **Contribution framing probe** — anchor questions to what the revised paper already claims; never propose, substitute, rank, expand, or select a contribution claim on the author's behalf.
4. **Explicit author triage** — record `will_address`, `wont_address`, or `not_on_point` for every source-ordered roadmap item, with no inferred work order.
5. **Counter-argument response** — guide the user to think through how to answer the strongest challenges (e.g., Devil's Advocate CRITICAL findings).
6. **Implementation planning** — confirm the exact block/operation scope of the revision.

The user can say "just fix it" at any point to skip guidance and move to direct execution. After the dialogue, the deliverable is the user's self-formulated revision strategy plus the unchanged roadmap and the completed author-adjudication sidecar.

### Mode selection quick reference (from academic-research-skills)

| Your situation | Use |
|---|---|
| Reviewer comments received, no response drafted yet | `revision-coach` (roadmap + response skeleton) |
| Ready to revise the manuscript and write responses | `revision` (revised draft + point-by-point responses) |
| A response draft exists and needs QA before submission | `rebuttal-audit` (per-comment coverage, gaps, risk flags) |
| Revised manuscript ready; check comments were addressed | reviewer `re-review` mode (traceability matrix + new decision) |
| Responding to a real committee or review office | `revision-coach` committee-correspondence variant (explicit identification only) |
| Pre-submission: want to find weaknesses before reviewers do | Phase A of the workflow above |

## Scripts & Resources

### ARS commands (`commands/ars/`)

| Command | Purpose |
|---|---|
| `commands/ars/ars-rebuttal-audit.md` | QA an existing rebuttal/response draft against reviewer comments. Requires BOTH the reviewer comments AND an existing draft. Produces an advisory QA report (per-comment coverage + gaps + risk flags); does not generate a new response. |
| `commands/ars/ars-revision.md` | Produce a revised draft plus point-by-point response-to-reviewers. High oversight. |
| `commands/ars/ars-revision-coach.md` | From reviewer comments, produce a Revision Roadmap plus Response Letter skeleton without writing the revision; committee-correspondence variant when the user explicitly identifies a real committee or review office. |

### ARS revision tooling (`scripts/academic-research-skills/scripts/`)

- `revision_roadmap.py` — hermetic validator/replayer for the revision-authority contract: joins reviewer-owned roadmap bytes, explicit author-choice bytes, registered claim surfaces, patch bytes, reports, and draft-chain bytes only through exact SHA-256 bindings; never supplies an author choice.
- `ars_apply_revision_patch.py` — apply a revision patch to a draft under the patch-discipline contract.
- `check_670_revision_roadmap_integration.py`, `check_390_revision_patch_discipline.py`, `check_revision_token_conservation.py`, `check_revision_claim_drift_suite_v2.py`, `check_re_review_synthesis.py` — contract/integrity checkers for roadmap integration, patch discipline, token conservation, claim drift, and re-review synthesis.
- Committee-correspondence checker: `check_committee_correspondence.py` (deterministic completeness check for the committee variant).
- Related reviewer skill (produces the roadmap upstream): `scripts/academic-research-skills/academic-paper-reviewer/` with `scripts/academic-research-skills/academic-paper-reviewer/templates/revision_response_template.md` (R→A→C format) and `scripts/academic-research-skills/academic-paper-reviewer/references/re_review_mode_protocol.md` (verification logic and R&R traceability matrix).

### Other sources

- phd-skills reviewer-defense methodology: `scripts/phd-skills/skills/reviewer-defense/` (vulnerability analysis, question generation, ablation selection, rebuttal structure).
- research-paper-writing adversarial self-review: `scripts/research-paper-writing/references/paper-review.md` (critical rule, rejection dimensions, self-review question list, adversarial writing workflow).
