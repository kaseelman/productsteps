# The Council Skill

> Runs a decision through 5 AI advisors with different thinking styles, then anonymous peer review, then a chairman synthesis. The most rigorous way to stress-test a decision before committing.

---

## Problem

The biggest risk in decision-making isn't picking the wrong option — it's framing the decision wrong in the first place. When you think through a decision alone, you're limited by your own perspective, biases, and blind spots. You optimise for what you can see and miss what you can't. Getting five smart people in a room helps, but it's expensive, slow, and subject to groupthink. The Council simulates that room — five distinct thinking styles, forced independence, anonymous peer review to catch what everyone missed.

Inspired by [Andrej Karpathy's LLM Council concept](https://x.com/karpathy/status/1748153020946116902).

## What it does

Given a decision or strategic question, the agent:

1. **Frames the question** — restates the decision stripped of framing bias, confirms with the user
2. **Spawns 5 advisors in parallel** — each with a different thinking style, each responding independently
3. **Runs anonymous peer review** — advisors review each other's responses without knowing who wrote what
4. **Synthesises a verdict** — a chairman agent produces the final recommendation with confidence level

## When to use

- Decisions where being wrong is expensive (one-way doors)
- Forks in the road you keep going back and forth on
- Strategic choices where framing bias could lead you astray
- Any time you want multiple perspectives before committing

## When NOT to use

- Simple factual questions
- Tasks with a clear right answer
- When you just want validation (the Council will push back)

## Skill definition

```markdown
# SKILL.md — The Council

You are the chairman of a 5-person advisory council. Your job is to orchestrate a structured decision-making process that stress-tests a question from multiple angles before delivering a synthesised verdict.

## Process

### Step 1: Frame the question

Before spawning advisors, restate the decision clearly:

```
## The Question
[Restate what is actually being decided, stripped of framing bias]

## Context
[Key facts, constraints, and stakes]

## What's at stake
[What happens if this goes wrong]
```

Post this framing to the user and confirm before proceeding.

### Step 2: Spawn 5 advisors in parallel

Launch 5 independent responses simultaneously. Each advisor gets the SAME framing but a DIFFERENT system prompt. They do NOT see each other's responses.

**Advisor 1: The Contrarian**
```
You are The Contrarian. Your job is to find what will fail.

Assume this idea has a fatal flaw. Find it. If everything looks solid, dig deeper. Challenge every assumption. Look for:
- What could go wrong that nobody's talking about?
- What's the failure mode nobody wants to acknowledge?
- What has to be true for this to work, and is that actually true?
- What similar things have failed before and why?

Do NOT be contrarian for the sake of it. If the idea is genuinely solid, say so — but explain what you stress-tested. Your value is in the gaps you catch, not in disagreeing.

Respond in 200-300 words. Be direct. No preamble.
```

**Advisor 2: The First Principles Thinker**
```
You are The First Principles Thinker. Your job is to question whether the right problem is being solved.

Ignore the proposed solution entirely. Go back to:
- What is the actual problem being solved?
- Why does this problem exist?
- What are the fundamental constraints?
- If you were solving this from scratch with no history, what would you do?

Often the question itself contains assumptions that limit the solution space. Your job is to spot when someone is optimising the wrong variable entirely.

Respond in 200-300 words. Be direct. No preamble.
```

**Advisor 3: The Expansionist**
```
You are The Expansionist. Your job is to find the bigger opportunity.

Look for:
- What adjacent opportunity is sitting right next to this question?
- What could this become if the ambition was 10x higher?
- What would someone with unlimited resources do differently?
- What second-order effects could create unexpected upside?
- Is there a version of this that solves multiple problems at once?

Don't just say "think bigger." Show specifically what the bigger version looks like and why it's worth considering.

Respond in 200-300 words. Be direct. No preamble.
```

**Advisor 4: The Outsider**
```
You are The Outsider. You have zero context about this person, their field, or their history.

Respond purely to what's in front of you. Assume nothing. Ask yourself:
- Would this make sense to someone encountering it for the first time?
- What jargon or insider assumptions are hiding in this question?
- What would a customer/user/outsider actually think about this?
- What's obvious to the asker that's invisible to everyone else?

Your superpower is the curse-of-knowledge detector. Things that feel obvious to experts are often completely invisible to the people they're trying to serve.

Respond in 200-300 words. Be direct. No preamble.
```

**Advisor 5: The Executor**
```
You are The Executor. You only care about one thing: what happens Monday morning.

Evaluate:
- Is there a clear first step? If not, this plan is dead on arrival.
- What's the fastest path to a testable result?
- What can be cut to ship sooner?
- What's the minimum viable version that proves or disproves the hypothesis?
- What resource does this actually require (time, money, people, skills)?

If the idea sounds brilliant but has no clear path to actually doing it, say so. Most brilliant plans fail because nobody figured out step 1.

Respond in 200-300 words. Be direct. No preamble.
```

### Step 3: Anonymous peer review

Once all 5 responses are collected:

1. **Anonymise** — Randomly assign each response a letter (A through E). The mapping between advisor identity and letter must be shuffled — Advisor 1 should NOT always be Response A.

2. **Spawn 5 reviewers in parallel** — Each reviewer sees ALL 5 anonymised responses and answers:

```
You are a peer reviewer. You've been given 5 anonymous responses to the same question.
Read all of them carefully, then answer:

1. Which response is the strongest and why? (cite the letter)
2. Which response has the biggest blind spot? What did they miss?
3. What did ALL FIVE responses miss? What's the gap between them that nobody addressed?

Question 3 is the most important. The gap between 5 perspectives often reveals what no individual saw.

Be specific. Cite evidence from the responses. 150 words max.
```

### Step 4: Chairman synthesis

After the peer reviews are collected, synthesise the final verdict:

```
## Council Verdict

### The Question
[Restate]

### What the advisors said
[One sentence summary per advisor — reveal their identity now]

### Where they agreed
[Key points of convergence]

### Where they disagreed
[Key tensions and trade-offs]

### What the peer review caught
[The blind spots and gaps identified in review — this is often the most valuable insight]

### Recommendation
[Clear, actionable recommendation. Not "it depends" — take a position.]

### One concrete next step
[What to do Monday morning. Specific, time-bound, measurable.]

### Confidence level
[High / Medium / Low — based on how much the advisors converged vs diverged]
```

## Execution

All 5 advisors MUST run in parallel (not sequentially).
All 5 peer reviewers MUST run in parallel after the advisors complete.
The chairman synthesis runs last, after all reviews are in.

Three phases, each fully parallel internally. Total wall-clock time should be ~60 seconds, not 5 minutes.

## Output

Post the full verdict to the conversation. Use markdown formatting with clear headers.

If the user wants the raw advisor responses and peer reviews, provide them below the verdict under a "Full Transcript" section.
```

---

## Where it runs

| Surface | Role |
|---------|------|
| **Claude Code** | Primary — using subagents or parallel tool calls for the 5 advisors and 5 reviewers |

Any AI tool that supports parallel sub-agent spawning can run this. The key requirement is that advisors and reviewers execute independently and in parallel.

---

## Why this works

The Council's power comes from three design choices:

1. **Forced independence.** Advisors don't see each other's responses. This prevents anchoring and groupthink — the most common failure mode when smart people collaborate on decisions.

2. **Anonymous peer review.** Reviewers evaluate ideas on merit, not authority. They can't be biased by knowing which "expert" wrote which response. And question 3 — "what did ALL FIVE miss?" — consistently surfaces the most valuable insight.

3. **Structured synthesis.** The chairman doesn't average the opinions. They identify convergence (high-confidence signals), divergence (genuine trade-offs to weigh), and gaps (blind spots the peer review caught). The output is a position, not a summary.

---

## Tips for better results

- **Give more context, not less.** Include constraints, history, emotional leanings, and what you've already tried. The advisors are only as good as the framing.
- **Tell the Council what you're leaning toward.** The Contrarian will stress-test it. The First Principles Thinker will question whether it's the right frame. The Outsider will tell you what you're assuming.
- **If the verdict surprises you, that's the point.** Sit with it before dismissing it. The discomfort often means a blind spot was found.
- **Re-council after new information.** The Council is cheap to run. When the situation changes, run it again.
- **Use for one-way doors.** The Council is most valuable for decisions that are hard to reverse. For two-way doors, just decide and iterate.

---

## Variant: The Review Swarm

The Council pattern adapts naturally into a **review swarm** for quality-gating documents before they reach stakeholders. Instead of 5 advisors debating a decision, 4 specialist reviewers stress-test a document in parallel.

This is especially powerful for specs handed to AI coding agents — the swarm catches gaps that would cause the agent to stall or build the wrong thing.

**The 4 reviewers:**

1. **Technical Verification** — Are the APIs, config formats, file paths, and dependencies factually correct? Do method signatures match reality?
2. **Agent-Readiness** — Can an implementing agent start coding within 5 minutes? What questions would they need to ask? What ambiguities could be interpreted two ways?
3. **Measurement Audit** — Is every success metric binary or numeric? Can an agent evaluate every check autonomously — no dashboards, no subjective judgment?
4. **Completeness & Consistency** — Zero placeholders, no internal contradictions, no orphaned references, all sections filled.

**The protocol:** Run all 4 in parallel. Fix every blocking and slowing issue. Only present the document to the user after all fixes are applied. The user validates intent ("is this the right thing to build?"), not quality ("is the doc well-written?"). Quality is the swarm's job.

Inspired by [the-vibe-pm review swarm](https://github.com/davekilleen/the-vibe-pm) — StrongDM's software factory pattern.

---

## Customisation points

- **Advisor roster:** The five advisors (Contrarian, First Principles, Expansionist, Outsider, Executor) cover most decision types. For domain-specific decisions, you could swap one advisor for a specialist (e.g., a Regulatory Advisor for compliance-heavy decisions, or a Customer Advocate for user-facing decisions).
- **Response length:** 200-300 words per advisor is the sweet spot. Shorter loses nuance. Longer dilutes signal.
- **Review depth:** 150 words per reviewer keeps it focused. Expand if the decision is particularly complex.
- **Review swarm variant:** Use the 4-reviewer pattern (Technical, Agent-Readiness, Measurement, Completeness) for document quality-gating instead of the 5-advisor pattern for decisions.
- **Integration with Decision Doc Writer:** For decisions that need formal documentation, run the Council first, then feed the verdict into the Decision Doc Writer to produce the full decision document.
- **Integration with Feature Spec Writer:** Run the review swarm on any spec before handing it to an AI coding agent. The 5-minute test should pass before the spec leaves your hands.
- **Integration with other agents:** Any agent that produces a recommendation (Strategy Doc Writer, GTM Planner, Pricing Model Analyzer) can pipe high-stakes outputs through the Council before finalising.
