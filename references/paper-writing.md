# Paper Writing — Consolidated Methodology

Merged guide covering five source methodologies for drafting, revising, and
quality-checking academic papers: reviewer-friendly rewriting
(research-paper-writing), precision + notation + figure discipline (phd-skills),
evidence-bound scientific writing (claude-scientific-writer), full pipeline
writing with anti-patterns (academic-research-skills), and outline/draft/review/
rebuttal assistance (ResearchClaw paper-writer).

## When to Use

- Drafting or revising any paper section (Abstract, Introduction, Related Work,
  Method, Experiments, Conclusion, Discussion).
- Restructuring a full draft, generating a paper outline, or planning a paper
  step by step ("write a paper", "论文大纲", "guide my paper").
- Polishing figures and tables, or checking claim-support alignment before
  submission.
- Auditing notation consistency, citation completeness, or evidence provenance.
- Performing adversarial self-review or simulated peer review of a draft.
- Drafting point-by-point rebuttals to reviewer comments.

Do NOT use for: pure literature discovery (use paper scout / reading list
guides), idea generation (see `idea-generation.md`), or structured external
review of someone else's paper (use the review guide).

## Workflow

A merged end-to-end pipeline. Enter at whichever stage matches the request.

1. **Clarify the story before sentence-level edits.**
   - Identify paper type, target venue, audience, citation format, output
     format (LaTeX / DOCX / PDF / Markdown), language, and word-count target
     (from academic-research-skills intake).
   - Answer: What technical problem do we solve? Why is there no established
     solution? What is the contribution? Why does it work in essence?
   - Build a mini-outline before drafting prose (from research-paper-writing).

2. **Gather evidence first.**
   - List claims that the paper will make; map each to its source (experiment
     result, table, proof, or citation).
   - Never state a result without pointing to its source; mark unverified
     items explicitly (`% TODO: verify this number`) rather than inventing
     plausible values (from phd-skills / claude-scientific-writer).

3. **Draft section by section.**
   - Load only the section guide needed for the current edit target; do not
     load all section references at once (from research-paper-writing). See
     `paper-sections.md` in this skill for the distilled per-section playbook.
   - Read the existing section before writing; identify the claim of each
     paragraph; find the evidence; write; re-read against source to catch
     drift (from phd-skills).
   - For each subsection include motivation, design, and technical advantage
     where applicable (from research-paper-writing).

4. **Maintain notation and terminology consistency.**
   - Follow the Notation Consistency Protocol below (from phd-skills).
   - Keep terminology stable across the full paper; avoid writing style that
     looks like incremental patching of a naive baseline (from
     research-paper-writing).

5. **Refine figures and tables.**
   - Follow the Figure Refinement Methodology below (from phd-skills) and the
     booktabs table rules in `paper-sections.md`.

6. **Run claim-evidence alignment.**
   - Check every major claim in Abstract/Introduction against experimental
     evidence. If a claim cannot be supported, weaken or remove it (from
     research-paper-writing).
   - Every factual or numeric claim must map to a verifiable source; search
     snippets and generated summaries aid discovery but do not verify (from
     claude-scientific-writer).

7. **Run adversarial self-review.**
   - Read the draft as a skeptical reviewer; answer the five-dimension
     self-review question list (contribution, writing clarity, experimental
     strength, evaluation completeness, method design soundness); mark each
     item pass / needs revision / needs new experiment; revise until no major
     rejection risk remains (from research-paper-writing).
   - Optionally produce a structured auto-review report with overall score,
     critical issues, major concerns, minor suggestions, strengths, and
     dimension scores (from ResearchClaw paper-writer).

8. **Handle revision and rebuttal.**
   - Classify each reviewer comment (major / moderate / minor; editorial,
     scientific, statistical, or policy).
   - Draft point-by-point responses following the Rebuttal Principles below
     (from ResearchClaw paper-writer).
   - Revise registries/evidence before prose when facts change; re-run audits
     (from claude-scientific-writer).
   - Do not accept feedback uncritically: use a REVIEWER_DISAGREE status with
     evidence when the reviewer is wrong; avoid scope creep during revision
     (from academic-research-skills).

9. **Prepare declarations and formatting.**
   - Include mandatory statements where applicable: data availability, ethics
     declaration, author contributions (CRediT), conflict of interest,
     funding, AI-use disclosure (from academic-research-skills /
     claude-scientific-writer).
   - Deliver LaTeX-ready output matching the paper's existing style; verify
     citation format compliance and DOI inclusion (from phd-skills /
     academic-research-skills).

## Techniques

### Paragraph Clarity Check (from research-paper-writing)

Use whenever a paragraph "doesn't flow":

1. Read as an external reader:
   - Does the paragraph have one explicit message?
   - Does the first sentence state what the paragraph will do?
   - Are all key nouns/terms readable without hidden context?
   - Does each sentence connect to the previous with a clear relation (cause,
     contrast, consequence, refinement, example)?
2. Reverse outline the current section:
   - Write the thesis/main claim, each paragraph's topic sentence, and the
     evidence under each paragraph.
   - Check mapping: topic sentence -> thesis; evidence -> topic sentence.
   - Revise or remove any paragraph that cannot be mapped cleanly.
3. If flow is still weak, add temporary section headers and explicit
   transitions during revision; remove unnecessary headers before finalizing.

Global principles: one paragraph = one message; state the message in the first
sentence; make nouns self-contained (define new terms before reuse); treat
visual quality (teaser figure, pipeline figure, clean tables) as core content,
not decoration.

### Notation Consistency Protocol (from phd-skills)

1. Read existing notation definitions before writing.
2. Use EXACTLY the same symbols; do not introduce synonyms.
3. Check new symbols do not clash with existing ones.
4. Maintain a notation table if the paper has one.

Common pitfalls: using both $x$ and $\mathbf{x}$ for the same concept; defining
$N$ as dataset size in methods but using $n$ in experiments; inconsistent
subscript conventions ($f_i$ vs $f(i)$).

### Figure Refinement Methodology (from phd-skills)

1. **Specification capture** — before modifying any figure record: exact data
   source, takeaway message, hard constraints (font size >= 8pt, column
   width, color scheme), and what must be preserved.
2. **Constraint preservation** — track revisions explicitly:
   ```
   Constraints for Figure N:
   - [KEEP] Y-axis range 0-100
   - [KEEP] Color scheme: blue=ours, gray=baselines
   - [CHANGE] Legend position: inside -> outside
   - [ADD] Error bars from std_results.json
   ```
3. **Variant generation** — produce 2-3 side-by-side variants, each changing
   ONE visual aspect; let the user choose.
4. **Visual verification** — always inspect the generated image; check data
   values match source, labels/legends are correct, and the takeaway is clear
   at a glance.

### No-Fabrication Rules (from claude-scientific-writer)

Never invent or complete: citations, DOIs, URLs, quotations; results, data
values, sample sizes, units, statistical tests; methods, materials, protocol
details, software versions; registrations, approvals, consent statements;
authors, author order, CRediT roles; funding, conflicts, data availability, or
AI disclosures. Use an explicit missing/unverified state instead of plausible
boilerplate. Do not send unpublished manuscripts or peer-review material to
external services without documented authorization. Preserve uncertainty;
distinguish confirmatory vs exploratory work; report negative and
inconclusive findings that belong to the study record; do not convert
association into causation.

### Evidence Binding (from claude-scientific-writer)

- Assign `E` IDs to sources and `C` IDs to claims; append `[claim:C001]
  [evidence:E001,E002]` markers during drafting.
- A human verifier must open each source and confirm the exact support before
  it is marked verified.
- Outline only from recorded evidence; keep unsupported content in an
  unresolved-issues list, not in manuscript prose.

### Anti-Patterns (from academic-research-skills)

| # | Anti-Pattern | Correct Behavior |
|---|--------------|------------------|
| 1 | AI-typical overused terms ("delve into", "crucial", "it is important to note") | Use discipline-specific vocabulary |
| 2 | Em dash abuse (>2 per page) | Use parentheses, commas, or restructure |
| 3 | Throat-clearing openers ("In this section, we will discuss...") | Start with the claim or finding |
| 4 | Uniform paragraph lengths (all 4-5 sentences) | Vary naturally (2-8 sentences) |
| 5 | Fabricated citations (IRON RULE) | Every citation verified via DOI or web search |
| 6 | Sycophantic revision (accepting all feedback) | Disagree with evidence when reviewer is wrong |
| 7 | Scope creep during revision | Address reviewer concerns only; new content needs approval |
| 8 | Ignoring desk-reject signals | Check failure paths; recover or restructure |

### Bilingual Abstract Quality (from academic-research-skills)

- zh-TW and EN abstracts are independently composed, not mechanical
  translations.
- Both cover the same key points in the same order.
- Keywords: 5-7 per language; EN 150-300 words, zh-TW 300-500 characters.

### Rebuttal Principles (from ResearchClaw paper-writer)

- Never be defensive — acknowledge valid concerns directly.
- Be concrete — cite specific table/figure numbers or added experiments.
- Be concise — 3-5 sentences per response.
- Distinguish "We will add X in revision" vs "This is already addressed in
  Section Y".
- Use polite academic framing ("We appreciate the reviewer's concern...",
  "To clarify...").
- Parse comments first: identify each reviewer, each distinct concern, and
  tone (positive / neutral / hostile); categorize as major (challenges core
  claims), moderate (requests experiments/clarification), or minor.

### Output Contract (from research-paper-writing)

When asked to rewrite or draft sections, return:

1. A compact section outline (3-7 bullets).
2. Revised paragraphs with explicit paragraph roles (opening / challenge /
   method / advantage / evidence / limitation).
3. A short self-review checklist (clarity, flow, terminology consistency,
   unsupported claims, missing evidence).
4. A claim-evidence map per major claim:
   `Claim: ... | Evidence: ... | Status: supported/needs evidence`.

## Scripts & Resources

- Section-specific writing guides and example bank:
  `scripts/research-paper-writing/references/` (introduction.md, abstract.md,
  method.md, experiments.md, related-work.md, conclusion.md, paper-review.md,
  does-my-writing-flow-source.md); examples in
  `scripts/research-paper-writing/references/examples/`.
- phd-skills paper-writing methodology (source of notation and figure
  protocols): `scripts/phd-skills/skills/paper-writing/`.
- claude-scientific-writer scientific writing (evidence workflow, IMRAD,
  reporting guidelines, citation styles, figure/table rules, local CLI tools
  such as `audit_claims.py`, `check_consistency.py`, `check_references.py`,
  `lint_manuscript.py`): `scripts/claude-scientific-writer/scientific-writing/`.
- academic-research-skills 12-agent pipeline (intake, literature strategist,
  structure architect, argument builder, draft writer, citation compliance,
  bilingual abstract, peer reviewer, formatter, socratic mentor,
  visualization, revision coach): `scripts/academic-research-skills/academic-paper/agents/*.md`
  with references (writing_quality_check, abstract_writing_guide,
  intro_title_rhetoric_guide, disclosure_mode_protocol, and others) and
  templates (imrad, literature_review, case_study, bilingual_abstract, and
  others) under `scripts/academic-research-skills/academic-paper/`.
- ResearchClaw paper-writer (outline template, auto-review rubric, rebuttal
  format): `scripts/ResearchClaw/skills/paper-writer/`.
- Per-section distilled playbook (companion guide in this skill):
  `references/paper-sections.md`.
