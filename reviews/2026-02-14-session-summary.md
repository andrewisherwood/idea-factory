# Session Summary — 2026-02-14

## Idea Factory: Built & Operational

- Scaffolded the `idea-factory` git repo — structured ideation pipeline for Dopamine Labs with lifecycle management (inbox → scoping → review → decision), evaluation framework, and project-aware `claude.md`
- Live on GitHub: github.com/andrewisherwood/idea-factory
- Added **marketing brief as a hard gate** — no idea gets promoted to MAI without a completed MARKETING-BRIEF.md
- Marketing Agent idea parked (depends on Eave capabilities)

## Tri-Model Adversarial Review: Designed & Proven

- Added tri-model adversarial review gate (Claude, GPT, Gemini) to the pipeline between scoping and decision
- Created TRI-MODEL-REVIEW.md template with adversarial prompt, synthesis tables, glaze check, and scoring impact
- **Ran the first live review on Eave** — all three models engaged critically, zero glazing, unanimous agreement on 10 critical issues
- The review prevented premature productisation and surfaced hidden assumptions that would have wasted weeks of build time

## Dual-Track Evaluation Framework: Created

- **Track A (Personal Tool)** — 6 criteria, max 30 points. Default track for all new ideas. No market sizing, no monetisation. Just: does this solve Andy's problem?
- **Track B (Product)** — 14 criteria, max 70 points. Only activates after 4+ weeks of personal use and independent interest
- TRACK-PROMOTION.md gate template created for the conscious decision between tracks
- This stopped the factory from evaluating personal tools as if they were SaaS businesses

## Eave: Scoped, Roasted, Reframed

- Started as a six-version SaaS platform (email → calendar → subscriptions → passwords → browser agent)
- Tri-model review killed V4 (password manager), parked V5 (browser agent), simplified V3 (subscriptions via email pattern matching, no Open Banking)
- Reframed V0 as the **ingest layer for the household management app** — not a standalone capture tool
- Reframed V1 email triage as a **CC workflow task** (20 min/week), not a product feature
- Track A score: 25 — "probably worth it, try simpler alternatives for email first"
- Removed Suppertime as a blocker, removed pricing, replaced Mac Mini with VPS

## Household Management App: Captured

- The real product that's been hiding behind Eave, Flare, and the Skylight concept for months
- Modules: Suppertime (already built), Rewards, Tasks (Reminders sync), Calendar (Calendar sync), Photos (Photos sync)
- Eave V0 is the front door / ingest layer
- Sitting in inbox ready for scoping

## Idea Factory as a Product: Captured & Parked

- Recognised the factory itself is the real product — a regulation tool for AuDHD polymaths
- Captures the insight that the factory protects you from your own enthusiasm
- Deliberately parked — the factory caught itself trying to become the next shiny thing

## Siri Shortcut: Built & Working

- Voice-to-repo capture in ~10 seconds via Siri → GitHub API
- Debugged through: bearer token formatting, base64 line breaks, filename collisions (added seconds), timestamp sourcing from correct variable, success/failure notifications
- "Hey Siri, Idea Factory" → dictate → committed to inbox

## Project Inbox Zero: 44,143 Emails Archived

| Account  | Before | After | Archived |
|----------|--------|-------|----------|
| Chef     | 13,921 | 748   | 13,173 (95%) |
| Traiteur | 25,004 | 2,049 | 22,955 (92%) |
| Bugle    | 10,336 | 2,327 | 8,009 (77%) |
| Yardsale | 32     | 26    | 6 (19%) |
| **Total** | **49,293** | **5,150** | **44,143 (90%)** |

- Built `email_triage.py` (~750 lines) — multi-account CLI with 2-pass workflow
- 7 bugs found and fixed live during the session
- Nothing deleted — all archived messages remain in All Mail
- Intelligent triage: CC classified by sender frequency, then body analysis for unknowns
- Only surfaced what needed human attention

## Password Consolidation: 1,599 Passwords Unified

- Exported from 1Password, two Google accounts, and iOS Keychain
- Imported everything into Apple Passwords
- One source of truth across all devices
- 1Password subscription can be cancelled
- 20 minutes of work

## Blur: Retrieved & Parked

- Pulled full spec from chat history into idea factory
- Parked in `parked/blur/` — vitamin with niche market, good weekend project when there's bandwidth

## Pipeline Status

The full ideation pipeline is now: **inbox → scoping (Track A default) → tri-model review → decision → marketing brief → promoted → MAI**

With a conscious gate between Track A (personal tool) and Track B (product).

---

*Total commits to idea-factory today: 10+*
*Total emails archived: 44,143*
*Total passwords consolidated: 1,599*
*Time: One Valentine's Day afternoon and evening*
