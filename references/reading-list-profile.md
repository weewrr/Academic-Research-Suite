# Reading List & Research Profile — Consolidated Guide

Merged from two ResearchClaw skills: reading-list (kanban-style paper
management with an HTML dashboard) and research-profile (maintaining and
visualizing the user's research taste profile). The profile personalizes
every other capability; the reading list is the corpus that feeds idea
generation and paper writing.

## When to Use

- "我的论文列表" / "reading list" / "加入待读 [link]" / "标记已读 [paper]" /
  "移除 [paper]" / "开始阅读 [paper]" — manage the personal reading list.
- "更新我的研究画像" / "我的研究画像" / "research profile" — view or update the
  research preference profile, or regenerate its HTML visualization.
- Auto-learning preferences from user feedback on paper recommendations.
- Any other capability that needs personalization (idea generation, paper
  scout) — Step 0 below always runs first.

## Workflow

### Step 0 — Read the research profile (always first)

Load the user's research profile before running ANY capability. If the config
file does not exist, use the defaults silently and mention at the end that
the user can customize their interests by updating the profile.

Default profile used when no config is found:

```yaml
research_direction: "Large language models, reinforcement learning, agentic AI"
seed_papers: []
keywords:
  - large language models
  - reinforcement learning
  - agentic AI / AI agents
  - retrieval-augmented generation
  - multimodal models
whitelist_authors: []
learned_preferences:
  accept: []
  reject: []
```

Config fields reference:

- `research_direction` — free-text description of the user's research focus.
- `seed_papers` — arXiv IDs the user considers gold-standard references.
- `keywords` — interest topics used for paper-scout search queries.
- `whitelist_authors` — researcher names to prioritize in recommendations.
- `learned_preferences.accept` — keywords/topics the user has explicitly
  liked.
- `learned_preferences.reject` — keywords/topics the user has skipped or
  disliked.
- `topic_stats` — per-topic paper counts and percentages (feeds the profile
  HTML chart).

### Step 1 — Reading list operations

Maintain a JSON data file (`research-claw-reading-list.json` in the
workspace). Schema:

```json
{
  "last_updated": "2026-03-26",
  "papers": [
    {
      "arxiv_id": "2503.19823",
      "title": "AutoRefine: Search and Refine During Think",
      "authors": "Shi et al.",
      "date_added": "2026-03-26",
      "status": "to_read",
      "score": 4.5,
      "tags": ["LLM Reasoning", "RAG"],
      "note_link": "research-claw-output/2503.19823.html"
    }
  ]
}
```

Status values: `"to_read"` · `"reading"` · `"done"`.

Operations:

- **View list** — load the JSON, count per status, regenerate the HTML
  dashboard (Step 3), report a text summary in chat.
- **Add paper** (`加入待读 [arXiv link or ID]`) — extract the arXiv ID from
  the link; fetch title + authors from `https://arxiv.org/abs/{ID}`; append
  an entry with `status: "to_read"` and today's date; save; regenerate the
  dashboard; confirm with title and to-read count.
- **Update status** (`标记已读 [title keyword or arXiv ID]`) — find the entry
  by fuzzy title match or exact ID; set `status: "done"`; update
  `last_updated`; save; regenerate.
- **Mark as reading** (`开始阅读 [paper]`) — set `status: "reading"`; save;
  regenerate.
- **Remove paper** (`移除 [title keyword or arXiv ID]`) — find and remove the
  entry; save; regenerate.

### Step 2 — Profile operations

Config file: `research-claw-config.md` in the workspace (YAML, auto-maintained
by the agent, manually editable). Example:

```yaml
research_direction: >
  PhD researcher in large reasoning models and agentic memory systems.
  Focus on RL-based training, long-context reasoning, and retrieval-augmented agents.
seed_papers:
  - 2503.19823   # AutoRefine
keywords:
  - large language models
  - reinforcement learning
  - agentic memory
whitelist_authors:
  - Yaorui Shi
learned_preferences:
  accept:
    - RL-based reasoning
    - memory augmentation
  reject:
    - pure NLP classification
topic_stats:
  - topic: "Reinforcement Learning"    count: 12   pct: 35
  - topic: "LLM Reasoning"             count: 9    pct: 26
```

Operations:

- **View profile** (`我的研究画像`) — load the config, regenerate the HTML
  profile (Step 4), and show a compact chat summary: direction (first
  sentence), keywords, seed-paper count, whitelisted authors, and
  accept/reject preferences.
- **Update profile** (`更新我的研究画像 [description]`) — parse from the
  user's message (or ask for): updated research direction, keywords to
  add/remove, new seed-paper IDs, new whitelist authors; update the config;
  regenerate the HTML profile; confirm.
- **Auto-learn** (triggered by feedback on recommendations) — parse the
  feedback signal; update `learned_preferences.accept` or `.reject`;
  increment `topic_stats` counts for relevant topics; save silently with a
  brief note (no separate confirmation needed).

### Step 3 — Regenerate the reading-list HTML dashboard

Template: `templates/ResearchClaw/reading-list.html`.

1. Load the template.
2. Compute counts: `TOTAL_PAPERS`, `TOREAD_COUNT`, `READING_COUNT`,
   `DONE_COUNT`, plus `WEEK_COUNT` (papers added in the last 7 days).
3. Set `LAST_UPDATED` to today's date.
4. Fill per-paper placeholders in each status group:
   - To-read papers: `{{PAPER_TITLE_1}}`, `{{AUTHORS_1}}`,
     `{{DATE_ADDED_1}}`, `{{SCORE_1}}`, `{{TAG_1A}}`, `{{TAG_1B}}`,
     `{{NOTE_LINK_1}}`, ...
   - Reading papers: `{{PAPER_TITLE_R1}}`, `{{AUTHORS_R1}}`, `{{DATE_R1}}`,
     `{{SCORE_R1}}`, `{{TAG_R1A}}`, `{{TAG_R1B}}`, `{{NOTE_LINK_R1}}`, ...
   - Done papers: `{{PAPER_TITLE_D1}}`, `{{AUTHORS_D1}}`, `{{DATE_D1}}`,
     `{{SCORE_D1}}`, `{{TAG_D1A}}`, `{{TAG_D1B}}`, `{{NOTE_LINK_D1}}`, ...
5. The template has a fixed number of slots per section. If the list has more
   papers than slots, duplicate the entry HTML block (copy the last entry and
   append before the section's closing tag) so all papers appear.
6. Save the filled HTML to the output directory and report the path.

### Step 4 — Generate the research-profile HTML page

Template: `templates/ResearchClaw/research-profile.html`.

1. Load the template.
2. Replace placeholders:

| Placeholder | Content |
|-------------|---------|
| `{{USER_NAME}}` | User's name from config or "Researcher" |
| `{{USER_TITLE}}` | User's title/affiliation if known |
| `{{RESEARCH_DIRECTION}}` | Full research-direction text |
| `{{LAST_UPDATED}}` | Today's date |
| `{{KEYWORD_COUNT}}` | Total number of keywords |
| `{{KW_1}}` ... `{{KW_10}}` | Keyword names (up to 10) |
| `{{SEED_COUNT}}` | Number of seed papers |
| `{{SEED_ID_1}}`, `{{SEED_TITLE_1}}` | First seed paper ID + title (same for 2, 3) |
| `{{AUTHOR_1}}` ... `{{AUTHOR_5}}` | Whitelist author names |
| `{{TOPIC_1}}` ... `{{TOPIC_5}}` | Top topic names |
| `{{CNT_1}}` ... `{{CNT_5}}` | Paper counts per topic |
| `{{PCT_1}}` ... `{{PCT_5}}` | Percentage per topic |
| `{{TOTAL_PAPERS}}` | Total papers across all topics |
| `{{PREF_ACCEPT_1..3}}`, `{{PREF_REJECT_1..2}}` | Accept/reject preference strings |

3. Save to the output directory and report the path.

## Techniques

### Placeholder filling rules (apply to all HTML templates)

1. Load the template with the read tool.
2. Build a complete `{{PLACEHOLDER}}` -> value mapping in reasoning before
   writing anything.
3. Perform a full string replacement for every placeholder.
4. If a placeholder has no content, use a sensible default: URLs -> `#`;
   text -> `N/A` or empty string; counts -> `0`.
5. Never leave unfilled `{{PLACEHOLDER}}` tags in the output HTML.

### Output directory

Default output directory holds generated artifacts (dashboard, profile, paper
notes); create it if missing. The user can override it via the `output_dir`
setting in their config. Suggested layout within the workspace:

```
research-claw-config.md            # profile (YAML)
research-claw-reading-list.json    # reading list data
research-claw-output/
  reading-list.html                # regenerated dashboard
  research-profile.html            # regenerated profile page
  {ARXIV_ID}.html                  # per-paper notes (note_link targets)
```

### Status lifecycle

`to_read` -> `reading` -> `done`. Only `done` and `reading` papers count as
"engaged with" for idea generation; `to_read` is the backlog. The dashboard's
`WEEK_COUNT` (papers added in the last 7 days) tracks recent intake.

### Error handling

| Error | Handling |
|-------|----------|
| Config file missing | Use defaults silently; note at end that the user can customize the profile |
| Reading-list JSON missing or malformed | Start fresh with an empty list; inform the user |
| Template file not found | Report the expected path and ask the user to check installation |
| arXiv fetch returns empty | Retry once with a broader query; if still empty, note the API is temporarily unavailable |
| User provides DOI/PDF instead of arXiv | Try to extract the arXiv ID from the DOI or search arXiv by title |

## Scripts & Resources

- Reading-list source skill (data schema, operations, dashboard procedure):
  `scripts/ResearchClaw/skills/reading-list/`.
- Research-profile source skill (config schema, operations, profile page
  procedure): `scripts/ResearchClaw/skills/research-profile/`.
- HTML templates bundled with this skill:
  - `templates/ResearchClaw/reading-list.html` — kanban dashboard.
  - `templates/ResearchClaw/research-profile.html` — visual profile page.
  - `templates/ResearchClaw/paper-note.html` — per-paper note page (target of
    the reading list's `note_link` field).
- Companion guides in this skill: `idea-generation.md` (consumes the corpus
  of engaged papers and the profile), `paper-writing.md` (consumes selected
  ideas).
