# Evaluation Framework

A dual-track decision framework for assessing ideas at Dopamine Labs. Most ideas are personal tools (Track A). Some become products (Track B). The framework ensures they're evaluated against the right criteria at the right time.

---

## Track A: Personal Tool

This is the default track. Most ideas start and stay here. The question is simple: does this solve Andy's problem well enough to be worth building and maintaining?

### Criteria (scored 1-5 each, max 30)

#### 1. Personal Pain Severity (1-5)

How much does this problem actually affect Andy's daily life? A 5 is a daily frustration that wastes time or causes stress. A 1 is an occasional annoyance that's easy to work around. Be specific — "email overwhelm" is vague, "spending 20 minutes every morning triaging 6 inboxes" is concrete.

#### 2. Build Simplicity (1-5)

Can the simplest possible version be built in a weekend? If not, can it be scoped down until it can? The best personal tools are embarrassingly simple v1s that work. A complex first version is a signal that the problem isn't well understood yet, or that the solution is over-engineered.

#### 3. Maintenance Burden (1-5)

Once built, how much ongoing attention does this need? A script that runs forever scores 5. An app that needs weekly fixes scores 1. Consider: API dependencies that break, platforms that change, libraries that need updating, data that needs cleaning. The best personal tools are the ones you forget are running.

#### 4. Overwhelm Impact (1-5)

Does building and maintaining this reduce net overwhelm, or does it become another thing to manage? This is the AuDHD-specific criterion. A tool that removes three tasks from your mental load but adds one new maintenance task is net positive (score 4). A tool that theoretically saves time but requires you to remember to use it, configure it, or fix it regularly is net negative (score 1-2). Be brutally honest.

#### 5. Simplest Platform (1-5)

Is the simplest implementation good enough? A Siri Shortcut before a PWA. A PWA before a native app. A CLI script before a GUI. A cron job before a server. Score 5 if the simplest possible platform works. Score 1 if you genuinely need a complex stack (native app with push notifications, real-time sync, etc.) and there's no simpler path.

#### 6. Already Exists Check (1-5)

Could Andy solve this with an existing tool, even imperfectly? If something gets you 70% of the way there, building the remaining 30% may not be worth it. Score 5 if nothing adequate exists. Score 1 if an existing tool (Apple's built-in apps, a £5 app, a Shortcut) solves 80%+ of the problem.

### Track A Scoring

| Total Score | Verdict | Action |
|-------------|---------|--------|
| **24-30** | **Build it** | The personal value is clear and the build cost is low. |
| **18-23** | **Probably worth it** | But check whether a simpler solution exists first. |
| **12-17** | **Park** | The pain isn't severe enough or the build cost is too high relative to the benefit. |
| **Below 12** | **Kill** | Solution looking for a problem, or maintenance burden exceeds benefit. |

### What Track A Does NOT Evaluate

- Market size
- Monetisation
- Competitive landscape at scale
- Compliance beyond personal data handling
- Multi-user architecture
- Pricing

---

## Track B: Product

This track activates ONLY when Andy decides to take a personal tool to market. This is a conscious decision gate — most personal tools should stay personal tools.

### Promotion Criteria (all must be true before entering Track B)

- The tool has been in personal use for at least 4 weeks
- Andy still uses it regularly and it genuinely reduces overwhelm
- At least one other person has independently expressed interest (not prompted)
- Andy actively wants to productise it (not just "it could be a product")

Use the `TRACK-PROMOTION.md` template in `ideas/_template/` to document the promotion decision.

### Track B Criteria (scored 1-5 each, max 50)

Track B uses the Track A score as a baseline, then adds product-specific criteria:

#### 1-6. Track A Criteria (carry forward)

The Track A score carries into Track B unchanged. A product that doesn't solve a personal problem well won't solve a market problem well.

#### 7. Portfolio Coherence (1-5)

Does it fit Dopamine Labs' emerging identity? The portfolio is building toward a clear thesis: tools for real life, often with a neurodivergent-friendly angle. Products that strengthen this narrative score high. Random side projects that dilute the brand score low.

#### 8. Solo Dev Viability (1-5)

Can this be maintained by one person working 15 hours/week alongside the existing portfolio? This isn't just about building — it's about the ongoing cost of maintenance, support, updates, and bug fixes. A product that demands constant attention is a liability.

#### 9. Monetisation Clarity (1-5)

Is the business model obvious? Subscription, one-time purchase, freemium? If you have to think hard about how to charge for it, that's a red flag. The best products have a monetisation model so natural that users expect to pay.

#### 10. Technical Leverage (1-5)

Does it reuse existing infrastructure, shared components, or patterns from the current portfolio? Building on what exists is a force multiplier. MAI, SwiftUI components, API patterns — if this idea can stand on the shoulders of what's already been built, it ships faster and costs less to maintain.

#### 11. Market Timing (1-5)

Is there a reason to productise now rather than later? New platform capabilities, shifting user behaviour, competitors dropping the ball, or a gap that's about to close.

#### 12. Executive Function Tax (1-5)

Does this product reduce cognitive load or add to it — both for users and for Andy as the maintainer? A product that simplifies life scores high. A product that requires complex onboarding, frequent decision-making, or ongoing configuration is taxing.

#### 13. Compliance Surface Area (1-5)

What regulations does this touch? GDPR, FCA, App Store guidelines? Score 5 if the compliance burden is minimal (a simple app with no sensitive data). Score 1 if the product touches financial data, health data, credentials, or requires regulatory approval. Can a solo dev handle the compliance burden?

#### 14. Platform Risk (1-5)

Are Apple, Google, or well-funded competitors building this natively? Score 5 if the space is clear. Score 1 if the platform owner is actively commoditising this exact capability. What's the timeline for commoditisation?

### Track B Scoring

| Total Score | Verdict | Action |
|-------------|---------|--------|
| **56-70** | **Strong candidate** | Proceed to marketing brief and tri-model review. |
| **42-55** | **Viable but risky** | Identify top 3 risks. Decide if they're acceptable. |
| **28-41** | **Weak case** | Park unless circumstances change dramatically. |
| **Below 28** | **Don't productise** | Keep as personal tool. |

---

## The Gate Between Tracks

```
Track A: build/park/kill as personal tool
                    ↓
        [personal use, 4+ weeks]
                    ↓
        Track B promotion gate
                    ↓
Track B evaluation + mandatory tri-model review
                    ↓
        marketing brief → promoted → MAI
```

- Default track is **always A**
- Most ideas should **stay on Track A** — a good personal tool is a success
- Track B is a **conscious decision**, not an assumption
- The tri-model adversarial review is **mandatory for Track B**, optional but recommended for Track A
- When scoping, always clarify: "Are we evaluating this as a personal tool or a product?"

---

## Tri-Model Adversarial Review

Every idea entering Track B must pass through adversarial review. For Track A ideas, the review is optional but recommended for anything with significant build investment.

The same scoping document is sent to three models (Claude, GPT, Gemini) with an adversarial prompt designed to surface blind spots, roadblocks, and weaknesses. The review prompt should be adjusted based on track:

- **Track A reviewers** should evaluate against personal utility, build simplicity, and maintenance burden
- **Track B reviewers** should evaluate against market viability, competitive landscape, and compliance

**This is a hard gate for Track B.** No idea gets promoted without a completed TRI-MODEL-REVIEW.md.

**What this catches that solo scoping doesn't:**
- Assumptions that feel obvious to the builder but aren't validated
- Competitive threats from products the builder doesn't know about
- Technical roadblocks that only surface when a different model reasons about the architecture
- For Track B: market sizing delusions (especially when GPT agrees the market is small)

**Scoring adjustment (Track B only):** If all three models flag the same critical issue, subtract 3 points from the evaluation score. If all three models independently validate the core thesis, add 2 points.

---

## How to Use This

- Score honestly. A high score that leads to a failed project is worse than a low score that saves you a weekend.
- The threshold is a guide, not a rule. Context matters more than arithmetic.
- Revisit scores when context changes. A parked idea might score differently six months later.
- Record every scored evaluation in `frameworks/DECISION-LOG.md`.
