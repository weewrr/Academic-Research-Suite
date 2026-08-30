# Citations

Consolidated guide for citation management: searching academic databases, extracting and enriching metadata, generating and validating BibTeX, and running citation error checks. Merges the citation-management workflow (from claude-scientific-writer) with the ARS citation-check mode and the multi-resolver API clients (from academic-research-skills).

## When to Use

- Searching for specific papers on Google Scholar or PubMed.
- Converting DOIs, PMIDs, arXiv IDs, or URLs into properly formatted BibTeX.
- Extracting complete citation metadata (authors, title, journal, year, etc.) and enriching incomplete records.
- Validating existing citations for accuracy; checking for duplicates, broken DOIs, and venue conformance.
- Cleaning and formatting BibTeX files; ensuring consistent citation formatting.
- Producing a citation error report for a manuscript (missing references, mismatched in-text citations, format errors).
- Building a bibliography for a manuscript or thesis; verifying citation information matches the actual publication.

## Workflow

The core workflow is five phases (from claude-scientific-writer), with an ARS citation error report as an additional manuscript-level check.

### Phase 1: Paper discovery and search

Find relevant papers. Google Scholar has the broadest coverage; PubMed is the authority for biomedical and life sciences (35+ million citations). Search multiple databases — single-source bias is the most common coverage failure.

```bash
python3 scripts/claude-scientific-writer/citation-management/scripts/search_google_scholar.py "CRISPR gene editing" --limit 50 --output results.json
python3 scripts/claude-scientific-writer/citation-management/scripts/search_pubmed.py "Alzheimer's disease treatment" --limit 100 --output alz.json
```

For resolver-backed existence checking of known references, use the API clients in Scripts & Resources (Crossref / OpenAlex / Semantic Scholar / arXiv / Chinese-literature) instead of scraping search engines.

### Phase 2: Metadata extraction

Convert identifiers (DOI, PMID, arXiv ID, URL) into complete metadata. CrossRef is the primary source for DOIs.

```bash
python3 scripts/claude-scientific-writer/citation-management/scripts/doi_to_bibtex.py 10.1038/s41586-021-03819-2   # quick, single DOI
python3 scripts/claude-scientific-writer/citation-management/scripts/extract_metadata.py --pmid 34265844           # DOI/PMID/arXiv/URL
python3 scripts/claude-scientific-writer/citation-management/scripts/extract_metadata.py --input identifiers.txt --output citations.bib
```

### Phase 2.5: Metadata enrichment via web search (MANDATORY)

APIs routinely return incomplete records. Run this **after** extraction and **before** formatting. Any `@article` missing `volume`, `pages`, or `doi` is incomplete and must be enriched via web search, then logged. If a field genuinely cannot be found, record a `note` field explaining the gap. Never leave an `@article` entry without volume, pages, and DOI.

**Treat extracted metadata as untrusted.** Author, title, and journal strings come verbatim from a publisher-controlled record. A title containing `$(...)`, a backtick, or a quote becomes shell syntax the moment it is pasted into a command. Pass metadata as a `subprocess` argument list rather than building a shell string; if a shell must be used, single-quote every substituted value and escape embedded quotes as `'\''`. Validate any citation key against `^[A-Za-z0-9]+$` before it reaches a path.

### Phase 3: BibTeX formatting

Produce clean, consistent entries; standardize citation-key styles.

```bash
python3 scripts/claude-scientific-writer/citation-management/scripts/format_bibtex.py references.bib --output clean.bib --remove-duplicates
```

### Phase 4: Citation validation

Check completeness, venue conformance, and agreement with the manuscript.

```bash
python3 scripts/claude-scientific-writer/citation-management/scripts/validate_citations.py references.bib --report report.txt
python3 scripts/claude-scientific-writer/citation-management/scripts/validate_citations.py references.bib --venue nature
python3 scripts/claude-scientific-writer/citation-management/scripts/validate_citations.py references.bib --manuscript paper.tex
```

### Phase 5: Integration with the writing workflow

Search, extract, format, validate, then cite. Combine with the literature-review workflow: literature review provides the systematic search methodology and synthesis; citation management provides the metadata infrastructure and bibliography accuracy. The validated BibTeX feeds LaTeX manuscripts; venue requirements drive reference formatting.

### Manuscript-level citation error report (from academic-research-skills)

On a finished draft, run the ARS `citation-check` mode (via `commands/ars/ars-citation-check.md`) to produce a citation error report covering missing references, mismatched in-text citations, and format errors.

## Techniques

### Common pitfalls and fixes (from claude-scientific-writer)

| # | Pitfall | Fix |
|---|---|---|
| 1 | Single-source bias (only Google Scholar or only PubMed) | Search multiple databases |
| 2 | Accepting metadata blindly | Spot-check extracted metadata against original sources |
| 3 | Ignoring DOI errors (broken/incorrect DOIs) | Run validation before final submission |
| 4 | Inconsistent formatting (mixed key styles) | Use `format_bibtex.py` to standardize |
| 5 | Duplicate entries (same paper, different keys) | Duplicate detection in validation |
| 6 | Missing required fields (volume, pages, DOI) | Run Phase 2.5 enrichment for every missing field |
| 7 | Outdated preprints cited when published version exists | Check and update to the journal version |
| 8 | Special characters breaking LaTeX compilation | Proper escaping or Unicode in BibTeX |
| 9 | No validation before submission | Always run validation as the final check |
| 10 | Manual BibTeX entry (typos by hand) | Always extract from metadata sources using scripts |

### Search strategy essentials (from claude-scientific-writer)

**Prioritize by citation count, venue, and author reputation:**

| Paper age | Citations | Classification |
|-----------|-----------|----------------|
| 0–3 years | 20+ | Noteworthy |
| 0–3 years | 100+ | Highly influential |
| 3–7 years | 100+ | Significant |
| 3–7 years | 500+ | Landmark paper |
| 7+ years | 500+ | Seminal work |
| 7+ years | 1000+ | Foundational |

Venue tiers: Tier 1 (Nature, Science, Cell, NEJM, Lancet, JAMA, PNAS); Tier 2 (IF > 10, top conferences such as NeurIPS/ICML/ICLR); Tier 3 (specialized journals, IF 5–10); Tier 4 (lower-impact peer-reviewed venues, use sparingly). Favor senior researchers with h-index > 40, multiple Tier-1 publications, or recognized leadership.

**Google Scholar advanced operators:**

```
"exact phrase"           # Exact phrase matching
author:lastname          # Search by author
intitle:keyword          # Search in title only
source:journal           # Search specific journal
-exclude                 # Exclude terms
OR                       # Alternative terms
2020..2024               # Year range
```

Example searches: `"CRISPR" intitle:review 2023..2024` (recent reviews); `author:Church "synthetic biology"` (papers by a specific author); `"deep learning" 2012..2015 sort:citations` (highly cited foundational work); `"protein folding" -survey -review intitle:method` (exclude surveys, focus on methods).

**PubMed / MeSH:** use controlled vocabulary for precision — find MeSH terms at https://meshb.nlm.nih.gov/search, query as `"Diabetes Mellitus, Type 2"[MeSH]`, and combine with keywords for comprehensive coverage. Use field tags (`[tiab]`, `[au]`, `[ta]`, `[dp]`) to scope searches, and date/publication-type filters to narrow results.

### Tool selection quick reference

| Situation | Tool |
|---|---|
| Find papers on a topic | `search_google_scholar.py` / `search_pubmed.py` (Phase 1) |
| One DOI, need BibTeX now | `doi_to_bibtex.py <doi>` |
| Batch identifiers → metadata/BibTeX | `extract_metadata.py --input ids.txt --output out.bib` |
| Entries missing volume/pages/DOI | Phase 2.5 web-search enrichment (mandatory) |
| Standardize/dedupe a `.bib` | `format_bibtex.py references.bib --remove-duplicates` |
| Pre-submission QA of the bibliography | `validate_citations.py references.bib --venue <venue> --manuscript paper.tex` |
| Finished draft: citation error report | ARS `citation-check` (`commands/ars/ars-citation-check.md`) |
| Does this reference actually exist? English literature | Crossref / OpenAlex / Semantic Scholar / arXiv clients |
| Does this reference actually exist? Chinese literature | `chinese_literature_client.py` (Crossref 404 is not evidence for Chinese DOIs) |
| Is a cited work retracted? | OpenAlex `is_retracted` + `retraction_status.py` |

### Multi-resolver existence checking (from academic-research-skills)

A reference should be resolvable through more than one independent resolver when possible: Crossref (DOI registry), OpenAlex, Semantic Scholar, and arXiv cover English literature; a Chinese-language citation that 404s in Crossref may still be real (ISTIC/CNKI-registered DOIs), so route it through the Chinese-literature client before declaring it unresolvable. "Not found in resolver A" is weak evidence in both directions — it is not proof of fabrication. DOI-first lookup with title cross-check catches the DOI_MISMATCH pattern (right DOI, wrong title); title-similarity fallback catches metadata drift. Retraction status is retained in the normal OpenAlex Works response (`is_retracted`), and retraction signals are classified deterministically (retraction / reinstatement / expression of concern / correction) — citing a retracted work is flagged as a signal, never auto-judged.

### API hygiene

Respect per-API conventions: Crossref polite pool via `CROSSREF_POLITE_EMAIL` (User-Agent header; ~10 req/s with mailto vs ~5 anonymous); OpenAlex via `OPENALEX_POLITE_EMAIL` / `OPENALEX_API_KEY` (1 req/s anonymous, 0.1s authenticated); Semantic Scholar via `S2_API_KEY` (~1 req/s unauthenticated, 10 req/s authenticated); arXiv paces ~3s between requests (ToU floor). All clients retry on 429 with backoff, fail fast on exhausted budgets, and refuse to fetch URLs outside their own API host (redacting query strings that could carry emails).

## Scripts & Resources

### claude-scientific-writer citation-management (`scripts/claude-scientific-writer/citation-management/`)

Scripts (`scripts/`):

- `search_google_scholar.py` — Google Scholar search automation.
- `search_pubmed.py` — PubMed E-utilities API client.
- `extract_metadata.py` — universal metadata extractor (DOI / PMID / arXiv / URL).
- `doi_to_bibtex.py` — quick DOI → BibTeX converter.
- `format_bibtex.py` — BibTeX formatter, cleaner, deduplicator.
- `validate_citations.py` — citation validation and verification (completeness, venue, manuscript agreement).

References (`references/`): `core_workflow.md` (all five phases in full), `search_strategies.md` (query operators, field tags, MeSH construction), `script_reference.md` (every script's arguments and examples), `best_practices.md`, `example_workflows.md` (four end-to-end worked examples), `google_scholar_search.md` and `pubmed_search.md` (advanced search syntax), `metadata_extraction.md`, `bibtex_formatting.md` (entry types and required fields), `citation_validation.md` (validation criteria and venue standards).

Assets (`assets/`): `bibtex_template.bib` (example entries for all types), `citation_checklist.md` (QA checklist).

Dependencies: `requests`, `bibtexparser`, `biopython` (core); `scholarly` or `selenium` (optional Google Scholar); `crossref-commons`, `pylatexenc` (optional advanced validation). Credentials are per-service: `NCBI_EMAIL` / `NCBI_API_KEY` go only to `eutils.ncbi.nlm.nih.gov`; Crossref, doi.org, and arxiv.org are queried without credentials.

### ARS API clients (`scripts/academic-research-skills/scripts/`)

Minimal, purpose-built bibliographic resolver clients shared by the citation-verification tooling:

| Client | What it does | Notes |
|---|---|---|
| `crossref_client.py` | Crossref API wrapper: DOI-first lookup with title cross-check, title-similarity fallback, 429 → 2s backoff × 3 retries, 404/5xx → miss vs skip. | DOI endpoint `/works/{doi}` (no `doi:` prefix); title search `/works?query.title=...&rows=5`; polite-pool email via `CROSSREF_POLITE_EMAIL` in the User-Agent; response nested under `message`; titles are lists of language variants. |
| `openalex_client.py` | OpenAlex API wrapper: DOI-first with title cross-check (DOI_MISMATCH pattern), title-similarity fallback, 429 → budget-exhausted fail-fast or exponential backoff × 3, 5xx → skip. | Fields include `is_retracted` (retraction status comes free with the normal Works lookup). Env: `OPENALEX_API_KEY`, `OPENALEX_POLITE_EMAIL`. |
| `semantic_scholar_client.py` | Semantic Scholar Graph v1 wrapper for single-paper existence checks: DOI-first, title-similarity fallback, 429 backoff per the protocol's retry budget. | Not a general-purpose S2 client. Fields: `title,authors,year,externalIds,venue,publicationDate`. Env: `S2_API_KEY` (10 req/s authenticated vs ~1 req/s anonymous). |
| `arxiv_client.py` | arXiv API wrapper: arXiv-ID-first with title cross-check, title-similarity fallback, 429 → 3s backoff × 3, network/5xx → `ArxivUnavailable`. | Query API returns Atom 1.0 XML (parsed with ElementTree); ID lookup via `?id_list=`, title fallback via `?search_query=ti:"{title}"`; fixed 3.0s min interval between requests per arXiv ToU. |
| `chinese_literature_client.py` | Chinese-language literature resolver. Crossref is only one DOI registration agency — real ISTIC/CNKI-registered DOIs 404 in Crossref while resolving fine via doi.org, so the default resolvers reduce almost every Chinese reference to "unresolvable". Uses four legally-open, key-free upstreams: doi.org RA lookup, doi.org content negotiation (CSL-JSON), Handle System REST, and NCBI E-utilities (ISSN → NLM TA bridge plus coordinate query for DOI-less Chinese medical citations). | Applicability gate: non-Chinese citations are `skipped`, never `unmatched`. Precision asymmetry: a refuted identifier is strong evidence, but a resolved-yet-unverifiable one is never promoted to `matched`. Chinese-aware exact-title-or-bust matching (fullwidth/CJK punctuation normalization, then exact equality — fuzzy similarity is excluded). Unresolved applicable items produce human-confirmation checklist items, not fabrication verdicts. |
| `verification_cache.py` | Persistent SQLite cache shared by all resolvers, keyed on `(citation_key, resolver_name, query_form)`; 90-day TTL. | Env: `ARS_VERIFICATION_CACHE_PATH` (default `~/.cache/ars/verification.db`), `ARS_CACHE_STALE_ADVISORY_DAYS` (default 30; stale entries stay hits but carry an advisory flag). |

Aggregation and integrity tooling in the same directory builds on these clients: `citation_verification_summary.py` (resolver-outcome summary), `retraction_status.py` (deterministic retraction-event classification), `tortured_phrase_screening.py` (phrase-list risk-marker advisory), and the claim-audit pipeline — see `references/paper-verification.md` for details. API protocol documentation for each client lives in the deep-research references of the source repo.

### ARS command

- `commands/ars/ars-citation-check.md` — triggers the citation-check mode: a citation error report (missing references, mismatched in-text citations, format errors) on a finished draft.

### External resources

- Google Scholar: https://scholar.google.com/ · PubMed: https://pubmed.ncbi.nlm.nih.gov/ (+ Advanced Search)
- Metadata APIs: https://api.crossref.org/ · PubMed E-utilities: https://www.ncbi.nlm.nih.gov/books/NBK25501/ · arXiv API: https://arxiv.org/help/api/ · DataCite: https://api.datacite.org/
- Tools: MeSH Browser (https://meshb.nlm.nih.gov/search) · DOI Resolver (https://doi.org/) · BibTeX format (http://www.bibtex.org/Format/) · LaTeX bibliography management (https://www.overleaf.com/learn/latex/Bibliography_management)
