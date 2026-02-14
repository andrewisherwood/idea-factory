# Eave — Scoping Document

> *The part of your house that shelters and overhears.*

**Status:** Re-scoped after tri-model adversarial review + reframing
**Track:** A (Personal Tool)
**Date Created:** 2026-02-13
**Last Updated:** 2026-02-14 (reframed — V0 is household app ingest layer, email triage is CC workflow)
**Decision:** V0 folded into household management app. Email triage validated as CC workflow before building product.
**Domain:** eave.is
**Suite:** S.A.F.E. — Suppertime · Adaptive Coach · Eave
**Source Chat:** https://claude.ai/chat/2cc1d680-b8fb-4847-aecf-3a6f75a691a8
**Tri-Model Review:** `TRI-MODEL-REVIEW.md` (completed 2026-02-14)

---

> **2026-02-14 Reframing:** Eave V0 has been reframed as the ingest layer for the household management app (see `inbox/2026-02-14-household-management-app.md`). Email triage (previously V1) has been reframed as a CC workflow task, not a product feature. V2+ remains speculative future vision.

---

## Vision

Eave is Andy's personal operations layer. The name and the core insight — **just talk, Eave figures out where it goes** — survive, but the shape has changed.

**V0 (capture + routing) is now the ingest layer for the household management app.** It's not a standalone tool. It's the voice/text front door that routes "Add pasta bake to next week" to Suppertime, "Flora needs PE kit tomorrow" to Tasks/Reminders, and "Book dentist" to Calendar. The capture-and-route insight is preserved but serving a specific product rather than floating on its own.

**Email triage is not a product. Email is a chore.** Andy's email problem doesn't need a product — it needs a periodic task. Claude Code connects via IMAP, reads the inbox, classifies, presents a digest. Andy decides. 20 minutes on a Sunday evening. If that workflow proves insufficient, the specific gaps will reveal exactly what automation is actually needed.

Built for a neurodivergent brain — not because the market demands it, but because Andy's brain demands it.

---

## Architecture Overview

### Infrastructure
- **Host:** VPS with LLM API calls (Claude API for classification and triage)
- **Single-tenant:** This is Andy's tool. One instance, one user, one context store.
- **Compute pattern:** Batch processing on a polling schedule (email every 5–10 mins, calendar hourly). Dormant most of the time. Lightweight and cheap to run.

### Email Integration
- Gmail: OAuth (note: requires Google Cloud verification even for personal use — see Risks)
- Apple/other IMAP: standard IMAP credentials
- Two parallel modes:
  - **Live triage** — watching `hello@andrewisherwood.com`, classifying incoming mail, building digest
  - **Backlog mode** — connected to old accounts, working through history one account at a time

### Backlog + Unified Inbox Strategy
- Day one: set up `hello@andrewisherwood.com`, turn on forwarding from all old accounts
- Eave connects to each old inbox via IMAP, processes backlog in situ (archives, unsubscribes, flags)
- Anything historically worth keeping gets indexed in a searchable archive (solves the "find my NI number in a 5-year-old payslip" problem)
- Once an old account's backlog is cleared, only forwarded mail arrives — old account becomes dormant
- End state: one clean inbox, full historical search, Eave watching everything

---

## Version Roadmap

*V0 is the ingest layer for the household management app. Email triage is a CC workflow, not a product version. V2+ is speculative future vision.*

### V0 — Household App Ingest Layer

**Core thesis:** Just talk. Eave figures out which module handles it.

V0 is the voice/text front door for the household management app. It only makes sense as part of that app, not standalone.

- Voice/text input via Siri Shortcut ("Hey Eave")
- LLM-powered intent classification routes to household app modules:
  - "Add pasta bake to next week" → **Suppertime** (meal planning)
  - "Flora needs her PE kit tomorrow" → **Tasks** (synced with iOS Reminders)
  - "Book dentist for Thursday" → **Calendar** (synced with iOS Calendar)
  - "Flora earned 5 stars today" → **Rewards** (family rewards system)
  - "Save this photo to Flora's album" → **Photos** (synced with iOS Photos)
  - "Idea: what if we built X" → **Idea Factory** (commits to inbox/ via GitHub API)
  - Unclassified → saves to a general capture log for manual triage
- Memory layer learns routing preferences from corrections over time
- Offline capture with local queue, sync when back online

**Error safeguards:**
- Confirmation prompt for any route that creates external side effects (GitHub commit, calendar event)
- "Undo last capture" action available for 60 seconds after submission
- Unclassified captures go to a safe default (general log), never silently routed to a wrong destination
- Classification confidence threshold — below 80%, ask Andy to confirm the route

**Build estimate:** V0 is scoped as part of the household management app. See `inbox/2026-02-14-household-management-app.md` for the parent idea.

### Email Triage — CC Workflow (not a product)

**Status:** Parked as product. Active as workflow.

**Core thesis:** Email triage doesn't need a product. It needs a periodic task.

**The workflow:**
- Claude Code connects to Andy's email accounts via IMAP
- CC reads inbox, classifies by priority and type, presents a structured digest
- Andy reviews and decides: archive, respond, flag, unsubscribe
- Weekly "inbox zero" session — 20 minutes on a Sunday evening
- CC does the reading, Andy does the deciding

**Why workflow over product:**
- No OAuth verification process (IMAP with app-specific passwords)
- No server infrastructure needed — runs on Andy's machine
- No maintenance burden — CC handles the classification, no custom code to maintain
- If the workflow proves insufficient, the specific gaps reveal exactly what automation is needed
- Avoids building a product to solve a problem that might be solved by a 20-minute weekly habit

**If the workflow fails:** The failure mode will be specific — "CC can't handle X" or "weekly isn't frequent enough for Y." Those specifics inform whether a product is needed and what it should actually do, rather than building a speculative V1 based on assumptions.

### V2 — Calendar + Reminders Integration (plausible extension)

**Core thesis:** Eave now knows your schedule and your email.

- Calendar integration (Apple Calendar, Google Calendar)
- Time-aware triage: "This email about tomorrow's meeting is urgent" vs "This can wait until Monday"
- Reminders integration: Eave can create, modify, and surface reminders
- Daily briefing: combines email digest with today's schedule
- Conflict detection: "You have two meetings at 3pm" / "This email asks for Thursday but you're fully booked"

*Build decision depends on V1 proving valuable in daily use.*

### V3 — Subscription Awareness (plausible extension)

**Core thesis:** Surface recurring costs from email patterns. Let Andy decide what to cancel.

Eave already reads email (V1). Subscription services send renewal emails, receipts, and billing notifications. V3 is pattern matching on existing data, not a new data source.

- Detect subscription-related emails (renewal notices, receipts, billing confirmations)
- Surface a "Your subscriptions" summary: service name, cost, frequency, last seen
- Reminder before renewal: "Netflix is renewing in 3 days for £15.99. Keep it?"
- Manual cancellation — Andy cancels subscriptions himself. Eave surfaces the information, not the action.
- No Open Banking. No bank API. No FCA compliance. No TrueLayer. Just email pattern matching.

*Build decision depends on V1 and V2 proving valuable.*

---

## Future Vision — Speculative

*The following are ideas for where Eave could go if V0-V3 prove valuable. They are not committed roadmap. They should not influence the build decision for V0-V1. They are preserved here for reference only.*

### Browser Agent (formerly V5)

Eave could eventually operate websites on Andy's behalf — ParentPay top-ups, school portal checks, form submissions. This requires browser automation (Puppeteer/Playwright) which is fragile and maintenance-heavy. All three models in the tri-model review flagged this as the highest-risk, highest-maintenance part of the roadmap. Worth revisiting only if V1-V3 are stable and running without significant maintenance burden.

### Email Simplification (formerly part of V4)

Consolidating accounts across services to a single `hello@` email address via automated site-by-site updates. Depends on browser agent capabilities. High effort, moderate value.

---

## Killed

### Password Manager (formerly V4)

**Killed during tri-model review (2026-02-14).** All three models flagged this unanimously.

**Rationale:** Liability profile is wrong for a solo dev personal tool. Storing credentials requires security infrastructure (encryption at rest, breach response, audit trails) that adds overwhelm rather than reducing it. Existing tools — 1Password, Bitwarden, Apple Keychain — are mature, security-audited, and actively maintained by dedicated teams. Building a password manager adds to Andy's cognitive load. Using an existing one reduces it. This is the opposite of Eave's thesis.

---

## UX Principles

### Never Overwhelm
- Drip-feed by default — triage questions chunked, never more than configured limit
- Decision batching — related decisions grouped together
- Progressive disclosure — start simple, reveal complexity only when needed
- Sensible defaults — if Eave is confident, act and report rather than ask

### Trust Ladder
Each capability earns more trust and more autonomy:
- V0: "I'll remember that" (capture and route to the right household app module)
- Email workflow: "I'll read your email so you don't have to" (CC-assisted weekly triage)
- V2: "I'll manage your time" (calendar + reminders integration, if V0 proves valuable)
- V3: "I'll surface your subscriptions" (email pattern matching, if V2 proves valuable)

---

## Risks & Compliance

### Gmail OAuth Verification
Even for personal use, accessing Gmail email scopes requires Google Cloud project setup and potentially verification. For a single-user app not distributed publicly, this may be simpler (unverified app warning is acceptable for personal use). But if Eave ever goes to Track B, full verification (CASA security audit, privacy policy, Google review) is required — budget 4-8 weeks for this alone.

### Personal Data Handling
Eave processes Andy's email — personal data under UK GDPR. For personal use this is straightforward (processing your own data for your own purposes). If Eave ever processes other people's data (Track B), GDPR obligations escalate significantly: data processing agreements, retention policies, breach notification, right to erasure.

### LLM API Costs
Every classification call costs money. Rough estimates:
- V0 capture routing: negligible (a few calls per day)
- V1 email triage: ~£5-15/month depending on email volume and model choice (Haiku for simple classification, Sonnet/Opus for nuanced triage)
- Backlog blitz: one-time cost, potentially £20-50 for thousands of historical emails

These are acceptable personal costs. They would need unit economics modelling before Track B.

### LLM Misclassification
The biggest trust risk. Mitigation strategy:
- Confidence thresholds — below threshold, ask don't act
- Nothing permanently deleted for 30 days
- Daily action summary with one-tap undo
- Escalation for unknown senders
- Weekly "might have missed" safety net digest

---

## Competitive Landscape (what exists that Andy could use instead)

| Alternative | What it does | Why it's not enough |
|-------------|-------------|-------------------|
| **Apple Intelligence (Mail)** | Email summarisation, smart sorting | Summarises but doesn't triage. Doesn't batch decisions. Doesn't learn Andy's specific priorities. No digest — still requires opening Mail. |
| **Gmail smart sorting** | Auto-categorises into tabs | Coarse categories. Doesn't handle cross-account triage. Doesn't learn. |
| **SaneBox** | ML email triage, digest | Closest competitor for V1. £3/month. Worth trying before building. If SaneBox solves 80%+ of the email problem, V1 may not be worth building. |
| **Apple Reminders / Notes** | Quick capture | No intelligent routing. No LLM classification. Fine for single-purpose capture, not for "just talk and it goes where it should." |
| **Siri Shortcuts** | Automation | Already built one for idea-factory. Limited to pre-defined routes. No LLM-powered intent classification. |

**Honest assessment:** SaneBox is the biggest threat to V1's justification. Before building V1, Andy should try SaneBox for 2 weeks. If it solves the email triage problem well enough, V1 can be descoped or killed. V0 (capture + routing) has no adequate existing alternative — nothing does LLM-powered intent routing from a single input.

---

## Sequencing

1. **Now:** Household management app captured in idea factory. Needs scoping.
2. **V0:** Build as part of household management app, not standalone. The ingest layer ships when the household app ships.
3. **Email triage:** Validate CC workflow approach. Weekly IMAP sessions. If it works, no email product needed.
4. **V2+:** Speculative. Calendar integration, subscription surfacing. Only revisit if V0 ships as part of the household app and proves valuable.
5. **Track B gate:** If the household management app has been in personal use for 4+ weeks and someone else expresses interest, consider productisation via Track B evaluation.

---

## Open Questions

- ~~Native iOS app from day one, or start with PWA/web digest and add native later?~~ Resolved: native iOS (Xcode) for personal use. No App Store submission needed initially.
- ~~Voice interface: Siri Shortcuts integration from V1, or wait until V5?~~ Resolved: V0 ships with Siri Shortcut trigger.
- LLM provider for triage classification: Claude API is the default. Haiku for simple classification, Sonnet for nuanced decisions.
- ~~Open Banking provider selection for V3~~ Resolved: V3 is email pattern matching only. No Open Banking.
- ~~Multi-user: when does Eave become available for Lily?~~ Deferred to Track B. Personal tool first.

---

## Track A Evaluation

*Evaluated as the Eave concept overall — V0 ingest layer + email CC workflow.*

| Criterion | Score | Rationale |
|-----------|-------|-----------|
| **1. Personal Pain Severity** | 5 | 6 email accounts, daily admin paralysis, family life scattered across apps, ideas lost because capture is too slow. Multi-times-daily frustration. |
| **2. Build Simplicity** | 4 | V0 is part of the household app build (not standalone). Email workflow is zero-build — just a CC session. The complexity is in the household app, not Eave itself. |
| **3. Maintenance Burden** | 4 | V0 maintenance is tied to the household app. Email CC workflow has zero maintenance — it's a manual process. No APIs to break, no OAuth to maintain. |
| **4. Overwhelm Impact** | 5 | This is the entire point. Capture routing + weekly email triage = significant daily overwhelm reduction. |
| **5. Simplest Platform** | 4 | Email triage reframed as CC workflow is the simplest possible platform — no app, no server, no code. V0 as part of the household app avoids building a standalone tool. |
| **6. Already Exists Check** | 3 | V0 routing: nothing does LLM-powered intent routing from a single input to household modules. Email: SaneBox and Apple Intelligence exist but don't match the CC workflow's flexibility. |
| **Total** | **25** | **Build it.** V0 as household app ingest layer. Email as CC workflow. |

The reframing significantly improved the score. Moving email to a CC workflow eliminated the biggest build complexity and maintenance concerns. V0 as part of the household app gives it a clear home instead of floating as a standalone tool.
