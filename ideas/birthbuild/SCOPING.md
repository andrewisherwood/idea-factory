# BirthBuild — Scoping Document

**Status:** In Review
**Track:** B (Product) — see note below
**Date Created:** 2026-02-16
**Last Updated:** 2026-02-16
**Decision:** Pending — awaiting tri-model review
**Domain:** birthbuild.com (registered, Namecheap)
**Repo:** github.com/andrewisherwood/birthbuild
**Supabase:** Project created, Pro tier

---

> **Track Note:** BirthBuild doesn't fit the standard Track A → Track B pipeline. It's not a personal tool — Andy doesn't need a doula website. It's a product from day one with an identified first customer. Evaluating directly on Track B criteria. The "personal use" promotion gate doesn't apply, but the rigour of Track B evaluation does.

---

## Problem Statement

Non-technical solo practitioners in the birth work industry — doulas, midwives, birth educators, lactation consultants — need professional websites but face a painful gap between "do it yourself" and "pay a professional":

| Option | Cost | Time | Quality |
|--------|------|------|---------|
| DIY on Squarespace/Wix | £13-25/mo ongoing | Days to weeks of struggle | Variable — depends on the practitioner's design sense |
| Pre-made doula template | £30-400 one-time + platform fees | Hours of setup, still confusing | Decent starting point but generic |
| Template + maintenance (e.g. Doula Darcy) | £250 one-time + £45/mo | Less DIY but still setup work | Good, within template constraints |
| Custom design agency (The Website Doula, Doula Design Co.) | £4,000-10,000+ one-time + platform fees | Weeks of turnaround | Professional, bespoke |

The price gap between "struggle with Squarespace" (£13/mo) and "get a professional site" (£4,000+) is enormous. For a newly certified doula whose business charges £800-3,500 per birth, the professional option is prohibitive.

**The specific first-customer problem:** A doula training instructor teaches students to build websites on Squarespace as part of their business setup. The students struggle. The process eats workshop time. The instructor wants a better path for her cohort.

---

## Proposed Solution

A guided chatbot that gathers preferences and content through conversation, produces a structured site specification, and triggers a build pipeline that generates and deploys a WCAG-compliant static site. The instructor creates workshop sessions, invites students via magic link, and monitors progress from an admin dashboard. Students can also edit via a form-based dashboard.

**V1 scope:**
- Chat-based site creation flow (style preferences, content gathering, photo upload)
- Birth-worker-specific content generation (services, about, testimonials, FAQ, booking)
- Static site generation and deployment to subdomain (e.g. jane-smith.birthbuild.com)
- Instructor dashboard (create sessions, invite students, view progress)
- Student dashboard (form-based editing, same data as chatbot)
- WCAG AA compliance
- Mobile-responsive output
- Local SEO basics (meta tags, schema markup for local business)

**What V1 is NOT:**
- A general-purpose website builder
- A drag-and-drop editor
- A hosting platform (sites deploy to Netlify)
- A replacement for Squarespace/Wix for ongoing site management

---

## Target Platform

PWA (React + Vite + Tailwind)

---

## Architecture

- **Frontend:** React + Vite + Tailwind CSS (PWA)
- **Backend:** Supabase (Postgres, Auth via magic link, Edge Functions, Storage)
- **AI:** Claude API for chatbot conversation and content generation, proxied through Supabase Edge Functions
- **Build Pipeline:** MAI generates static HTML/CSS/JS from structured site specification
- **Hosting:** Netlify (auto-provisioned subdomains under birthbuild.com)
- **Multi-tenant:** Instructor = tenant, students scoped by Row Level Security

---

## Competitive Landscape

### The AI Website Builder Market Is Saturated

There are 15+ significant AI website builder products in early 2026. The "describe your business, get a website" interaction model is **table stakes, not a differentiator**:

| Product | Model | Price | Notes |
|---------|-------|-------|-------|
| **Wix Harmony + Aria** | Conversational chat (ChatGPT-powered) | £14/mo+ | Most mature chat-to-site implementation. Full editing via natural language. |
| **Durable** | Prompt-based, 30-second generation | £10/mo | Targets service professionals specifically. Built-in CRM, invoicing. |
| **Squarespace Blueprint** | Guided questionnaire + brand personality | £13/mo+ | Best design quality. 20 years of expertise. |
| **Hostinger** | Prompt-based, under 1 minute | £2.50/mo intro | Cheapest. Aggressive pricing. |
| **Framer** | Prompt-based, Figma-quality output | £8/mo | Best design output. Designers love it. |
| **Hocoos** | 8-question questionnaire | £12/mo | Simplest UX. All-in-one. |
| **HeyBoss** | Chat-based, builds sites + apps | £16/mo | Broad ambition beyond websites. |
| **Mixo** | Prompt-based landing pages | £7/mo | Landing pages only. 750k+ creators. |
| **B12** | AI draft + optional human design | £35/mo+ | Service-professional focused. Optional "done for you" at £1,700. |

**BirthBuild cannot compete on technology.** The chat-to-site model is commoditising. Any of these platforms will produce a functional site from a description.

### The Birth Worker Vertical Is Unserved By AI

No AI-powered website builder targets birth workers specifically. The vertical is served entirely by:

1. **Human design agencies** — The Website Doula, Doula Design Co., For Doulas. Excellent but £4,000-10,000+.
2. **Template sellers** — Doula Darcy (£250 + £45/mo), Etsy Squarespace templates (£25-125).
3. **DIY on general platforms** — Squarespace, Wix, WordPress. No birth-worker-specific guidance.

The therapy/psychology vertical has **Brighter Vision** (£80-290/mo, managed service). Birth workers have nothing equivalent.

### Where The Gap Actually Is

The gap is NOT "chat to website" — that's been done. The gap is:

1. **Vertical-specific AI that speaks "birth worker"** — understands the difference between a birth doula, postpartum doula, lactation consultant, and childbirth educator. Generates appropriate content, terminology, trust signals, and visual language.
2. **Price point between DIY and agency** — something in the £15-50/mo range that produces a professional site without the DIY struggle.
3. **Workshop/cohort model** — no website builder has an instructor-student flow. This is genuinely novel and maps directly to how birth workers are trained (cohort-based certification programmes).

### Competitive Risks

- **Wix/Squarespace could add doula templates.** A "Birth Doula" business type with pre-configured content closes much of the gap overnight. This is a when, not if.
- **Durable already targets service professionals.** If they add vertical-specific content packs, they're the closest competitor.
- **Brighter Vision could expand** from therapists to birth workers. Same business model, adjacent vertical.

---

## First Customer Model

**The instructor relationship is the real asset, not the technology.**

- Doula training instructor with a current cohort of students
- She pays Claude API costs; Andy retains all IP
- Students are the usability test and the distribution channel
- If they produce professional sites, those sites are marketing for BirthBuild
- Workshop sessions create natural word-of-mouth among birth workers

**Risk:** Single customer dependency. If the instructor relationship doesn't work out, the product has no distribution channel.

**Mitigation:** V1 is small enough to be a portfolio piece regardless. The IP and codebase have value even without this specific instructor.

---

## Revenue Model

| Phase | Model | Notes |
|-------|-------|-------|
| **V1** | Free. Instructor pays API costs. | Portfolio piece. Customer relationship. No revenue pressure. |
| **V2** | License per instructor/tenant. Free (5 students) / Pro £49/mo (unlimited) / Enterprise (white-label, custom). | Only if V1 proves the model works. |
| **Horizontal** | Per-profession vertical licensing. Same platform, different content packs. | Speculative. Do not evaluate V1 on this basis. |

**Honest assessment:** V1 generates zero revenue. V2 revenue depends on whether the workshop model scales beyond one instructor. The horizontal play is the standard "and then it works for everyone" vision that the factory is designed to catch. Evaluate V1 on its own merits, not on hypothetical horizontal scaling.

---

## Evaluation

### Track B Criteria

#### 1-6: Track A Criteria (adapted — this isn't a personal tool)

| Criterion | Score | Rationale |
|-----------|-------|-----------|
| **1. Personal Pain Severity** | 3 | Andy has built small business sites for years and knows the pain of clients struggling with builders. Not a personal daily pain — it's empathy-driven, not itch-driven. |
| **2. Build Simplicity** | 4 | V1 is a known stack: React, Supabase, Claude API, Netlify. MAI can scaffold. The chatbot flow is the main engineering challenge — content generation, site spec structure, deployment pipeline. Tight but feasible for a focused sprint. |
| **3. Maintenance Burden** | 3 | Static sites on Netlify are low-maintenance. But: the chatbot needs prompt tuning, the deployment pipeline needs monitoring, student sites need uptime. This isn't "set and forget" — it's a service. |
| **4. Overwhelm Impact** | 3 | Doesn't reduce Andy's personal overwhelm. Adds a new product to maintain. Net neutral to slightly negative on the overwhelm axis. |
| **5. Simplest Platform** | 4 | PWA is right. Static site output is right. Netlify hosting is right. Supabase for multi-tenant is right. No over-engineering in the stack choice. |
| **6. Already Exists Check** | 4 | For the specific combination of (AI-powered + birth-worker-specific + workshop/cohort model), nothing exists. For "AI website builder" generically, everything exists. The vertical specificity and cohort model are the genuine gap. |
| **Track A Subtotal** | **21** | |

#### 7-14: Track B Product Criteria

| Criterion | Score | Rationale |
|-----------|-------|-----------|
| **7. Portfolio Coherence** | 3 | AI-powered tool — fits Dopamine Labs' thesis. But it's not neurodivergent-focused, not personal productivity, not family-life tooling. It's a B2B SaaS for birth workers. Coherent-ish but a different flavour than Suppertime/Eave. |
| **8. Solo Dev Viability** | 3 | V1 is buildable solo. But running a service — uptime, customer support, bug fixes for student sites, prompt tuning — is ongoing operational load. Every student site that breaks is Andy's problem. At 15 hrs/week with Suppertime, Adaptive Coach, and MAI already in play, this is tight. |
| **9. Monetisation Clarity** | 2 | V1 is free. V2 pricing (£49/mo per instructor) is hypothetical. The workshop/cohort model is novel — there's no market validation for whether instructors will pay monthly for a website builder. The therapy vertical charges £80-290/mo (Brighter Vision) but that's a managed service, not a self-serve tool. Unclear if birth workers will pay enough to cover costs. |
| **10. Technical Leverage** | 5 | Strong. MAI for build pipeline. Supabase expertise from Suppertime. React/Tailwind from existing projects. Claude API integration is well-understood. This is one of the few ideas in the pipeline that can genuinely leverage the existing toolchain. |
| **11. Market Timing** | 3 | AI website builders are exploding — that's both opportunity and threat. The general market is crowding fast. The birth-worker vertical is unserved now, but any general platform could add a doula template pack. Timing is decent but the window may be short. The instructor relationship creates urgency — she has a cohort now. |
| **12. Executive Function Tax** | 2 | This is a service product with external users. Student sites that break, instructor questions, deployment issues, prompt tuning, content quality complaints. This adds cognitive load. It's not a personal tool that runs quietly — it's a product with customers who expect it to work. |
| **13. Compliance Surface Area** | 4 | Low compliance burden. Static sites. No health data. No payments in V1. GDPR applies to student personal data (names, emails, site content) but straightforward. Cookie consent on generated sites needed. Accessibility (WCAG AA) is a feature, not a burden. No regulated industry data. |
| **14. Platform Risk** | 2 | High. Wix and Squarespace already have AI builders. If either ships a "Doula" business type with pre-configured content, the value proposition narrows dramatically. Durable targets service professionals. The entire AI website builder category is converging on "describe and deploy." Andy's differentiation is vertical depth and the cohort model — both can be replicated by well-funded competitors. |
| **Track B Subtotal** | **24** | |

### Total Score: 45 / 70

**Verdict: Viable but risky (42-55 range).**

### Scoring Adjustments

No tri-model review completed yet. No scoring adjustments applied. Recommended before committing build time.

---

## Honest Assessment

### What's genuinely good

1. **Real customer, real distribution.** An instructor with students is the best possible V1 test — built-in users, built-in feedback, built-in word-of-mouth. Most ideas in this factory have no customer. This one does.

2. **Technical leverage is strong.** This is one of the few ideas that genuinely builds on Andy's existing stack. MAI, Supabase, React, Claude — no new technology risk.

3. **The cohort/workshop model is novel.** No website builder has this. It maps to how birth workers are actually trained. This is the one thing competitors can't trivially replicate.

4. **V1 is low financial risk.** Instructor pays API costs. No infrastructure spend beyond existing Supabase and Netlify accounts. If it fails, the sunk cost is Andy's time.

### What's concerning

1. **The chat-to-website model is not a differentiator.** Wix does this with ChatGPT. Durable does it in 30 seconds. The technology is commoditising. The vertical knowledge and cohort model are the only real differentiation.

2. **Defensibility is weak.** If Wix ships a "Birth Doula" business type — and they have 900+ business categories — the vertical gap closes. Andy's moat is execution speed and vertical depth. That's a head start, not a moat.

3. **V1 generates zero revenue.** V2 pricing is hypothetical. The business model is unvalidated. "Build it, then figure out pricing" has killed many products.

4. **This adds operational load.** Student sites that break at 11pm. Instructor questions about the dashboard. Prompt tuning when the AI generates bad content. At 15 hrs/week, every hour on BirthBuild is an hour not on Suppertime or Adaptive Coach.

5. **The "horizontal scale play" is scope gravity.** "Every solo practitioner in every profession" is the textbook seductive vision. V1 is birth workers. The decision to go horizontal is a separate decision made later with real data.

6. **Market size is niche.** Doulas are solo practitioners charging £800-3,500 per birth, often sliding scale. They are price-sensitive. The total addressable market for "doulas who need websites and will pay monthly" is small. The business only becomes meaningful if the horizontal play works — and that's speculative.

---

## Risk Register

| Risk | Severity | Likelihood | Mitigation |
|------|----------|-----------|------------|
| Wix/Squarespace ships doula-specific AI templates | High | Medium (12-18 months) | Ship first. Build vertical depth they can't replicate quickly. |
| Instructor relationship fails | High | Low | V1 has portfolio value regardless. Codebase is reusable. |
| 15 hrs/week capacity exceeded | High | High | Suppertime must be submitted to App Store first. Explicit time-boxing. |
| Student sites break / quality complaints | Medium | Medium | Automated testing. Template constraints (limited customisation = fewer failure modes). |
| AI content generation quality insufficient | Medium | Low | Claude is good at this. Prompt engineering and review flow. |
| Revenue model doesn't work | Medium | Medium | V1 is free — no revenue pressure. Evaluate V2 pricing only with real data. |

---

## Sequencing

1. **Now:** Complete Suppertime App Store submission. This is priority #1.
2. **When Suppertime is submitted:** Begin BirthBuild V1 build. Estimated 2-3 weeks focused work.
3. **After V1 ships:** Run with instructor's cohort. Gather feedback. Measure: do students actually produce professional sites?
4. **After cohort feedback (4-6 weeks):** Evaluate Track B viability with real data. Does the cohort model scale? Will other instructors pay?
5. **Only then:** Consider horizontal expansion, V2 pricing, and marketing brief.

---

## Open Questions

- What is the instructor's timeline? When is her next cohort? This determines urgency.
- How much prompt engineering is needed to generate birth-worker-specific content that's actually good? Needs prototyping.
- Custom domain support in V1 or V2? Subdomains are simpler but less professional.
- Does the instructor want white-label (her brand) or BirthBuild-branded?
- What's the instructor's reaction to Andy retaining IP vs. a licensing arrangement?

---

## Decision

**Pending.** Tri-model review recommended before committing build time. The score (45/70) puts this in "viable but risky" territory. The top three risks to evaluate:

1. **Capacity** — Can this be built without derailing Suppertime?
2. **Defensibility** — Is the vertical depth + cohort model enough to survive Wix/Squarespace adding doula templates?
3. **Revenue** — Is there a realistic path from "free for one instructor" to "sustainable income"?

The honest answer may be: **build V1 as a portfolio piece and client project (low risk), but don't evaluate it as a product until there's real usage data.**
