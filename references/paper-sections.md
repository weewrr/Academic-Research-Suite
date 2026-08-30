# Paper Sections — Per-Section Writing Playbook

Distilled from the seven section guides of the research-paper-writing skill:
Introduction, Abstract, Method, Experiments, Related Work, Conclusion, and
Paper Review. Each section below is a self-contained playbook: logic first,
then structure, then quality checklist. Worked examples are NOT reproduced
here — they live in the example bank (see Scripts & Resources).

## When to Use

- Drafting a specific section from scratch ("write the introduction", "写方法",
  "写实验").
- Diagnosing why a specific section feels weak (unclear challenge, missing
  motivation, unconvincing tables).
- Deciding which variant/template fits your paper's situation (e.g., novel
  task vs existing task; one contribution vs several).
- Running the end-of-draft adversarial self-review before submission.

Load only the section you are currently editing; do not load all guides at
once.

## Workflow

The same three-step loop applies to every section:

1. **Think through the section logic** — answer the pre-writing questions for
   that section before writing any prose.
2. **Apply a suitable template** — pick the variant that matches your
   situation (task familiarity, number of contributions, task novelty).
3. **Revise repeatedly** — check paragraph-level and sentence-level clarity,
   claim-evidence alignment, and terminology stability.

Cross-section invariants:

- One paragraph carries one message, stated in its first sentence.
- Every major claim (especially in Abstract and Introduction) must be
  supported by experimental evidence — otherwise weaken or remove it.
- Terminology and notation stay stable across all sections.
- Do not present a naive baseline and then describe your improvement over it;
  that framing makes the work look like an incremental patch even when it is
  not.

## Techniques

### Introduction

**Logic chain** (backward first, then forward):

- Backward reasoning — answer first: What technical problem do we solve, and
  why is there no well-established solution? What are the contributions
  (new task / new metric / new problem / new technique)? Why do they solve
  the challenge, and what new insight do they bring? How do prior methods
  lead readers to our challenge?
- Forward story — write in this order: (1) task and application; (2) prior
  methods leading to the technical challenge; (3) our contributions; (4)
  technical advantages and the new insight, stated explicitly; (5)
  experiments; (6) contribution list.

**Part A — Task and application openings** (choose by task familiarity):

- Version 1 (niche task): define the task in one clear sentence (`what output`
  from `what input`), optionally scope, then 2-3 application scenarios.
- Version 2 (familiar task): skip formal definition; open with application
  importance in one sentence.
- Version 3 (new setting, recommended): general task first, then narrow to
  the specific setting with exact input/output boundary.
- Version 4 (familiar task, aggressive): open with importance, summarize how
  representative previous methods work, and immediately expose the unresolved
  failure case + technical reason. Expert note: stating the problem in
  paragraph 1 is often good, but this style needs the right conditions.

**Part B — Technical challenge for previous methods (most important)**:

- Purpose: discuss around the exact challenge you solve, build curiosity,
  and make your method's motivation clear.
- Version 1 (existing task): general challenge -> traditional methods +
  limitation -> recent methods (1) + limitation with technical reason ->
  recent methods (2) + limitation with technical reason; ensure the final
  limitation is exactly what your method solves.
- Version 2 (insight with historical roots): mainstream limitation ->
  classical line containing similar insight -> why it is still insufficient ->
  modern methods' unresolved technical reason -> bridge to your method.
- Version 3 (novel task, no direct prior): state the goal, then decompose the
  challenge into N concrete points with `First/Second/Finally`, each with an
  observable limitation and a technical reason.
- Warning: never write "naive solution, then our improvement on it" — it
  erases reader curiosity and reads as a low-score incremental patch.

**Part C — Presenting the pipeline**:

- Version 1: one contribution with multiple advantages + teaser figure
  (`In this paper, we propose... The basic idea is illustrated in Figure...
  Specifically... In contrast... Another advantage...`).
- Version 2: two contributions + teaser figure; contribution 2 answers a
  remaining challenge introduced after contribution 1.
- Version 3: build on a prior pipeline and introduce one new module,
  motivated by an observation (`We observe that...`).
- Version 4: observation-driven — state the key innovation first, then the
  intuitive observation, then details, then benefits.
- Not recommended: hiding concrete method design behind abstract insight to
  make a simple pipeline sound novel. Introducing many new terms without
  mechanism-level explanation creates a novelty illusion that reviewers read
  as shallow.

**Checklist**: first sentence of each paragraph states its message; one
message per paragraph; challenge, technical reason, and solved mechanism all
explicit; claims aligned with experiment evidence; terminology stable.

### Abstract

**Pre-writing questions**: What problem do we solve and why is there no
established solution? What is the contribution? Why does it work in essence?
What technical advantage and new insight do we provide?

**Templates**:

- Version 1 (Challenge -> Contribution): task; technical challenge for
  previous methods; 1-2 sentences presenting the contribution that solves the
  challenge (name the technical term only — do not explain every step);
  benefits; experiment summary.
- Version 2 (Challenge -> Insight -> Contribution): task; challenge; one
  clear sentence with the insight; 1-2 sentences with the contribution
  implementing the insight; benefits; experiment summary.
- Version 3 (Multiple contributions): task; optional contrast sentence about
  prior methods; then one "contribution + technical advantage" sentence per
  contribution; experiment summary.

**Expert notes**: discuss previous work around the challenge you actually
solve; the technical term must be easy to understand with no reading jump;
the ability to state "contribution + advantage" in one sentence is the core
skill of a good abstract.

**Checklist**: a reader can identify task, challenge, insight/contribution,
and results in one pass; all major claims supported by experiments; technical
names self-contained; no sentence mixing too many messages.

### Method

**Pre-writing**: list all pipeline modules; for each module answer three
questions — How does it run? Why do we need it? Why does it work?

**Writing steps**:

1. Draw the pipeline figure sketch.
2. Map Method subsections from the sketch (one subsection per technical
   module).
3. Plan each subsection with the three elements: motivation, module design,
   technical advantages.
4. Write the module design first to build a concrete backbone; add motivation
   and advantages afterward.

**Three elements of a pipeline module**:

- **Module design**: define key structures (representation / network / data
  structure), then describe the forward process in strict execution order
  (given input -> step 1 -> step 2 -> output), ending with output
  interpretation (`We represent ... with ...`, `Given [input], we first ...
  then ... finally ...`).
- **Motivation**: problem-driven — because problem X exists, we design module
  Y (`A remaining challenge is ...`, `Previous methods have difficulty in ...`).
- **Technical advantages**: why this module beats alternatives, tied to
  measurable behavior when possible.

**Overview subsection**: 1-2 sentences of task setting; 1-2 sentences of core
contribution; pipeline figure pointer if the framework is novel; a map of
what Sections 3.1/3.2/3.3 cover.

**Implementation details**: hyperparameters, coordinate transforms,
normalization, and other practical details go near the end of Method or in a
dedicated subsection.

**Clarity check at three levels**: logic level (re-summarize the writing
logic — is it smooth?); paragraph level (first sentence signals the topic;
one message per paragraph); sentence level (the motivation of each sentence
is explicit — why is this content needed; sentence-to-sentence flow; term
consistency).

### Experiments

**Three core questions** the section must answer:

1. Is the method better than strong baselines? — compare against strong,
   recent baselines (include SOTA, not only weak baselines) on standard
   metrics with a fair protocol (same split, preprocessing, evaluation
   settings).
2. Which modules/design choices make the gain? — ablations per key module
   using remove/replace/disable variants, reporting the delta to the full
   model; include component-interaction ablations when modules are coupled.
3. How far does the method generalize? — harder or out-of-distribution
   settings, stress tests (more complex scenes, rarer cases, noisier inputs,
   stricter constraints); report gains AND failure modes to show realistic
   boundaries.

**Experiment planning**: derive validation experiments from claimed
contributions (one per contribution) and ablation studies from the pipeline
figure (one per module or key parameter). Section decomposition: experimental
setup -> validation experiments -> ablation studies.

**Table hard rules**: caption above the table; no vertical lines; booktabs
style (`\toprule`, `\midrule`, `\bottomrule`); as few horizontal rules as
possible (lines separate groups, not rows); restrained highlighting of best /
second-best numbers.

**Table readability rules**: label metric direction in headers (`PSNR ↑`,
`LPIPS ↓`); add units; text columns left-aligned, numeric columns
consistently aligned; consistent decimal precision per metric; group
multi-dataset results with `\multicolumn` + `\cmidrule`; one table one
message; encode ablation attributes in row names; captions focus on
setting/protocol, not discussion; in two-column papers prefer placing
single-column displays in the right column to preserve reading flow.

**Recommended ablation package**: one core ablation table for all major
contributions; several focused mini-ablations for module-level design
choices; matching qualitative results for each important ablation.

**Rigor checklist**: baselines recent and relevant; metrics sufficient and
standard; ablation tied to every key design claim; Abstract/Introduction
claims supported by reported numbers; evaluation-scope limitations stated.

### Related Work

**Workflow**: list directly competing and recent baseline papers first; group
literature by technical topic (not publication year); per topic summarize the
common paradigm, then the key limitation relevant to your challenge; end each
topic with your distinction.

**Topic design**: 2-4 focused topics, e.g., task-specific mainstream methods;
methods closest to your core idea; auxiliary techniques your method builds
on.

**Paragraph template**: (1) topic sentence defining scope; (2) representative
methods in one compact summary; (3) limitation tied to your target technical
challenge; (4) transition leading to your method.

**Do and don't**: compare mechanisms, assumptions, and failure modes;
emphasize the exact gap you fill; do NOT make Related Work a citation dump;
do NOT hide the strongest baselines.

**Checklist**: all strongest/recent competitors covered; each topic connected
to your problem setting; differences explained in technical (not marketing)
terms; citation coverage complete for all core claims.

### Conclusion

**Structure**: (1) restate the solved problem and core technical idea; (2)
summarize the strongest experimental evidence; (3) state practical impact or
new insight; (4) limitation paragraph; (5) concrete future direction.

**Limitation guidance**: prefer limitations tied to task/setting boundaries —
data regime (only short sequences), assumptions (controlled viewpoints only),
deployment scope (specific sensor setup). Avoid framing the conclusion around
fixable implementation flaws unless they critically define the method's
scope.

**Distinguish limitation types**: a technical defect (underperforms strong
baselines or causes unacceptable tradeoffs) vs a scope limitation (bounded by
the task setting but still competitive with current SOTA).

**Sentence template**: "This paper addresses [problem] by proposing [method].
The key idea is [core insight], which enables [main benefit]. Experiments
show [main gains] across [datasets/settings]. A current limitation is [scope
boundary], and extending to [future setting] is an important next step."

Never introduce new claims in the Conclusion.

### Paper Review (adversarial self-review)

**Critical rule**: every major claim, especially in Abstract and
Introduction, must be (1) technically correct and (2) explicitly supported by
experimental evidence. Add evidence or weaken/remove the claim.

**What gets papers accepted**: sufficient contribution (novel task, pipeline,
module, design choice, finding, or insight); better empirical performance
under fair comparisons; sufficient comparison experiments and ablations.

**Common rejection dimensions**:

| Dimension | Typical failure signals |
|-----------|------------------------|
| Insufficient contribution | targeted failure cases too common; technique already well explored; gains predictable |
| Unclear writing | missing technical details (not reproducible); module lacks motivation |
| Weak empirical effect | marginal improvement; absolute performance still weak |
| Incomplete evaluation | missing ablations; missing baselines or metrics; datasets too simple |
| Problematic method design | unrealistic setting; technical flaws; needs per-scenario tuning; new limitations outweigh benefits |

**Five-dimension self-review question list** (append near the end of the
draft; each question triggers a concrete edit):

1. *Contribution* — What new knowledge does the paper give? Is the failure
   case truly meaningful (not trivial/common)? Is the idea non-obvious beyond
   well-explored practice? Is the gain surprising or insightful? Is there at
   least one clear novelty type?
2. *Writing clarity* — Can a knowledgeable reader reproduce the method? Enough
   detail per key module? Is every module's motivation explicit and connected
   to a challenge? Terms and notation consistent? One message per paragraph?
3. *Experimental strength* — Are improvements meaningful, not statistically
   tiny? Is absolute performance competitive for the venue? Are gains
   consistent across datasets/settings/metrics? Are failure cases reported
   honestly?
4. *Evaluation completeness* — Ablations for all key design choices? All
   strong/recent baselines under fair settings? Standard, sufficient metrics?
   Challenging enough datasets? Documented protocols?
5. *Method design soundness* — Is the setting realistic? Hidden defects or
   unreasonable assumptions? Robust without per-case hyperparameter retuning?
   Do benefits outweigh complexity? Could reviewers argue net benefit is
   negative?

**Adversarial workflow**: read as a skeptical reviewer; answer every question
with explicit evidence from the paper; mark each item `pass` / `needs
revision` / `needs new experiment`; revise claims, writing, experiments, or
method scope; repeat until no major rejection risk remains.

## Scripts & Resources

All source guides and examples live under
`scripts/research-paper-writing/references/`:

- `introduction.md`, `abstract.md`, `method.md`, `experiments.md`,
  `related-work.md`, `conclusion.md`, `paper-review.md` — full source guides
  with sentence skeletons and expert notes.
- `does-my-writing-flow-source.md` — paragraph flow source reference.
- `examples/index.md` — example bank index.
- `examples/introduction/` — one worked example per Introduction variant
  (version-1-task-then-application, version-2-application-first,
  version-3-general-to-specific-setting, version-4-open-with-challenge,
  technical-challenge-version-1/2/3, pipeline-version-1/2/3/4,
  pipeline-not-recommended-abstract-only).
- `examples/abstract/` — template-a.md (challenge -> contribution),
  template-b.md (challenge -> insight -> contribution), template-c.md
  (multiple contributions).
- `examples/method/` — module triad examples, module design and motivation
  patterns, overview template, section skeleton, common-issues note.
- `examples/introduction-examples.md`, `examples/abstract-examples.md`,
  `examples/method-examples.md` — aggregate example banks.

When this guide's condensed patterns are not enough, load the corresponding
source guide for the full sentence skeletons and the matching example file
for a worked instance.
