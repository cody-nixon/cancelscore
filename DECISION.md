# Decision — Choosing the Best Problem

## Scoring Matrix (1-10 per criterion)

| # | Problem | Urgency | Impact | Novelty | Feasibility | Monetization | Bookmark Test | Uniqueness | TOTAL |
|---|---------|---------|--------|---------|-------------|--------------|---------------|------------|-------|
| 1 | BT Privacy Exposure | 8 | 7 | 7 | 5* | 5 | 5 | 10 | 47 |
| 2 | Elderly Web Accessibility | 7 | 6 | 6 | 4* | 4 | 5 | 10 | 42 |
| 3 | Creator Audience Ownership | 8 | 7 | 4 | 6 | 7 | 6 | 10 | 48 |
| **4** | **Dark Pattern Cancellation** | **10** | **10** | **9** | **9** | **8** | **10** | **10** | **66** |
| 5 | Freelancer Late Payments | 8 | 7 | 6 | 7 | 7 | 6 | 10 | 51 |
| 6 | Subscription Fatigue | 7 | 8 | 2 | 8 | 7 | 5 | 3 | 40 |
| 7 | Business Busywork | 6 | 7 | 3 | 6 | 6 | 5 | 10 | 43 |
| 8 | Warranty Tracking | 6 | 6 | 6 | 8 | 5 | 7 | 10 | 48 |
| 9 | Data Broker Exposure | 8 | 8 | 6 | 4* | 7 | 5 | 10 | 48 |
| 10 | Consumer Outage Awareness | 5 | 7 | 2 | 8 | 4 | 7 | 10 | 43 |
| 11 | SaaS License Tracking | 7 | 6 | 5 | 7 | 7 | 6 | 8 | 46 |
| 12 | Meeting Action Items | 6 | 7 | 3 | 7 | 6 | 5 | 10 | 44 |
| 13 | Digital Receipt Aggregation | 6 | 6 | 5 | 5 | 5 | 5 | 10 | 42 |
| 14 | Digital Minimalism | 6 | 5 | 5 | 6 | 4 | 4 | 10 | 40 |

*Low feasibility: BT scanning can't happen in a browser; data broker scraping has legal/technical barriers; elderly accessibility needs browser extension, not web app.

## Why #4: Dark Pattern Subscription Cancellation Traps

### The Problem in One Sentence
Companies deliberately make it 5-10x harder to cancel a subscription than to start one, and there's no centralized resource that scores this behavior, provides step-by-step escape instructions, or holds them accountable.

### Why This Scores Highest

**Urgency (10/10)**: The FTC's Click-to-Cancel rule just took effect in 2026. This is THE consumer protection topic of the moment. FTC says 38% of its fines relate to subscription traps. Dark pattern articles are being published weekly by major outlets (Dark Reading, Kaspersky, law firms). The regulatory and media spotlight is white-hot.

**Impact (10/10)**: There are 200M+ Americans with active subscriptions. Nearly everyone has struggled with cancellation at some point. This isn't niche — it's universal frustration. The r/Entrepreneur thread about "things you wish you never had to do" highlighted how even business owners hate managing subscription cancellations. One click to subscribe, five to cancel is the universal complaint.

**Novelty (9/10)**: There is literally no centralized database that:
- Scores companies on cancellation difficulty
- Provides step-by-step cancellation walkthroughs
- Crowdsources real cancellation experiences
- Tracks FTC Click-to-Cancel compliance
- Holds companies publicly accountable

Some scattered Reddit threads share cancellation tips. Some blog posts cover individual services. But nobody has created the **definitive, searchable, scored database** of cancellation experiences. This is a massive gap.

**Feasibility (9/10)**: This is a content + community web app. No exotic APIs needed. Core features:
1. Searchable database of services with cancellation scores
2. Step-by-step cancellation guides (crowdsourced + editorial)
3. User-submitted cancellation reports (took X clicks, Y minutes, had to call, etc.)
4. Score algorithm based on aggregated reports
5. FTC compliance tracker

All of this is standard web app territory: React frontend, API backend, PostgreSQL database. No hardware access needed, no scraping, no legal gray areas.

**Monetization (8/10)**:
- Free core product (search + read guides)
- **Affiliate revenue**: When someone wants to cancel Service X, suggest alternatives (competitors pay for referrals)
- **Premium features**: Cancellation concierge (we cancel for you), alerts when your subscriptions get flagged
- **API access**: Allow consumer advocacy groups, journalists to query the database
- **B2B angle**: Companies can claim/verify their profile to show good faith

**Bookmark Test (10/10)**: "How do I cancel [service name]?" is a query people make regularly. This app would become the go-to answer. You'd bookmark it and return every time you need to cancel something. Unlike a one-time tool, subscription cancellation is a recurring need throughout your life.

**Uniqueness from Past Builds (10/10)**: 
- PriceShift monitors pricing changes (what does it COST). CancelScore monitors cancellation difficulty (how hard is it to LEAVE).
- PolicyPulse tracks ToS/privacy policy text changes. CancelScore tracks actual user EXPERIENCE of the cancellation process.
- These are genuinely different products solving different problems.

### The Core Insight
**Cancellation difficulty is a feature, not a bug.** Companies invest millions in UX to make signup frictionless because every removed friction point increases conversion. They apply the OPPOSITE logic to cancellation: every added friction point reduces churn. This creates an information asymmetry — companies know exactly how hard they've made it, but consumers don't find out until they try to leave. CancelScore eliminates this asymmetry by making cancellation difficulty transparent, searchable, and scored before you even sign up.

### Who Is the User?
1. **The Active Canceler**: "I want to cancel Adobe right now. How do I actually do it?" — Needs step-by-step instructions.
2. **The Comparison Shopper**: "I'm choosing between two CRM tools. Which one won't trap me?" — Needs to compare cancellation scores before committing.
3. **The Journalist/Advocate**: "Which companies are the worst offenders?" — Needs data for articles and reports.
4. **The Business Owner**: "How does our cancellation process compare?" — Needs to benchmark against competitors.

### Selected: **CancelScore** — "Know before you subscribe. Leave when you want."

Product name: **CancelScore**
Tagline: "The exit difficulty score for every subscription."
