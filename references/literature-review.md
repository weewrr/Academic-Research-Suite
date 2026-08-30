# Literature Review: Systematic Synthesis from Research Question to Report

Merged reference for conducting systematic literature reviews: scoping, multi-database
search, screening, quality appraisal, synthesis, citation verification, and report
generation, including PRISMA-compliant systematic review and meta-analysis modes.

## When to Use

- Conducting a systematic literature review, scoping review, or meta-analysis for
  research or publication (from claude-scientific-writer).
- Synthesizing current knowledge on a topic across multiple sources; investigating the
  state of the art; identifying research gaps and future directions.
- Writing the literature review section of a paper or thesis.
- Finding related work, surveying a research area, or discovering open-source
  implementations — triggers: "find papers on", "related work", "literature review",
  "what papers exist", "papers with code" (from phd-skills).
- Deep research on any topic requiring a full report with verified citations — triggers:
  "deep research", "literature review", "systematic review", "meta-analysis", "PRISMA",
  "evidence synthesis", "fact-check", "guide my research", "help me think through",
  研究/深度研究/文獻回顧/系統性回顧/後設分析 (from academic-research-skills).
- Producing an annotated bibliography rendered as a literature review section via the
  `/ars-lit-review` command (from academic-research-skills commands).

## Workflow

### Step 1 — Scope the review

- Clarify the exact research question or topic boundary (from phd-skills).
- Define key terms and synonyms (e.g., "content moderation" = "safety filtering" =
  "NSFW detection"), and inclusion/exclusion criteria: year range, venue type,
  methodology type, paper types (primary research, reviews, methods papers,
  preprints acceptable?).
- Ask whether the user has seed papers to start from.
- For deep-research runs, score the research question against FINER criteria
  (Feasible, Interesting, Novel, Ethical, Relevant), set scope boundaries (in-scope /
  out-of-scope), and derive 2–3 sub-questions (from academic-research-skills).

### Step 2 — Select the operating mode (from academic-research-skills)

```
Already have a clear research question?
├─ Yes → Need PRISMA-compliant systematic review / meta-analysis?
│        ├─ Yes → systematic-review mode
│        └─ No → Need a full report? → full mode
│                 Only need literature? → rapid paper comparison?
│                                      ├─ Yes → three-way-scan mode
│                                      └─ No → lit-review mode
└─ No → Want to be guided through thinking? → socratic mode
```

Mode table: `full` (all core agents, APA 7.0 report, 3,000–8,000 words), `quick`
(500–1,500-word brief), `lit-review` (annotated bibliography + synthesis, 1,500–4,000
words), `three-way-scan` (paper shortlist compared by WHY/HOW/WHAT, 800–2,000 words),
`fact-check` (verification report), `socratic` (guided research dialogue),
`systematic-review` (PRISMA 2020 report + forest plot data + GRADE table, 5,000–15,000
words). If intent is ambiguous between socratic and full, prefer socratic — guide
first, produce reports later.

### Step 3 — Systematic search

Use multiple search strategies in order (from phd-skills):

1. **Direct search** — key terms via web search and academic databases; vary phrasings
   to catch different framings of the same concept
2. **Citation chaining** — from seed papers: forward (who cited this?) and backward
   (what does it cite?); catches papers using different terminology for the same problem
3. **Venue mining** — identify top conferences/journals/workshops for the topic; check
   recent proceedings; workshop papers often contain early-stage work
4. **Open-source discovery** — search GitHub for implementations; check Papers With
   Code for the task/dataset; look for community "awesome-X" lists

Search discipline (from claude-scientific-writer):

- Use a minimum of 3 databases; include preprint servers (bioRxiv, medRxiv, arXiv) to
  capture the latest findings
- **Record every search string, date, and result count** — a review that cannot
  reproduce its own search is not systematic
- Run pilot searches, review results, refine terms
- Sort by citation count when available to surface influential work first
- Use extraction on promising URLs to verify relevance before full-text screening

### Step 4 — Screen and select

- Screen systematically: title → abstract → full text (from claude-scientific-writer).
- Keep counts at each stage for the PRISMA flow diagram.
- Document exclusion reasons; consider dual independent screening for systematic
  reviews.

### Step 5 — Extract data and appraise quality

- Structured, per-study data extraction.
- Risk-of-bias or quality appraisal: RoB 2 for RCTs, ROBINS-I for non-randomized
  studies (from academic-research-skills).
- Grade sources against the evidence hierarchy — meta-analyses > RCTs > cohort studies >
  case reports > expert opinion — but grading is discipline-relative: a source meeting
  its own field's gold standard can reach Grade A even at a low design level.
- Screen for predatory journals, flag conflicts of interest, and assess currency
  (publication-date relevance).

### Step 6 — Synthesize

- Organize thematically, NOT study-by-study (from claude-scientific-writer); group by
  themes, compare and contrast across studies, identify patterns.
- Map contradictions and resolve them with evidence-quality comparison; if sources
  disagree, report both sides (from academic-research-skills).
- Identify knowledge gaps (from phd-skills):
  1. Build a coverage matrix — rows = problem aspects, columns = existing approaches
  2. Empty cells = candidate gaps
  3. For each candidate gap, search specifically for work filling it (it may exist
     under different terms), check very recent papers (last 6 months), and assess
     whether filling it would meaningfully advance the field

### Step 7 — Verify citations

- Every citation checked against the actual source (from claude-scientific-writer).
- Every paper mentioned must have verified metadata: author names cross-checked, venue
  and year confirmed against the published version (not the preprint), and result
  claims traceable to a specific table/figure (from phd-skills).
- If uncertain about any detail, flag it explicitly rather than guessing.
- Anti-patterns to enforce (from academic-research-skills): no confirmation bias in
  source selection (the checkpoint must include counter-evidence search); no
  cherry-picking; no "vibe citing" (mixing elements of real papers into a fabricated
  reference); **if a reference cannot be confirmed to exist, it does not go in the
  report — the gray zone is a FAIL, not "uncertain"**.

### Step 8 — Generate the report

- Follow a structured template: Title, Abstract (150–250 words), Introduction,
  Literature Review / Theoretical Framework, Methodology, Findings, Discussion
  (interpretation, implications, limitations), Conclusion & Recommendations, References
  (APA 7.0), Appendices (from academic-research-skills).
- Include explicit limitations and an AI-assistance disclosure.
- For papers worth citing, provide BibTeX entries verified against DBLP (from
  phd-skills).
- Structure the output as: topic summary, categorized paper table (Year / Venue /
  Approach / Key Result / Code?), gap table with confidence levels, recommended
  readings (top 5–10), and candidate citations (from phd-skills).
- Every literature review must include 1–2 AI-generated figures (PRISMA flow diagram,
  search strategy flowchart, thematic synthesis diagram, research gap map, citation
  network, or conceptual framework) via the scientific-schematics tooling (from
  claude-scientific-writer).

### Step 9 — Review checkpoints (from academic-research-skills)

Between phases, run adversarial review:

- **Devil's advocate** — challenge assumptions, test for logical fallacies, find
  alternative explanations, check for cherry-picking and confirmation bias; three
  mandatory checkpoints; CRITICAL-severity issues block progression
- **Editorial review** — originality, methodological rigor, evidence sufficiency,
  argument coherence, writing quality; verdict Accept / Minor / Major / Reject
- **Ethics review** — AI disclosure compliance, attribution integrity, dual-use
  screening; integrity verdict only (CLEARED / CONDITIONAL / BLOCKED)
- Revision loops capped at 2; remaining issues become "acknowledged limitations"

### Step 10 — Hand off to paper writing

After research completes, hand off the research question brief, methodology blueprint,
annotated bibliography, and synthesis report to paper-writing skills; they will skip
redundant scoping and literature search (from academic-research-skills). When the user
wants a literature review section in paper format, prefer the paper-side lit-review
command; for the research-side annotated bibliography + synthesis, prefer the
deep-research lit-review mode.

## Techniques

### Three-Way Scan: WHY / HOW / WHAT (from academic-research-skills)

For a disciplined shortlist of papers compared in a stable frame, extract per paper:

- **WHY**: what problem or bottleneck the paper addresses and why it matters
- **HOW**: what strategy, method, or technical route it uses
- **WHAT**: what it found, built, or still leaves unresolved

Then synthesize: the common WHY, the divergent HOW approaches, the strongest WHAT
results, and the unresolved global gap. Escalate to lit-review or systematic-review
when a broader evidence matrix or PRISMA-like coverage is needed.

### Gap validation checklist (from phd-skills)

A gap is real only if: searched with ≥ 3 different phrasings; top-3 venue proceedings
from the last 2 years checked; no arXiv preprint addresses it; it is technically
feasible to address; filling it would be a meaningful contribution. Rate confidence
HIGH (extensively searched, clearly missing), MEDIUM (might have missed niche work),
or LOW (limited search).

### Socratic mode intent signals (from academic-research-skills)

Activate guided-research dialogue when the user has no clear research question and
wants mentoring, asks to be led or guided, expresses uncertainty about what to
research, wants to brainstorm or clarify direction, or describes a vague interest
without an answerable question — regardless of language. While active, never give
direct answers; ask genuine questions that expose assumptions.

### Mode spectrum (from academic-research-skills)

*fidelity* = template-heavy, predictable output (quick, three-way-scan, fact-check,
lit-review, systematic-review); *balanced* = default (full, review); *originality* =
exploratory, template-light (socratic). Not sure? Start with socratic.

### Visual schematics requirement (from claude-scientific-writer)

Generate publication-quality diagrams by describing them in natural language — PRISMA
flow diagrams, search strategy flowcharts, thematic synthesis diagrams, gap
visualization maps, citation networks, conceptual frameworks. The schematic generator
iterates automatically and produces colorblind-friendly, high-contrast figures saved
to `figures/`.

### Common pitfalls (from claude-scientific-writer)

Single-database search; no search documentation; study-by-study summaries instead of
thematic synthesis; unverified citations; too-broad or too-narrow searches; ignoring
preprints; no quality assessment; publication bias left unmentioned; stale searches
without a stated search date.

### Failure paths (from academic-research-skills)

| Scenario | Recovery |
|---|---|
| Research question cannot converge | Summarize user-expressed directions, suggest lit-review mode, or switch to full mode |
| Insufficient literature (<5 sources) | Expand search strategy, try alternative keywords |
| Methodology mismatch | Return to scoping, propose 3 alternative methods |
| Critical logical flaw found | Stop, explain the issue, require correction |
| User abandons mid-process | Save progress, provide a re-entry path |
| English search returns empty for a Chinese topic | Switch to Chinese academic databases |

## Scripts & Resources

### Skill sources (relative to the skill root)

- `scripts/claude-scientific-writer/literature-review/SKILL.md` — systematic review
  workflow with full command documentation
- `scripts/claude-scientific-writer/literature-review/references/core_workflow.md` —
  full seven-phase workflow details
- `scripts/claude-scientific-writer/literature-review/references/search_and_citation.md`
  — per-database search guidance and citation styles
- `scripts/claude-scientific-writer/literature-review/references/database_strategies.md`
  — comprehensive database search strategies
- `scripts/claude-scientific-writer/literature-review/references/citation_styles.md` —
  APA, Nature, Vancouver, Chicago, IEEE formatting
- `scripts/claude-scientific-writer/literature-review/references/example_workflow.md` —
  a full worked review
- `scripts/claude-scientific-writer/literature-review/assets/review_template.md` —
  complete review template with all sections
- `scripts/phd-skills/skills/literature-research/SKILL.md` — literature research
  methodology
- `commands/ars/ars-lit-review.md` — slash command that triggers lit-review mode

### Script invocations (from claude-scientific-writer)

```bash
# Verify DOIs and generate formatted citations
python scripts/claude-scientific-writer/literature-review/scripts/verify_citations.py

# Process, deduplicate, and format search results
python scripts/claude-scientific-writer/literature-review/scripts/search_databases.py

# Convert markdown to professional PDF
python scripts/claude-scientific-writer/literature-review/scripts/generate_pdf.py

# Generate publication-quality schematic figures
python scripts/claude-scientific-writer/literature-review/scripts/generate_schematic.py \
  "PRISMA flow diagram for the systematic review" -o figures/prisma.png
```

### Literature client scripts (from academic-research-skills)

- `scripts/academic-research-skills/scripts/arxiv_client.py`
- `scripts/academic-research-skills/scripts/crossref_client.py`
- `scripts/academic-research-skills/scripts/openalex_client.py`
- `scripts/academic-research-skills/scripts/semantic_scholar_client.py`
- `scripts/academic-research-skills/scripts/chinese_literature_client.py`
- `scripts/academic-research-skills/scripts/retraction_status.py`
- `scripts/academic-research-skills/scripts/citation_verification_summary.py`

### External guidelines and tools

- PRISMA (systematic reviews): http://www.prisma-statement.org/
- Cochrane Handbook: https://training.cochrane.org/handbook
- AMSTAR 2 (review quality): https://amstar.ca/
- MeSH Browser: https://meshb.nlm.nih.gov/search
- PubMed Advanced Search: https://pubmed.ncbi.nlm.nih.gov/advanced/
- Boolean Search Guide: https://www.ncbi.nlm.nih.gov/books/NBK3827/
- Citation styles: APA (https://apastyle.apa.org/), Nature Portfolio, NLM/Vancouver
  (https://www.nlm.nih.gov/bsd/uniform_requirements.html)
