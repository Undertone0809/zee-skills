---
name: flomo-local-api
description: Query, summarize, export, create, and edit a user's flomo memos through local desktop auth and the flomo API, without Chrome UI automation. Use for fast memo lookup, tag filtering, markdown export, attachment/image metadata reads, lightweight memo creation, or direct text edits to existing memos.
---

# Flomo Local API

## Overview

Use this skill for fast flomo access when local desktop auth is available.

When the user wants to create or revise a memo:
1. **Tag reuse is required**: Prefer the user's existing flomo tag system over inventing new tags. Aim for existing-tag reuse in at least 95% of memo-writing cases.
2. **Title required by default**: For every new memo written for Zeeland, start with a concise title. Unless the user asks for no title, render the title as bold in flomo.
3. **Use flomo-native formatting, not Markdown syntax**: flomo does not render Markdown syntax such as `**bold**`, `- [ ]` checkboxes, or `# headers`. For bold titles, use the CLI's `--html` mode and wrap the title in `<strong>...</strong>`. Use plain paragraphs for the body.

This skill now supports direct text edits to existing memos through the same local auth flow, including when the user provides a flomo memo URL like `https://v.flomoapp.com/mine/?memo_id=...`.

## Preconditions

- `flomo.app` has been logged in on this Mac before
- Local flomo storage exists under `~/Library/Containers/com.flomoapp.m/...`
- The request can be handled with local auth and API access

## Use This Skill When

- The user asks to search flomo memos by keyword, tag, or time range
- The user asks whether flomo memos contain images or attachments, or wants image URLs included in read/export output
- The user asks what they have been thinking about recently
- The user wants monthly markdown export or tag statistics
- The user wants to create a simple memo without Chrome UI automation
- The user wants to edit an existing text memo by slug or by flomo memo URL without opening the Web UI
- The user wants faster querying than Chrome UI automation
- The user wants a draft memo that fits their existing flomo tag taxonomy before creating it

## Do Not Use This Skill When

- The user wants to delete a memo
- The user wants to operate through the live Web UI
- The local desktop login state is missing or broken and the request cannot be completed from local auth

## Default Workflow

1. Use `scripts/flomo_local_api.py`.
2. Prefer `query` for direct lookup.
3. Prefer `summarize` for reflective prompts such as "最近在想什么".
4. Prefer `export-monthly` when the user wants markdown output.
5. Use `--include-files` on `query` or `export-monthly` when the user asks about images, attachments, screenshots, audio, or media in memos.
6. Before creating or editing a memo with tags, inspect the user's existing tag taxonomy with `tags`.
7. Draft a short title first, then the memo body, then choose 1-4 tags by reusing existing tags that are already in the system.
8. For ordinary memo creation/editing, use `--html` when a bold title is needed; otherwise use plain text mode only when the user explicitly does not want formatted title output.
9. Prefer `create` for lightweight memo creation without attachments.
10. Prefer `edit` for updating the text content of an existing memo by slug or `memo_id` URL.
11. Treat delete as out of scope unless the skill is expanded again later.
12. When the user says "最近 N 条", "latest N", or "recent N", sort by `created_at` newest-first before selecting the N memos, and verify the selected timestamps before editing.
13. For batch tagging existing memos, default to 3 tags per memo unless the user gives a different count. Apply the count to every selected memo, including memos that already have tags. Preserve good existing tags, add or replace only enough trailing hashtag text to reach the requested count, and verify the final `tag_count` for each memo.

## Commands

### Command Template

Set the script path from the current environment instead of a hardcoded user directory:

```bash
SKILL_ROOT="${CODEX_HOME:-$HOME/.codex}/skills/flomo-local-api"
SCRIPT="$SKILL_ROOT/scripts/flomo_local_api.py"
```

### Query

Use for direct search by keyword, tag, date range, or recent window.

```bash
python3 "$SCRIPT" query --keyword "openclaw" --limit 10
python3 "$SCRIPT" query --tag "Proj/mf" --days 30
python3 "$SCRIPT" query --start-date 2026-03-01 --end-date 2026-03-11 --format markdown
python3 "$SCRIPT" query --limit 5 --sort newest
python3 "$SCRIPT" query --keyword "截图" --limit 5 --include-files --format json
python3 "$SCRIPT" query --days 7 --include-files --format markdown
```

`query` returns newest-first by default. Use `--sort oldest` only when the user explicitly asks for earliest or chronological results.

`query` results include both `slug` and the corresponding flomo memo URL so later edit steps can reuse them directly.

Use `--include-files` when the user asks about images or attachments. JSON output includes a `files` array with safe attachment metadata (`id`, `type`, `name`, `size`, `url`, and `thumbnail_url`). Markdown output renders image attachments as Markdown image links and non-image attachments as ordinary links.

### Summarize

Use for "我最近在关注什么 / 最近状态如何" style requests.

```bash
python3 "$SCRIPT" summarize --days 30
python3 "$SCRIPT" summarize --days 90 --limit 80
```

### Export Monthly

Use when the user wants a monthly markdown dump plus `tag-stats.md`.

```bash
python3 "$SCRIPT" export-monthly
python3 "$SCRIPT" export-monthly --output-dir ~/download/flomo-markdown-export-monthly
python3 "$SCRIPT" export-monthly --include-files --output-dir ~/download/flomo-markdown-export-monthly-with-images
```

Use `--include-files` when the exported Markdown should preserve image links. Without it, monthly exports keep the previous compact behavior and only record attachment counts as `- files: N`.

### Tags

Use when the user wants to create or edit a memo and you need to reuse the existing tag system instead of inventing new tags.

```bash
python3 "$SCRIPT" tags --roots-only --limit 20
python3 "$SCRIPT" tags --query "agent" --limit 20 --min-total-count 2
python3 "$SCRIPT" tags --prefix "area/ai/agent" --limit 20 --format markdown --min-total-count 2
python3 "$SCRIPT" tags --query "设计" --days 365 --limit 20 --min-total-count 2
```

Recommended pattern for memo writing:

1. Draft a concise title first, then the memo body without tags.
2. Extract 2-5 likely concepts from the title and body.
3. Start with one cheap scan: `tags --roots-only --limit 12`.
4. Then make at most 2 focused lookups, usually one concrete entity query and one abstract theme query. If the memo names a specific project, person, product, or proper noun, spend the first focused lookup on that exact entity string before abstract theme queries. Prefer `--min-total-count 2`.
5. If a root is obvious, prefer one `--prefix` lookup inside that subtree over spraying many synonym queries.
6. Reuse 1-4 existing tags, preferring mature path tags with repeated usage.
7. Stop once you have 2 strong tags and at most 1-2 optional supporting tags. Do not keep searching just to exhaust every possible synonym.
8. If the best hit is a singleton leaf under an unfamiliar root, widen once and prefer a broader, better-established parent or nearby mature sibling.
9. If the memo clearly centers on a named project, person, or product and an existing `Proj/*`, `p/*`, or product tag exists, include that concrete taxonomy tag before adding more generic thematic tags.
10. When an exact or near-exact named-entity tag is found, treat it as required evidence, not optional flavor. Do not drop it in favor of only abstract theme tags.
11. Only create a new tag when there is no close existing tag. If you do, tell the user briefly why reuse was not possible.

Search budget:

- Default budget is 3 tag lookups total:
  - 1 roots-only scan
  - 1 focused entity/theme lookup
  - 1 optional disambiguation lookup
- Going beyond 3 lookups needs a clear reason, such as no mature match after the first pass.
- Avoid synonym fan-out like querying `prompt`, `eval`, `policy`, `rules`, `safety`, `design`, `comment` one by one unless earlier results were genuinely inconclusive.

### Batch Tagging

Use when the user asks to tag recent memos, add tags to multiple memos, or make every selected memo have a fixed number of tags. If the user does not specify a tag count, use exactly 3 tags per memo by default.

Workflow:

1. Select the target memos first. For "最近 N 条" or "latest N", use newest-first ordering and show or internally confirm the `created_at`, `slug`, existing `tags`, and a short snippet before editing.
2. Inspect existing tag taxonomy with `tags`. Reuse concrete `Proj/*`, `p/*`, product, or person tags when they match named entities; otherwise use mature thematic tags.
3. If a memo already has tags, treat them as candidates to preserve, not as a reason to skip the memo.
4. Each selected memo must end with the requested tag count, or exactly 3 parsed tags when no count is specified. Prefer preserving the strongest existing tags and adding mature supporting tags.
5. When rewriting tag lines, remove only trailing hashtag-only lines, then append one final hashtag line that exactly matches the selected tags. Do not alter the memo body.
6. After editing, query or fetch the selected memos again and verify each memo's parsed `tags` and `tag_count`.

For ad-hoc batch edits, a small Python helper may import `flomo_local_api.py` and call `fetch_memo_by_slug`, `plain_text_to_html`, `api_put`, and `add_derived_fields` directly. Keep the helper ephemeral unless the user asks to persist it.

### Create

Use when the user wants to quickly create a text memo from local auth.

**Text Formatting Note**: flomo does not render Markdown syntax (e.g., `**bold**`, `# headers`, lists). For Zeeland's memos, add a concise bold title by using `--html` and a first paragraph like `<p><strong>Title</strong></p>`:
- For the title: use raw HTML `<strong>...</strong>` with `--html`, not `**...**`.
- For emphasis inside the body: prefer `「」` quotes or direct wording instead of extra bold.
- For lists: use simple line breaks or plain bullets like `•` only when they read naturally.
- Do NOT use checkboxes or todo lists in flomo memos; flomo is for recording thoughts, not task management
- Keep paragraphs separated by blank lines for readability

```bash
# Plain-text mode is available for tests or explicit no-format requests.
python3 "$SCRIPT" create --content "测试 memo #codex/demo"
printf '第一行\n\n第二段 #codex/demo\n' | python3 "$SCRIPT" create --stdin
# For Zeeland's normal memos, prefer this HTML form so the title is actually bold in flomo.
cat <<'EOF' | python3 "$SCRIPT" create --stdin --html
<p><strong>Memo title</strong></p><p>Memo body in plain paragraphs.</p><p>#area/ai/agent/skill</p>
EOF
```

### Edit

Use when the user wants to update the text of an existing memo and already has either the memo `slug` or a flomo memo URL.

**Text Formatting Note**: Same as `create` — for Zeeland's memos, preserve or add a first bold title paragraph with `--html` unless the user asks otherwise. Do not fake bold with Markdown syntax.

```bash
python3 "$SCRIPT" edit --slug "MTIzMDgzNzgz" --content "更新后的 memo 内容 #codex/demo"
python3 "$SCRIPT" edit --url "https://v.flomoapp.com/mine/?memo_id=MTIzMDgzNzgz" --content "更新后的 memo 内容 #codex/demo"
printf '第一行修改后\n\n第二段也更新\n' | python3 "$SCRIPT" edit --slug "MTIzMDgzNzgz" --stdin
```

## Output Conventions

- `query --format json` returns structured memo hits
- `query --format markdown` returns a readable markdown list
- `query --include-files` returns or renders attachment metadata, including image `url` and `thumbnail_url` when flomo provides them
- `summarize` returns memo counts, top tags, and supporting memos
- `export-monthly` writes one markdown file per month plus `tag-stats.md`
- `export-monthly --include-files` renders image attachments as Markdown image links and non-image attachments as ordinary links
- `tags` returns existing tag rows with `tag`, `depth`, `parent`, `direct_count`, `total_count`, and supports `--min-total-count` for filtering toward mature tags
- `create` returns the created memo payload with parsed markdown and tags
- `edit` returns the updated memo payload with parsed markdown and tags
- When `create` or `edit` is used with `--html`, verify the returned `content` starts with `<p><strong>...` for titled memos, and verify the returned `tags` match the selected tags.
- Monthly memo files keep only `created_at` metadata by default
- When producing `tag_decision.json`, `selected_tags` must use the raw existing tag strings from inventory, without a leading `#`. The memo body can render them as hashtags, but the JSON list should stay normalized.
- Finalize `selected_tags` first, then render exactly those same tags in the memo body as hashtags. Do not widen, shorten, or swap a parent/child variant between `tag_decision.json` and `draft.md`.

## Implementation Notes

- The update path is `PUT /memo/<slug>` using the same local auth token and request signing as the other commands.
- flomo memo payloads can include a `files` array. Read paths may expose attachment metadata and image URLs with `--include-files`; write paths do not upload new attachments.
- `edit` preserves the memo's current `files` and `pin` state so a text update does not accidentally drop attachments or pin status.
- flomo share links of the form `https://v.flomoapp.com/mine/?memo_id=...` can be used directly; the skill extracts `memo_id` as the target slug.
- `tags` is the default lookup tool for existing tag reuse. It is intentionally read-only and fast enough to call before most memo-writing requests.
- For live-user edits, prefer small and explicit changes unless the user clearly asks for a rewrite.
- For capability verification, use an idempotent marker string or a temporary test memo, then confirm by reading the updated payload.

## Tag Reuse Policy

For memo creation and memo edits that touch hashtags, follow this policy by default:

- Reuse before inventing. The user's existing tag taxonomy is the source of truth.
- Aim for existing-tag reuse in at least 95% of memo-writing cases.
- Prefer established roots already common in the user's system, such as `self/`, `area/`, `Proj/`, `产品/`, `p/`, `resources/`, `Book/`, `others/`, `TODO/`, and `SL/`.
- Prefer mature tags with repeated usage (`total_count >= 2`) over singleton leaves when both are plausible.
- If the memo explicitly names an existing project, person, or product, prefer attaching that concrete existing taxonomy tag as one of the selected tags.
- If an exact or near-exact existing project/person/product tag is found for the memo's named entity, include it as one of the selected tags unless the mention is clearly incidental.
- Prefer the closest existing leaf tag when a clear match exists.
- If several existing tags are plausible, prefer an existing broader parent over inventing a new leaf.
- Treat singleton leaves under weak or unfamiliar roots as weak evidence, not default choices.
- Keep tags sparse for single-memo create/edit flows. Default to 1-4 tags for a single memo unless the user asks for more. For batch tagging existing memos, default to exactly 3 tags per memo.
- Prefer fewer, better tags over exhaustive tagging. Retrieval quality matters more than semantic completeness.
- Avoid English-only ad hoc tags when the user's existing taxonomy already has a better-fitting path tag.
- If no close existing tag is found, you may introduce at most one new tag, and you should mention that choice explicitly.
- In structured outputs such as `tag_decision.json`, never prepend `#` to the stored tag names. Use `self/反思`, not `#self/反思`.
- `draft.md` hashtags must exactly correspond to `selected_tags` after adding `#`. If you chose `Proj/rudder/想法`, the draft should contain `#Proj/rudder/想法`, not `#Proj/rudder`.

Examples:

- Bad: `#AI/claude-code #prompt-engineering` when the user's taxonomy already contains nearby tags under `area/ai/agent/*`, `产品/*`, or `编程/*`.
- Better: inspect `tags --query "agent" --min-total-count 2`, `tags --query "coding" --min-total-count 2`, `tags --query "prompt" --min-total-count 2`, or `tags --query "产品" --min-total-count 2` first, then reuse the closest mature existing tags.

## Safety Rules

- Only use `create` when the user explicitly asks for a new memo
- Only use `edit` when the user explicitly asks to modify an existing memo
- `query --include-files` and `export-monthly --include-files` are read/export features only; do not imply that this skill can upload, delete, OCR, or rewrite attachment files
- `edit` preserves the memo's current `files` and `pin` state; it is for text updates, not attachment management
- Do not invent new tags casually; check existing tags first with `tags`
- Do not delete flomo memos from this skill
- Do not persist extra raw dumps unless the user explicitly asks
- Treat summaries as patterns from memo evidence, not diagnoses
- **Never use Markdown syntax** (bold, headers, lists, checkboxes) in memo content — flomo renders it as plain text. The only default bold formatting for Zeeland's memo title should be raw HTML `<strong>` sent through `--html`.
- **Never create task lists or checkboxes** in flomo memos; flomo is for capturing thoughts and reflections, not task management

## Validation Cases

### Case: Create a normal research memo

Input:
User asks to write a flomo memo about a paper or idea.

Expected behavior:
Draft a concise title, inspect existing tags, create the memo with `--html`, render the first paragraph as `<p><strong>Title</strong></p>`, put body paragraphs after it, append the selected existing hashtags, and verify the returned `content` and `tags`.

Must not:
Use `**Title**`, Markdown headers, checkboxes, or omit the title.

### Case: Edit an existing memo after style feedback

Input:
User asks to optimize or humanize a memo that lacks a title.

Expected behavior:
Fetch or use the known memo slug, rewrite the memo with a concise bold title via `--html`, preserve appropriate existing tags, and verify the returned memo payload.

Must not:
Only rewrite prose while leaving the memo untitled, or replace mature existing tags with newly invented tags.

### Case: Read memo images

Input:
User asks whether the API can read images in flomo memos or asks to inspect memos with screenshots.

Expected behavior:
Use `query --include-files` or a direct read helper to check the raw memo `files` array, report whether image attachments are present, and mention that available fields include image `url` and `thumbnail_url` when flomo provides them.

Must not:
Answer from the text-only `snippet` field, claim images are unavailable because default `query` output omits them, or attempt to edit/upload attachments.

### Case: Export monthly notes with image links

Input:
User asks to export flomo notes to Markdown and preserve images.

Expected behavior:
Run `export-monthly --include-files --output-dir <target>`, then verify at least one exported monthly file contains Markdown image links for image attachments when matching memos have files.

Must not:
Only write `- files: N` counts when the user explicitly asked to preserve images, or download/persist extra raw files unless the user asked for local attachment downloads.

## Resources

### scripts/

- `scripts/flomo_local_api.py`: CLI for flomo query, summarize, export, lightweight create, and text edit
