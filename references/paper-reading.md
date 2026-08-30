# Paper Reading: Screening, Deep Reading Notes, and Citation Traversal

Merged reference for evaluating papers, producing structured reading notes, following
citation networks, scaling with subagents, and consolidating research sessions.

## When to Use

- A literature search has returned results and papers must be screened for relevance
  before detailed reading.
- The user asks a research question needing specific data in the literature ("find
  papers about...", "what is known about...") — the full search → evaluate → traverse →
  synthesize loop.
- A single paper (typically an arXiv link) needs deep, structured reading notes —
  triggers: "帮我读一下" (help me read this), "DNL", "论文速读", "paper notes".
- A highly relevant paper (score ≥ 7) needs its citation network explored.
- 50+ papers must be screened and parallel subagents would help.
- A completed research session needs consolidation or safe cleanup.

## Workflow

### Phase 0 — Parse the question and clarify

Extract keywords (main concepts + synonyms), data types needed (measurements,
protocols, datasets, code), and constraints (dates, organisms, publication types).
Ask clarifying questions when needed: in vitro or in vivo data? Time frame? Which
targets? **What email should be used for Unpaywall requests?** (Required — never use a
placeholder address.)

### Phase 1 — Initialize the research session

Propose a folder `research-sessions/YYYY-MM-DD-brief-description/` (confirm with the
user), create `papers/` and `citations/` subdirectories, and initialize:

- `papers-reviewed.json` — `{}`; the deduplication database. **Add every evaluated
  paper regardless of score** to prevent re-review and track complete history
- `citations/citation-graph.json` — `{}`; citation relationships only
- `SUMMARY.md` — query header, keywords, data types sought, then sections for Highly
  Relevant (score ≥ 8), Relevant (7), Possibly Relevant (5–6), Search Progress, and Key
  Findings. Always use clickable markdown links for DOIs and PMIDs

For small searches (<50 papers) use only these core files. For large searches (>100
papers) add `README.md` (project overview, quick-start, file inventory, methodology),
`TOP_PRIORITY_PAPERS.md` (tiered must-read list), and `evaluated-papers.json`.

### Phase 2 — Build and validate a screening rubric (optional, 50+ papers)

Before bulk screening (from research-superpower):

1. **Brainstorm criteria with the user**: core terms and synonyms, exclusion terms
   (false-positive triggers), data types that make a paper valuable, paper types
   (primary research vs reviews vs methods), whether related concepts (homologs,
   analogs, related diseases) count, and known edge cases
2. **Propose the initial rubric** and save it to `screening-criteria.json` (keywords,
   data types, scoring weights, special rules, threshold)
3. **Build a test set**: search 20 candidate papers, show 10–15 abstracts to the user
   one at a time, record y/n/maybe judgments in `test-set.json`
4. **Score the test set** with the rubric and report accuracy, flagging each false
   positive/negative with a score breakdown and why it was missed
5. **Iterate** until accuracy ≥ 80% (do not chase 95% — diminishing returns), then run
   bulk screening and cache all abstracts in `abstracts-cache.json`
6. Later, if the user reports misclassifications, update the rubric, bump its version,
   and re-score all cached abstracts; show a diff of status changes

### Phase 3 — Stage 1: Abstract screening

Score each abstract 0–10:

- **Keywords match (0–3)**: abstract mentions key terms from the query
- **Data type match (0–4)**: it contains the specific information needed (IC50,
  expression levels, protocols, datasets, structures, code)
- **Specificity (0–3)**: specific to the question vs general background/review

Decision rules: score < 5 → skip; 5–6 → note as "possibly relevant," skip for now;
≥ 7 → proceed to the deep dive (Stage 2).

Report to the user for EVERY paper — never screen silently:

```
📄 [15/127] Screening: "Selective BTK inhibitors..."
   Abstract score: 8 → Fetching full text...
```

### Phase 4 — Stage 2: Deep dive

For each paper scoring ≥ 7:

1. **Check ChEMBL first** (medicinal chemistry papers, from research-superpower):
   query by DOI — `curl -s "https://www.ebi.ac.uk/chembl/api/data/document.json?doi=$doi"`.
   ChEMBL accepts DOI only, never PMID. If found, record the `document_chembl_id` and
   activity count; structured SAR data is then available without PDF parsing. Still
   fetch full text for context, methods, and discussion
2. **Fetch full text**: PMC → DOI resolution → Unpaywall (mandatory when paywalled) →
   preprints (bioRxiv, arXiv). If unavailable after Unpaywall, note it in SUMMARY.md
   and continue abstract-only
3. **Scan for relevant content**: focus on Methods, Results, tables/figures, and
   supplementary information (key data often hides in SI). Grep the full text for
   domain terms (e.g., "IC50|Ki|MIC" for medchem; "expression|FPKM|RNA-seq" for
   genomics; "algorithm|github|code" for computational work)
4. **Extract findings** into structured JSON per paper: `doi`, `title`,
   `relevance_score`, `findings.data_found` (what and where, e.g., "IC50 values for
   compounds 1–12 (Table 2)"), `findings.key_results` (specific values with numbers)
5. **Download materials**: PDFs and supplementary files to `papers/`
6. **Update tracking files**: add every paper to `papers-reviewed.json` with status,
   score, source, timestamp, found data, full-text availability; add relevant papers to
   SUMMARY.md with clickable DOI/PMID links and key findings

### Phase 5 — Deep reading notes (DNL, from ResearchClaw)

For a paper selected for deep reading (e.g., an arXiv link), extract the arXiv ID from
the URL pattern, fetch `https://arxiv.org/abs/{ARXIV_ID}` for metadata and
`https://arxiv.org/html/{ARXIV_ID}v1` for structured content and figures, then produce a
markdown DNL note using the 7-section framework (see Techniques). Default output is
markdown (~2–3K tokens, git-friendly); generate HTML from
`templates/ResearchClaw/paper-note.html` only when explicitly requested. Save as
`YYYY-MM-DD_{alias}.md`. Use real numbers — never placeholders; figures must include
their arXiv HTML URLs.

### Phase 6 — Traverse citations

For papers scoring ≥ 7 (from research-superpower):

1. Resolve the Semantic Scholar paperId:
   `curl "https://api.semanticscholar.org/graph/v1/paper/DOI:{doi}?fields=paperId,title,year"`
2. **Backward (references)**:
   `GET /graph/v1/paper/{paperId}/references?fields=contexts,intents,title,year,abstract,externalIds&limit=100`
3. **Forward (citations)**:
   `GET /graph/v1/paper/{paperId}/citations?fields=title,year,abstract,externalIds&limit=100`
4. Filter before queueing (scoring rules below; only queue items scoring ≥ 5)
5. Deduplicate against `papers-reviewed.json` before and after evaluation; record
   relationships in `citations/citation-graph.json` (keys are DOIs; values hold
   `references` and `cited_by` lists)
6. Process the queue through Phase 3/4; traverse citations of papers scoring ≥ 9

**Traversal limits** (avoid exponential explosion): only traverse papers scoring ≥ 7;
depth limit 2 levels; check with the user every 50 papers total. Free Semantic Scholar
tier: 100 requests / 5 minutes — request multiple fields in one call, use `limit=100`,
cache responses; when rate-limited, wait 5 minutes and say so.

### Phase 7 — Scale with parallel subagents (50+ papers)

Core principle: fresh subagent per batch + consolidation between batches. Decision tree:
<20 papers → screen manually; 20–50 → subagents only if time-sensitive; 50+ → parallel
subagents (5–10 max at once) for screening, batched (5 at a time) for deep dives, one
pair per seed paper for citation exploration.

- Split the PMID list into non-overlapping batches of 15–25
- Dispatch all subagents in parallel (single message, multiple Task calls)
- Subagents return JSON only — they never update tracking files; the main agent
  consolidates, deduplicates, and writes `papers-reviewed.json` with `source` marked
  per batch
- Include the rate-limit sharing rules in every subagent prompt (see Techniques)
- Review consolidated results: one batch with a wildly different hit rate signals
  inconsistent scoring — re-screen it

### Phase 8 — Checkpoints and progress reporting

Check after every 50 papers or 5 minutes: ask the user to continue, stop, or see a
summary. Report every paper screened, every finding immediately, every blocker at once.
Every 5–10 papers give a progress summary with counts. Be specific ("Found IC50 = 12 nM
for compound 7 (Table 2)", not "Found data").

### Phase 9 — Synthesize and consolidate

When the queue empties or the user stops:

1. Filter `papers-reviewed.json` to score ≥ 7 and save as `relevant-papers.json`
2. Group findings by data type in a Key Findings section (values with source
   attributions), plus identified gaps
3. Enhance the top of SUMMARY.md with methodology: search strategy (keywords, data
   types, the exact query strings), screening rubric and thresholds, results statistics
   (counts per band, data extracted, citation traversal stats), file inventory, and a
   reproducibility note
4. For large sessions (>50 papers), a synthesis script reading `evaluated-papers.json`
   can generate the aggregate SUMMARY.md with consistent formatting

### Phase 10 — Clean up the session (optional)

After the user has reviewed outputs (from research-superpower): list all files with
sizes, categorize (protected / methodology / intermediate / temporary), show a dry-run
deletion plan, get explicit confirmation, then delete — prefer moving to trash over rm.
Afterward verify core files still exist and JSON is valid.

## Techniques

### DNL 7-section framework (from ResearchClaw)

| Section | What to extract |
|---|---|
| 0) Metadata | Title, alias, authors, venue, date, links, tags, rating, scoring breakdown |
| 1) Why-read | One sentence: key claim + key observation |
| 2) CRGP | Context, Related work, Gap, Proposal — from the Introduction |
| 3) Figures | Key figures with arXiv HTML URLs + one-line interpretations |
| 4) Experiments | Main results table (Benchmark / Metric / This Work / Best Baseline / Delta), ablation highlights, limitations |
| 5) Why it matters | 2–4 insights connecting to the reader's own research |
| 6) Next steps | Actionable follow-up items as checkboxes |
| 7) Scoring | Rating breakdown explanation |

DNL scoring: base 1 (complete paper with benchmarks) + quality bonus 0–2 (+1 solid
experiments with proper ablation; +2 strong ablation + SOTA + novel methodology) +
observation bonus 0–2 (+1 directly relevant to the reader's research; +2
paradigm-shifting). Max 5/5; the breakdown math must be explicit.

### Citation relevance filtering (from research-superpower)

Backward (references): context keywords match +3; title keywords match +2; intent is
methodology/result (vs background) +2; recent (<5 years) +1. Forward (citations): title
keywords +3; abstract keywords +2; recent (<2 years) +2; moderate recency (2–5 years)
+1. Queue only when score ≥ 5, the DOI/PMID exists, and the paper is not already in
`papers-reviewed.json`. Track the source as `backward_from:{doi}` / `forward_from:{doi}`.

### ChEMBL structured SAR check (from research-superpower)

- Query by DOI only: `document.json?doi=...` → `document_chembl_id`; then
  `activity.json?document_chembl_id={ID}&limit=1` → activity count
- ~30–40% of medicinal chemistry papers are in ChEMBL; typical activity types: IC50,
  MIC, Ki, EC50, Kd
- Curated data beats PDF parsing: standardized units, validated values, SMILES
  structures, no OCR errors
- Coverage gaps: papers <6 months old (curation lag), reviews, non-drug-discovery work

### Subagent rate-limit sharing (from research-superpower)

PubMed limits are shared across all parallel subagents: 1 of 2 parallel → 1 s delays;
1 of 3 → 1.5 s; 1 of 5 → 2.5 s. On HTTP 429: wait 5 seconds, then use 5-second delays
for remaining requests. Always state the parallel count in the subagent prompt.

### Subagent prompt template — batch screening (from research-superpower)

```
Screen papers 1-20 from this PMID list for relevance to [QUERY].
PMIDs: [list]
Score 0-10 based on: Keywords [list]; Data types needed [list].
Return JSON:
{ "screened_papers": [{"pmid": "...", "score": 8, "status": "relevant", "reason": "..."}],
  "stats": {"highly_relevant": 3, "relevant": 5, "not_relevant": 12} }
Do NOT update papers-reviewed.json — return results only.
[Rate limiting rules for this parallel count]
```

Deep-dive subagents return: `pmid`, `doi`, `full_text_source` (PMC / Unpaywall /
paywalled), `data_sources` (tables, figures, SI), `key_measurements`,
`methods_summary`, `key_findings`, `data_availability`.

### Session cleanup protected list (from research-superpower)

Never delete: `SUMMARY.md`, `relevant-papers.json`, `papers-reviewed.json`, `papers/`,
`citations/citation-graph.json`, `screening-criteria.json`, `test-set.json`,
`abstracts-cache.json` (ask before compressing/deleting very large ones),
`rubric-changelog.md`, `README.md`, `TOP_PRIORITY_PAPERS.md`, `evaluated-papers.json`,
`*.py` helper scripts, and project settings. Candidates for removal (with
confirmation): `initial-search-results.json`, `*.tmp`, `*.swp`, `.DS_Store`,
`__pycache__/`, `*.log`. Verify integrity after cleanup: core files present, JSON valid.

### Common mistakes to avoid

- Only tracking relevant papers → track every paper to prevent re-review
- Skipping Unpaywall after a paywall hit → ~50% of paywalled papers have free versions
- Following all citations without filtering → exponential explosion
- Silent screening → the user cannot course-correct
- Plain-text DOIs/PMIDs → always markdown links
- Querying ChEMBL by PMID → always returns 0 results; DOI only
- Subagents writing shared files → return JSON, main agent consolidates
- Creating custom tracking/citation files (e.g., `forward_citation_pmids.txt`) → use
  only `papers-reviewed.json`, `SUMMARY.md`, `citation-graph.json`

## Scripts & Resources

### Skill sources (relative to the skill root)

- `scripts/ResearchClaw/skills/paper-reader/SKILL.md` — DNL deep-reading workflow
- `scripts/research-superpower/research/evaluating-paper-relevance/SKILL.md` — two-stage
  screening and full-text fetching
- `scripts/research-superpower/research/traversing-citations/SKILL.md` — Semantic
  Scholar citation traversal
- `scripts/research-superpower/research/answering-research-questions/SKILL.md` — full
  session orchestration and consolidation
- `scripts/research-superpower/research/subagent-driven-review/SKILL.md` — parallel
  screening patterns and prompt templates
- `scripts/research-superpower/research/building-screening-rubrics/SKILL.md` —
  collaborative rubric design and test-driven refinement
- `scripts/research-superpower/research/cleaning-up-research-sessions/SKILL.md` — safe
  cleanup workflow
- `scripts/research-superpower/research/checking-chembl/SKILL.md` — ChEMBL lookups

### Templates

- `templates/ResearchClaw/paper-note.html` — HTML reading-note page for DNL output
  (fill `{{PLACEHOLDER}}` tags from extracted content; opt-in only)

### Research session file layout

```
research-sessions/YYYY-MM-DD-topic/
├── SUMMARY.md                  # Findings + methodology (final)
├── relevant-papers.json        # Score ≥ 7 subset
├── papers-reviewed.json        # Every paper screened
├── papers/                     # PDFs + supplementary
├── citations/citation-graph.json
├── screening-criteria.json     # Rubric (50+ paper searches)
├── test-set.json               # Rubric ground truth
├── abstracts-cache.json        # Cached abstracts for re-screening
└── rubric-changelog.md         # Rubric version history
```

### External APIs

- PubMed E-utilities: `https://eutils.ncbi.nlm.nih.gov/entrez/eutils/` (esearch,
  esummary, efetch)
- PubMed Central: `https://pmc.ncbi.nlm.nih.gov/articles/{PMCID}/`
- Unpaywall: `https://api.unpaywall.org/v2/{DOI}?email=EMAIL`
- Semantic Scholar: `https://api.semanticscholar.org/graph/v1/`
- ChEMBL: `https://www.ebi.ac.uk/chembl/api/data/`
