# Eave — Idea Capture / Quick Thought Input

**Date:** 2026-02-14
**Source:** Conversation about idea-factory mobile access. Andy built a Siri Shortcut to capture ideas to GitHub and realised the UX gap between "technically inclined person wiring up API tokens" and "normal person who just wants to talk and capture."

## Raw Capture

Eave should have a universal capture layer — voice or text input that routes thoughts to the right place. "Hey Eave, idea: what if we built X" and it lands in the idea factory inbox, formatted, committed, done. But it's bigger than just ideas. The same mechanic works for:

- Capture an idea → routes to idea-factory inbox
- Remind me to do X → creates a reminder
- Email thought: tell Y about Z → drafts an email for review
- Note about Flora's school thing → routes to family/school context
- Suppertime: we liked that recipe → flags it in Suppertime

The insight is that capture and routing are Eave's core competency. The user shouldn't need to know where things go — they just talk, and Eave figures out the destination based on context and intent.

This also has standalone product potential. A £0.99 LTD "quick capture" app that just does voice/text → structured markdown in a repo. No GitHub setup, no API tokens, no Shortcuts builder. Download, sign in, talk, done. Could serve as Eave's Trojan horse — get users into the ecosystem with a dead simple capture tool, upsell to full Eave when it ships.

**Key question:** Is this a standalone product, an Eave feature, or both (standalone app that becomes Eave's capture UI when Eave launches)?

## Initial Gut Check

- **Problem it solves:** Friction between having a thought and getting it recorded in the right place. The Siri Shortcut proves the concept but the UX is developer-only.
- **Who has this problem:** Anyone whose brain moves faster than their organisation system. Especially neurodivergent brains where the gap between thought and capture is where ideas die.
- **Why me / why now:** The routing insight — talk and Eave figures out the destination — is the core of what Eave is. This could be V0: the capture layer that exists before email triage even ships.
