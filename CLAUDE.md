# PM Knowledge OS — Claude Briefing

## What this repo is

This is the product team's single source of truth: a git-managed, LLM-queryable knowledge base for a travel app. It stores strategy, domain context, feature documentation, decision records, research, and meeting notes in structured markdown. It is designed to be queried through Claude Code so PMs can get accurate, context-grounded answers without hunting across Notion, Slack, and email.

The product is a **travel app** (iOS, Android, Web) with **7 product surfaces**:
- **Inspire** — discovery and trip ideas
- **Track** — live trip logging
- **Plan** — itinerary and pre-trip planning
- **Relive Physical** — physical artefacts (prints, books)
- **Relive Digital** — digital memory and sharing
- **Users** — account, profiles, social graph
- **Focus Projects** — special initiatives that span surfaces

## Folder structure

```
pm-knowledge-os/
├── CLAUDE.md                  ← you are here
├── context/                   ← cross-domain knowledge
│   ├── strategy/              ← north star, OKRs, bets
│   ├── decisions/             ← cross-domain ADRs
│   ├── roadmap/               ← planning artefacts
│   ├── research/              ← cross-domain insights
│   └── stakeholders/          ← stakeholder maps, notes
├── domains/                   ← one folder per surface
│   ├── inspire/
│   ├── track/
│   ├── plan/
│   ├── relive-physical/
│   ├── relive-digital/
│   ├── users/
│   └── focus-projects/
│   (each: context.md, features/, decisions/, research/)
├── inbox/                     ← raw, unprocessed drops
├── docs/
│   ├── contribution-guide.md
│   └── templates/             ← reusable file templates
└── .claude/
    ├── skills/                ← PM-specific Claude skills
    └── commands/              ← custom slash commands
```

## How to navigate and load context

Follow these rules in order:

1. **Domain-specific questions**: load `domains/<domain>/context.md` first, then check `domains/<domain>/decisions/` for relevant ADRs before making any recommendation.
2. **Cross-domain or strategic questions**: load `context/strategy/` first, then load the `context.md` for each relevant domain.
3. **Before recommending any direction**: check `context/decisions/` — a decision may already be made. If so, honour it or explicitly flag that you are proposing to revisit it.
4. **`inbox/` is raw and unprocessed** — treat anything here as lower-confidence signal. Do not synthesise inbox content as if it were confirmed fact.
5. **When files contradict each other**: surface the conflict explicitly. Do not silently resolve it by picking one version.
6. **When product context is missing**: say so explicitly. Do not invent goals, user segments, metrics, or decisions.

## Frontmatter standard

Every markdown file in this repo (except `README.md` and `CLAUDE.md`) must open with this YAML frontmatter:

```yaml
---
title: ""
updated: YYYY-MM-DD
owner: ""
domain: ""           # one of: inspire, track, plan, relive-physical, relive-digital, users, focus-projects, cross-domain
tags: []
status: draft        # draft | active | superseded
---
```

Files without valid frontmatter should be flagged when encountered.

## Skills

`.claude/skills/` will contain PM-specific skills — check there for available workflows before doing manual work. Examples to come: `/inbox-triage`, `/weekly-brief`, `/write-decision-record`.

## Important caveats

- This repo contains the team's working knowledge, not a finished product spec. Expect gaps.
- Dates in filenames and frontmatter are when content was written, not necessarily when it is still accurate. Always check the `updated` field.
- When asked to generate content (summaries, suggestions, drafts), ground the output in what is in this repo. Flag assumptions clearly.
