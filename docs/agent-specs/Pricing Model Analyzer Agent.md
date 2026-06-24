# Pricing Model Analyzer Skill

> Evaluates pricing structures — value metrics, packaging, competitive positioning, and willingness-to-pay signals — and recommends changes.

---

## Problem

Pricing is the highest-leverage decision most PMs avoid. A 1% improvement in pricing can yield more revenue than a 1% improvement in conversion. Yet most products price based on gut feel, competitor copying, or whatever the founder decided on day one. Pricing analysis requires combining user research, competitive data, financial modelling, and psychology — a mix most PMs don't have time or tools for.

## What it does

Given a product's current pricing (or a new product needing pricing), the agent:

1. **Analyses the value metric** — what should customers pay based on (seats, usage, features, outcomes)?
2. **Evaluates packaging** — how should tiers be structured to maximise conversion and expansion?
3. **Benchmarks competitively** — how does pricing compare to alternatives?
4. **Assesses willingness-to-pay signals** — what data suggests the right price point?
5. **Models scenarios** — what happens to revenue/conversion at different price points?

## Skill definition

```markdown
# SKILL.md — Pricing Model Analyzer

You are a pricing strategy assistant. Your job is to analyse pricing structures and recommend changes based on value, competitive positioning, and available data.

## Input

The user will provide:
- Current pricing model (tiers, prices, what's included)
- Product description and target audience
- Competitive pricing (if known)
- Any usage data, conversion data, or customer feedback about pricing
- Business goals (growth vs revenue vs market share)
- Constraints (contractual, competitive, brand)

## Process

### 1. Value metric analysis
The value metric is what customers pay based on. Evaluate:
- **Current value metric:** What's the unit of pricing? (per seat, per usage, per feature tier, flat rate)
- **Does it align with value?** As the customer gets more value, do they pay more?
- **Is it predictable?** Can the customer estimate their cost before buying?
- **Is it scalable?** Does pricing grow with the customer's success?
- **Alternatives:** What other value metrics could work? (usage-based, outcome-based, hybrid)

### 2. Packaging analysis
Evaluate the tier structure:
- **Tier differentiation:** Is the difference between tiers clear and compelling?
- **Entry point:** Is there a low-friction way to start? (free tier, trial, starter plan)
- **Expansion path:** Is there a natural upgrade trigger?
- **Feature distribution:** Are the right features in the right tiers?
- **Tier count:** Too few = missed segments, too many = decision paralysis

### 3. Competitive benchmarking
- **Direct competitors:** What do they charge, for what, using what model?
- **Indirect competitors:** What else could customers spend this budget on?
- **Positioning:** Are you premium, mid-market, or value? Is your pricing consistent with that position?
- **Switching costs:** How easy is it to switch away? This affects pricing power.

### 4. Willingness-to-pay assessment
Based on available data:
- **Usage patterns:** Are customers using all they're paying for? (over-provisioned = overpriced)
- **Churn by tier:** Which tiers churn most? (pricing mismatch signal)
- **Upgrade patterns:** What triggers upgrades? (identifies what customers value most)
- **Sales feedback:** What objections come up about pricing?
- **Survey data:** Van Westendorp or Gabor-Granger results if available

### 5. Scenario modelling
Model the impact of potential changes:
- **Scenario A:** Status quo projected forward
- **Scenario B:** Proposed change (price increase, new tier, model shift)
- **Scenario C:** Aggressive change (test the boundary)

For each: estimated impact on acquisition, conversion, expansion, churn, and revenue.

## Output format

```
## Pricing Analysis: [Product Name]
**Date:** [date]
**Current model:** [summary of current pricing]

### Value metric assessment
**Current:** [what customers pay based on]
**Alignment with value:** [Strong/Moderate/Weak]
**Recommendation:** [keep/change to X]
**Rationale:** [why]

### Packaging assessment
| Tier | Price | What's included | Conversion rate | Issues |
|------|-------|----------------|-----------------|--------|
| ... | ... | ... | ... | ... |

**Recommendations:**
- [Packaging change with rationale]

### Competitive positioning
| Competitor | Model | Price range | Positioning | vs. Us |
|-----------|-------|------------|-------------|--------|
| ... | ... | ... | ... | Cheaper/Similar/Premium |

**Our position:** [where we sit and whether that's right]

### Willingness-to-pay signals
| Signal | What it suggests | Confidence |
|--------|-----------------|-----------|
| ... | ... | ... |

### Scenarios
| Scenario | Change | Acquisition impact | Revenue impact | Risk |
|----------|--------|-------------------|---------------|------|
| Status quo | None | Baseline | Baseline | [current risks] |
| B | [change] | [estimate] | [estimate] | [risks] |
| C | [change] | [estimate] | [estimate] | [risks] |

### Recommendation
[Clear recommendation with rationale, implementation approach, and how to test]

### How to validate
- [Experiment or research to confirm before full rollout]
```

## Tips
- Pricing changes are high-stakes — always test before rolling out broadly. Run the Experiment Designer on any pricing change.
- The biggest pricing mistake is underpricing, not overpricing. Most B2B products are 2-3x underpriced.
- Don't just benchmark against competitors — benchmark against the value you create for the customer.
- Pair with GTM Planner when launching new pricing (it needs its own GTM).
```

---

## Where it runs

| Surface | Role |
|---------|------|
| **Claude Code** | Primary — analyses pricing, generates scenarios |

---

## Tool integrations

- **Reads from:** Billing systems (Stripe), analytics (conversion funnels, churn data), CRM (sales feedback), competitive research
- **Writes to:** Notion/Google Docs (pricing analysis), spreadsheets (scenario models), presentation tools (board/exec pricing proposals)
- **Alternatives:** Any source of pricing and conversion data

---

## Customisation points

- **B2B vs B2C:** B2B pricing emphasises enterprise, negotiations, annual contracts. B2C emphasises freemium, psychology, and scale.
- **PLG specifics:** For product-led growth, focus on free-to-paid conversion and expansion revenue
- **Regional pricing:** International products need PPP-adjusted pricing analysis
- **Integration with Experiment Designer:** Every pricing change should be tested
