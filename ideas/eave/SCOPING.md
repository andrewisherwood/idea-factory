# Eave — Scoping Document

> *The part of your house that shelters and overhears.*

**Status:** Re-scoped after tri-model adversarial review
**Track:** A (Personal Tool)
**Date Created:** 2026-02-13
**Last Updated:** 2026-02-14 (re-scoped as Track A personal tool)
**Decision:** Pending — evaluate V0 + V1 for personal build
**Domain:** eave.is
**Suite:** S.A.F.E. — Suppertime · Adaptive Coach · Eave
**Source Chat:** https://claude.ai/chat/2cc1d680-b8fb-4847-aecf-3a6f75a691a8
**Tri-Model Review:** `TRI-MODEL-REVIEW.md` (completed 2026-02-14)

---

## Vision

Eave is Andy's personal operations layer. It starts with capture and grows into a life-management tool that can read, think, and act on his behalf. The first promise: **just talk, Eave figures out where it goes.** Then: **you never open an email again.**

This is a personal tool first. Built to reduce Andy's daily overwhelm — 6 email accounts, scattered capture, admin paralysis. It may become a product later if it proves valuable and there's genuine external interest, but productisation is a separate decision (Track B) that happens after the tool has earned its place.

Built for a neurodivergent brain — not because the market demands it, but because Andy's brain demands it. The friction of mundane digital tasks overwhelms executive function. Every version reduces friction. Every version earns more trust and more autonomy.

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

*V0 and V1 are the build scope. V2 and V3 are plausible extensions. Everything beyond is speculative future vision.*

### V0 — Universal Capture

**Core thesis:** Just talk. Eave figures out where it goes.

- Native iOS app (Xcode, personal use — no App Store submission needed initially): one text/voice input field, one submit button
- LLM-powered intent classification routes input to the right destination:
  - "Idea: what if we built X" → creates markdown in idea-factory `inbox/`, commits via GitHub API
  - "Remind me to do X" → creates a Reminder via iOS Reminders API
  - "Email thought: tell Y about Z" → drafts a note (queued for V1 email capabilities)
  - "Suppertime: we liked that recipe" → flags in Suppertime context (queued for integration)
  - Unclassified → saves to a general capture log for manual triage
- Siri Shortcut: "Hey Eave" triggers the input
- Offline capture with local queue, sync when back online

**Error safeguards:**
- Confirmation prompt for any route that creates external side effects (GitHub commit, sending email draft)
- "Undo last capture" action available for 60 seconds after submission
- Unclassified captures go to a safe default (general log), never silently routed to a wrong destination
- Classification confidence threshold — below 80%, ask Andy to confirm the route

**Build estimate:** 1 weekend for the core app + routing. Iteration over the following week for edge cases.

### V1 — Email Triage + Backlog Clearing

**Core thesis:** You never open an email again.

- Connect email accounts (Gmail OAuth, IMAP for others)
- Backlog blitz: Eave works through old inboxes — classifies, archives, unsubscribes, flags
- Ongoing triage: daily digest of what matters, everything else handled silently
- Searchable archive of all historical email (indexed, not just forwarded)
- Context store that learns priorities over time (who matters, what's urgent, what's noise)
- Digest delivery: push notification or iOS widget — "3 things that need you today"

**Triage UX (critical for AuDHD):**
- Drip-feed by default — never more than the user's configured limit at once
- Decision batching: related decisions grouped ("Here are 5 newsletters — keep any?")
- Preference setting: "How do you want Eave to check in?" — daily digest / real-time max 3 questions per day / batch weekly review
- Sensible defaults with quiet confidence — if Eave is 90%+ sure, she acts and reports rather than asking

**Error recovery:**
- Nothing is permanently deleted in the first 30 days — archived, not destroyed
- Daily "Eave acted on these" summary with one-tap undo for any action
- "Never auto-archive from unknown senders" rule for the first 2 weeks (learning period)
- Escalation path: if Eave isn't sure, it asks. If it acted wrong, undo is always available.
- Missed item safety net: weekly "Eave might have missed these" digest of low-confidence classifications

**Build estimate:** 4-6 weeks. Gmail OAuth verification is the biggest unknown — Google requires privacy policy, security assessment, and manual review even for personal-use apps accessing email scopes. IMAP for Apple Mail is simpler. The LLM classification pipeline, digest generation, and backlog processing are the core build work.

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
Each version earns more trust and more autonomy:
- V0: "I'll remember that" (capture and route, no access to anything)
- V1: "I'll sort your email" (read-only actions, archive/label/flag)
- V2: "I'll manage your time" (create reminders, surface conflicts)
- V3: "I'll surface your subscriptions" (pattern matching on existing email data)

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

1. **Now:** Buy eave.is. Set up `hello@andrewisherwood.com` email infrastructure.
2. **V0 build:** Ship the capture app. Andy dogfoods immediately. Validates core UX.
3. **V1 build:** After V0 is stable and in daily use. But first: try SaneBox for 2 weeks. If SaneBox is good enough, reconsider V1 scope.
4. **V2-V3:** Only if V0 + V1 are stable, in daily use, and genuinely reducing overwhelm. Evaluated individually against Track A criteria.
5. **Track B gate:** If Eave has been in personal use for 4+ weeks and someone else expresses interest, consider productisation via Track B evaluation.

---

## Open Questions

- ~~Native iOS app from day one, or start with PWA/web digest and add native later?~~ Resolved: native iOS (Xcode) for personal use. No App Store submission needed initially.
- ~~Voice interface: Siri Shortcuts integration from V1, or wait until V5?~~ Resolved: V0 ships with Siri Shortcut trigger.
- LLM provider for triage classification: Claude API is the default. Haiku for simple classification, Sonnet for nuanced decisions.
- ~~Open Banking provider selection for V3~~ Resolved: V3 is email pattern matching only. No Open Banking.
- ~~Multi-user: when does Eave become available for Lily?~~ Deferred to Track B. Personal tool first.

---

## Track A Evaluation

| Criterion | Score | Rationale |
|-----------|-------|-----------|
| **1. Personal Pain Severity** | 5 | 6 email accounts, daily admin paralysis, ideas lost because capture is too slow. This is a daily, multi-times-daily frustration. |
| **2. Build Simplicity** | 4 | V0 is a weekend build. V1 is 4-6 weeks but well-understood technically. Not a weekend project, but not a moonshot. |
| **3. Maintenance Burden** | 3 | V0 is low maintenance (simple routing). V1 depends on email provider APIs which can change. Gmail OAuth is a recurring maintenance concern. LLM costs are ongoing but predictable. |
| **4. Overwhelm Impact** | 5 | This is the entire point. If V0 + V1 work, the daily email/capture overwhelm drops dramatically. Net overwhelm reduction is the core thesis. |
| **5. Simplest Platform** | 3 | V0 could be simpler (Siri Shortcut exists already) but LLM routing adds genuine value over static shortcuts. V1 genuinely needs a server + iOS app. Can't shortcut this further without losing the value. |
| **6. Already Exists Check** | 3 | V0: nothing does LLM-powered capture routing from a single input. V1: SaneBox gets close. Need to try SaneBox before committing to V1. Honest score reflects that V1 might be partially solved already. |
| **Total** | **23** | **Probably worth it — but try SaneBox before committing to V1.** |

V0 alone scores higher (would be 26-27) because the build is simpler and nothing adequate exists. The combined V0+V1 score is pulled down by V1's maintenance burden and the SaneBox alternative. The recommendation: **build V0 now, try SaneBox for V1's problem space, then decide on V1.**
