# Eave — Scoping Document

> *The part of your house that shelters and overhears.*

**Status:** Scoped
**Date Created:** 2026-02-13
**Last Updated:** 2026-02-14 (V0 capture phase added)
**Decision:** Pending (blocked by Suppertime shipping first)
**Domain:** eave.is
**Suite:** S.A.F.E. — Suppertime · Adaptive Coach · Eave
**Source Chat:** https://claude.ai/chat/2cc1d680-b8fb-4847-aecf-3a6f75a691a8

---

## Vision

Eave is a personal operations layer. It starts with capture and grows into a life-management agent that can read, think, and act on your behalf. The first promise: **just talk, Eave figures out where it goes.** Then: **you never open an email again.** Over time, you never manage a password, chase a subscription, or fight a clunky website again either.

Built for neurodivergent brains first — people drowning in admin not because they're incapable, but because the friction of mundane digital tasks overwhelms executive function. Every version reduces friction. Every version earns more trust and more autonomy.

---

## Architecture Overview

### Infrastructure
- **Host:** Mac Mini (M3/M4) or VPS — single machine for early users
- **Per-tenant model:** Each user gets an isolated instance with their own context store, triage preferences, and integration credentials
- **Compute pattern:** Batch processing on a polling schedule (email every 5–10 mins, calendar hourly). Dormant most of the time. A single Mac Mini can serve hundreds of users.

### Email Integration
- Gmail: OAuth for modern access
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

*Sequenced by pain point severity, not strict technical dependency. V0 ships before Suppertime is done — it's small, standalone, and validates the core UX. V1 and V2 are locked. V3–V5 resequenced from original plan to prioritise subscriptions (independent, high pain) over browser agent capabilities.*

### V0 — Universal Capture

**Core thesis:** Just talk. Eave figures out where it goes.

- Native iOS app: one text/voice input field, one submit button
- LLM-powered intent classification routes input to the right destination:
  - "Idea: what if we built X" → creates markdown in idea-factory `inbox/`, commits via GitHub API
  - "Remind me to do X" → creates a Reminder via iOS Reminders API
  - "Email thought: tell Y about Z" → drafts a note (queued for V1 email capabilities)
  - "Suppertime: we liked that recipe" → flags in Suppertime context (queued for integration)
  - Unclassified → saves to a general capture log for manual triage
- Sign in with Apple, no GitHub tokens, no API setup — zero friction for non-technical users
- Offline capture with sync when back online
- Siri Shortcut: "Hey Eave" triggers the input

**Why V0 ships first:**
- Small build — native iOS app with one screen, one input, one API call per route
- Validates capture-and-route as Eave's core interaction pattern before investing in email infrastructure
- Becomes Eave's permanent front door — every future version inherits this input layer
- Can ship while Suppertime is still in flight without competing for the same dev hours
- Dogfooding: Andy uses it daily, proving the UX before anything else gets built

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

### V2 — Calendar + Reminders Integration

**Core thesis:** Eave now knows your schedule and your email.

- Calendar integration (Apple Calendar, Google Calendar)
- Time-aware triage: "This email about tomorrow's meeting is urgent" vs "This can wait until Monday"
- Reminders integration: Eave can create, modify, and surface reminders
- Daily briefing: combines email digest with today's schedule
- Conflict detection: "You have two meetings at 3pm" / "This email asks for Thursday but you're fully booked"

### V3 — Subscription Audit

**Core thesis:** Find every recurring charge and kill the ones you don't use.

- Open Banking integration (TrueLayer or similar) to pull transaction history
- Dashboard: all active subscriptions, cost, last usage, cancellation difficulty rating
- Cross-reference with account inventory from V1 backlog sweep
- One-tap cancel via browser agent where possible
- Annual cost projection: "You're spending £2,400/year on subscriptions. Here's what you could save."
- Ongoing monitoring: "New recurring charge detected — £9.99/month to [service]. Keep it?"

### V4 — Email Simplification + Password Consolidation

**Core thesis:** Consolidate your digital identity. One email, one password store.

**Email simplification:**
- Account inventory from V1 backlog sweep: "You have accounts at 47 services across 3 email addresses"
- Eave navigates to each site via browser agent (Puppeteer), logs in, changes email to `hello@`
- Progress tracked: "Updated 34/47. 8 need manual intervention (2FA). 5 are defunct."
- Old email accounts become truly dormant, then closable

**Password manager:**
- Secure credential store (encrypted at rest, biometric unlock)
- Import from 1Password, Chrome saved passwords
- Consolidation: "You have 3 different logins for Amazon — merge them"
- Auto-fill via iOS extension or browser agent
- Credential health audit: weak, reused, breached passwords

### V5 — Autonomous Web Agent

**Core thesis:** Eave can operate any website on your behalf. Voice-activated life admin.

**Launch use case: ParentPay top-up**
- Voice command (Siri Shortcut): "Hey Eave, top up ParentPay twenty quid"
- Eave navigates to ParentPay, logs in, finds top-up page, enters £20, triggers direct debit
- Confirmation: "Done. ParentPay topped up £20. Balance now £35."
- Handles ParentPay's non-responsive site layout that's impossible on mobile while managing kids and trays

**General capabilities unlocked:**
- Browse Firefly and other school platforms for updates
- Submit forms on behalf of user
- Check and respond to school communications
- Any cumbersome website workflow reduced to voice command or single tap
- Action library: users define recurring web tasks as saved routines
- Safety: actions require user confirmation on first run, then can be set to auto-approve for trusted routines

**Note on Flare:** Originally conceived as a separate S.A.F.E. product for school platform integration (FireFly, ParentPay, SOCS APIs). Absorbed into Eave V5 — if Eave can navigate any website with credentials, Flare is just "Eave pointed at school websites," not a separate product. S.A.F.E. suite is now: Suppertime · Adaptive Coach · Eave.

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
- V3: "I'll watch your money" (read-only bank access, no spending actions)
- V4: "I'll simplify your accounts" (browser agent, credential management)
- V5: "I'll do it for you" (autonomous web actions, voice commands)

### Monetisation
- Freemium: V1 email triage free (limited accounts), paid for multi-account + backlog
- Subscription: £4.99/month or £39.99/year
- Each version tier unlocks with subscription — natural upsell as capabilities grow
- Lock-in through learned context — users don't churn because they'd lose their trained assistant

---

## Sequencing

1. **Now:** Buy eave.is. Set up `hello@andrewisherwood.com` email infrastructure.
2. **V0 build:** Ship the capture app. Andy dogfoods while Suppertime is still in flight. Validates core UX.
3. **Suppertime first:** Get Suppertime generating revenue, prove the ship-and-monetise loop.
4. **V1 build:** Eave as email triage for Andy (user zero). V0 capture app becomes the input layer. Run for 1–2 months, iterate.
5. **Eave beta:** Open to 50–100 users, same TestFlight model as Suppertime.
6. **V2–V5:** Sequential releases, each earning trust for the next.

---

## Open Questions

- Native iOS app from day one, or start with PWA/web digest and add native later?
- ~~Voice interface: Siri Shortcuts integration from V1, or wait until V5?~~ Resolved: V0 ships with Siri Shortcut trigger.
- LLM provider for triage classification: Claude API, local model on Mac Mini, or hybrid?
- Open Banking provider selection for V3 (Plaid, TrueLayer, etc.)
- Multi-user: when does Eave become available for Lily? Shared family context store or separate tenants with shared calendar visibility?

---

## Evaluation Criteria

- [x] Does Andy personally have this problem? **Absolutely — 6 email accounts, drowning in admin**
- [x] Would Andy use this daily? **Multiple times daily**
- [ ] Can v1 ship in under 2 weeks? **V1 email triage — yes, tight but feasible**
- [x] Does it fit under Dopamine Labs? **Core product — the flagship of S.A.F.E.**
- [ ] Does it overlap existing products? **No — complements Suppertime (meal admin) with life admin**
- [x] Clear monetisation path? **Freemium → subscription, strong lock-in through learned context**
- [x] Vitamin or painkiller? **Painkiller — email overload is a daily pain**
- [x] Reduces executive function load? **That's the entire point**
