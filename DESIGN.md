# CancelScore — Final Design Document

## Executive Summary

**Problem:** Companies deliberately make it 5-10x harder to cancel a subscription than to start one. The FTC's Click-to-Cancel rule (2026) now requires parity, but enforcement is slow and consumers have no centralized resource to check cancellation difficulty, find step-by-step guides, or hold companies accountable.

**Solution:** CancelScore is a crowdsourced database that scores every subscription service on how hard it is to cancel (1-10 scale), provides step-by-step cancellation guides, and publishes rankings of the worst and best offenders — all powered by real user reports.

**Key Insight:** Cancellation difficulty is a *feature*, not a bug. Companies invest millions making signup frictionless and cancellation painful. CancelScore eliminates this information asymmetry by making exit difficulty transparent, searchable, and scored *before* you sign up.

---

## Target User Persona

**Primary: "Active Canceler" — Sarah, 32, Marketing Manager**
- Has 12 active subscriptions averaging $147/month
- Currently wants to cancel 3 of them but has been "putting it off for months"
- Last time she tried to cancel a gym membership, she had to go in person and was guilt-tripped
- Would Google "how to cancel [service]" and land on CancelScore
- Wants: Step-by-step instructions, know what obstacles to expect, do it fast

**Secondary: "Comparison Shopper" — Marcus, 28, Freelance Developer**
- Evaluating 2-3 project management tools for his workflow
- Wants to know which one won't trap him if he wants to switch later
- Would search both tools on CancelScore before committing
- Wants: Quick score comparison, exit ease as a selection criterion

**Tertiary: "Advocate/Journalist" — Dana, 45, Consumer Reporter**
- Writing an article about subscription dark patterns for a tech publication
- Needs data: which industries are worst, how many companies comply with FTC rules
- Would use rankings and category filters to build her story
- Wants: Sortable data, specific examples with report counts

---

## Complete Technical Spec

### Architecture (Simplified from Review)
```
┌──────────────────────┐     ┌─────────────────────────┐
│   React + Vite       │     │      Supabase            │
│   (Vercel)           │ ──→ │  ┌───────────────────┐  │
│                      │     │  │ PostgreSQL (DB)    │  │
│  • Search UI         │     │  │ PostgREST (API)   │  │
│  • Service profiles  │     │  │ Auth (Supabase)   │  │
│  • Report forms      │     │  │ Edge Functions    │  │
│  • Rankings          │     │  │  (score calc)     │  │
│                      │     │  └───────────────────┘  │
└──────────────────────┘     └─────────────────────────┘
```

No separate API server. React calls Supabase directly via client SDK. Row Level Security (RLS) enforces access. Edge Functions handle score recalculation on new reports.

### Tech Stack
| Component | Choice | Why |
|-----------|--------|-----|
| Frontend | React 19 + Vite + TypeScript | Modern, fast, ecosystem |
| UI | shadcn/ui + Tailwind CSS | Per coding standards |
| Backend/DB | Supabase (PostgreSQL) | Free tier, auth built-in, PostgREST API |
| Auth | Supabase Auth | Email/password + Google OAuth |
| Search | pg_trgm + full-text search | Fuzzy name matching, no external service |
| Logos | Clearbit Logo API | Dynamic, no storage needed |
| Hosting | Vercel (free tier) | Zero-config React deploys |
| Score calc | Supabase Edge Functions (Deno) | Triggered on report insert |

### Database Schema
(See PLAN.md for full SQL — 5 tables: services, cancellation_reports, report_votes, user_subscriptions, service_suggestions)

### API Endpoints
(See PLAN.md for full API design — 8 endpoints: search, service detail, reports, rankings, submit report, vote, user subscriptions, suggest service)

### Score Algorithm (V1 — Simplified)
```
score = 10 - (
  (normalized_steps * 0.35) +
  (normalized_time * 0.35) +
  (method_penalty * 0.30)
)
```
Where:
- `normalized_steps` = min(avg_steps / 10, 1) * 10
- `normalized_time` = min(avg_time / 60, 1) * 10
- `method_penalty`: web=0, chat=3, email=4, phone=7, in_person=9

Score clamped to 1.0–10.0. Minimum 5 reports required to display score.

### Auth Flow
1. **Email/Password**: Supabase handles signup, verification email, login, password reset
2. **Google OAuth**: Supabase handles full OAuth flow; auto-links if email matches existing account
3. **JWT**: Stored in httpOnly cookie, refreshed automatically
4. **Authorization**: Browsing is public; submitting/voting requires auth; 24h account age for reporting

---

## Wireframes

See WIREFRAMES.md for 9 detailed text-based screen layouts:
1. Landing page / homepage
2. Search results
3. Service profile (Guide + Reports + Alternatives tabs)
4. Submit report form
5. Rankings page
6. Auth screens (Sign up + Sign in)
7. Error states (not found, no results)
8. Empty states (new account)
9. Loading states (skeletons)

---

## Implementation Roadmap

### Sprint 1: Foundation (Days 1-3, ~20 hours)
- [ ] Supabase project setup (DB, auth, RLS policies)
- [ ] Database schema creation + seed data (top 50 services with editorial guides)
- [ ] React app scaffolding (Vite + shadcn/ui + Tailwind + routing)
- [ ] Auth implementation (email/password + Google OAuth)
- [ ] Search functionality (pg_trgm fuzzy search)
- **Deliverable:** Can search for services, sign up/in, see seeded data

### Sprint 2: Core Features (Days 4-6, ~24 hours)
- [ ] Service profile page (score card, guide, stats)
- [ ] Report submission form (structured data + comment)
- [ ] Score calculation Edge Function (auto-recalculate on new report)
- [ ] Report display with voting
- [ ] Seed 150 more services with editorial guides (total: 200)
- **Deliverable:** Full loop — search → view → report → see updated score

### Sprint 3: Rankings & Polish (Days 7-8, ~16 hours)
- [ ] Rankings page (worst/best/most reported, with category filter)
- [ ] Homepage with worst/best cards + recent reports feed
- [ ] Mobile responsive optimization
- [ ] Guide feedback ("Was this accurate?")
- [ ] Rate limiting (5 reports/day, 50 votes/day)
- **Deliverable:** Complete MVP with rankings and polished homepage

### Sprint 4: SEO & Launch Prep (Days 9-10, ~12 hours)
- [ ] SEO: Meta tags, OG images, structured data (Schema.org)
- [ ] Sitemap generation for service pages
- [ ] "Suggest a service" form (simple, no approval UI)
- [ ] About page with methodology explanation
- [ ] Performance optimization (lazy loading, image optimization)
- [ ] Final QA pass
- **Deliverable:** Production-ready, SEO-optimized, launchable

### Total Estimated Effort: ~72 hours (9-10 working days for one experienced developer)

---

## Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Cold start (no reports) | HIGH | HIGH | Seed 200 services with editorial guides. First value is the GUIDE, not the score. Scores come later from community. |
| Spam/fake reports | MEDIUM | HIGH | Auth required, 24h account age, rate limits, structured data scoring (hard to fake step count matching others), community flagging |
| Legal threats from companies | LOW | MEDIUM | Reports are factual user experiences (protected speech, CDA 230). No defamation — just structured data. |
| Companies gaming scores | MEDIUM | MEDIUM | Scores based on STRUCTURED data (steps, time, method), not sentiment. Outlier reports get diluted by volume. |
| Stale guides | HIGH | MEDIUM | Show dates on guides. "Was this accurate?" feedback. Recent reports surface changes naturally. |
| Low user engagement | MEDIUM | HIGH | SEO is the primary acquisition channel ("how to cancel X"). This is a utility — people find it via search, not social. |

---

## Success Metrics

### Launch (Month 1)
- 200+ services with editorial guides seeded
- 50+ organic cancellation reports
- 500+ monthly visitors (from SEO)
- First page Google results for "how to cancel [top service]" queries

### Growth (Month 3)
- 500+ services in database
- 1,000+ user-submitted reports
- 5,000+ monthly visitors
- 100+ registered users

### Product-Market Fit (Month 6)
- 2,000+ services
- 10,000+ reports
- 50,000+ monthly visitors
- Reports submitted daily without prompting
- Cited by at least one media outlet

### Revenue Signal (Month 12)
- Alternative service referral revenue > $500/month
- Or: Premium subscription launched with > 100 paying users
- Or: API access interest from > 5 organizations

---

## Monetization Strategy (Post-MVP)

1. **Alternative Referrals** (Primary): When users view a service with a bad score, show easier alternatives. Affiliate links to competitor services generate revenue when users switch. This aligns incentives — we only get paid when we help users find better options.

2. **Premium Features** (V2):
   - Cancellation concierge: We handle the cancellation process for you ($5-10/service)
   - Subscription dashboard: Track all your active subscriptions + total spend
   - Advance alerts: Get notified when a service you use gets flagged

3. **API Access** (V3):
   - Consumer advocacy organizations
   - Journalism/research
   - Comparison shopping sites that want to show cancellation scores

4. **Business Profiles** (V3):
   - Companies can claim their profile
   - Respond to reports
   - Verify FTC compliance
   - "CancelScore Certified" badge for their website (fee)
