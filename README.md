# Idea Factory

A structured ideation pipeline for [Dopamine Labs](https://dopaminelabs.io). Capture, develop, evaluate, and promote product ideas — all in markdown, all in git.

## What is this?

Idea Factory is where product ideas go to be taken seriously. It's a low-friction system for a solo dev with AuDHD who needs to externalise executive function and make decisions systematically rather than impulsively.

The primary interface is [Claude Code](https://claude.ai/code) with Opus. No web app, no build tools — purely markdown-native with git as the backbone.

## Lifecycle

Ideas are evaluated on two tracks. Most stay on Track A (personal tool). Track B (product) activates only when a personal tool has proven its value.

### Track A: Personal Tool (default)

```
inbox/ → ideas/ → [optional review] → build / park / kill
```

1. **Inbox** — Brain dump. A single sentence, a paragraph, a voice-to-text ramble. Drop it in `inbox/` as `YYYY-MM-DD-short-slug.md` and move on.
2. **Scoping** — When an idea has legs, it gets promoted to `ideas/<idea-name>/` with a structured scoping document. Evaluated against Track A criteria (personal utility, build simplicity, maintenance burden).
3. **Decision** — Build it, park it, or kill it.

### Track B: Product (conscious promotion)

```
[personal use 4+ weeks] → promotion gate → tri-model review → marketing brief → promoted/ → MAI
```

When a personal tool has proven its value (4+ weeks of use, genuine interest from others, Andy actively wants to productise), it can be promoted to Track B for full product evaluation, adversarial review, and marketing brief.

- **Promoted** — Green light. `MARKETING-BRIEF.md` completed, moved to `promoted/` for handoff to MAI.
- **Parked** — Not now, but not never. Moved to `parked/`, revisited during reviews.
- **Killed** — Dead, with dignity. Moved to `killed/`, preserved for reference.

### Review
Periodic review sessions triage the inbox, reassess parked ideas, and check portfolio health.

## How to use it

1. Open this repo in Claude Code
2. Dump an idea — just describe it in conversation
3. Claude captures it to `inbox/` and offers to develop it further
4. When ready, scope it using the evaluation framework in `frameworks/EVALUATION.md`
5. Make a decision: promote, park, or kill
6. Run `reviews/` sessions periodically to stay on top of the pipeline

## Structure

```
idea-factory/
├── CLAUDE.md              # Project prompt for Claude Code
├── README.md              # You are here
├── inbox/                 # Raw idea captures
├── ideas/                 # Ideas being scoped
│   └── _template/         # Templates for new ideas
├── promoted/              # Ideas green-lit for build
├── parked/                # Ideas on hold
├── killed/                # Ideas that didn't make it
├── reviews/               # Periodic review reports
│   └── _template/         # Review template
└── frameworks/            # Evaluation criteria & decision log
```

## What's next

This is the foundation layer. Future integrations planned:

- **MAI V3 integration** — Auto-scaffold repos from promoted ideas
- **iOS capture app** — Quick capture from mobile
- **Workflow integration** — Connect with existing Dopamine Labs tooling
