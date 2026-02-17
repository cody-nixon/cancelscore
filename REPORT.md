# Report — CancelScore

## The Problem

**Companies deliberately make it 5-10x harder to cancel a subscription than to start one.**

Subscribing to a service is typically 1-2 clicks. Canceling? That's 5-12 steps, retention teams, discount offers, phone calls, in-person visits, and early termination fees. This asymmetry is intentional — every friction point in cancellation reduces churn. The FTC calls these "subscription traps" and says 38% of its enforcement fines relate to them. The Click-to-Cancel rule (effective 2026) requires parity between signup and cancellation ease, but enforcement is slow.

**Sources:**
- FTC Click-to-Cancel Rule (2026) — https://cookie-script.com/privacy-laws/dark-patterns-2026-the-ftc-new-click-to-cancel-rule
- Dark Reading: "Dark Patterns Undermine Security" (Feb 2026) — https://www.darkreading.com/cyber-risk/dark-patterns-undermine-security-one-click-at-a-time
- Substack (Nita Farahany, Duke law professor): "Resubscribing was one click, canceling was five" — https://nitafarahany.substack.com/p/why-dark-patterns-work-inside-my
- ZwillGen law firm: FTC rising risk of dark patterns in subscription design — https://www.zwillgen.com/publication/ftc-enforcement-rising-risk-dark-patterns-subscription-design/
- Airbridge: "1/3 of streaming users canceled citing 'too many subscriptions'" — https://www.airbridge.io/blog/why-subscription-churn-happens-top-5-cancellation-reasons-forecast-for-mobile-apps-in-2026
- Jalopnik: "25% of Americans pay $100+/month in subscriptions" (Feb 15, 2026)

## Why I Chose It

Out of 18 candidate problems researched across Twitter/X, Reddit, Hacker News, Google News, and industry reports, this scored highest on every dimension:

1. **Urgency (10/10):** FTC enforcement is active right now. Articles about subscription dark patterns are published weekly.
2. **Impact (10/10):** 200M+ Americans have active subscriptions. Everyone has struggled with cancellation.
3. **Novelty (9/10):** Zero centralized databases score cancellation difficulty or provide crowdsourced guides.
4. **Feasibility (9/10):** Standard web app — React + Supabase. No exotic APIs or hardware access needed.
5. **Bookmark test (10/10):** "How do I cancel [service]?" is a query people make repeatedly throughout their lives.

The runner-up was Bluetooth privacy exposure (inspired by a 428-point HN thread), but browser limitations prevent a web app from scanning BT devices directly, making it educational rather than functional.

## Design Summary

**CancelScore** is a crowdsourced database that scores every subscription on cancellation difficulty (1-10), provides step-by-step escape guides, and publishes worst/best offender rankings.

**Architecture:** React (Vercel) → Supabase (PostgreSQL + Auth + PostgREST + Edge Functions). No separate API server.

**Key features:**
- Search any service → see CancelScore (1-10, color-coded)
- Step-by-step cancellation guides with pre-cancel checklists
- Submit structured cancellation reports (steps, time, method, fees)
- Rankings: worst/best offenders by category
- FTC Click-to-Cancel compliance tracking
- Easier alternatives shown on bad-score services

**Score algorithm:** Weighted formula based on structured data — average steps (35%), average time (35%), cancellation method (30%). Scored from real user reports, minimum 5 reports to display.

**Cold start solution:** Seed 200 services with editorial cancellation guides. Guides provide immediate value; scores build over time from community reports.

**Estimated build effort:** ~72 hours across 4 sprints.

## GitHub Repo

https://github.com/cody-nixon/cancelscore

Contains: RESEARCH.md, DECISION.md, PLAN.md, REVIEW.md, WIREFRAMES.md, DESIGN.md, REPORT.md

## Key Insight

**Cancellation difficulty is a feature, not a bug.** Companies optimize signup UX to minimize friction (every removed step increases conversion). They apply the *inverse* logic to cancellation: every *added* step reduces churn. This creates an information asymmetry — companies know exactly how hard they've made it, but consumers don't find out until they try to leave. CancelScore eliminates this asymmetry by making exit difficulty transparent, searchable, and scored before you even sign up. It's the missing consumer tool for the subscription economy.
