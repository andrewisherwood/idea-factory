# Tri-Model Adversarial Review — Eave

**Date:** 2026-02-14
**Scoping Doc Version Reviewed:** 862ff2d
**Models Used:** Claude (Opus 4.6) · GPT-4o · Gemini 2.5 Pro

---

## Review Brief

The following SCOPING.md was provided to each model with identical instructions:

> Review this product scoping document. Your job is adversarial — find what's wrong, what's missing, and what will fail. Specifically:
>
> 1. **Blind spots** — What hasn't been considered? What assumptions are unstated?
> 2. **Roadblocks** — What technical, market, or practical obstacles could kill this?
> 3. **Weaknesses in the evaluation** — Where is the scoping document being generous to itself?
> 4. **Competitive threats** — What existing products or approaches make this redundant?
> 5. **The kill case** — Make the strongest possible argument for why this idea should NOT be built.
> 6. **The save** — If you had to rescue one thing from this idea, what's the core insight worth preserving even if the product as scoped is wrong?
>
> Be direct. Do not glaze. The builder has limited time and needs honest assessment, not encouragement.

---

## Claude Assessment

### 1. Blind spots

**LLM costs are unaddressed.** Every V0 capture requires an API call to classify intent. Every V1 email requires classification. At even modest scale (hundreds of users, dozens of emails each per day), LLM costs become a material line item. The £4.99/month subscription needs to cover these costs and still generate margin. The unit economics haven't been modelled.

**V0 "zero friction for non-technical users" is a contradiction.** The routing destinations — GitHub API for idea-factory, iOS Reminders, Suppertime context — are developer-specific. A non-technical user doesn't have a GitHub repo. V0 either ships as Andy's personal tool (honest, but not a product) or it needs a generic routing layer for normal people (much bigger build).

**Offline capture with sync is not simple.** Conflict resolution, queue management, retry logic, handling partial syncs when the user goes online mid-queue. On iOS this means background task scheduling, which Apple throttles aggressively. "Offline capture" sounds like a checkbox but it's a meaningful engineering challenge.

**Data privacy and regulatory surface area.** Eave reads email, accesses bank accounts (V3), stores credentials (V4), and browses on behalf of users (V5). In the UK, this touches GDPR, FCA regulations for Open Banking, and potentially the Computer Misuse Act if the browser agent interacts with sites in ways their ToS don't permit. There is zero mention of privacy, compliance, or legal risk in the entire scoping document.

**The trust ladder assumes linear adoption.** Users might desperately want V3 (subscription audit) but not care about V2 (calendar). The sequential unlock model might gate features behind versions that don't matter to the user, creating friction rather than reducing it.

**"A single Mac Mini can serve hundreds of users" is untested.** Email polling for hundreds of accounts, LLM classification per email, backlog processing, searchable indexing — the compute math hasn't been done. This could be true or wildly optimistic.

**Email deliverability.** Forwarding from 6 accounts to one address can trigger spam filters. High-volume forwarding from multiple sources to a single inbox is a known deliverability problem.

### 2. Roadblocks

**Gmail OAuth verification is a multi-week process.** Google requires a privacy policy, security assessment, and manual review for apps accessing email scopes. This isn't "OAuth for modern access" — it's a bureaucratic process that has blocked indie devs for months. This alone could delay V1 by 4-8 weeks.

**Open Banking (V3) is FCA-regulated.** You can't just call TrueLayer's API. You either need to be an authorised agent or work through a regulated intermediary. The compliance overhead for a solo dev is significant — this isn't a weekend integration.

**Browser automation (V4/V5) is fragile at scale.** Websites change layouts, add CAPTCHAs, implement bot detection. Puppeteer scripts break constantly. This is the hardest thing in the roadmap to maintain and directly contradicts Solo Dev Viability. Companies with 50+ engineers struggle to make browser automation reliable.

**The password manager (V4) is a security liability.** One breach and the product is dead. The reputational and legal exposure of storing user credentials as a solo dev with no security team is enormous. This is not a feature to ship casually.

**App Store review for V1+ will be intense.** An app requesting email access, calendar access, bank access, and credential storage will receive heavy scrutiny from Apple. Review times and rejections are unpredictable for apps with this permission scope.

### 3. Weaknesses in the evaluation

**"Can V1 ship in under 2 weeks?" is generous.** Gmail OAuth verification alone can take 2+ weeks. Add multi-provider IMAP, backlog processing, LLM classification pipeline, digest generation, push notifications, and a functional iOS app — this is a 6-8 week build minimum for a working V1, not 2 weeks.

**V0 doesn't compete for dev hours — except it does.** V0 requires a native iOS app with LLM integration, offline sync, multiple API integrations (GitHub, Reminders), and Sign in with Apple. Building this alongside an active Suppertime App Store submission is non-trivial context switching, even if the hours don't directly overlap.

**"Lock-in through learned context" cuts both ways.** If Eave archives something important or misclassifies urgency early on, trust is broken permanently. There's no mention of error recovery, undo flows, or trust repair mechanisms. For an AuDHD user, one bad experience ("Where did that important email go?") triggers avoidance, not loyalty.

**The evaluation checkboxes are mostly checked despite several being aspirational.** "Does it overlap existing products? No" — but V0's capture-and-route overlaps with Siri, Reminders, Notes, and the Siri Shortcut already built in this repo. The overlap isn't with Dopamine Labs products, it's with Apple's built-in tools.

### 4. Competitive threats

**Apple Intelligence is the existential threat.** Apple is building email summarisation and triage directly into Mail.app. By the time V1 ships, Apple may have native email triage that's "good enough" for most users — free, pre-installed, no setup. Eave's differentiator (neurodivergent-first design, aggressive automation) needs to be dramatically better than Apple's built-in to justify a subscription.

**SaneBox, Superhuman, Shortwave** have been doing email triage for years. SaneBox has a decade of ML-based email classification. Superhuman has massive funding and a polished UX. The email triage market is crowded and mature. "Built for AuDHD" is a positioning angle, not a technical moat.

**1Password, Bitwarden** are mature, well-funded, security-audited password managers. No solo dev should be competing in this space. V4's password manager feature is the weakest part of the roadmap.

**Notion, Obsidian, Apple Notes** — V0's capture-and-route competes with tools people already use for quick capture, plus the Shortcuts/Zapier/IFTTT ecosystem for routing.

### 5. The kill case

**Eave is five products in a trenchcoat.** V0 is a capture app. V1 is an email client. V2 is a calendar assistant. V3 is a fintech product. V4 is a password manager plus browser automation. V5 is a general-purpose web agent. These are fundamentally different products with different technical stacks, different regulatory requirements, and different competitive landscapes.

No solo dev working 15 hours/week can build, ship, maintain, and support five different products. The roadmap describes 3-5 years of work. By V3, the LLM landscape, Apple's capabilities, the email market, and UK financial regulations will have changed so dramatically that V3-V5 as scoped today will be irrelevant.

The sequential trust ladder sounds elegant but creates a product that's always incomplete. V1 users waiting for V3 will churn before it arrives. The subscription needs to justify itself at every version, not just at V5's theoretical end state.

The honest truth: V0 and V1 are real products. V2 is plausible. V3-V5 are speculative roadmap items that make the scoping document feel more complete than the actual plan warrants. They create a false sense of a coherent product arc that doesn't account for solo dev capacity, regulatory complexity, or market evolution.

### 6. The save

The core insight is genuinely valuable: **neurodivergent brains need a single low-friction input that handles routing automatically, and an aggressive email triage system that acts with confidence rather than asking permission.** The "drip-feed, batch decisions, sensible defaults" UX philosophy is the real product — not the feature list. V0 + V1 scoped tightly, with the AuDHD-first design principles applied rigorously, is a compelling product. Everything from V2 onward should be treated as future vision, not committed roadmap, and should not influence the build decision for V0-V1.

### Key Findings

- V3-V5 are speculative and should be separated from the core V0-V1 build decision — they add false confidence to the scoping
- V1's "2 weeks" estimate is significantly undercooked — Gmail OAuth verification alone could eat that
- Apple Intelligence is the biggest competitive threat and isn't mentioned anywhere in the doc
- Data privacy, regulatory compliance, and LLM unit economics are completely unaddressed
- The password manager (V4) should probably be killed entirely — the liability isn't worth it for a solo dev

---

## GPT Assessment

### Key Findings

1. Blind Spots
   A. You’re Underestimating Trust by an Order of Magnitude

You casually escalate from:

“I’ll remember that” → “I’ll do it for you”

SCOPING

This is not a smooth ladder.

It’s a chasm.

Reading email is one thing.
Canceling subscriptions is another.
Logging into bank accounts and changing emails/passwords via browser agent is existentially different.

You are assuming:

Users will trust an early-stage solo-built product

With their inbox

Then their bank data

Then their credentials

Then autonomous browser control

That’s not a product roadmap. That’s a liability ladder.

B. Security + Compliance Is Barely Addressed

You mention:

Open Banking (TrueLayer)

SCOPING

Password storage

Browser automation with credentials

IMAP access to multiple inboxes

But there is zero discussion of:

GDPR obligations (you are UK-based)

Data retention policies

Breach response plan

SOC2 / ISO27001 requirements if handling financial + credential data

Liability if your browser agent makes a financial mistake

This product handles:

Email (identity backbone)

Banking (money)

Passwords (everything)

That’s Tier 1 risk infrastructure.

And you’ve scoped it like a side project.

C. The “Hundreds of Users on a Mac Mini” Assumption

“A single Mac Mini can serve hundreds of users.”

SCOPING

Based on what?

LLM calls per email

Backlog processing

Search indexing

Browser automation sessions

Banking API calls

Per-tenant isolation

You’re dramatically underestimating:

Concurrency

Spikes (Monday morning inbox surge)

Browser agent memory load

LLM cost variability

This is infrastructure-heavy. Not a lightweight polling script.

D. You Assume Email Is the Core Pain for Everyone

You wrote:

“Painkiller — email overload is a daily pain.”

SCOPING

For you? Yes.

For the market? Debatable.

Many neurodivergent people:

Avoid inboxes entirely

Use filters already

Let Gmail sort automatically

Or simply accept chaos

Your own evaluation is extremely self-referential.

E. Market Size Is Not Evaluated

You didn’t answer:

How many people will pay £4.99/month for inbox triage?

How many trust a new company with banking + passwords?

How many already use:

Gmail smart sorting

Superhuman

Hey

1Password

Copilot Money

Monarch

You assume unmet need without proving underserved segment.

2. Roadblocks That Could Kill This
1. Gmail API Limits + Compliance

Gmail:

Rate limits heavily

Requires app verification for sensitive scopes

Can suspend apps that do bulk modification

Flags aggressive auto-unsubscribe

You are proposing:

Backlog blitz

Auto-archive

Auto-unsubscribe

Google may not love that.

2. Browser Automation Is Brittle

You plan to use Puppeteer for:

Changing emails

Canceling subscriptions

ParentPay top-up

SCOPING

Reality:

Sites change DOM constantly

CAPTCHA

MFA

Device fingerprinting

Anti-bot measures

Random layout shifts

You will spend your life chasing broken flows.

And users will blame you when:

£200 gets charged twice

A cancellation fails

A form submits wrong data

3. Open Banking ≠ Subscription Detection

Subscription detection is messy:

Variable billing cycles

Annual vs monthly

Merchant renaming

Shared merchant descriptors

Hidden subscriptions inside Apple/Google

This is not trivial logic.

Monarch and Copilot already have teams solving this.

4. Legal Exposure from Autonomous Actions

V5 says:

“Eave can operate any website on your behalf.”

SCOPING

That is an enormous liability statement.

If it:

Submits wrong form

Pays wrong amount

Cancels wrong account

Misses a deadline

You are responsible.

Insurance? Not discussed.

3. Where the Document Is Being Generous to Itself
   A. “No Overlap with Existing Products”

You marked:

“Does it overlap existing products? No.”

SCOPING

That is objectively false.

You overlap with:

Gmail native AI sorting

Superhuman

Hey

SaneBox

Spark

Copilot Money

Monarch

Rocket Money

1Password

Bitwarden

Reclaim.ai

Motion

Apple Mail Smart Folders

Siri + Shortcuts automation

Your differentiation is integration breadth, not uniqueness.

B. “V1 Can Ship in 2 Weeks”

You marked:

“V1 email triage — yes, tight but feasible”

SCOPING

Realistically, for production-grade:

OAuth flow

IMAP handling

LLM classification

Digest UI

Search indexing

Preferences engine

Error handling

Logging

Retry logic

GDPR compliance

Onboarding UX

Two weeks is fantasy unless you mean “barely works for me.”

C. You Assume Lock-In Is a Strength

“Lock-in through learned context”

SCOPING

This can easily become:

“I don’t trust it enough to give it more data”

“I don’t want vendor lock-in with my identity”

Lock-in works when trust is high.
Here trust is fragile.

4. Competitive Threats
   Email Triage

Superhuman

SaneBox

Spark AI

Gmail native AI

Hey.com

Subscription Audit

Rocket Money

Copilot

Monarch

Password Management

1Password

Bitwarden

Apple Keychain

Autonomous Agents

OpenAI Operator-like systems

Browser automation startups

Apple integrating more automation natively

You are not competing with “no one.”
You are competing with deeply capitalised tools.

5. The Kill Case

Here is the strongest argument against building this:

It is too broad.

It touches high-liability surfaces (email, money, passwords).

It requires security posture of a serious company, not a solo founder side project.

It competes in crowded markets.

It assumes trust that takes years to build.

It will fragment your focus away from Suppertime (which is simpler and revenue-adjacent).

The “autonomous web agent” dream is intoxicating but operationally brutal.

Your core motivation is personal overwhelm — not validated market demand.

Most likely outcome:

You build V0.

You half-build V1.

Infrastructure complexity explodes.

Security concerns slow you down.

Suppertime momentum suffers.

Nothing fully ships at scale.

That’s the realistic kill scenario.

6. The Save — The Core Insight Worth Preserving

There is something powerful here.

Not the browser agent.
Not password consolidation.
Not subscription audit.

The real insight:

“Just talk. It goes where it should.”

SCOPING

That’s clean.

V0 is the gold.

Universal capture + intelligent routing
for neurodivergent executive function relief.

That is focused.
That is buildable.
That is low-liability.
That is differentiable if done beautifully.

If you were forced to salvage this:

Kill V3–V5 entirely.

Do not touch banking.

Do not touch passwords.

Do not build a browser agent.

Stay away from “autonomous web operator.”

Instead:

Build the world’s best neurodivergent-first capture + triage front door.

Integrate only via safe APIs.

Become the “cognitive inbox,” not the “digital executor.”

That keeps:

The psychological insight.

The AuDHD UX principles.

The friction-reduction thesis.

Without:

The existential risk.

Hard Truth

Right now this reads like:

A brilliant mind trying to unify every pain point in life into one system.

That’s seductive.
It’s also how projects die.

Suppertime is tight.
Eave is sprawling.

If you build it, shrink it to the one thing only you care about deeply enough to refine daily.

Everything else is scope gravity.

If you want, next step:
I can rewrite this into a brutally realistic “V0–V1 only” execution plan that doesn’t implode under its own ambition.

---

## Gemini Assessment

### Key Findings

Gemini said
This scoping document outlines an ambitious "God Mode" for personal life administration. While the vision is compelling for the target demographic, the technical and security assumptions are dangerously optimistic.

1. Blind Spots
   The "Hallucination" Liability: In V0, if the LLM misclassifies an intent (e.g., sends a private "Email thought" to a public GitHub repo), the trust is broken instantly. There is no mention of a "Review before execution" buffer for high-stakes routing.

Security as an Afterthought: You are proposing a single Mac Mini or VPS to store OAuth tokens, IMAP credentials, bank access (TrueLayer), and passwords. This is a tier-one security honeypot. A breach doesn't just leak data; it grants an attacker full autonomous control over the user’s digital life.

The "Missing" Problem: "Never open an email again" assumes 100% recall and precision. If Eave silently archives a "Final Notice" or a school emergency because it looked like "noise," the user suffers real-world consequences. The document lacks a "fail-safe" UI for missed items.

API Rate Limits & Costs: Running "Backlog Blitz" (V1) for hundreds of users on a single Mac Mini will hit Gmail/IMAP rate limits and astronomical LLM token costs. Processing 10,000 emails per user through an LLM is not a "batch processing" triviality.

2. Roadblocks
   Platform Enclosure: Apple (Apple Intelligence) and Google (Gemini/Workspace) are currently building exactly this into the OS. They have native access to the kernel, local processing, and the hardware. Competing with "Siri, do my admin" when Siri has system-level permissions is a losing battle.

V5 Web Agent Rot: Using Puppeteer/Playwright for ParentPay or school portals is a maintenance nightmare. These sites change their DOM (Document Object Model) constantly. You will spend 80% of your dev time fixing broken scrapers for sites you don't control.

Gmail OAuth Complexity: Getting "Restricted Scope" Google OAuth approval for an app that reads and deletes emails is an expensive, multi-month security audit process (CASA). You cannot ship this to the public in "2 weeks."

3. Weaknesses in the Evaluation
   The "Two-Week" Fallacy: Claiming V1 (Email Triage + Backlog) can ship in under 2 weeks is delusional. Implementing secure IMAP/OAuth, a per-tenant database architecture, and a production-grade LLM classification pipeline is a multi-month project for a solo dev.

Hardware Scaling: "A single Mac Mini can serve hundreds of users." If those users have 50GB inboxes and need real-time triage, the I/O and inference latency will choke the machine.

Monetization vs. Cost: £4.99/month likely won't cover the LLM token costs for a user doing a "Backlog Blitz" of 20,000 emails using high-reasoning models (Claude 3.5/GPT-4o).

4. Competitive Threats
   Superhuman/Shortwave: Already winning at AI-assisted email triage with massive funding and security teams.

Rocket Money: Already owns the "Subscription Audit" (V3) space with direct bank integrations.

1Password: Already owns the "Password Consolidation" (V4) space.

MultiOn / Skyvern: Emerging autonomous web agents that are much further ahead on the V5 "Browser Agent" tech.

5. The Kill Case
   Eave is a "High-Liability, Low-Moat" product. You are asking users for the keys to their entire kingdom (Email, Bank, Passwords) to be stored on a Mac Mini in your office or a single VPS. The technical debt of maintaining scrapers for school portals will eventually consume all development, leaving no time for the "Adaptive Coach" or "Suppertime." Furthermore, Apple and Google will integrate these "agents" for free within 12–18 months, rendering your middleware redundant. You are building a complex, high-risk bridge to a destination the platform owners are already paving.

6. The Save
   The "Executive Function" UI Pattern.
   The core insight worth saving is not the "Agent" itself, but the UX of Drip-Feeding and Decision Batching. The true pain for neurodivergent users isn't just "the admin"; it's the interface of the admin. If you strip away the high-risk "autonomous browser agent" and "password manager" bloat, you are left with a Cognitive Buffer Layer. A product that simply aggregates decisions from existing apps and presents them as a "Daily 3-Question Digest" is a lower-risk, high-value tool that doesn't require you to be a security firm or a scraper-maintenance slave.

---

## Andy's Response to the Review

The review evaluated Eave as a venture-backed SaaS product heading to the App Store. That's not what Eave is. Eave is a personal tool first — built to solve Andy's problem, with the option to productise later if it proves valuable. Key corrections:

1. **Eave is a personal tool first.** The correct first question is: does this solve Andy's problem? Not: will the market pay for it?
2. **V3 doesn't need banking access.** Eave reads email. Subscription renewal emails are a pattern match. Surface them with reminders, let Andy act manually. No FCA, no Open Banking, no TrueLayer.
3. **Mac Mini doesn't work.** VPS with LLM API calls is the model, even for personal use.
4. **£4.99/month pricing was premature.** Monetisation belongs in the productisation phase, not the personal tool phase.
5. **Suppertime is not a blocker.** Suppertime is built and in TestFlight. It shouldn't gate Eave.
6. **The goal is reducing personal overwhelm programmatically.** Not a SaaS business. Not competing with Superhuman.
7. **The review surfaced hidden intent.** Andy scoped a personal tool but wrote it like a product pitch. The review held up a mirror. This is the most valuable outcome.

---

## Synthesis

### Points of Agreement (all three flagged)

These carry the most weight — three different models with different training data identified the same issue.

| Issue | Claude | GPT | Gemini | Severity |
| ----- | ------ | --- | ------ | -------- |
| V3-V5 are speculative / too broad for a solo dev | ✓ | ✓ | ✓ | High |
| Password manager (V4) should not be built | ✓ | ✓ | ✓ | High |
| Security and compliance completely unaddressed | ✓ | ✓ | ✓ | High |
| V1 "2 weeks" timeline is unrealistic | ✓ | ✓ | ✓ | High |
| Mac Mini infrastructure assumption is untested | ✓ | ✓ | ✓ | Medium |
| Browser automation (V4/V5) is a maintenance nightmare | ✓ | ✓ | ✓ | High |
| LLM costs not modelled against subscription pricing | ✓ | ✓ | ✓ | Medium |
| No error recovery / trust repair for misclassification | ✓ | ✓ | ✓ | High |
| Apple Intelligence is a platform-level competitive threat | ✓ | ✓ | ✓ | High |
| The core insight (capture + triage UX for ND brains) is worth preserving | ✓ | ✓ | ✓ | — |

**Andy's response:** The unanimous agreement on scope, security, and timeline issues is validated. The re-scope addresses all of these. The V3 banking concern is eliminated by reframing as email pattern matching. V4 is killed. V5 moves to speculative future vision. Infrastructure moves to VPS. Pricing removed from personal tool phase.

### Points of Disagreement

| Issue | Claude Says | GPT Says | Gemini Says | Andy's Call |
| ----- | ----------- | -------- | ----------- | ----------- |
| V0 viability | V0 is valuable but "zero friction for non-technical users" is a contradiction | V0 is "the gold" — the best part of the whole idea | V0 has LLM hallucination risk for routing | V0 is Andy's personal tool. Non-technical user framing was wrong. Hallucination risk is real — add confirmation for destructive routes. |
| Email as core pain | Valid for Andy, flagged as self-referential | Challenged whether ND people actually want inbox management vs accepting chaos | Didn't challenge the email pain point specifically | It's a personal tool — Andy's pain is the only pain that matters for Track A. Market validation is a Track B question. |
| Competitive landscape framing | Flagged Apple Intelligence, SaneBox, Superhuman as competitors | Listed 15+ competitors across all version categories | Flagged platform enclosure (Apple/Google building this natively) as existential | For a personal tool, "competitors" = "could Andy use an existing tool instead?" Apple Intelligence and Gmail smart sorting are the real alternatives to evaluate. Not Superhuman at $30/month. |
| Kill V3-V5 or keep as vision? | Separate V3-V5 as future vision, not committed roadmap | Kill V3-V5 entirely | Strip everything except the "cognitive buffer layer" | Keep V3 (simplified as email pattern matching). Kill V4. V5 to speculative future vision. The phased approach is sound — the problem was scoping V3-V5 like committed products. |

### Glaze Check

| Model | Adversarial Quality | Notes |
| ------ | ------------------- | ----- |
| Claude | Strong | Identified the right issues but over-indexed on market/SaaS concerns that don't apply to a personal tool. The "five products in a trenchcoat" framing was the sharpest insight. |
| GPT | Strong | Surprisingly aggressive — zero glaze detected. "Scoped it like a side project" and "liability ladder" were direct hits. The line-by-line quoting of the doc was effective. Formatting was messy but substance was strong. |
| Gemini | Strong | Most concise of the three. "High-Liability, Low-Moat" was a clean summary. Best framing of the platform enclosure risk. The "Cognitive Buffer Layer" save was the most actionable reframe. |

**Calibration update:** All three models performed well on this review. GPT did not glaze — this is notable and should be recorded as a calibration data point. When GPT is this critical, the issues are real. Gemini's "God Mode" framing was apt. All three converged on the same core save (V0 + V1 capture/triage UX), which gives high confidence in that direction.

### SCOPING.md Updates Required

Based on combined findings from all three models and Andy's corrections:

- [ ] Reframe entire document as Track A (personal tool), not product/SaaS
- [ ] Add "Risks & Compliance" section — scoped to personal data handling (GDPR for personal email), not FCA/SOC2
- [ ] Add "Competitive Landscape" section — framed as "what exists that Andy could use instead" (Apple Intelligence, Gmail smart sorting, SaneBox)
- [ ] Revise V1 timeline from "2 weeks" to realistic estimate (4-6 weeks including Gmail OAuth verification for personal use)
- [ ] Add error recovery / trust repair section — what happens when Eave misclassifies, undo flows, confidence thresholds, "never auto-archive from unknown senders" safeguards
- [ ] Reframe V3 as email-based subscription detection + reminders, remove all Open Banking / TrueLayer / FCA references
- [ ] Kill V4 (password manager) entirely — rationale: liability profile wrong for solo dev, existing tools are mature and security-audited
- [ ] Move V5 (browser agent) to clearly marked "Future Vision — Speculative" section
- [ ] Replace Mac Mini infrastructure with VPS + LLM API calls throughout
- [ ] Remove £4.99/month pricing — monetisation is a Track B concern
- [ ] Remove Suppertime as a sequencing dependency
- [ ] Add honest framing: "personal operations layer that may become a product" not "SaaS email client"
- [ ] Run Track A evaluation criteria against the re-scoped version

### Impact on Decision

- **Before review:** Pending (blocked on Suppertime)
- **After review:** Pending — needs re-scope as Track A personal tool, then re-evaluate
- **Rationale:** The core insight survives and is strong. The scoping document was over-indexed on productisation. Eave needs to be re-scoped as a personal tool first (Track A evaluation) with a clear gate before any productisation effort. V0 + V1 are worth building for Andy. V2 is plausible. V3 (simplified) is plausible. V4 is killed. V5 is speculative future vision. The tri-model review successfully prevented premature productisation and surfaced hidden assumptions — exactly what the review process is designed to do.
