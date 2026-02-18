# BirthBuild

**Status:** Inbox → Ready for Scoping
**Date Captured:** 2026-02-16
**Source:** Chat with doula training instructor + claude.ai session
**Source Chat:** Current session

---

## One-Liner

AI-powered static website builder for birth workers — chat your way to a live site in minutes.

## Problem

Non-technical solo practitioners (doulas, midwives, birth workers) overpay for Squarespace/Wix, spend days or weeks wrestling with drag-and-drop builders, and still end up with mediocre sites. Professional web design costs £1,500–5,000. The decision tree for a small business static site is shallow — style, colours, fonts, content slots — but the existing tools make it feel deep.

## Solution

A chatbot gathers preferences and content through a guided conversation, produces a structured site spec, and triggers a MAI build pipeline that generates and deploys a WCAG-compliant static site to a provisioned subdomain. An instructor creates workshop sessions, invites students via magic link, and monitors progress from an admin dashboard. Students can also edit via a form-based dashboard that reads/writes the same data as the chatbot.

## Why Now

- AI content generation is good enough to produce professional copy from a few prompts
- MAI V3 can scaffold a full static site from a structured spec
- The doula instructor is a ready-made first customer with a cohort of students
- "Drag and drop is dead, long live chat" — the interaction model shift is happening

## Differentiator

Not a drag-and-drop builder. Not a template marketplace. A conversation that produces a deployed site. Zero technical skill required. Professional quality output at a fraction of the cost and time.

## Architecture

- **Frontend:** React + Vite + Tailwind (PWA)
- **Backend:** Supabase (Postgres, Auth via magic link, Edge Functions, Storage)
- **AI:** Claude API (chatbot + content generation, proxied through Edge Functions)
- **Build:** MAI pipeline (site spec → static HTML/CSS/JS)
- **Hosting:** Netlify (auto-provisioned subdomains under birthbuild.com)
- **Multi-tenant:** Instructor = tenant, students scoped by RLS

## First Customer

Doula training instructor who currently teaches students to build websites on Squarespace. She pays Claude API costs, Andy retains all IP. Her students are the usability test and the distribution channel.

## Horizontal Scale Play

The entire architecture is profession-agnostic. Swap the system prompt context, colour palette presets, and FAQ templates → same platform serves therapists, personal trainers, nutritionists, photographers, tutors, wedding planners. Each profession vertical is a tenant configuration, not a new codebase.

## Revenue Model

- V1: Portfolio project. Instructor pays API costs. Andy retains IP.
- V2: License per instructor/tenant — Free (5 students) / Pro £49/mo (unlimited) / Enterprise (white-label, custom)
- Long-term: Per-profession vertical licensing. Each new profession is minimal marginal effort.

## Scoring (Idea Factory Evaluation)

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| Personal pain point | 3 | Built small business sites for years, knows the pain |
| Solo dev feasibility (V1 < 2 weeks) | 5 | PWA + Supabase + MAI, all known stack |
| Dopamine Labs fit | 5 | AI-powered tool, accessibility-first |
| Monetisation path | 5 | Clear licensing model, horizontal scaling |
| Market size | 5 | Every solo practitioner in every profession |
| Defensibility | 3 | Execution speed + MAI pipeline, not deep moat |
| Neurodivergent-friendly build | 5 | Hyperfocus project, clear scope, fast feedback loop |
| User excitement | 4 | Instructor was immediately interested |
| **Total** | **35/40** | **Strong promote** |

## Existing Artefacts

- `SCOPING.md` — Full product specification (data model, features, build phases, chatbot flow)
- `CLAUDE.md` — MAI project config (folder structure, coding standards, constraints)
- `uk-doula-website-research.md` — Market research (design patterns, SEO, accessibility, colour palettes)
- Domain: `birthbuild.com` (registered, Namecheap)
- Repo: `github.com/andrewisherwood/birthbuild`
- Supabase project: created, Pro tier
- Netlify: existing Pro account

## Decision

**PROMOTE.** All artefacts exist. Infrastructure provisioned. First customer identified. Ready for MAI.
