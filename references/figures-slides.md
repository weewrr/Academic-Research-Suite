# Figures, Schematics, Slides, and Posters — Visual Generation Reference

Merged guide covering six visual-generation capabilities: **infographics**,
**scientific-schematics**, **scientific-slides**, **latex-posters**, **pptx-posters**,
and **generate-image**. All AI generation flows through the OpenRouter API and needs the
`OPENROUTER_API_KEY` environment variable (get a key at https://openrouter.ai/keys).

## When to Use

| Need | Capability |
|---|---|
| Technical/scientific diagrams: flowcharts, neural-network architectures, biological pathways, circuit diagrams, CONSORT/PRISMA methodology figures | scientific-schematics |
| Data-driven infographics: statistics, timelines, comparisons, process maps, hierarchies, social content | infographics |
| Full slide decks for research talks (conference, seminar, defense, journal club) | scientific-slides |
| LaTeX research posters (beamerposter / tikzposter / baposter) | latex-posters |
| Editable, print-ready PowerPoint posters with strict provenance and security gates | pptx-posters |
| Photos, illustrations, concept art, logos, image editing and compositing from references | generate-image |

Shared principles across all six:

- **Generate–review–refine loop.** Nano Banana (image model) generates; Gemini 3.6 Flash
  reviews quality against a document-type threshold; the loop stops as soon as the score
  passes, so a simple figure usually costs one iteration.
- **Accessibility is not optional.** Colorblind-safe palettes (Okabe-Ito / Wong),
  redundant encoding (shapes + patterns + line styles, never color alone), grayscale
  test, minimum readable font size at final output size.
- **Prompt specificity.** State the diagram type, the exact components, flow direction,
  labels, and style. "Make a flowchart" fails; "CONSORT flowchart, 500 screened, 150
  excluded, 350 randomized, left-to-right flow" works.
- **Vector preferred for publication.** PDF/SVG for LaTeX and journals; PNG at 300+ DPI
  as raster fallback.

## Workflow

1. **Route the request** using the table above. Decide the deliverable format first:
   PDF slides vs editable PPTX; LaTeX poster vs editable PowerPoint poster.
2. **Plan every visual before generating.**
   - Slides: one plan block per slide (title, 3-4 key points, visual element, citations).
   - Posters: 1-3 core messages, 3-6 main visuals, 300-800 words total, at most 5-6 sections.
   - Infographics: pick `--type` and `--style`; add `--research` when facts must be current.
3. **Generate the visuals** with the appropriate script (see Techniques). Always attach
   the previous slide (`--attach`) when generating a slide deck so styling stays uniform.
4. **Review gates** (automatic via quality thresholds; manual and mandatory for posters):
   - *Before generating:* each planned graphic is 3-4 items, one message, under the word
     limit — otherwise split it.
   - *After generating:* open at 25% zoom; all text readable, few enough elements,
     50%+ white space, understandable in 2 seconds. Any failure means regenerate or split.
5. **Assemble the deliverable:** compile LaTeX (`pdflatex`/`lualatex`), combine slide
   images to PDF (`slides_to_pdf.py`), run the pptx-poster generator, or build a PPTX
   via the office-documents guide.
6. **Final QA:** overflow is an error, not a warning. Validate packages/files, inspect
   all four edges at 100% zoom, verify cross-references and captions.

## Techniques

### Quality thresholds and smart iteration (from infographics / scientific-schematics)

The reviewer stops iterating as soon as quality ≥ threshold, so thresholds encode how
much quality each deliverable needs:

| Document type | Threshold (schematics) | Document type | Threshold (infographics) |
|---|---|---|---|
| journal | 8.5/10 | marketing | 8.5/10 |
| conference / thesis / grant | 8.0/10 | report | 8.0/10 |
| preprint / report | 7.5/10 | presentation | 7.5/10 |
| poster | 7.0/10 | social / internal | 7.0/10 |
| presentation | 6.5/10 | draft | 6.5/10 |
| default | 7.5/10 | default | 7.5/10 |

Pass `--doc-type` to control the bar; pass `--iterations` (max 2) to allow more
refinement rounds. Output is versioned images plus a review log with scores, critiques,
and early-stop information.

### Infographic types, styles, and research mode (from infographics)

- Ten types via `--type`: `statistical`, `timeline`, `process`, `comparison`, `list`,
  `geographic`, `hierarchical`, `anatomical`, `resume`, `social`.
- Eight industry styles via `--style`: `corporate`, `healthcare`, `technology`,
  `nature`, `education`, `marketing`, `finance`, `nonprofit`.
- Colorblind-safe palettes via `--palette`: `wong` (most recommended), `ibm`, `tol`.
- `--research` enables a Perplexity Sonar phase that gathers 5-8 current facts,
  statistics, dates, and sources (prioritizes 2023-2026) and bakes them into the prompt;
  results land in `{name}_research.json`. Use it for statistical/market/scientific
  accuracy; skip it for conceptual or speed-critical work.
- Good prompts name the content and the numbers: "Market growth from $10B (2020) to
  $45B (2025), CAGR 35%", not "market is growing".

### Scientific diagram standards (from scientific-schematics)

- Scientific quality guidelines applied automatically: clean white/light background,
  high contrast, ≥10pt labels, sans-serif typography, Okabe-Ito palette, proper spacing,
  scale bars/legends/axes where appropriate.
- Technical requirements for publication: vector (PDF/SVG) preferred or 300+ DPI raster;
  PDF for LaTeX, SVG for web, PNG fallback; RGB digital / CMYK print; line weights
  ≥0.5pt (typically 1-2pt); text ≥7-8pt at final size.
- Pre-submission checklist: no overlapping elements, arrows connect properly,
  grayscale-safe, consistent styling with other manuscript figures, comprehensive
  caption defining all abbreviations, referenced in the narrative, journal dimension and
  format requirements met, source files and quality reports archived in version control.

### Slide generation protocol (from scientific-slides)

Two workflows:

- **PDF slides (default, recommended):** plan each slide → generate each slide as a
  complete image with `generate_slide_image.py` → combine with `slides_to_pdf.py`.
- **PPTX workflow:** generate visuals with `--visual-only`, then build the deck and add
  text separately with the PPTX capability (see office-documents.md).

Formatting-consistency protocol (critical):

1. Define one FORMATTING GOAL (colors, typography, visual style, layout) and repeat it
   in EVERY prompt.
2. Always `--attach` the previous slide so the model matches the established style.
3. Put citations directly in the prompt: "CITATIONS: Include at bottom: (LeCun et al.,
   2015; Goodfellow et al., 2016)".
4. Before results slides, list `figures/`, `results/`, `plots/` and attach the actual
   data figures (`--attach` is repeatable); describe in the prompt how to incorporate them.
5. Attach diagrams for methodology slides and logos/institutional images for title slides.

Prompt template:
```
[Slide content description]
CITATIONS: Include at bottom: (Author1 et al., Year; Author2 et al., Year)
FORMATTING GOAL: [background], [text color], [accent color], minimal professional
design, no decorative elements, consistent with attached slide style.
```

Deck principles: visual-first (every slide has a figure/image/diagram), 3-4 bullets of
4-6 words at 24-28pt, 40-50% white space, 40-50% of time on results, ~1 slide per
minute, practice 3-5 times, 7:1 contrast preferred. A 15-minute conference talk targets
15-18 slides and roughly 5-6 hours total (plan → generate → review → practice → finalize).

### LaTeX poster hard limits (from latex-posters)

Standard workflow: generate ALL major visuals with AI first, targeting 60-70% of poster
area as visuals and 30-40% text. These are limits, not guidelines — violating them is
the most common cause of a failed poster:

| Constraint | Limit |
|---|---|
| Elements per AI-generated graphic | 3-4 maximum (3 ideal) |
| Words per graphic | 10 maximum |
| White space per graphic | 50% minimum (60% better) |
| Key numbers / metrics | 120pt+ |
| Labels | 80pt+ |
| Body text on the poster | 24pt+ |
| Content sections (A0) | 5-6 maximum |
| Total words on the poster | 300-800 |
| Figure width | `0.85\linewidth`, never `1.0` |

Every graphic prompt must include: `POSTER FORMAT for A0`, an explicit element/word
count (`ONLY 3 icons`, `3 words total`), a font size (`GIANT (120pt+)`), `60% white
space`, and a viewing distance (`readable from 10-12 feet`).

Two mandatory review gates (see Workflow step 4). Patterns that always fail: 7-stage
workflows, timelines with annual milestones, 3 case studies in one graphic, comparisons
of 5+ methods, architectures with all layers — collapse each to 3 high-level items or
split into separate graphics.

Package choice: **beamerposter** (Beamer syntax, institutional themes), **tikzposter**
(modern, colorful, flexible), **baposter** (structured multi-column). Install with
`tlmgr install beamerposter tikzposter baposter` (MiKTeX auto-installs). Six-stage
process: plan content → generate visuals → design layout (title 10-15%, content 70-80%,
footer 5-10%) → integrate content (bullets, active voice, QR codes, 5-10 key references)
→ test readability (print at 25% scale, read from 2-3 feet) → compile and deliver
(`pdflatex` or `lualatex`; embed fonts; test print before professional printing).

### PPTX poster manifest pipeline (from pptx-posters)

Use only when the deliverable is an editable PowerPoint poster. Version 2.x generates a
real one-slide `.pptx` from a strict local JSON manifest — no HTML conversion, external
templates, network requests, or figure styles. Requires Python 3.10+, uv, and exact pins
`python-pptx==1.0.2`, `Pillow==12.3.0`, `lxml==6.1.1`.

Hard gates — stop instead of guessing when any is unmet:

1. Author has not supplied exact poster content and source records.
2. Any claim, number, citation, author, affiliation, funding statement, figure, license,
   or QR target is unresolved.
3. Current conference and printer requirements are not confirmed.
4. Author approval is not bound to the current manifest content hash.
5. An asset is remote, outside the manifest directory, unhashed, or unapproved.
6. An input is `.pptm`, contains macros/external relationships/OLE/embedded files, or is
   an untrusted template.
7. The workflow needs PowerPoint to be opened or executed automatically.
8. A script reports a package, layout, DPI, contrast, or output-plan blocker.

Never fabricate missing material or leave a plausible placeholder; drafts fail closed.

Pipeline: copy `assets/poster_manifest_template.json` → fill every source ID, SHA-256
hash, alt text, reading order, contrast pair, and conference/printer rule → obtain the
content hash with `validate_manifest.py --print-content-hash` → author approves →
validate manifest → audit assets (`inventory_images.py`, `check_palette.py`,
`plan_export.py`) → generate (`generate_poster.py`) → technical audits
(`inspect_pptx.py`, `check_layout.py`) → manual PowerPoint/accessibility gate → export
PDF at Standard/high print quality and verify independently.

Key facts: effective DPI is pixels ÷ final placed inches (not metadata DPI); contrast
uses WCAG 2.2 sRGB math as a design target; PowerPoint custom dimensions are limited to
1-56 inches; the generator refuses overlaps, out-of-bounds shapes, low final font
size/DPI, and unsafe packages; automation cannot certify accessibility — run
Review > Check Accessibility, inspect reading order, test every QR code, and get author
sign-off manually.

### General image generation and editing (from generate-image)

One script reaches Gemini, FLUX, Seedream, Recraft, GPT-Image, and ~30 more models
behind one request shape (Python 3.9+, standard library only, bills per request;
`--list-models` needs no key).

| Need | Model |
|---|---|
| General quality, prompt adherence (default) | `google/gemini-3.1-flash-image` |
| Highest Gemini tier | `google/gemini-3-pro-image` |
| Photoreal control, reproducible seeds | `black-forest-labs/flux.2-pro` |
| Cheap iteration | `black-forest-labs/flux.2-klein-4b` |
| Several images per request | `bytedance-seed/seedream-4.5`, `openai/gpt-image-2` |
| Vector / SVG output | `recraft/recraft-v4-vector` |
| Transparent background | `openai/gpt-image-2` with `--background transparent` |

Parameter support varies by model — sending an unsupported parameter is **rejected, not
ignored**: `--resolution` (not FLUX), `--output-format` (FLUX only), `--quality` /
`--background` / `--output-compression` (OpenAI family), `--seed` (FLUX/Seedream/Krea,
not Gemini/OpenAI), `--n` (capped: 1 Gemini/FLUX, 6 Recraft, 10 Seedream/OpenAI).
`--aspect-ratio` is nearly universal but the allowed enum differs.

Editing: `-i/--input` is repeatable and accepts local paths, HTTP(S) URLs, or data URLs
(reference limits: 16 OpenAI, 14 Gemini/Seedream, 8 FLUX, 1 Recraft/MAI). Prompt the
edit specifically ("change the sky to sunset colours" beats "edit the sky"). Output
defaults to `generated_image.<ext>` following the returned media type. Prefer a cheap
model while iterating; reference images are uploaded to OpenRouter, so never send
unpublished or sensitive data; never hardcode the API key.

## Scripts & Resources

All scripts require Python 3 (run with `python3`/`python`). Paths relative to skill root:

| Script | Purpose |
|---|---|
| `scripts/claude-scientific-writer/infographics/scripts/generate_infographic.py` | Generate infographics (types, styles, palettes, `--research`, thresholds) |
| `scripts/claude-scientific-writer/scientific-schematics/scripts/generate_schematic.py` | Generate publication-quality scientific diagrams |
| `scripts/claude-scientific-writer/scientific-schematics/scripts/generate_schematic_ai.py` | AI-generation variant of the schematic pipeline |
| `scripts/claude-scientific-writer/scientific-slides/scripts/generate_slide_image.py` | Generate full slide images (`--attach`, `--visual-only`) |
| `scripts/claude-scientific-writer/scientific-slides/scripts/slides_to_pdf.py` | Combine slide PNGs into a PDF deck |
| `scripts/claude-scientific-writer/scientific-slides/scripts/pdf_to_images.py` | Convert a PDF deck back to images for visual review |
| `scripts/claude-scientific-writer/scientific-slides/scripts/validate_presentation.py` | Presentation validation |
| `scripts/claude-scientific-writer/latex-posters/scripts/generate_schematic.py` | Poster-oriented diagram generation |
| `scripts/claude-scientific-writer/latex-posters/scripts/review_poster.sh` | Poster review and validation (bash) |
| `scripts/claude-scientific-writer/pptx-posters/scripts/validate_manifest.py` | Strict manifest/provenance/approval validator (also `--print-content-hash`) |
| `scripts/claude-scientific-writer/pptx-posters/scripts/generate_poster.py` | Exact-pinned local PPTX poster generator |
| `scripts/claude-scientific-writer/pptx-posters/scripts/inspect_pptx.py` | Non-executing ZIP/XML security inspector |
| `scripts/claude-scientific-writer/pptx-posters/scripts/check_layout.py` | Bounds, overlap, reading-order, final-font checker |
| `scripts/claude-scientific-writer/pptx-posters/scripts/inventory_images.py` | Asset hash/metadata/effective-DPI manifest |
| `scripts/claude-scientific-writer/pptx-posters/scripts/check_palette.py` | WCAG contrast and palette report |
| `scripts/claude-scientific-writer/pptx-posters/scripts/plan_export.py` | Export/print preflight plan |
| `scripts/claude-scientific-writer/generate-image/scripts/generate_image.py` | OpenRouter multi-model image generation and editing |

Environment: `export OPENROUTER_API_KEY='...'` for every AI generation script.
pptx-posters additionally pins `python-pptx==1.0.2`, `Pillow==12.3.0`, `lxml==6.1.1` in
a uv environment.

Key upstream reference files (bundled in this suite under
`scripts/claude-scientific-writer/`):
infographics `scripts/claude-scientific-writer/infographics/references/infographic_type_catalog.md`,
`design_principles.md`,
`color_palettes.md`; scientific-slides `scripts/claude-scientific-writer/scientific-slides/references/slide_capabilities.md`,
`presentation_workflow.md`, `prompt_writing.md`, `common_pitfalls.md` and assets
(beamer conference/seminar/defense templates, PowerPoint design guide, timing
guidelines); latex-posters `scripts/claude-scientific-writer/latex-posters/references/ai_graphics_for_posters.md`,
`latex_poster_reference.md`, `compilation_and_quality_control.md` and templates in
`assets/`; pptx-posters `scripts/claude-scientific-writer/pptx-posters/references/manifest_spec.md`, `pptx_security.md`,
`poster_quality_checklist.md` (assets); generate-image `scripts/claude-scientific-writer/generate-image/references/models.md`.
