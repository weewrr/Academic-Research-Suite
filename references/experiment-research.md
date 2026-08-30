# Experiment Design, Execution & Reproduction

Consolidated guide for the machine-learning experiment lifecycle: designing rigorous experiments, curating datasets, launching long training runs, diagnosing failures, comparing runs fairly, and reproducing papers from scratch. Merged from the six operational skills of the phd-skills plugin.

## When to Use

- **Experiment design** — "design ablation", "plan experiment", "what experiments should I run", "baseline comparison", "experiment matrix". Use whenever a hypothesis needs an ablation study, baseline structure, or incremental evaluation strategy.
- **Dataset curation** — "dataset bias", "stratified sample", "class imbalance", "data distribution", "fairness analysis", "ethical review". Use before creating splits, expanding a dataset, or assessing annotation quality.
- **Launch** — launch / kick off / start / restart / kill a training run, or any multi-hour GPU job (`python train.py`, `accelerate launch`, `torchrun`, `deepspeed`, `sbatch train.sh`, tmux training sessions, `wandb sweep`).
- **Debug** — "why is X failing / diverging / NaN / OOM / hung / slow / crashed", "the loss is going up", "metrics look weird", "GPU util is 0", or a pasted log excerpt asking what is wrong.
- **Compare** — "compare run A to baseline / to run B", "is my run improving / catching up / falling behind", "rank these experiments", "track lag against baseline".
- **Reproduce** — "reproduce / implement / replicate / re-run paper X", an arxiv or OpenReview link with reproduction intent, or "the paper has no code, can we build it".

## Workflow

Walk the lifecycle in order. Each phase has verification criteria; do not advance until the current one passes.

### 1. Understand the research question

Before designing any experiment:

- Identify the specific hypothesis or claim the experiment must support.
- Identify the dependent variable (metric) and the independent variables (factors).
- Clarify the baseline: what is the current best result or default configuration?

### 2. Design the experiment matrix

Every ablation must change exactly ONE variable at a time:

1. **Define the factor** — what is being varied (loss function, learning rate, architecture component).
2. **List levels** — all values the factor will take (CE, focal, VAR).
3. **Fix everything else** — document what stays constant (seed, data split, epochs, hardware).
4. **Predict the outcome** — before running, state what you expect and why.

Template for each ablation row:

```
| Run ID | Factor | Value | Fixed Config | Expected Outcome |
|--------|--------|-------|-------------|-----------------|
```

Matrix strategy by problem shape:

- **Full factorial** — few factors (≤3) and few levels (≤3 each).
- **Sequential elimination** — many factors: run single-factor ablations first, then combine winners.
- **Latin square** — full factorial too expensive: sample representative combinations.

Always compute the budget before committing:

```
Total runs = product of all factor levels
GPU hours  = total runs × hours_per_run
```

Also estimate API costs (tokens × price), wall-clock time (sequential dependencies, GPU availability), and storage (checkpoint sizes × runs). Flag and suggest prioritization if the total exceeds reasonable bounds.

### 3. Curate the dataset

Before any curation action, understand the current state:

- **Per-class distribution**: counts per label, imbalance ratio (max/min), classes under 5% of the max class; bar chart sorted by count.
- **Co-occurrence**: which labels appear together; spurious correlations (e.g., "violence" always co-occurring with "male"); label leakage between splits.
- **Metadata**: source diversity, temporal coverage, content/genre/style/domain spread.

Assess each imbalance: is it real-world reflective? Is it harmful (would a model trained on it make unfair predictions)? Is it fixable (collect more, resample, reweight)?

When creating splits:

1. Primary stratification by label/class distribution.
2. Secondary stratification by source (prevent source leakage across splits).
3. Validate: chi-squared test for label similarity across splits, no source overlap, rare classes minimally represented in each split.

Split ratios by dataset size: large (>50k) 80/10/10 or 90/5/5; medium (5k–50k) 70/15/15 or 80/10/10; small (<5k) prefer k-fold cross-validation.

### 4. Pre-flight before launching

Long jobs are expensive to fail. Five checks before committing the GPUs:

1. **Config diff against a reference run.** Find the most-recently-modified config resembling the intended run (same model family, task) and diff it. Walk every diff line asking: intentional and motivated, or a stale default? Common silent regressors: `num_workers` (default 8 is wrong on NFS), `batch_size` (per-device vs global under DDP), `learning_rate` (should scale with batch size), optimizer betas/weight decay, `fp16` vs `bf16`, gradient accumulation steps, `seed`.
2. **Run name discipline.** The name lives in trackers, checkpoint dirs, and reports forever. Pattern: `<dataset/task>-<model>-<key-config>-<distinctive-recipe-piece>` (e.g., `coco-baseline-bs256-lr3e-4`). If you cannot describe the experiment from the name in one sentence, the name is wrong.
3. **Path verification.** Confirm the dataset path, pretrained checkpoint, output parent directory, and config file all exist. Never trust a path recalled from memory.
4. **Monitoring setup.** Auto-detect the tracker (`WANDB_API_KEY` / `wandb` import → wandb; `NEPTUNE_API_TOKEN` → neptune; `MLFLOW_TRACKING_URI` / `mlruns/` → mlflow; `runs/` or `lightning_logs/` → tensorboard; otherwise ask). "No monitoring" is rarely right for a multi-hour run.
5. **ETA in local timezone.** `epochs × seconds-per-epoch / 3600 = hours`. If the run straddles a meeting/sleep window, decide whether to defer or split.

Generate config stubs that match the project's existing format (YAML/JSON/TOML, key naming, output directory structure, tracker integration), then order runs by dependency (baselines first, then ablations), parallelize across GPUs where possible, and include a checkpointing strategy for long runs.

### 5. Debug failures with evidence before action

The most expensive mistake is asserting a plausible cause, then "fixing" something that masks the real problem. Enforce probe → hypothesis → smoke → controls → claim, in that order.

1. **Cheap probes** (seconds each): process state (`ps aux | grep -E '(python|train|torchrun|accelerate)'`), kernel events (`dmesg | tail -100`, `journalctl -xe --since "1 hour ago"`), GPU state (`nvidia-smi` — an idle GPU during "training" means data-loading block or a dead process), disk (`df -h`, `du -sh` on the run dir), log scrollback (read the last few hundred lines yourself — tracebacks, `loss=NaN`, `grad_norm=Inf`, the last successful step), checkpoint state (`ls -la .../checkpoints/` — an empty `.pt` differs from a 2GB one cut short).
2. **Hypothesis, labeled as hypothesis.** State what might be happening plus alternative hypotheses not yet ruled out. Never skip to "the cause is X."
3. **Smoke run.** Reproduce the failure shape under a controlled condition: OOM hypothesis → `batch_size=1` for 1 step; data hypothesis → synthetic in-memory dataset; model hypothesis → forward pass on one batch in `eval()`; optimizer hypothesis → `lr=0` (if loss still explodes, the loss is broken, not the optimizer); distributed hypothesis → 1 GPU only. A 30-second smoke beats a 30-minute restart-and-pray.
4. **Controls.** If the smoke is ambiguous, change exactly one variable from the failing config and rerun: single- vs multi-source data, worker count, mixed precision on/off, gradient checkpointing, `torch.compile`.
5. **Claim cause** only when evidence stacks up, citing the specific tool output that proves it. If it does not stack, say "I don't yet know" and propose the next probe.

### 6. Compare runs at the same epoch

The most common comparison error is reporting "run A is 4 points behind baseline" when A is at epoch 11 of 100 and the baseline number is from epoch 100.

1. Identify the runs by full name (no shortcodes); clarify which variant of a baseline is meant.
2. Fetch the full metric history, not just final values (final-value-only hides convergence dynamics).
3. Find the student's current step from the latest history row.
4. Slice the baseline at the same step; interpolate or pick the nearest step and state which.
5. Separate proxy metrics (cheap, during training: loss, kNN accuracy, perplexity) from downstream targets (expensive, periodic: linear-probe accuracy, task F1). Report both, separately. Never declare a winner from proxy alone.
6. Use full run names in every line of the report; each cell traceable to a tracker run-id and step.

Interpret with slope-based framing ("on track to catch up at step N"), not premature "winning/losing" verdicts. Note variance if known; otherwise label single-seed.

### 7. Reproduce a paper end to end

Seven stages from "I have an arxiv link" to "a replication run with measurable delta vs the paper's number." Each stage has its own success criteria; do not advance until the current one passes:

| Stage | What | Reference |
| ----- | ---- | --------- |
| 1 | Paper acquisition (arxiv HTML → structured extract) | `scripts/phd-skills/skills/reproduce/references/01-paper-fetch.md` |
| 2 | Existing code discovery + inventory | `scripts/phd-skills/skills/reproduce/references/02-code-clone.md` |
| 3 | Gap analysis (extract every missing hyperparam from the prose) | `scripts/phd-skills/skills/reproduce/references/03-gap-analysis.md` |
| 4 | Implementation (uv venv, fill gaps, commit per gap) | `scripts/phd-skills/skills/reproduce/references/04-implement.md` |
| 5 | Dataset acquisition (HF datasets first; substitute if private) | `scripts/phd-skills/skills/reproduce/references/05-dataset.md` |
| 6 | Smoke runs (forward pass → 1 step → 20 iters) | `scripts/phd-skills/skills/reproduce/references/06-smoke.md` |
| 7 | Replication runs + comparison at paper's reported epochs | `scripts/phd-skills/skills/reproduce/references/07-replicate.md` |

Set up a dedicated workspace per reproduction:

```
repro/<paper-arxiv-id>/
├── paper.md                     # structured extract from stage 1
├── inventory.md                 # what exists / missing from stage 2
├── gaps_filled.md               # hyperparam table with provenance from stage 3
├── code/                        # implementation from stage 4 (or cloned + extended)
├── data/                        # dataset symlinks or actual data from stage 5
├── dataset_substitution.md      # if a public dataset stood in for a private one
├── smoke_logs/                  # outputs from stage 6
└── results.md                   # replication outcomes from stage 7
```

Cross-stage discipline: implementation commits reference the paper section justifying each filled value; smoke failures route to the debug protocol above (not ad-hoc fixes); replication launches go through the pre-flight checklist; replication comparisons align at the paper's reported epochs (never current-vs-final).

## Techniques

### Ablation and analysis planning (from phd-skills: experiment-design)

- Define the analysis plan before execution: primary and secondary metrics, statistical significance test if applicable (paired t-test, bootstrap CI), handling of failed/crashed runs, and which visualizations to generate (comparison tables, bar charts, learning curves).
- Verification checklist before finalizing a plan: each ablation changes exactly one variable; baseline clearly defined and run with the same setup; resource estimate within budget; config stubs match project format; analysis plan defined before execution; seeds fixed for reproducibility.

### Bias metrics and annotation quality (from phd-skills: dataset-curation)

- Fairness dimensions to check where applicable: gender, racial/ethnic, age, geographic/cultural representation.
- Bias metrics: demographic parity (equal positive rates across groups), equalized odds (equal TPR and FPR across groups), representation ratio (group proportion in data vs population).
- Annotation quality: inter-annotator agreement (Cohen's kappa, Fleiss' kappa, Krippendorff's alpha), label-noise estimation by manually verifying a sample, edge-case identification, automated consistency rules.
- Expansion strategy: prioritize classes that benefit most from more data, suggest sources for underrepresented classes, choose among active learning / targeted scraping / synthetic augmentation, and estimate cost per approach.
- Ethical review checklist before using or publishing a dataset: content sensitivity, consent, privacy/anonymization, licensing terms, potential misuse, documentation (datasheet/data card).

### Restart and kill cleanup (from phd-skills: launch)

If restarting a failed run or killing before a replacement, purge stale artifacts in this exact order:

1. Local checkpoint dir on the launching machine (`rm -rf /local/runs/<run-name>` — verify the path first).
2. Remote artifact dir on the cluster / NFS / object store.
3. Experiment-tracker run (delete via the tracker API; stale tracker runs corrupt later comparisons).
4. Scheduler reservation (`scancel <jobid>`, reservation, cron entry). "Killed but GPUs still allocated" is recurring waste.

Skipping any step creates ghost state that confuses the next launch or comparison.

### Debug anti-patterns (from phd-skills: debug)

- "It's probably X, let me try Y" — no. Probe first.
- Restarting the run with a small change as the diagnostic — smoke first, then restart deliberately.
- Citing only the user's narrative as evidence — re-read the actual log.
- Stopping at the first plausible cause when artifacts contradict it.

### Comparison anti-patterns (from phd-skills: compare)

- "X is behind baseline by 4pp" without saying at what step — almost always wrong.
- "X has converged" without showing the last 5 epochs of the curve.
- "Best run is Y" based on a metric logged differently across runs (different reduction or eval set).
- Single-seed comparison treated as definitive.

Tracker auto-detection order: `WANDB_API_KEY` / wandb imports → wandb; `NEPTUNE_API_TOKEN` → neptune; `MLFLOW_TRACKING_URI` / `mlruns/` → mlflow; `runs/` or `lightning_logs/` → tensorboard; `*results*.json` / `*meta*.json` in run dirs → local file format; if none, ask where metrics live before guessing. Fetch full histories, e.g. for wandb: `api.run("entity/project/run-id").history(samples=10000)`; for tensorboard parse event files with `EventAccumulator`.

### Output formats (from phd-skills)

Every engagement ends with concrete deliverables, not prose advice:

- **Experiment plan**: the experiment matrix table (all runs with configurations), the resource estimate (GPU hours, API costs, storage), a ready-to-run execution script matching project conventions, and the analysis plan (metrics, comparisons, visualizations).
- **Dataset audit**: distribution report (per-class counts, imbalance ratios, co-occurrence matrix), bias findings with severity and actionability, split recommendation with validation results, prioritized expansion plan, and the completed ethics checklist with notes per item.
- **Launch report**: which of the five checks passed and which failed — block the launch on any failure unless the user explicitly waives the check. For a clean launch, end with the launch command itself in a fenced block, ready to copy.
- **Diagnostic report**: (1) what the probes showed, (2) the hypothesis, (3) the smoke outcome, (4) the cause-or-uncertain verdict, (5) the recommended next action — each claim citing the tool output that backs it.
- **Comparison report**: compact table per metric pair (proxy + downstream), each row aligned at the student's current step, each cell traceable to a specific tracker run-id and step, ending with one or two sentences of slope-based interpretation ("on track to catch up at step N, projected from current slope").

### Reproduction result labeling (from phd-skills: reproduce)

The final artifact `results.md` records absolute deltas (not just %) with one of three labels per metric:

- `[matched within 0.X pp]` — within the paper's reported variance.
- `[gap, hypothesis: ...]` — measurable underperformance with a stated cause hypothesis.
- `[fundamental disagreement, see X]` — result and paper claim are inconsistent in a way that needs investigation, not more compute.

## Scripts & Resources

All paths relative to the skill root. Full source skills are preserved for deep dives:

- `scripts/phd-skills/skills/experiment-design/SKILL.md` — full design methodology with config-stub and execution-plan detail.
- `scripts/phd-skills/skills/dataset-curation/SKILL.md` — full curation methodology with output-format spec.
- `scripts/phd-skills/skills/launch/SKILL.md` — full pre-flight checklist with command examples.
- `scripts/phd-skills/skills/debug/SKILL.md` — full five-step diagnostic protocol.
- `scripts/phd-skills/skills/compare/SKILL.md` — full same-epoch comparison protocol.
- `scripts/phd-skills/skills/reproduce/SKILL.md` — overview of the seven reproduction stages.
- `scripts/phd-skills/skills/reproduce/references/01-paper-fetch.md` … `07-replicate.md` — per-stage detailed procedures (paper fetch, code clone, gap analysis, implementation, dataset, smoke, replicate). Point users at these rather than duplicating them.

Shell hooks (require bash) used around launching and artifact hygiene:

- `scripts/phd-skills/scripts/destructive_path_guard.sh` — blocks obviously dangerous `rm`/`mv` paths (launch cleanup relies on it).
- `scripts/phd-skills/scripts/timezone_scrub.sh` — validates stated timezones for ETA reporting.
- `scripts/phd-skills/scripts/save_state.sh`, `scripts/phd-skills/scripts/notify.sh` — run-state persistence and completion notification.

Related agent (markdown prompt, no runtime requirement):

- `scripts/phd-skills/agents/paper-auditor.md` — audits papers/claims; useful after reproduction for verifying claimed results against artifacts.

Python snippets embedded in the compare skill (`wandb.Api()`, tensorboard `EventAccumulator`) require Python 3 with the respective SDKs installed; all shell snippets require bash.
