# Idea Factory — Claude Code Scaffold Prompt

Scaffold a git repository called `idea-factory`. This is a structured ideation pipeline for Dopamine Labs — a place to capture, develop, evaluate, and promote product ideas. The primary interface is Claude Code with Opus 4.6. No web app, no build tools, purely markdown-native with git as the backbone.

## Repository Structure

```
idea-factory/
├── claude.md
├── README.md
├── .gitignore
├── inbox/
│   └── .gitkeep
├── ideas/
│   └── _template/
│       ├── SCOPING.md
│       └── CAPTURE.md
├── parked/
│   └── .gitkeep
├── killed/
│   └── .gitkeep
├── promoted/
│   └── .gitkeep
├── reviews/
│   └── _template/
│       └── WEEKLY-REVIEW.md
└── frameworks/
    ├── EVALUATION.md
    └── DECISION-LOG.md
```

## File Contents

### claude.md

Write a comprehensive project prompt with the following sections:

**Identity & Context:**
- You are the ideation partner for Andy Isherwood, solo dev at Dopamine Labs
- Andy is a solutions architect and product engineer based in Folkestone, UK
- He works ~15 hours/week while being a stay-at-home dad to Flora
- He has AuDHD — low-friction capture and externalised executive function are critical
- He's been building for the web for 32 years, has 18 years of professional kitchen experience
- His products ship under the Dopamine Labs umbrella

**Portfolio Context (existing products):**
- **Suppertime** — Meal planning app for families. iOS (SwiftUI). 100 TestFlight beta users. Preparing for App Store submission.
- **MAI (Multi-Agent Infrastructure)** — Claude Code plugin system for automating dev workflows. V3 with three-layer memory architecture.
- **Adaptive Coach** — AI-powered cycling training platform.
- **Eave** — Personal AI assistant for email management and life admin. Part of S.A.F.E. suite for neurodivergent brains.
- **Blur** — RSVP speed reading app. Early concept stage.

**Your Role:**
- Help capture raw ideas with minimal friction (even a single paragraph brain dump is valid input)
- Develop ideas from raw capture through structured scoping
- Stress-test ideas against the evaluation framework in `frameworks/EVALUATION.md`
- Be direct and honest — kill bad ideas early, don't be precious
- Track decisions and rationale in `frameworks/DECISION-LOG.md`
- Run periodic review sessions when asked

**Idea Lifecycle:**
1. **Inbox** → Raw captures land in `inbox/` as individual markdown files. Naming: `YYYY-MM-DD-short-slug.md`. These are brain dumps — no structure required.
2. **Scoping** → When an idea has legs, promote it to `ideas/<idea-name>/` with a `CAPTURE.md` (the original dump, preserved) and a `SCOPING.md` (structured assessment using the template).
3. **Decision** → After scoping, the idea gets one of three outcomes:
   - **Promoted** → Moved to `promoted/` with a summary. A new repo will be created (MAI integration in future).
   - **Parked** → Moved to `parked/` with rationale. Revisited during periodic reviews.
   - **Killed** → Moved to `killed/` with rationale. Preserved for reference, not deleted.
4. **Review** → Weekly review sessions generate a report in `reviews/YYYY-MM-DD.md`.

**Working Style:**
- Autonomous execution with quality gates, not permission prompts
- When Andy dumps a raw idea, capture it to inbox immediately, then ask if he wants to develop it further now or leave it for later
- During scoping, use the evaluation framework but don't be formulaic — think critically
- When an idea conflicts with or overlaps an existing product, flag it immediately
- Always ask which codebase/platform a brief targets before generating it (PWA vs iOS vs both)
- Scoping documents must be named SCOPING.md

**Periodic Review Protocol:**
When Andy asks for a review (or weekly when prompted):
1. Surface all inbox items — triage into scope/park/kill
2. Check parked ideas — has context changed? New capabilities? Market shifts?
3. Review active scoping items — any blockers or decisions needed?
4. Assess portfolio balance — is Andy spreading too thin?
5. Generate `reviews/YYYY-MM-DD.md` with findings and recommendations
6. Be honest about capacity — 15hrs/week is the hard constraint

**Future Extensions (do not build now, but design for):**
- MAI V3 integration: auto-scaffold repos from promoted ideas
- iOS app interface for capture and review
- Integration with Andy's other tools and workflows

### README.md

Write a clean README that explains:
- What this repo is (structured ideation pipeline for Dopamine Labs)
- The lifecycle (inbox → scoping → decision → promotion/park/kill)
- How to use it (open with Claude Code, dump ideas, run reviews)
- That this is the foundation layer — MAI integration and iOS app coming later

### .gitignore

Standard markdown project gitignore. Include `.DS_Store`, `*.swp`, `.vscode/`, `.idea/`, `node_modules/` (future-proofing).

### ideas/_template/CAPTURE.md

```markdown
# [Idea Name]

**Date:** YYYY-MM-DD
**Source:** [Where did this come from? Chat, shower thought, user feedback, etc.]

## Raw Capture

[Paste or write the original idea here. No structure needed. Stream of consciousness is fine.]

## Initial Gut Check

- **Problem it solves:**
- **Who has this problem:**
- **Why me / why now:**
```

### ideas/_template/SCOPING.md

```markdown
# [Idea Name] — Scoping Document

**Status:** Draft | In Review | Decision Made
**Date Created:** YYYY-MM-DD
**Last Updated:** YYYY-MM-DD
**Decision:** Pending | Promoted | Parked | Killed

---

## Problem Statement

[What specific problem does this solve? Who experiences it? How painful is it?]

## Proposed Solution

[High-level description. What does v1 look like?]

## Target Platform

[iOS / PWA / Web / CLI / API — be specific]

## Evaluation Criteria

### Personal Dog-fooding
- [ ] Does Andy personally have this problem?
- [ ] Would Andy use this daily/weekly?

### Build Feasibility
- [ ] Can v1 ship in under 2 weeks of solo dev time?
- [ ] Does it fit within the ~15hr/week constraint?
- [ ] Are there hard technical unknowns?

### Portfolio Fit
- [ ] Does this fit under the Dopamine Labs umbrella?
- [ ] Does it overlap with or cannibalise an existing product?
- [ ] Does it strengthen the portfolio narrative?

### Market & Monetisation
- [ ] Is there a clear monetisation path?
- [ ] What's the competitive landscape?
- [ ] Is this a vitamin or a painkiller?

### Neurodivergent Design
- [ ] Does it reduce executive function load (not add to it)?
- [ ] Is the core interaction low-friction?
- [ ] Does it work with how Andy's brain works, not against it?

## Technical Considerations

[Stack choices, dependencies, integration points, known risks]

## Open Questions

[What needs answering before a decision can be made?]

## Decision & Rationale

[Filled in when a decision is made. Include reasoning, not just the outcome.]
```

### reviews/_template/WEEKLY-REVIEW.md

```markdown
# Weekly Idea Review — YYYY-MM-DD

## Inbox Triage

| Idea | Action | Rationale |
|------|--------|-----------|
| | | |

## Parked Ideas Reassessment

| Idea | Status Change? | Notes |
|------|---------------|-------|
| | | |

## Active Scoping Updates

| Idea | Progress | Blockers | Next Step |
|------|----------|----------|-----------|
| | | | |

## Portfolio Health Check

- **Current active builds:**
- **Estimated weekly hours committed:**
- **Capacity for new work:** Yes / No / Maybe
- **Spreading thin?**

## Recommendations

[What should Andy focus on? What should he stop? What's the highest-leverage move?]
```

### frameworks/EVALUATION.md

Write a decision framework document with these criteria (expand on each with 2-3 sentences of guidance):

1. **Personal Pain** — Does Andy personally experience this problem? Products born from genuine need ship better and faster.
2. **Ship Speed** — Can v1 be built and shipped in ≤2 weeks? If not, can it be scoped down until it can?
3. **Portfolio Coherence** — Does it fit Dopamine Labs' emerging identity? (Tools for real life, often with a neurodivergent-friendly angle.)
4. **Solo Dev Viability** — Can this be maintained by one person working 15hrs/week alongside existing products?
5. **Monetisation Clarity** — Is the business model obvious? Subscription, one-time purchase, freemium? If you have to think hard about it, that's a flag.
6. **Technical Leverage** — Does it reuse existing infrastructure (MAI, shared components, existing APIs)?
7. **Market Timing** — Is there a reason to build this now rather than later?
8. **Executive Function Tax** — Does this product reduce cognitive load or add to it? Both for users and for Andy as the maintainer.

Include a simple scoring guide: each criterion scored 1-5, with a total threshold of 28+ to proceed to build, 20-27 to park for review, below 20 to kill.

### frameworks/DECISION-LOG.md

```markdown
# Decision Log

A chronological record of all idea decisions. Preserved for pattern recognition and accountability.

| Date | Idea | Decision | Score | Key Rationale | Revisit? |
|------|------|----------|-------|---------------|----------|
| | | | | | |
```

## After Scaffolding

1. Initialise as a git repo with an initial commit
2. Confirm the structure is correct
3. Do NOT populate with any existing ideas yet — Andy will do that in a follow-up session

## Notes

- Keep all markdown clean and consistent
- No unnecessary tooling or dependencies
- This repo should be useful from the first commit with nothing but Claude Code and a terminal
