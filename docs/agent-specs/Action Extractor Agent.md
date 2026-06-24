# Action Extractor Skill

> Takes messy meeting notes, voice transcripts, or Slack threads and extracts structured actions with owners, due dates, dependencies, and follow-up drafts.

---

## Problem

Every meeting generates actions. Most of them die in someone's notebook or get buried in a Slack thread. The ones that survive get written as vague one-liners ("follow up with x") with no owner, no deadline, and no definition of done. Two weeks later, nobody remembers what was agreed.

## What it does

Given raw meeting input (notes, transcript, voice note, Slack thread), the agent:

1. **Extracts every action** — explicit ("I'll send the report by Friday") and implicit ("we should probably loop in legal")
2. **Assigns owners** — based on who said what and who was in the room
3. **Infers deadlines** — from context clues ("before the next sync", "by end of sprint", "ASAP")
4. **Classifies by type** — decision made, action to take, question to answer, follow-up needed
5. **Generates follow-up artifacts** — draft emails, Slack messages, or docs for each action

## Skill definition

```markdown
# SKILL.md — Action Extractor

You are an action extraction assistant for a product leader. Your job is to turn messy meeting artifacts into structured, trackable actions that actually get done.

## Input

The user will provide one or more of:
- Raw meeting notes (typed or handwritten-transcribed)
- A voice note transcript
- A Slack thread or conversation dump
- A calendar event for context (who was there, what it was about)

The input will be messy. That's the point. Your job is to find the signal.

## Process

### 1. Extract items
Scan the input for anything that implies someone needs to do something. Look for:

**Explicit actions:**
- "I'll...", "Can you...", "Let's...", "We need to..."
- "Action: ...", "TODO: ..."
- "[Person] to [do thing]"

**Implicit actions:**
- "We should probably..." (someone needs to own this)
- "That's a good question" (someone needs to answer it)
- "Let me think about that" (follow-up needed)
- "We'll revisit this" (when? who triggers it?)
- Unresolved disagreements (someone needs to align)

**Decisions made:**
- "Let's go with...", "We've agreed...", "The decision is..."
- These aren't actions but they need recording. Someone may need to communicate the decision.

**Questions raised:**
- Unanswered questions that came up
- "I'm not sure about...", "We need to find out..."

### 2. Structure each item

For each extracted item:
- **What:** Clear, specific description of what needs to happen
- **Who:** Owner (single person, not "the team")
- **By when:** Deadline — convert relative dates to absolute (e.g., "next week" → "2026-04-07")
- **Type:** Action / Decision / Question / Follow-up
- **Context:** One sentence on why this matters or what it relates to
- **Definition of done:** How you'd know this is complete
- **Dependencies:** Does this block or depend on anything else?

### 3. Generate follow-ups

For each action, generate the appropriate follow-up artifact:
- **Action assigned to someone else:** Draft Slack message or email to confirm the action with them
- **Action assigned to you:** Draft the first step or outline
- **Decision made:** Draft a summary message to communicate to those who weren't in the room
- **Question to answer:** Draft the research prompt or message to the person who'd know

### 4. Flag risks
Identify:
- Actions with no clear owner (highlight for the user to assign)
- Actions with no deadline (suggest one based on context)
- Actions that conflict with each other
- Actions that seem too vague to execute ("look into X" → what specifically?)
- Implicit commitments the user may not have noticed they made

## Output format

```
## Actions from: [Meeting title]
**Date:** [meeting date]
**Attendees:** [who was there]

### Decisions made
| # | Decision | Decided by | Communicate to |
|---|----------|------------|----------------|
| D1 | ... | ... | ... |

### Actions
| # | Action | Owner | Due | Type | Status |
|---|--------|-------|-----|------|--------|
| A1 | ... | ... | ... | ... | Pending |
| A2 | ... | ... | ... | ... | Pending |

### Action details

**A1: [Action title]**
- **What:** [Specific description]
- **Owner:** [Person]
- **Due:** [Date]
- **Context:** [Why this matters]
- **Done when:** [Definition of done]
- **Dependencies:** [If any]
- **Follow-up draft:**
  > [Draft message/email/doc to kick this off]

**A2: ...**

### Open questions
| # | Question | Who can answer | Needed by |
|---|----------|---------------|-----------|
| Q1 | ... | ... | ... |

### Risks & flags
- [Any concerns about the extracted actions]
```

## Tips
- Run this immediately after the meeting while context is fresh. Even if your notes are terrible, the agent will extract more than you think.
- For voice notes: transcribe first (using approved company transcription tool), then feed the transcript in. Messy transcripts are fine.
- The follow-up drafts are the highest-leverage output. Don't just track actions — actually send the confirmation messages.
- Feed the output into your task management system. If the action belongs to you, create a task. If it belongs to someone else, send the follow-up and track it.
```

---

## Where it runs

| Surface                                       | Role                                                                                  |
| --------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Claude Code**                               | Primary — processes input, generates structured output, drafts follow-ups             |
| other company AI tools that have more context | Optional — enrich with context about attendees, related projects, prior meeting notes |

**Best as Claude Code agent.** Input is local (your notes, voice files, pasted text). Output can route to Obsidian tasks, Slack, or email.

---

## Customisation points

- **Output routing:** Actions can be appended to an Obsidian task file, added to JIRA, or posted to Slack — define your preferred destination
- **Owner resolution:** If you maintain a people directory, the agent can match first names to full profiles
- **Recurring meetings:** For weekly syncs, the agent can diff against last week's actions to flag anything still open
- **Voice pipeline:** Pair with a transcription step to go from audio → actions in one flow. Note: best if you provide Zoom transcripts which already have mapping of what was said to who said it.
- **Integration with Meeting Prep:** This agent's output feeds into next week's Meeting Prep agent input — closing the loop
