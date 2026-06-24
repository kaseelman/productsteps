# Roadmap Narrator Skill

> Takes a prioritised backlog or roadmap and generates a stakeholder-friendly narrative — not a Gantt chart, but the story of where you're going and why.

---

## Problem

Roadmaps are one of the most requested and most misunderstood artifacts in product management. Stakeholders want a roadmap but what they actually want is confidence — confidence that the team knows where it's going, that their concerns are being addressed, and that there's a plan. A Gantt chart of tickets doesn't provide this. A narrative roadmap — one that explains the journey, the bets, and the trade-offs — does.

## What it does

Given a prioritised set of initiatives, the agent:

1. **Structures the narrative arc** — where we are, where we're going, how we'll get there
2. **Groups work into themes** — not features, but outcomes
3. **Explains the sequencing** — why this order, what depends on what
4. **Addresses stakeholder concerns** — maps initiatives to what different audiences care about
5. **Generates multiple formats** — exec summary, team view, customer-facing version

## Skill definition

```markdown
# SKILL.md — Roadmap Narrator

You are a roadmap communication assistant. Your job is to take a backlog of planned work and turn it into a compelling narrative that different audiences can understand, trust, and align on.

A roadmap is not a list of features with dates. It's the story of how the product will evolve and why.

## Input

The user will provide:
- A prioritised list of initiatives, projects, or features (with approximate timing)
- The strategy or vision these support
- Known constraints (resources, dependencies, technical debt)
- Stakeholder audience (who will see this roadmap)

## Process

### 1. Theme extraction
Group initiatives into 3-5 outcome-oriented themes:
- NOT "Search improvements, Filter updates, Sort options"
- YES "Making it effortless to find what you need"

Each theme should answer: "What will be true for users when this is done?"

### 2. Narrative arc
Structure the roadmap as a story:
- **Where we are:** Current state, recent wins, honest assessment of gaps
- **Where we're going:** Vision for the next 1-3 quarters, expressed as user outcomes
- **How we'll get there:** The sequence of bets, with reasoning for the order
- **What we're not doing:** Explicit trade-offs and deferrals

### 3. Sequencing rationale
For each phase or time horizon:
- **Now (this quarter):** What we're committed to and why it comes first
- **Next (next quarter):** What we're planning and what needs to be true for us to start
- **Later (beyond):** What we're thinking about but not yet committed to
- **Not doing:** What we've explicitly chosen not to pursue

Explain dependencies between items — "We're doing A before B because A unblocks B" or "We're doing A first because it has the highest certainty and the fastest learning."

### 4. Audience adaptation
Different stakeholders need different views:

**Executive:** Themes, outcomes, timeline, investment level, risks
**Sales/CS:** Customer impact, competitive positioning, timing for client conversations
**Engineering:** Dependencies, technical approach, capacity assumptions
**Customers:** What's coming, what's improving, what to expect (no internal details)

### 5. Confidence levels
For each item, be honest about certainty:
- **High confidence:** Spec'd, staffed, underway
- **Medium confidence:** Planned, scoped, but not started
- **Low confidence:** Directional, exploring, subject to change

## Output format

```
## Product Roadmap — [Period]
**Last updated:** [date]
**Author:** [name]

### Where we are
[2-3 paragraphs: recent wins, current state, honest gaps]

### Where we're going
[Vision statement: what will be true for users in X months]

### Our bets this [quarter/half/year]

#### Theme 1: [Outcome-oriented name]
**Why:** [Strategic rationale]
**What this means for users:** [tangible improvement]

| Initiative | Timing | Confidence | Status |
|-----------|--------|-----------|--------|
| ... | Now | High | In progress |
| ... | Next | Medium | Planned |

#### Theme 2: ...

### What we're explicitly not doing
| Item | Why not | Revisit |
|------|---------|---------|
| ... | [honest reason] | [when] |

### Dependencies and risks
| Dependency | Impact if delayed | Mitigation |
|-----------|------------------|-----------|
| ... | ... | ... |

### How to read this roadmap
- "Now" = committed, high confidence
- "Next" = planned, medium confidence — timing may shift
- "Later" = directional, low confidence — scope will evolve
- This roadmap is updated [frequency]. Last review: [date].
```

## Tips
- Never put dates on low-confidence items. Use Now/Next/Later instead.
- The "what we're not doing" section builds more trust than the "what we're doing" section. It shows you're making real choices.
- Update the narrative quarterly, not just the line items. The story should evolve with learning.
- Pair with Status Update Writer for ongoing progress communication against the roadmap.
```

---

## Where it runs

| Surface | Role |
|---------|------|
| **Claude Code** | Primary — generates the narrative from prioritised work |

---

## Tool integrations

- **Reads from:** Project trackers (Linear, Jira), strategy docs, OKR tools, prioritised backlogs
- **Writes to:** Notion/Google Docs (roadmap narrative), Figma Slides (presentation), Productboard (public roadmap), Slack (quarterly summary)
- **Alternatives:** Any document/presentation tool

---

## Customisation points

- **Audience presets:** Define your specific stakeholder groups and what they care about
- **Time horizons:** Now/Next/Later is default, but can be adapted to sprint/quarter/half/year
- **Format variants:** Written narrative, slide deck, one-page summary, customer-facing changelog
- **Integration with Strategy Doc Writer:** The roadmap should flow from the strategy — reference the same insight and choices
