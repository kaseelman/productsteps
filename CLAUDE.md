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

`.claude/skills/` contains PM-specific skills. **Check here before doing manual work** — if a skill covers the task, use it.

### Available skills

| Skill | Trigger | What it does |
|---|---|---|
| `council` | "council this", "help me decide", "should I" | Runs a decision through 5 AI advisors (Contrarian, First Principles, Expansionist, Outsider, Executor) → peer review → chairman synthesis. Use for high-stakes forks. |
| `coding-standards` | Any coding or implementation task | Enforces chunked implementation, documented changes, commented code, automated tests, and strict git discipline across all AI-generated code. |
| `inbox-triage` | "/inbox-triage", "triage the inbox", "process inbox files" | Reads every file in `inbox/`, classifies type and domain, drafts a structured note using the right template, and commits to the correct folder (or surfaces for review if ambiguous). |

More skills to add: `/weekly-brief`, `/write-decision-record`.

## Agent specs

`docs/agent-specs/` contains 25 reusable agent specifications for PM workflows. Each is a self-contained skill definition — paste the system prompt block into any AI assistant. Available agents:

**Discovery & Research**: Customer Insight Synthesiser, Competitive & Market Analyst, Data Storyteller

**Strategy & Planning**: Strategy Document Writer, Quarterly Planning, OKR Health Check, GTM Planner, Roadmap Narrator

**Specify & Design**: PRD Generator, Feature Spec Writer, Experiment Designer, UX Audit, Pricing Model Analyzer

**Execution & Delivery**: Meeting Prep, Action Extractor, Status Update Writer, Decision Doc Writer, Retrospective Facilitator, Delivery Health Monitor

**People & Org**: 1-1 Coach, Interview Kit Generator, Onboarding Guide Generator, Team Health Assessor

**Cross-cutting**: Narrative Writer, Blueprint Advisor, The Council

See `docs/agent-specs/Agent Ecosystem — Blueprint Mapping.md` for how these chain together across workflow loops.

## Blueprints

`docs/blueprints/` contains 53 PM reference documents. When answering questions about PM practice, frameworks, or strategy, check here first — there is likely a relevant blueprint. Structure:

- `pm-blueprints/` — 27 deep-reference blueprints (experimentation, roadmapping, PLG, pricing, delivery, and more)
- `company-playbooks/` — 13 breakdowns of how Notion, Linear, Figma, Shopify, Duolingo, and others build product
- `situation-playbooks/` — first 90 days, 0-to-1, turnarounds, IC-to-manager, working with founders
- `reference/` — metrics benchmarks, framework index, hiring questions, quote bank, tool stack guide
- `frameworks/` — AI-era product competency framework, planning playbook by level and cadence

Example: "Check the pricing blueprint and help me structure our monetisation model" → load `docs/blueprints/pm-blueprints/Blueprint - Pricing and Monetisation.md` first.

## Important caveats

- This repo contains the team's working knowledge, not a finished product spec. Expect gaps.
- Dates in filenames and frontmatter are when content was written, not necessarily when it is still accurate. Always check the `updated` field.
- When asked to generate content (summaries, suggestions, drafts), ground the output in what is in this repo. Flag assumptions clearly.
