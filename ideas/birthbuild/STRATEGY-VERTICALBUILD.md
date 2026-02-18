# VerticalBuild — Platform Strategy

**Author:** Andrew Isherwood / Dopamine Labs
**Date:** February 2026
**Status:** Strategy — pre-scoping
**Context:** This document defines the overarching product strategy for a horizontally scalable AI website builder platform. It provides the strategic frame for the technical SCOPING.md that follows.

---

## 1. Thesis

Every solo practitioner and small practice needs a website. Most of them have either no website, a bad one, or a generic one that says nothing specific enough for clients or AI search engines to find them.

The problem isn't building websites. The problem is getting the specification right. A website builder that asks the right questions, in the right order, with the right emotional intelligence for that profession, will produce a better site than one that asks generic questions or dumps the user into a blank template.

**Specification density determines output quality.** The denser the specification fed to the LLM, the more specific, professional, and findable the generated site. This is the core insight from BirthBuild and the foundation of everything that follows.

The product is not a website builder. The product is a domain-aware elicitation system that happens to output websites.

---

## 2. Product Architecture

### 2.1 Three Layers

```
┌──────────────────────────────────────────────────────────┐
│                      Core Engine                          │
│                                                           │
│  Chatbot framework · Follow-up logic · Density scoring    │
│  Site generation pipeline · Template system                │
│  Agentic SEO module · Persona testing harness              │
│  Dashboard · Auth · Billing · Deployment                   │
│                                                           │
│  ONE CODEBASE. Every vertical shares this.                │
└──────────────────────────┬───────────────────────────────┘
                           │
              Vertical Definition Layer
                           │
     ┌─────────┬─────────┬─────────┬─────────┬────────┐
     │  Birth  │ Physio  │Fitness  │Therapy  │ Trade  │ ...
     │ Workers │ therapy │         │         │        │
     │         │         │         │         │        │
     │  JSON   │  JSON   │  JSON   │  JSON   │  JSON  │
     └─────────┴─────────┴─────────┴─────────┴────────┘
                           │
              Customer-Facing Layer
                           │
     ┌─────────┬─────────┬─────────┬─────────┬────────┐
     │birthbui │physiopr │trainer  │therapy  │trusted │ ...
     │ld.com   │actice   │sites    │sites    │trades  │
     │         │.com     │.com     │.com     │.co.uk  │
     └─────────┴─────────┴─────────┴─────────┴────────┘
```

**Core Engine** — single repository. Chatbot, dashboard, billing, site generation, SEO, density scoring, persona harness. Shared across all verticals. Fix a bug once, every vertical benefits.

**Vertical Definition** — a JSON document per vertical. Defines services, credentials, follow-up triggers, emotional texture, personas, search patterns, schema.org types. Not code. Configuration. A new vertical is a new JSON file, not a new codebase.

**Customer-Facing** — separate domains per vertical. Each domain loads the core app with its vertical JSON. The physio who lands on physiopractice.com sees an app that was built for physiotherapists. Not an app that was built for everyone with a physio dropdown.

### 2.2 Perceived Customisation

This is the strategic insight that matters most. A physio who finds "PhysioPractice.com — AI websites built for physiotherapists" trusts that product immediately. It knows their credentials. It knows HCPC. It knows the difference between sports rehab and women's health. It asks intelligent questions. It feels like it was made for them.

A physio who finds "PractitionerBuild.com" and selects "Physiotherapy" from a dropdown gets the same product underneath. But the trust signal is weaker. The perception is "this was adapted for me" not "this was built for me."

Same engine. Same JSON. Different domain. Different landing page. Completely different conversion rate.

### 2.3 Why Not One Brand

One brand is easier to manage. One marketing site. One set of social accounts. One SEO strategy. But it loses the vertical specificity that makes the product compelling.

The approach: an umbrella entity (Dopamine Labs) owns the platform and the IP. Each vertical gets its own brand, domain, and landing page. The umbrella brand is invisible to end users. Practitioners see only the vertical brand. The umbrella brand surfaces only in investor decks, partnership conversations, and the blog series on andrewisherwood.com.

---

## 3. Vertical Definition Schema

Each vertical is a JSON document that tells the engine everything it needs to know about a profession. The engine reads this at runtime and adapts its behaviour accordingly.

```json
{
  "vertical": {
    "id": "physiotherapy",
    "name": "Physiotherapy",
    "singular": "physiotherapist",
    "plural": "physiotherapists",
    "domain": "physiopractice.com",
    "tagline": "AI websites built for physiotherapists"
  },

  "services": [
    {
      "id": "sports-rehab",
      "label": "Sports Rehabilitation",
      "subtypes": ["ACL recovery", "rotator cuff", "running injuries"],
      "searchTerms": ["sports physio", "sports injury physiotherapist"]
    },
    {
      "id": "post-surgery",
      "label": "Post-Surgical Rehabilitation",
      "subtypes": ["hip replacement", "knee replacement", "spinal surgery"]
    },
    {
      "id": "chronic-pain",
      "label": "Chronic Pain Management"
    },
    {
      "id": "womens-health",
      "label": "Women's Health Physiotherapy",
      "subtypes": ["pelvic floor", "postnatal", "pregnancy-related"]
    }
  ],

  "credentials": [
    { "id": "hcpc", "label": "HCPC Registered", "required": true },
    { "id": "csp", "label": "CSP Member" },
    { "id": "degree", "label": "BSc/MSc Physiotherapy" },
    { "id": "macp", "label": "MACP (Musculoskeletal)" }
  ],

  "experienceLevels": [
    "Newly qualified (0-2 years)",
    "Developing (2-5 years)",
    "Experienced (5-10 years)",
    "Senior/Specialist (10+ years)"
  ],

  "elicitation": {
    "followUpTriggers": [
      {
        "condition": "mentions sports",
        "ask": "What sports do you mainly work with? Any particular injuries you see most?"
      },
      {
        "condition": "mentions women's health",
        "ask": "Do you focus on pre or postnatal, pelvic floor rehabilitation, or both?"
      },
      {
        "condition": "mentions home visits",
        "ask": "What area do you cover for home visits? Any radius or postcode zones?"
      }
    ],
    "sensitiveTopics": [
      {
        "topic": "experience_level",
        "ifNew": "Frame as 'recent training means you're up to date with the latest evidence-based approaches'",
        "avoid": "Never ask 'how many patients have you treated'"
      },
      {
        "topic": "testimonials",
        "ifNone": "Normalise: 'Many physios starting out haven't collected testimonials yet. We can add these later as they come in.'"
      }
    ],
    "payoffSignals": {
      "location": "Patients searching for a physio in {area} will find your site",
      "specialism": "People looking for {specialism} specifically will see you, not just generic results"
    }
  },

  "personas": {
    "sparse": {
      "name": "Minimal Mike",
      "background": "Newly qualified, minimal info, short answers",
      "expectedDensity": [8, 14],
      "hardFails": ["More than 2 follow-ups on same topic"]
    },
    "detailed": {
      "name": "Detailed Diana",
      "background": "15 years experience, multiple specialisms, verbose",
      "expectedDensity": [21, 25],
      "hardFails": ["Asking for already-provided information"]
    },
    "nervous": {
      "name": "Nervous Nikhil",
      "background": "Just qualified, NHS background, first private practice",
      "expectedDensity": [10, 17],
      "hardFails": ["Asking about patient numbers without context"]
    }
  },

  "searchPatterns": [
    "physiotherapist near {location}",
    "sports physio {location}",
    "{condition} treatment {location}",
    "HCPC registered physiotherapist {area}",
    "private physiotherapy {location}"
  ],

  "schemaOrg": {
    "type": "MedicalBusiness",
    "additionalProperties": ["medicalSpecialty", "availableService"]
  },

  "landingPage": {
    "hero": "Your physiotherapy practice deserves a website that works as hard as you do",
    "subhead": "Answer a few questions. Get a professional site that patients can actually find.",
    "socialProof": "Join {count} physiotherapists who've built their practice website in under 15 minutes",
    "exampleServices": ["Sports Rehabilitation", "Post-Surgical Recovery", "Chronic Pain"],
    "ctaText": "Build your physio website"
  },

  "templates": {
    "family": "health-wellness",
    "colorSchemes": ["clinical-trust", "warm-approachable", "modern-minimal"]
  }
}
```

This is the entire vertical. Everything the engine needs. Adding physiotherapy as a vertical means writing this JSON (informed by automated research), reviewing it, and deploying it.

---

## 4. Automated Vertical Research

The research phase for each vertical follows a repeatable process. Most of it can be automated.

### 4.1 Process

```
1. CRAWL    — Search for 50-100 existing websites in the profession
2. EXTRACT  — Pull common patterns: services, credentials, page structure,
               terminology, client search queries
3. SYNTHESISE — LLM generates draft vertical JSON from extracted data
4. REVIEW   — Human reviews and refines. Adds emotional texture,
               sensitive topic handling, persona definitions
5. TEST     — Generate system prompt from JSON. Run persona harness.
               Iterate until all three personas pass
6. DEPLOY   — Add JSON to config. Create landing page. Launch
```

### 4.2 What's Automated vs Manual

| Step | Automated | Manual | Time |
|------|-----------|--------|------|
| Crawl | Web search + fetch | — | 5 mins |
| Extract | LLM pattern extraction | — | 10 mins |
| Synthesise | LLM generates draft JSON | — | 10 mins |
| Review | — | Refine JSON, add nuance | 2-3 hours |
| Test | Persona harness runs | Review results, adjust | 1-2 hours |
| Deploy | Script creates site | Domain purchase, DNS | 30 mins |
| **Total** | | | **~half a day** |

At half a day per vertical, 50 verticals is 25 working days. At 15 hours/week that's roughly 12 weeks. Achievable within 3 months alongside other work, or faster if batched.

### 4.3 Research Output

The crawl and extract steps produce a research document per vertical:

```json
{
  "vertical": "physiotherapy",
  "sitesCrawled": 87,
  "commonPages": ["home", "about", "services", "conditions", "testimonials", "contact", "booking"],
  "servicesFound": {
    "sports-rehabilitation": { "frequency": 0.72, "terms": ["sports injury", "sports rehab", ...] },
    "post-surgery": { "frequency": 0.58, "terms": ["post-operative", "surgical rehab", ...] }
  },
  "credentialsMentioned": {
    "HCPC": 0.95,
    "CSP": 0.78,
    "MACP": 0.23
  },
  "commonStructuredData": ["LocalBusiness", "MedicalBusiness"],
  "clientSearchQueries": ["physio near me", "sports physio london", ...],
  "tonePatterns": ["professional but approachable", "evidence-based", "patient-centred"],
  "gaps": ["very few mention AI search optimisation", "testimonials often generic"]
}
```

This feeds directly into the vertical JSON creation. The human review step is about adding judgment: which services to prioritise, how to handle newly qualified practitioners sensitively, what follow-up questions actually matter.

---

## 5. Technical Architecture

### 5.1 Single App, Multiple Tenants

One application codebase deployed once. The domain determines which vertical JSON loads. All verticals share the same database, auth, and API.

```
Request to physiopractice.com
  → DNS points to app
  → App reads domain
  → Loads physiotherapy.json from vertical configs
  → Chatbot, dashboard, generation pipeline all use this config
  → Generated site deploys to customer's chosen domain or subdomain
```

### 5.2 Infrastructure

| Component | Service | Plan | Cost | Notes |
|-----------|---------|------|------|-------|
| App hosting | Netlify / Vercel | Pro | ~$20/mo | Single app deployment |
| Database | Supabase | Pro | $25/mo | Multi-tenant, `vertical_id` on all tables |
| Customer sites | Netlify | Pro | ~$19/mo | One Netlify site per customer, deployed via API |
| LLM | Claude API (Sonnet) | Standard | ~$0.20/onboarding | Per-conversation cost |
| Domains | Various registrars | — | ~£10/yr each | One per vertical |
| DNS | Cloudflare | Free | Free | Manage all domains centrally |

### 5.3 Database Schema (Multi-Tenant)

```sql
-- Core tables, all filtered by vertical_id

verticals
  id              uuid PK
  slug            text UNIQUE      -- "physiotherapy"
  name            text             -- "Physiotherapy"
  domain          text             -- "physiopractice.com"
  config_json     jsonb            -- The full vertical definition
  status          text             -- "active", "draft", "paused"
  created_at      timestamptz

customers
  id              uuid PK
  vertical_id     uuid FK → verticals
  email           text
  business_name   text
  plan            text             -- "free", "pro"
  stripe_id       text
  created_at      timestamptz

specifications
  id              uuid PK
  customer_id     uuid FK → customers
  vertical_id     uuid FK → verticals
  data            jsonb            -- The collected specification
  density_score   integer          -- Computed density
  conversation_log jsonb           -- Full chatbot transcript
  version         integer          -- Spec version (re-edits increment)
  created_at      timestamptz

generated_sites
  id              uuid PK
  customer_id     uuid FK → customers
  specification_id uuid FK → specifications
  netlify_site_id text             -- Netlify API reference
  domain          text             -- Customer's site domain
  status          text             -- "building", "live", "error"
  deployed_at     timestamptz

-- RLS policies filter on vertical_id for isolation
-- Customers in one vertical never see another vertical's data
```

### 5.4 Deployment Pipeline (Per Customer Site)

```
1. Customer completes chatbot → specification saved
2. Density score computed
3. If score >= threshold:
   a. Generation pipeline runs (Claude API)
   b. HTML/CSS generated from specification + template
   c. Schema.org structured data generated
   d. llms.txt generated
   e. Netlify site created via API
   f. Files deployed to Netlify site
   g. Domain configured (customer's domain or {slug}.{vertical-domain})
   h. Status → "live"
4. If score < threshold:
   a. Dashboard shows gaps
   b. Customer prompted to "improve your site" (re-enter chatbot for missing fields)
```

### 5.5 Vertical Launch Pipeline (Admin Dashboard)

The admin dashboard for launching new verticals:

```
┌─────────────────────────────────────────────────┐
│  New Vertical: Physiotherapy                     │
│                                                   │
│  ☑ Vertical JSON validated                       │
│  ☑ Domain purchased: physiopractice.com           │
│  ☑ DNS pointed to app                            │
│  ☑ Landing page content in JSON                  │
│  ☑ Persona harness: 3/3 passed                   │
│  ☐ Stripe product created                        │
│  ☐ Supabase vertical row created                 │
│                                                   │
│  [Launch Vertical]                                │
│                                                   │
│  Status: 5/7 requirements met                    │
└─────────────────────────────────────────────────┘
```

Press the button. Script runs. Vertical goes live. The button triggers:

1. Create Supabase `verticals` row from JSON
2. Create Stripe products (free trial, pro plan) for this vertical
3. Deploy landing page (generated from JSON `landingPage` fields + shared template)
4. Verify domain DNS resolves
5. Run smoke test (hit the landing page, verify chatbot loads with correct vertical config)
6. Set status to "active"

---

## 6. Vertical Map

### 6.1 Target Verticals (Phase 1-3)

Grouped by template family (shared design language within each group):

**Health & Wellness** (template: clinical-trust)
Physiotherapists, osteopaths, chiropractors, sports therapists, massage therapists, acupuncturists, nutritionists, dietitians

**Mental Health** (template: warm-professional)
Counsellors, psychotherapists, CBT therapists, clinical psychologists, life coaches

**Birth & Family** (template: warm-organic)
Doulas (BirthBuild — exists), midwives (private), lactation consultants, sleep consultants, antenatal teachers

**Fitness** (template: energetic-bold)
Personal trainers, yoga teachers, pilates instructors, fitness coaches, sports coaches

**Beauty & Aesthetics** (template: luxe-minimal)
Aestheticians, beauty therapists, nail technicians, hair stylists (independent), tattoo artists

**Trades** (template: solid-practical)
Plumbers, electricians, builders, decorators, landscapers, gardeners, cleaners

**Education** (template: friendly-structured)
Tutors, music teachers, language teachers, driving instructors

**Creative Services** (template: portfolio-forward)
Photographers, videographers, florists, wedding planners, event planners

**Professional Services** (template: authoritative-clean)
Accountants, bookkeepers, solicitors (small practice), financial advisers, mortgage brokers

### 6.2 What This Doesn't Cover

Businesses that need e-commerce. Multi-location enterprises. Anything that needs complex custom functionality beyond a brochure site. Restaurants, retail, product-based businesses. Agencies with 50+ staff.

This is deliberate. The product covers the 80% of practitioners who need a professional, findable brochure site. Not the 20% who need a custom web application.

### 6.3 Vertical Prioritisation Criteria

Not all verticals are equal. Prioritise by:

| Factor | Why it matters |
|--------|----------------|
| Training pipeline exists | Distribution channel. Can you partner with a training provider? |
| Credential-heavy | More structured data = more SEO value = stronger value prop |
| Solo practitioners dominate | Solo = makes own website decisions. Partnership/corporate = procurement |
| Existing sites are poor | Bigger gap between current state and what you can offer |
| High search volume | "Physio near me" gets more searches than "craniosacral therapist near me" |
| Low competition from website builders | Nobody else targeting this vertical specifically |

Birth workers scored high on all of these. Physiotherapy, personal training, and counselling likely score similarly. Trades are massive volume but lower specification density (simpler sites).

---

## 7. Distribution Model

### 7.1 Training Provider Partnerships

Every profession has a moment where someone qualifies and needs a website. Training providers are the distribution channel.

```
Practitioner qualifies
  → Training provider offers "build your website" as part of the course
  → Practitioner clicks link to {vertical}build.com
  → Completes chatbot
  → Site live within 15 minutes
  → Training provider sees dashboard of their cohort's sites
```

The training provider gets value (their graduates look professional immediately). You get qualified leads who arrive motivated and pre-educated about their profession. The provider effectively does your marketing.

**Revenue share option:** Training providers could white-label the tool or receive a referral commission. This depends on volume and negotiation per vertical.

### 7.2 Professional Bodies

Same pattern, bigger scale. HCPC, CSP, BACP, CIMSPA. "Recommended website builder for members." Harder to land but higher volume and stronger trust signal.

### 7.3 Direct

SEO + content marketing. The blog series on andrewisherwood.com builds authority. Each vertical landing page targets "{profession} website builder" search queries. Direct sign-up, no partner involved.

### 7.4 Sequencing

```
Phase 1: One vertical, one training provider (BirthBuild — in progress)
Phase 2: Three verticals, organic + direct marketing
Phase 3: Ten verticals, first professional body partnership
Phase 4: 50 verticals, platform licensing to training providers
```

---

## 8. Unit Economics

### 8.1 Per-Vertical Costs

| Item | Cost | Frequency |
|------|------|-----------|
| Domain | ~£10 | Annual |
| Research + JSON creation | ~4 hours of time | One-off |
| Landing page | Generated from JSON | One-off |
| Persona testing | ~$3-5 | One-off |
| **Total setup cost** | **~£15 + 4 hours** | |

### 8.2 Per-Customer Costs

| Item | Cost | Frequency |
|------|------|-----------|
| Claude API (onboarding) | ~$0.20 | One-off |
| Claude API (site generation) | ~$0.50 | One-off |
| Netlify hosting (customer site) | ~$0 (within limits) | Ongoing |
| Stripe transaction fee | ~2.9% + 20p | Monthly |
| **Total per-customer marginal cost** | **~$1 setup, ~£0.45/mo** | |

### 8.3 Revenue Model

| Plan | Price | What they get |
|------|-------|---------------|
| Free | £0 | Chatbot experience + preview. Site not deployed. |
| Launch | £9/mo | Live site, custom domain, SEO, edits via chatbot |
| Practice | £19/mo | Multi-page, booking integration, priority rebuild |

### 8.4 Revenue Projections

| Scenario | Verticals | Customers/vertical | Price | Monthly Revenue | Monthly Cost | Margin |
|----------|-----------|-------------------|-------|----------------|-------------|--------|
| Seed | 5 | 20 | £9 | £900 | ~£100 | 89% |
| Early | 15 | 50 | £9 | £6,750 | ~£150 | 98% |
| Growth | 50 | 100 | £9 | £45,000 | ~£300 | 99% |
| Scale | 50 | 500 | £12 avg | £300,000 | ~£2,000 | 99% |

Infrastructure costs are essentially flat. Revenue scales linearly with customers. Margin improves with every customer because the fixed costs (Supabase, Netlify, your time on the engine) are amortised across more users.

---

## 9. Competitive Moat

### 9.1 What Competitors Would Need to Replicate

1. **Specification density as a concept.** Nobody is measuring or optimising for this. The blog series establishes prior art and public awareness.

2. **Domain-aware elicitation.** The vertical JSON contains accumulated knowledge about each profession. What questions to ask, what follow-ups matter, what emotional sensitivities exist. This compounds with every user conversation and every vertical added.

3. **Persona testing infrastructure.** Automated quality assurance for conversational UX. The harness catches regressions. Competitors would need to build this from scratch.

4. **Training provider relationships.** Distribution partnerships are relationship-based and exclusive-ish. Once a training provider embeds your tool in their curriculum, switching cost is high.

5. **Generated content quality.** Every site built produces data. What density scores produce the best sites. Which follow-ups actually improve output. Which templates convert. This data loop makes the product better over time. A new entrant starts at zero.

### 9.2 What Competitors Can Easily Copy

The idea of vertical-specific website builders. The use of chatbots for onboarding. The general architecture. None of this is protectable. The moat is execution speed, accumulated domain knowledge, and the relationships.

### 9.3 Speed as Strategy

Publish the thesis. Ship the product. Be three iterations ahead with real user data. The blog series documents the thinking publicly, establishing timestamps and authority. A competitor who reads the blog in 6 months still needs 6 months to build what you've already shipped.

---

## 10. Phased Roadmap

### Phase 1: Prove (Now → 3 months)

**Goal:** Validate the thesis with one vertical. Ship the density module, persona harness, and agentic SEO for BirthBuild. Measure the output quality difference. Collect before/after evidence.

**Deliverables:**
- Specification density module (SCOPING.md exists)
- Persona testing harness (SCOPING.md exists)
- Agentic SEO module (SCOPING.md exists)
- Blog posts 1-3 published with real evidence
- 5-10 live BirthBuild sites with density scores
- At least one training provider partnership active

**Success criteria:**
- Measurable density score improvement (old flow: 8-10, new flow: 18-22)
- Visual side-by-side difference between low and high density sites
- At least one user testimonial
- Blog post 1 published and cross-posted

### Phase 2: Abstract (3-6 months)

**Goal:** Extract the engine from BirthBuild. Create the vertical definition layer. Launch 2-3 additional verticals.

**Deliverables:**
- Core engine refactored into vertical-agnostic codebase
- Vertical JSON schema finalised
- Automated research pipeline (crawl → extract → draft JSON)
- Admin dashboard for vertical launch
- Second vertical live (likely physiotherapy or personal training)
- Third vertical live
- Blog posts 4-6 published

**Success criteria:**
- New vertical launched in under 1 day of manual work
- Persona harness runs against all verticals on every deploy
- 50+ total customers across all verticals

### Phase 3: Scale (6-9 months)

**Goal:** Horizontal scaling. 10-20 verticals live. First professional body partnership. Revenue sustaining.

**Deliverables:**
- 10-20 verticals launched
- Template families designed (5-8 covering all vertical groups)
- Professional body partnership (at least one)
- Pricing tiers implemented (free preview → paid)
- Blog post 7 published (compounding stack manifesto)
- Content strategy shifts from personal blog to product marketing

**Success criteria:**
- £5,000/month recurring revenue
- 200+ paying customers
- Automated vertical launch (half a day including research)

### Phase 4: Platform (9-12 months)

**Goal:** Platform play. Training providers can self-serve. Licensing model. Seed round narrative if desired.

**Deliverables:**
- Training provider dashboard (see their cohort's sites, track adoption)
- White-label option for training providers
- 50+ verticals
- API for third-party integration
- Comprehensive documentation

**Success criteria:**
- £20,000+/month recurring revenue
- Multiple training provider partnerships across different professions
- Platform defensible enough to raise if desired

---

## 11. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| LLM costs increase significantly | Low | Medium | Output is static HTML. Generation cost is one-off per site. Even 5x cost increase is negligible per customer. |
| Squarespace/Wix add AI chatbot onboarding | Medium | Medium | They'll go generic. Vertical specificity and domain knowledge is the moat. They won't build 50 vertical-specific prompt sets. |
| Training providers won't partner | Medium | High | Direct distribution via SEO works without partners. Partners accelerate but aren't required. |
| One vertical doesn't generalise to others | Low | High | BirthBuild architecture already separates domain knowledge from engine. The abstraction is designed in. |
| Customer churn at £9/month | Medium | Medium | Sites become the practitioner's online presence. Churn means taking their site down. Switching cost is real. Add SEO value over time that makes leaving costly. |
| Time constraint (15 hrs/week) | High | Medium | Automation reduces per-vertical effort. Engine improvements benefit all verticals. Focus on leverage. |

---

## 12. Open Questions for SCOPING.md

The following technical decisions need resolving in the detailed scoping document:

1. **App framework.** The core app that serves all verticals. React/Vite (consistent with BirthBuild)? Next.js? The choice affects deployment model.

2. **Customer site hosting model.** One Netlify site per customer (clean isolation, domain independence) vs subdomain routing from a single deployment. Former is cleaner, latter is cheaper.

3. **Billing.** Stripe. But how does multi-vertical billing work? One Stripe account with products per vertical? Or one Stripe Connect setup?

4. **Template system.** How do templates relate to vertical families? Is a template a full HTML/CSS bundle per family? Or a base template with CSS variable overrides?

5. **Domain management.** Customer domains vs subdomains. Custom domain setup flow (DNS instructions, verification). Who manages SSL?

6. **Vertical config hot-reload.** Can vertical JSON be updated without redeployment? Stored in Supabase and cached? Or deployed as static config files?

7. **Content editing post-generation.** Can customers re-enter the chatbot to update their spec? Or edit generated content directly? Both?

8. **Multi-page sites.** BirthBuild generates single-page sites. The platform should support multi-page (about, services, testimonials, contact as separate pages). How does this affect the generation pipeline?

9. **Image handling.** Where do customer images (headshots, clinic photos) get stored and served from? Supabase Storage? Cloudflare Images?

10. **Monitoring.** How do you monitor 500+ customer sites for uptime, build failures, SSL expiry? Automated alerts?

---

## 13. Immediate Next Actions

1. **Publish "The Spec Is the Prompt"** — timestamps the core thesis publicly
2. **Rebuild andrewisherwood.com** — blog module + case studies live
3. **Ship BirthBuild spec density module** — evidence for blog posts 2-4
4. **Ship persona testing harness** — evidence for blog post 5
5. **Ship agentic SEO module** — evidence for blog post 6
6. **Write SCOPING.md for platform abstraction** — resolves the open questions above
7. **Register domains** for top 5 target verticals before publishing blog posts

---

## 14. The Compound Loop

```
Better questions
    → Denser specifications
        → Richer generated sites
            → Richer structured data
                → Better AI search visibility
                    → More clients for practitioners
                        → More referrals to the platform
                            → More sites built
                                → More data to improve questions
                                    → Better questions
```

Every site built makes the next site better. Every vertical added makes the platform smarter. Every blog post makes the thesis more credible. Every training provider partnership opens a new cohort of practitioners.

The flywheel spins. The moat deepens. The spec is the prompt.

---

*STRATEGY-VERTICALBUILD.md — Dopamine Labs*
*February 2026*
