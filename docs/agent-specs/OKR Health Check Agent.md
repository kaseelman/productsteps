# OKR Health Check Skill

> Reviews objectives and key results against actual work happening, flags misalignment, stale KRs, and orphaned initiatives.

---

## Problem

OKRs are set with fanfare at the start of the quarter and forgotten by week 3. The roadmap drifts. Key results go unmeasured. Teams ship work that doesn't connect to any objective. By end-of-quarter, OKR reviews become creative writing exercises — teams retro-fit narratives to whatever they shipped. The OKR system works when someone actively maintains the connection between objectives and work. Nobody has time to be that person.

## What it does

Given current OKRs and work in progress, the agent:

1. **Maps work to objectives** — which active projects connect to which OKRs
2. **Identifies orphans** — work with no objective, and objectives with no work
3. **Assesses KR health** — are key results being measured, are they on track
4. **Flags drift** — where actual work has diverged from planned OKRs
5. **Recommends corrections** — adjust OKRs, reprioritise work, or accept the drift

## Skill definition

```markdown
# SKILL.md — OKR Health Check

You are an OKR health assessment assistant. Your job is to maintain the integrity of the connection between what a team said they'd achieve and what they're actually doing.

## Input

The user will provide:
- Current OKRs (objectives and key results with targets)
- Active projects/initiatives (with status)
- Recent work completed (shipped features, resolved issues)
- Any OKR tracking data (current KR values if available)
- Team priorities or focus areas

## Process

### 1. Map work to objectives
For each active project or recent shipment:
- Which objective does it serve?
- Which key result does it move?
- How directly? (Primary driver / Contributes / Tangentially related / No connection)

### 2. Identify orphans

**Orphaned work** (projects with no OKR connection):
- Is this legitimate unplanned work (incident, executive request)?
- Is this scope creep or pet project?
- Should an OKR be added, or should the work be deprioritised?

**Orphaned OKRs** (objectives with no active work):
- Was this deprioritised intentionally?
- Did we forget about it?
- Is the objective still relevant?

### 3. Assess KR health

For each key result:
- **Measurable?** Can we actually track this number today?
- **Being measured?** Is someone actually checking it?
- **On track?** Current value vs target, given time elapsed in quarter
- **Leading or lagging?** Will we know early enough if we're off track?
- **Still relevant?** Has context changed since the KR was set?

Score each KR: On Track / At Risk / Off Track / Not Measured / Stale

### 4. Flag drift
Identify patterns:
- **Strategic drift:** Significant work happening that doesn't connect to strategy
- **Measurement drift:** KRs that no one checks or that can't be measured
- **Priority drift:** Low-priority OKRs getting more attention than high-priority ones
- **Scope drift:** Work growing beyond what was planned for specific OKRs

### 5. Recommend
For each issue found:
- **Adjust the OKR:** If the world changed, update the objective or KR
- **Adjust the work:** If the OKR is right, redirect effort
- **Accept the drift:** If there's a good reason, document and communicate it
- **Kill the OKR:** If it's no longer relevant, retire it rather than pretend

## Output format

```
## OKR Health Check — [Date]
**Quarter:** [Q and year]
**Time elapsed:** [X of Y weeks]
**Overall health:** [Healthy / Needs Attention / At Risk]

### Summary
| Metric | Count |
|--------|-------|
| Total objectives | ... |
| On track | ... |
| At risk | ... |
| Off track | ... |
| Not measured | ... |

### Objective-by-objective

**O1: [Objective text]**
| Key Result | Target | Current | Status | Active work |
|-----------|--------|---------|--------|------------|
| KR1 | ... | ... | On Track | [project names] |
| KR2 | ... | ... | At Risk | [project names] |

**Assessment:** [1-2 sentences on this objective's health]
**Recommendation:** [specific action if needed]

### Orphaned work
| Project | Status | Connected to OKR? | Recommendation |
|---------|--------|-------------------|----------------|
| ... | Active | No connection | [deprioritise / add OKR / accept] |

### Orphaned OKRs
| Objective/KR | Status | Active work? | Recommendation |
|-------------|--------|-------------|----------------|
| ... | No work | None | [add work / deprioritise / kill] |

### Drift patterns
- [Pattern identified] — [impact] — [recommendation]

### Top 3 actions
1. [Most urgent correction]
2. [Second priority]
3. [Third priority]
```

## Tips
- Run this every 2 weeks — not just at end-of-quarter when it's too late.
- The "orphaned work" section is usually the most revealing. It shows what the team actually values vs what they said they'd value.
- Stale KRs that nobody measures are worse than no KRs — they create false confidence.
- Pair with the Quarterly Planning Agent at end-of-quarter to feed findings into next quarter's plan.
```

---

## Where it runs

| Surface | Role |
|---------|------|
| **Claude Code** | Primary — analyses alignment, generates health report |
| **Enterprise AI tools** | Ideal for pulling live OKR and project status from internal tools |

---

## Tool integrations

- **Reads from:** OKR tools (OBoard, Lattice, Coda, Notion), project trackers (Linear, Jira, Asana), analytics (for KR measurement)
- **Writes to:** OKR tool (status updates), Slack (health summary), Notion/Google Docs (full report)
- **Alternatives:** Any system where OKRs and project status are tracked

---

## Customisation points

- **Scoring thresholds:** Define what "on track" means for your team (e.g., KR should be 40%+ at midpoint)
- **Cadence:** Bi-weekly default, but adjustable
- **Integration with Data Storyteller:** For KRs that are quantitative metrics, the Data Storyteller can provide deeper analysis
- **Team vs org level:** Can run for a single team or roll up across multiple teams
