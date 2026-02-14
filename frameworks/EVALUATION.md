# Evaluation Framework

A decision framework for assessing product ideas at Dopamine Labs. Every idea that reaches scoping gets evaluated against these eight criteria.

## Criteria

### 1. Personal Pain (1-5)

Does Andy personally experience this problem? Products born from genuine need ship better and faster — you understand the edge cases, you feel the friction, and you don't need user research to know what matters. A score of 5 means this is a daily irritation. A 1 means you're building for a problem you've only heard about secondhand.

### 2. Ship Speed (1-5)

Can v1 be built and shipped in two weeks or less of solo dev time? If not, can it be ruthlessly scoped down until it can? The best ideas are the ones where a useful v1 is small. If the minimum viable version is still months of work, that's a signal — either the scope is wrong or the idea isn't suited for a solo operation. Two weeks means ~30 hours of actual build time.

### 3. Portfolio Coherence (1-5)

Does it fit Dopamine Labs' emerging identity? The portfolio is building toward a clear thesis: tools for real life, often with a neurodivergent-friendly angle. Products that strengthen this narrative score high. Random side projects that dilute the brand score low. Consider how this would look on the Dopamine Labs landing page alongside Suppertime, Eave, and Adaptive Coach.

### 4. Solo Dev Viability (1-5)

Can this be maintained by one person working 15 hours/week alongside the existing portfolio? This isn't just about building — it's about the ongoing cost of maintenance, support, updates, and bug fixes. A product that demands constant attention is a liability. The best products are the ones that run quietly once shipped, needing only periodic updates.

### 5. Monetisation Clarity (1-5)

Is the business model obvious? Subscription, one-time purchase, freemium? If you have to think hard about how to charge for it, that's a red flag. The best products have a monetisation model so natural that users expect to pay. Complicated pricing models or "we'll figure it out later" approaches score low.

### 6. Technical Leverage (1-5)

Does it reuse existing infrastructure, shared components, or patterns from the current portfolio? Building on what exists is a force multiplier for a solo dev. MAI, SwiftUI components, API patterns — if this idea can stand on the shoulders of what's already been built, it ships faster and costs less to maintain. Greenfield stacks with no overlap score low.

### 7. Market Timing (1-5)

Is there a reason to build this now rather than later? Market timing isn't about being first — it's about whether conditions are ripe. New platform capabilities, shifting user behaviour, competitors dropping the ball, or a gap that's about to close. If it'll be just as viable in six months, there's no urgency penalty, but there's no timing bonus either.

### 8. Executive Function Tax (1-5)

Does this product reduce cognitive load or add to it? This cuts both ways: for users, and for Andy as the maintainer. A product that simplifies life scores high. A product that requires complex onboarding, frequent decision-making, or ongoing configuration is taxing. For Andy specifically: does maintaining this product fit into a low-friction workflow, or will it become a source of dread?

## Scoring Guide

Each criterion is scored 1-5. Maximum total: 40.

| Total Score | Verdict | Action |
|-------------|---------|--------|
| **28-40** | **Proceed** | Promote to build. Create the repo. |
| **20-27** | **Park** | Promising but not ready. Park for review. Revisit when context changes. |
| **Below 20** | **Kill** | Not viable right now. Kill with rationale. No shame in it. |

## How to Use This

- Score honestly. A high score that leads to a failed product is worse than a low score that saves you two weeks.
- The threshold is a guide, not a rule. An idea scoring 26 with a perfect 5 on Personal Pain might still be worth building. An idea scoring 30 with a 1 on Solo Dev Viability probably isn't.
- Revisit scores when context changes. A parked idea might score differently six months later.
- Record every scored evaluation in `frameworks/DECISION-LOG.md`.

## Tri-Model Adversarial Review

Every idea that reaches a complete SCOPING.md must pass through adversarial review before a promote/park/kill decision. The same scoping document is sent to three models (Claude, GPT, Gemini) with an adversarial prompt designed to surface blind spots, roadblocks, and weaknesses.

**This is a hard gate.** No idea gets promoted without a completed TRI-MODEL-REVIEW.md.

**What this catches that solo scoping doesn't:**
- Assumptions that feel obvious to the builder but aren't validated
- Competitive threats from products the builder doesn't know about
- Technical roadblocks that only surface when a different model reasons about the architecture
- Market sizing delusions (especially when GPT agrees the market is small — if even the glazer can't find a big market, there isn't one)

**Scoring adjustment:** If all three models flag the same critical issue, subtract 3 points from the evaluation score. If all three models independently validate the core thesis, add 2 points. These adjustments are applied after the base scoring in the evaluation criteria above.
