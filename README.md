# Idea Factory

A structured ideation pipeline for [Dopamine Labs](https://dopaminelabs.io). Capture, develop, evaluate, and promote product ideas — all in markdown, all in git.

## What is this?

Idea Factory is where product ideas go to be taken seriously. It's a low-friction system for a solo dev with AuDHD who needs to externalise executive function and make decisions systematically rather than impulsively.

The primary interface is [Claude Code](https://claude.ai/code) with Opus. No web app, no build tools — purely markdown-native with git as the backbone.

## Lifecycle

Every idea follows the same path:

```
inbox/ → ideas/ → tri-model review → promoted/ | parked/ | killed/
```

1. **Inbox** — Brain dump. A single sentence, a paragraph, a voice-to-text ramble. Drop it in `inbox/` as `YYYY-MM-DD-short-slug.md` and move on.
2. **Scoping** — When an idea has legs, it gets promoted to `ideas/<idea-name>/` with a structured scoping document. This is where it gets stress-tested against the evaluation framework.
3. **Tri-Model Review** — Completed scoping doc is sent to Claude, GPT, and Gemini with an adversarial prompt. Findings synthesised, SCOPING.md updated with surfaced issues.
4. **Decision** — Every reviewed idea gets a verdict:
   - **Promoted** — Green light. A `MARKETING-BRIEF.md` is completed, then moved to `promoted/` for handoff to MAI.
   - **Parked** — Not now, but not never. Moved to `parked/`, revisited during reviews.
   - **Killed** — Dead, with dignity. Moved to `killed/`, preserved for reference.
5. **Review** — Periodic review sessions triage the inbox, reassess parked ideas, and check portfolio health.

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
