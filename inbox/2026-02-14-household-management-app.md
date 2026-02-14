# Household Management App

**Date:** 2026-02-14
**Source:** Reflection during idea-factory session. Emerged from re-scoping Eave after tri-model review.

## Raw Capture

The household management app is the thing I've been circling around for months. It's not Eave. It's not Suppertime. It's the umbrella that ties everything together for running a family.

Modules:
- **Suppertime** — meal planning (already built, in TestFlight)
- **Rewards** — family rewards/chore tracking system
- **Tasks** — synced with iOS Reminders
- **Calendar** — synced with iOS Calendar
- **Photos** — synced with iOS Photos
- **Eave V0** — the ingest layer. Voice/text capture that routes to the right module.

The insight is that Eave V0 isn't a standalone product. It's the front door for this app. "Add X to Y" and it figures out which module handles it.

Email is explicitly NOT part of this. Email triage is a periodic CC workflow task, not a product feature.

This may be the Dopamine Labs flagship — the thing that ties the portfolio together. Or Suppertime may remain standalone and this becomes a separate product that integrates with it. Needs scoping.

## Initial Gut Check

- **Problem it solves:** Family life admin is scattered across 6 different apps. Executive function tax of context-switching between them is enormous.
- **Who has this problem:** Every family, but especially neurodivergent parents who struggle with the cognitive overhead of managing multiple systems.
- **Why me / why now:** Suppertime already exists as one module. The ingest layer insight crystallised today. The rest is incremental.
