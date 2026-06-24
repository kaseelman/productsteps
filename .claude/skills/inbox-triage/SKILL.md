---
name: inbox-triage
description: "Process raw files in the inbox/ folder — classify each one, draft a structured note using the right template, and commit it to the correct domain or context folder. Use when the user says '/inbox-triage', 'triage the inbox', 'process inbox files', or when running on a scheduled cron. Also use when a new inbox file has just been committed and needs routing."
---

# Inbox Triage

> Reads every unprocessed file in `inbox/`, classifies it, drafts a structured note, determines the right destination, then either commits directly (high confidence) or opens a summary for human review (ambiguous).

---

## When to use

- Manually: whenever a PM wants to flush the inbox
- Automatically: on a weekly cron schedule (Monday 9am)
- After a new file lands in `inbox/` from Granola or a Notion webhook

---

## Process

### Step 1: Inventory the inbox

Read `inbox/` and list every file that is NOT:
- `README.md`
- Anything inside `inbox/archive/`

If the inbox is empty, report "Inbox is clean." and stop.

For each file, note:
- Filename and approximate age (from frontmatter `updated` field or filename date prefix if present)
- File size / rough length

### Step 2: Classify each file

For each inbox file, determine:

| Field | Options |
|---|---|
| **Type** | `meeting-note` / `decision-record` / `feature-doc` / `research` / `strategic-doc` / `unknown` |
| **Domain** | `inspire` / `track` / `plan` / `relive-physical` / `relive-digital` / `users` / `focus-projects` / `cross-domain` |
| **Destination folder** | e.g. `domains/track/research/` or `context/decisions/` |
| **Confidence** | `high` (clear signals) / `medium` (probable) / `low` (ambiguous) |

**Classification signals to look for:**

- *Meeting note*: date + attendees, agenda structure, action items, "meeting with", "call with", Granola-style headers
- *Decision record*: "we decided", options/rationale structure, ADR-style content
- *Feature doc*: feature name in title, problem/approach/platform structure, Notion feature page export
- *Research*: user quotes, survey data, interview findings, competitor mentions, data tables
- *Strategic doc*: OKRs, bets, north star framing, roadmap content

**Domain signals:**

Load `domains/<domain>/context.md` for each plausible domain before assigning — match content vocabulary against that domain's stated scope. When content spans multiple domains, assign `cross-domain` and note which domains it touches.

### Step 3: Draft the structured note

For each file, produce a draft using the correct template from `docs/templates/`:

- `meeting-note` → `docs/templates/meeting-note.md`
- `decision-record` → `docs/templates/decision-record.md`
- `feature-doc` → `docs/templates/feature.md`
- `research` / `strategic-doc` → use the standard frontmatter block (no dedicated template yet); structure as: Summary, Key findings, Implications, Open questions

Fill in every field you can infer from the raw content. Mark genuinely unknown fields with `<!-- TODO -->` — do not invent facts. Flag if the raw content contradicts anything already in the destination domain folder.

### Step 4: Route and commit

**High confidence (commit directly):**
- Write the structured draft to the destination folder with a dated filename: `YYYY-MM-DD-<slug>.md`
- Delete (or move to `inbox/archive/YYYY-MM/`) the original raw inbox file
- Use a clear git commit message: `feat(inbox-triage): route <filename> → <destination>`

**Medium or low confidence (surface for review):**
- Do NOT commit automatically
- Present the proposed draft inline with:
  - The raw source (collapsed if long)
  - Your classification reasoning
  - The proposed destination
  - Specific questions that would resolve the ambiguity
- Ask the PM to confirm before committing

**Unknown type:**
- Flag it explicitly: what you found, why it couldn't be classified, what information is needed
- Leave the file in `inbox/` and add a `<!-- TRIAGE: needs human review — reason -->` comment at the top

### Step 5: Staleness sweep

After processing all current files, check `inbox/` for any remaining files older than 30 days (compare `updated` frontmatter field or file mtime against today's date).

For each stale file:
- If still unclassifiable: move to `inbox/archive/YYYY-MM/` with a note in the commit message
- Report to the PM: "Moved X stale files to archive — review if any are important"

### Step 6: Report

After processing all files, output a triage summary:

```
## Inbox Triage — YYYY-MM-DD

| File | Type | Domain | Destination | Action taken |
|------|------|--------|-------------|--------------|
| meeting-2026-06-20.md | meeting-note | track | domains/track/ | ✅ Committed |
| notion-export-pricing.md | strategic-doc | cross-domain | context/strategy/ | ⏳ Needs review — see below |
| old-research-notes.md | unknown | — | inbox/archive/ | 📦 Archived (32 days old) |

## Needs your input
[For each medium/low confidence file: draft + questions]

## Inbox status
X files processed. Inbox now contains Y files.
```

---

## Confidence rules (guidance)

| Signal | Effect on confidence |
|---|---|
| Clear type structure (agenda, actions, options table) | +high |
| Domain vocabulary matches a single domain's context.md | +high |
| Granola-formatted meeting notes | +high for type; domain still needs inference |
| Notion export with database properties intact | +medium to high |
| Free-form brain dump, no structure | −medium |
| Content spans 3+ domains equally | −medium (assign cross-domain) |
| Title or filename is ambiguous ("notes", "draft", "export") | −low |
| Content contradicts existing domain files | flag conflict, do not commit |

---

## Destination folder reference

| Type | Primary destination |
|---|---|
| Meeting note | `domains/<domain>/` (top level, prefixed with date) |
| Decision record | `domains/<domain>/decisions/` or `context/decisions/` (cross-domain) |
| Feature doc | `domains/<domain>/features/` |
| Research | `domains/<domain>/research/` or `context/research/` (cross-domain) |
| Strategic doc | `context/strategy/` |

---

## Important constraints

- **Never invent content.** If a field can't be inferred from the raw source, leave it as `<!-- TODO -->`.
- **Never silently resolve conflicts.** If the raw content contradicts an existing file in the destination folder, surface the conflict explicitly — do not pick a version.
- **Honour the frontmatter standard.** Every committed file must open with valid YAML frontmatter: `title`, `updated`, `owner`, `domain`, `tags`, `status`. If `owner` can't be inferred, leave it blank but include the field.
- **Inbox is lower-confidence signal.** Do not treat raw inbox content as confirmed team thinking when classifying or when cross-referencing with existing domain files.

---

## Where it runs

Claude Code (run `/inbox-triage` from the repo root), or triggered automatically via GitHub Actions cron.
