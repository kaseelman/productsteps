---
title: "Contribution Guide"
updated: 2026-06-24
owner: ""
domain: cross-domain
tags: [process, contribution]
status: active
---

# Contribution Guide

## Ownership model

- **Domain leads** own their `domains/<domain>/` folder. Add, edit, and reorganise freely.
- **`context/`** (strategy, cross-domain decisions, roadmap, research) is owned by the PM lead. Changes via PR with a one-line description of what changed and why.
- **`inbox/`** is open to all — drop raw content freely. The PM processes inbox items and moves them to the right folder with a structured summary.

## File naming

- **Dated content** (meeting notes, snapshots, research rounds): `YYYY-MM-DD-short-description.md`
- **Evergreen content** (context files, templates, guides): `descriptive-slug.md`

## Frontmatter is required

Every file must open with the standard frontmatter block (see `CLAUDE.md` for the schema). PRs that add files without valid frontmatter will be asked to fix before merge.

## Three rules to keep the repo healthy

1. **No raw files outside `inbox/`** — process before committing anywhere else.
2. **Supersede, don't append** — when a file becomes outdated, mark it `status: superseded` and create a new file rather than editing the old one in place. This preserves history and keeps Claude from mixing old and new signals.
3. **Quarterly pruning** — every quarter, review files older than 90 days. Archive or delete anything that no longer reflects reality. Flag superseded files you haven't yet cleaned up.

## How to mark a file superseded

1. Change `status: draft` or `status: active` to `status: superseded` in the frontmatter.
2. Add a `superseded-by: path/to/new-file.md` field to the frontmatter.
3. Optionally add a one-line note at the top of the file body: `> Superseded by [new-file.md](path/to/new-file.md) on YYYY-MM-DD.`

## Templates

Reusable templates live in `docs/templates/`. Use them as starting points — don't create new file formats from scratch.

| Template | Use for |
|---|---|
| `feature.md` | Documenting a feature across its lifecycle |
| `decision-record.md` | Recording a significant product decision (ADR style) |
| `meeting-note.md` | Processed notes from meetings, reviews, syncs |
