# Paper Discovery: Literature Search, Daily Recommendations, and Open Access Retrieval

Merged reference for finding papers: interest-driven daily arXiv scouting, structured
PubMed searching, manuscript-grade evidence packets, and open access full-text retrieval.

## When to Use

- **Daily paper scouting** — user says "推荐今日论文" (recommend today's papers), "每日论文",
  "paper scout", "daily papers", or wants a cron-driven digest of new arXiv papers
  matching their research interests (from ResearchClaw).
- **Initial literature search** — starting a new research question, user asks "find papers
  about...", building the initial paper list for evaluation, or searching for specific
  methods, compounds, diseases, or techniques (from research-superpower).
- **Manuscript evidence compilation** — user explicitly asks to gather literature,
  references, background evidence, competing findings, a structured evidence matrix, or
  a manuscript research packet targeting ~60 verified, unique references (from
  claude-scientific-writer).
- **Open access retrieval** — DOI resolution hits a paywall, full text is not in PubMed
  Central, or full text is needed for a highly relevant paper. Always try this before
  giving up on full text access (from research-superpower).

Not for: casual factual questions that need no research, private or unpublished material,
or claims answerable from user-provided files.

## Workflow

### Step 1 — Load the research profile or capture manuscript context

For interest-driven discovery (from ResearchClaw), load the user's research profile first:

- Location: `~/.openclaw/workspace/research-claw-config.md`
- Fields: `research_direction` (free-text focus), `seed_papers` (gold-standard arXiv IDs),
  `keywords` (search topics), `whitelist_authors`, and `learned_preferences.accept` /
  `learned_preferences.reject` (explicitly liked/disliked topics)
- If missing, use defaults silently and mention at the end: "想定制推荐兴趣？试试说
  「更新我的研究画像」"

For manuscript evidence (from claude-scientific-writer), constrain retrieval with a
context object instead — research question, study type, population/system, intervention,
comparator, outcomes, field, date range, target journal. Pass via `--context-file`:

```json
{
  "research_question": "How does intervention X affect outcome Y?",
  "study_type": "prospective cohort",
  "population": "adults with condition Z",
  "outcomes": ["primary outcome Y", "adverse events"],
  "field": "clinical epidemiology"
}
```

Do not invent missing study details. A bare topic works, but the packet will flag its
section briefs as broad.

### Step 2 — Parse the information need

Extract from the request (from research-superpower):

- **Keywords**: main concepts plus synonyms (e.g., "Bruton's tyrosine kinase" = "BTK")
- **Data types**: what is actually needed (IC50 values, methods, structures, datasets,
  results, code)
- **Constraints**: date ranges, specific journals, author names
- If the workflow will touch Unpaywall, ask for the user's real email up front. Never
  use placeholder emails.

### Step 3 — Select the search route

| Need | Route |
|---|---|
| Today's top arXiv papers matching interests | arXiv export API + profile scoring |
| Biomedical literature, initial paper set | PubMed E-utilities (esearch → esummary/efetch) |
| Manuscript packet (~60 verified references) | `research_lookup.py --academic` |
| Fast bounded web lookup | `research_lookup.py --no-academic` |
| Deep, exhaustive multi-source report | `research_lookup.py --force-backend research` |
| Full text behind paywall | PMC → DOI → Unpaywall → preprints |

### Step 4 — Build queries

**arXiv (from ResearchClaw):**

```
http://export.arxiv.org/api/query?search_query=all:{KEYWORD}&sortBy=submittedDate&sortOrder=descending&max_results=25&start=0
```

- Replace spaces in keywords with `+` (e.g., `large+language+models`)
- Run 2–4 queries covering different interest areas
- If the user has `seed_papers`, fetch their metadata via
  `https://export.arxiv.org/abs/{ARXIV_ID}` to calibrate what "relevant" means (topics,
  methods, problem framing)

**PubMed (from research-superpower):**

```
"BTK inhibitor"[Title/Abstract] AND selectivity[Title/Abstract]
("kinase inhibitor" OR "protein kinase") AND (selectivity OR "off-target")
"ibrutinib"[Title/Abstract] AND ("IC50" OR "inhibitory concentration")
```

- Field tags: `[Title/Abstract]`, `[Title]` (more precise), `[Author]`, `[Journal]`, `[Date]`
- AND narrows, OR broadens, NOT excludes
- Too few results → add OR synonyms, remove field tags; too many (>500) → add AND terms,
  field tags, date constraints, or split into sub-queries

### Step 5 — Execute searches with rate limiting

**PubMed E-utilities endpoints:**

```bash
curl "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=QUERY&retmax=100&retmode=json&sort=relevance"
curl "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id=PMID1,PMID2&retmode=json"
```

Start with `retmax=100`, `sort=relevance` (or `pub_date` for newest). Rate limits are
shared across all processes and subagents:

| Situation | Delay |
|---|---|
| Single agent/script | 500 ms (2 req/sec, safe margin) |
| 2 parallel subagents | 1 second each |
| 3 parallel subagents | 1.5 seconds each |
| 5 parallel subagents | 2.5 seconds each |
| HTTP 429 received | Wait 5 seconds, resume with doubled delays |

Formula: `delay_seconds = (num_parallel / rate_limit) + safety_margin`. Without an API
key the official limit is 3 req/sec; with a key, 10 req/sec.

### Step 6 — Fetch and parse metadata

**arXiv Atom XML fields** (from ResearchClaw): `<title>`, `<author><name>`,
`<summary>` (abstract), `<id>` (canonical URL), `<published>`, and
`<arxiv:primary_category term="cs.LG"/>`. Filter to papers published within the last 3
days; if fewer than 5 remain, extend to 7 days and note it.

**PubMed esummary fields** (from research-superpower): title, authors, journal,
publication date, PMID, and DOI from `articleids` (`{"idtype": "doi", "value": "..."}`).
If DOI is missing, use PMID as the fallback identifier. Use `efetch` instead of
`esummary` when full abstract text is needed.

Normalize into paper objects: `pmid`, `doi`, `title`, `authors`, `journal`, `year`,
`abstract`, `source`.

### Step 7 — Score, rank, and present (daily scout)

Score each arXiv paper 1–5 on relevance, sort descending, keep the Top 5 (or Top 10 if
asked), and render the digest:

```
📡 今日论文推荐 | Daily Paper Scout
🗓️ {DATE} | 匹配兴趣: {KEYWORDS}

1️⃣ **{Title}**
   👤 {First Author} et al. ({Year})
   🏷️ {category, e.g. cs.LG · cs.AI}
   💡 {One-sentence summary}
   🎯 相关原因: {why this matches the profile}
   🔗 {arXiv URL}

📝 想深读某篇？发链接说 "帮我读一下" | 加入待读说 "加入待读 [链接]"
```

### Step 8 — Retrieve full text (open access chain)

Try in order (from research-superpower):

1. **PubMed Central** — search `db=pmc` for the PMID, then fetch
   `https://pmc.ncbi.nlm.nih.gov/articles/PMCID/` (use `pmc.ncbi.nlm.nih.gov`, not www)
2. **DOI resolution** — `curl -L "https://doi.org/{doi}"`; check for a paywall (403,
   subscription required)
3. **Unpaywall (mandatory if paywalled)** —
   `curl "https://api.unpaywall.org/v2/{DOI}?email=USER_EMAIL"`; many paywalled papers
   have free versions in repositories or preprint servers
4. **Preprints directly** — bioRxiv (`https://www.biorxiv.org/content/10.1101/{doi}`),
   arXiv for computational papers

If no full text is available after Unpaywall, note it and continue with abstract-only
evaluation. If the user supplies a PDF or DOI instead of an arXiv ID, try extracting the
arXiv ID from the DOI or search arXiv by title.

### Step 9 — Learn from feedback (daily scout)

If the user reacts to a recommended paper (from ResearchClaw):

- Positive ("不错", "这个好", "有意思", "精读") → extract keywords from that paper's
  title/abstract, add to `learned_preferences.accept`
- Negative ("skip", "没意思", "不相关") → add to `learned_preferences.reject`

Update the config file immediately. For daily delivery, set a cron job at 9:00 AM
(Asia/Shanghai) with the prompt "推荐今日论文".

### Error handling

| Error | Handling |
|---|---|
| arXiv API returns empty results | Retry once with a broader query; if still empty, note "arXiv API temporarily unavailable" |
| arXiv HTML/PDF unavailable | Fall back to abstract-only mode; try `https://ar5iv.labs.arxiv.org/html/{ARXIV_ID}` |
| PubMed search empty | Broader terms, remove field tags, check typos, add OR synonyms |
| Unpaywall DOI not found | Check DOI format, try alternative identifiers |
| Network errors (Unpaywall) | Retry with exponential backoff, max 3 attempts |
| Config file missing | Use defaults silently; suggest "更新我的研究画像" |
| Reference shortfall (packet) | Inspect `coverage.json`; refine question, dates, terminology, or domains — never pad with weak duplicates |

## Techniques

### Relevance scoring rubric for daily scouting (from ResearchClaw)

| Signal | Score boost |
|---|---|
| Title contains exact keyword from user profile | +2 |
| Abstract contains ≥3 keyword matches | +1.5 |
| Author in `whitelist_authors` | +2 |
| Paper cites or builds on seed paper | +1.5 |
| Novel-contribution words ("propose", "novel", "outperform", "state-of-the-art", "benchmark") | +0.5 |
| Survey/review signal ("survey", "overview", "analysis of existing") | −1 |
| Topic in `learned_preferences.accept` | +1 |
| Topic in `learned_preferences.reject` | −2 |

### Six-pass academic search strategy (from claude-scientific-writer)

The academic pipeline runs bounded `advanced` search passes for: (1) recent peer-reviewed
primary studies; (2) systematic reviews, meta-analyses, and consensus evidence; (3)
seminal and foundational publications; (4) methods, protocols, validation, benchmarks,
and mechanisms; (5) contradictory, null, negative, replication, and limitation evidence;
(6) an unrestricted companion search when filtered passes fall short of the target. It
prioritizes PubMed/PMC, Europe PMC, Crossref, OpenAlex, Semantic Scholar,
arXiv/bioRxiv/medRxiv, and authoritative institutional sources.

### Reference quality rules (from claude-scientific-writer)

1. Deduplicate by DOI, PMID, canonical URL, and normalized title
2. Exclude retracted or withdrawn sources from claim support
3. Clearly identify preprints and lower confidence pending peer review
4. Prefer direct topical relevance and appropriate study design
5. Treat systematic reviews/meta-analyses and directly relevant controlled studies as
   strong evidence when their methods support the claim
6. Use citation counts, author reputation, and journal prestige only as secondary
   signals; they are age- and field-biased
7. Preserve contradictory and null evidence rather than optimizing for agreement
8. Never invent missing authors, venues, effect sizes, DOIs, or conclusions
9. Do not pad a shortfall with weak or duplicate records — report the gap and refine
10. Do not claim full-text review when only an abstract or paywalled landing page was
    available

### Unpaywall response interpretation (from research-superpower)

- `is_oa` — boolean; a free version exists
- `best_oa_location` — Unpaywall's recommended source, with `url_for_pdf` when available
- `oa_locations` — all known locations (repositories, preprint servers, institutional
  sites), ordered by quality
- Version priority: `publishedVersion` > `acceptedVersion` (author manuscript) >
  `submittedVersion` (preprint)
- Add ~100 ms delay between requests, cache responses, and only check papers you
  actually need. If `best_oa_location` is missing, fall back to the `oa_locations` array

## Scripts & Resources

### Skill sources (relative to the skill root)

- `scripts/ResearchClaw/skills/paper-scout/SKILL.md` — daily arXiv discovery workflow
- `scripts/claude-scientific-writer/research-lookup/scripts/research_lookup.py` —
  academic evidence pipeline
- `scripts/research-superpower/research/searching-literature/SKILL.md` — PubMed search
- `scripts/research-superpower/research/finding-open-access-papers/SKILL.md` — Unpaywall
  usage
- `scripts/research-superpower/getting-started/SKILL.md` — overview of the full research-superpower skill set (search → screen → extract → synthesize) and how the pieces chain together

### Manuscript evidence pipeline invocations (from claude-scientific-writer)

```bash
# Default academic packet (~60 verified references)
python scripts/claude-scientific-writer/research-lookup/scripts/research_lookup.py \
  "Evidence relevant to the manuscript's research question" \
  --academic --target-references 60 \
  --context-file manuscript-context.json \
  --packet-dir sources/manuscript-research --json

# Explicit deep research
python scripts/claude-scientific-writer/research-lookup/scripts/research_lookup.py \
  "Comprehensive review of the requested scientific topic" \
  --force-backend research --processor pro -o sources/deep-research.md

# Fast bounded lookup
python scripts/claude-scientific-writer/research-lookup/scripts/research_lookup.py \
  "Latest official guidance on the requested topic" \
  --no-academic --search-mode basic --json

# Batch mode (isolates failures by query)
python scripts/claude-scientific-writer/research-lookup/scripts/research_lookup.py \
  --batch "query one" "query two" "query three" \
  --academic --packet-dir sources/batch-research --json
```

The `--packet-dir` output contains: `packet.json`/`packet.md` (complete packet),
`references.json`/`references.bib` (citation-ready records), `evidence-matrix.json`,
`claim-source-map.json`, `synthesis.json` (consensus candidates, conflicts, gaps),
`section-briefs.json` (Introduction/Methods-rationale/Discussion evidence),
`coverage.json`, and `search-ledger.json` (objectives, filters, timestamps, counts).

### External APIs

- arXiv export API: `http://export.arxiv.org/api/query`
- PubMed E-utilities: `https://eutils.ncbi.nlm.nih.gov/entrez/eutils/`
- Unpaywall: `https://api.unpaywall.org/v2/{DOI}?email=EMAIL`
- Semantic Scholar (paper lookup): `https://api.semanticscholar.org/graph/v1/`
- Preprints: bioRxiv, medRxiv, arXiv, ChemRxiv
