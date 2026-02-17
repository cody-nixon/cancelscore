# Research Notes — 2026-02-17

## Sources Scanned
- **Twitter/X**: 7 search queries ("looking for a tool", "someone should build", "why is there no app", "frustrated with", "wish there was", "paying for a solution", "startup idea") + trending topics
- **Reddit**: r/SomebodyMakeThis, r/AppIdeas, r/startups, r/Entrepreneur, r/SaaS
- **Hacker News**: Front page + Ask HN (30+ threads)
- **Web Search**: Broad searches across emerging problems, gaps, and pain points
- **Industry Reports**: Data from Jobbers, Airbridge, FTC, Kaspersky, Optery, Security.org

---

## Candidate Problems (18 collected)

### 1. Bluetooth/Wireless Device Privacy Exposure
- **Source**: HN front page (428 points, 160 comments) — "What your Bluetooth devices reveal"
- **Problem**: Bluetooth devices constantly broadcast data revealing location, habits, device brands. WhisperPair vulnerability (Jan 2026) affects millions of headphones. Cars broadcast via TPMS/WiFi. Malls use BT/WiFi for tracking.
- **Who**: Every consumer with Bluetooth devices (billions)
- **Existing solutions**: Bluehood (CLI tool, not consumer-friendly), ExpressVPN blog guides
- **Why inadequate**: No consumer-facing web tool to audit your own wireless exposure
- **Overlap with past builds**: None

### 2. Modern Web Hostility for Elderly Users
- **Source**: HN Ask (28 points, 9 comments) — "Watching an elderly relative trying to use the modern web"
- **Problem**: Popups, cookie banners, dark patterns, complexity make the web nearly unusable for seniors. A 77-year-old tech veteran says it's getting worse.
- **Who**: Elderly internet users (~70M+ in US alone)
- **Existing solutions**: Browser reader mode, large text settings
- **Why inadequate**: Fragmented, doesn't address full complexity (cookie banners, login flows, etc.)
- **Overlap with past builds**: None

### 3. Creator Deplatforming / Audience Ownership Fear
- **Source**: Reddit r/Entrepreneur (15 upvotes, 13 comments) + Forbes (Jan 2026) + multiple industry reports
- **Problem**: Creators fear algorithm changes and deplatforming. Platforms take 8-30%. "Even Discord is getting corporatized." Need to own audience.
- **Who**: 50M+ content creators globally
- **Existing solutions**: Substack, Circle, Patreon, Beehiiv
- **Why inadequate**: Fragmented — each solves one piece. Migration between platforms is painful.
- **Overlap with past builds**: None

### 4. Dark Pattern Subscription Cancellation Traps
- **Source**: FTC Click-to-Cancel rule (2026), Dark Reading, multiple legal/policy sources
- **Problem**: Subscribing is 1 click; canceling is 5+ clicks, phone calls, chat agents. FTC says 38% of its fines relate to subscription traps. The asymmetry is deliberate and profitable.
- **Who**: Every consumer with subscriptions (200M+ in US)
- **Existing solutions**: No centralized database of cancellation difficulty. FTC complaint system exists but is slow. Some Reddit threads share cancellation tips.
- **Why inadequate**: Information is scattered across forums, no scoring system, no accountability mechanism
- **Overlap with past builds**: None (PolicyPulse tracks ToS changes, not cancellation difficulty)

### 5. Freelancer Late Payment Crisis
- **Source**: Jobbers 2026 report, GetOnTop, AfricanFreelancers
- **Problem**: 65% of freelancers wait 30+ days for payment. 80% say it's significant. 19% have unpaid invoices at any time. Average time-to-pay: 60 days.
- **Who**: 73M+ freelancers in the US
- **Existing solutions**: Moxie, FreshBooks, QuickBooks (invoicing tools)
- **Why inadequate**: They help create invoices but don't solve the COLLECTION problem. No client reputation system.
- **Overlap with past builds**: None

### 6. Subscription Fatigue / Too Many Subscriptions
- **Source**: Airbridge 2026 report, Jalopnik, Subscrybe
- **Problem**: 25% of Americans pay $100+/month in subscriptions. 1/3 of streaming users canceled at least one service. "Tolerance threshold reached."
- **Who**: Every consumer with subscriptions
- **Existing solutions**: Rocket Money (Truebill), Trim, bank subscription tracking
- **Why inadequate**: Market is saturated with subscription trackers. Problem is well-served.
- **Overlap with past builds**: PriceShift is adjacent (tracks pricing)

### 7. Business Operational Busywork
- **Source**: Reddit r/Entrepreneur — "What's the one thing you wish you never had to do again?"
- **Problem**: CRM updates, follow-ups, spreadsheet updates, reports — all eat time. Automation exists but setup is too complex.
- **Who**: Small business owners, solopreneurs
- **Existing solutions**: Zapier, Make, n8n, HubSpot
- **Why inadequate**: Setup complexity. Too many tools to connect.
- **Overlap with past builds**: None

### 8. Personal Product Warranty Tracking
- **Source**: XDA Developers (1 week ago), Reddit r/ProductivityApps
- **Problem**: People lose receipts, forget warranty dates, miss claims worth hundreds of dollars.
- **Who**: Every consumer who buys electronics/appliances
- **Existing solutions**: Warracker (self-hosted), Warranty Vault (Android only)
- **Why inadequate**: No simple consumer web app. Self-hosted is too technical for most people.
- **Overlap with past builds**: None

### 9. Personal Data Broker Exposure
- **Source**: Optery, Security.org, ExpressVPN, PrivacySavvy, CalPrivacy enforcement
- **Problem**: Data brokers publish personal info for anyone to see. 90%+ of adults have info on data broker sites.
- **Who**: Everyone with an online presence
- **Existing solutions**: Optery ($5-25/mo), Incogni ($7/mo), DeleteMe ($10/mo)
- **Why inadequate**: All paid. No free self-serve audit that shows extent of exposure and DIY removal steps.
- **Overlap with past builds**: None

### 10. Consumer Service Outage Awareness
- **Source**: IsDown, Apple iCloud outages (Feb 2026), Microsoft 365 outages
- **Problem**: Consumers waste time troubleshooting when a service is actually down. Status pages are built for businesses, not consumers.
- **Who**: Every internet user
- **Existing solutions**: DownDetector, IsItDown
- **Why inadequate**: DownDetector exists and works reasonably well. Market is served.
- **Overlap with past builds**: None

### 11. SaaS License/Renewal Tracking for SMBs
- **Source**: Spiceworks community, InvGate, Appventory, MS 365 price changes
- **Problem**: Small businesses lose track of renewal dates, pay for unused licenses, get auto-renewed at higher rates.
- **Who**: SMBs (30M+ in US)
- **Existing solutions**: Enterprise SAM tools (InvGate, Zylo), spreadsheets
- **Why inadequate**: Enterprise tools are expensive and complex. Spreadsheets don't scale.
- **Overlap with past builds**: PriceShift tracks prices; this is different (internal license management)

### 12. Meeting Action Item Follow-Through
- **Source**: Web search, ASBN article
- **Problem**: Action items get lost after meetings. AI transcription exists but nobody follows through on the items.
- **Who**: Knowledge workers in meetings (millions)
- **Existing solutions**: Otter.ai, Fireflies, Fellow, Notion
- **Why inadequate**: They capture items but don't create accountability loops. Very crowded market.
- **Overlap with past builds**: None

### 13. Digital Receipt Aggregation
- **Source**: Reddit r/Entrepreneur — "services don't send receipt in email, need to login to download"
- **Problem**: Digital receipts scattered across email, portals, apps. No single view.
- **Who**: Everyone with digital subscriptions
- **Existing solutions**: Email-based receipt scanners, bank statement categorization
- **Why inadequate**: Many services don't email receipts. Need to log into each portal.
- **Overlap with past builds**: None

### 14. Digital Minimalism / Phone Addiction
- **Source**: HN Ask (11 points) — "Want to move to dumb phone"
- **Problem**: People want to reduce phone usage but can't because they depend on apps (maps, banking, 2FA).
- **Who**: Growing digital minimalism movement
- **Existing solutions**: iOS Screen Time, Android Digital Wellbeing, Forest app
- **Why inadequate**: Built-in tools are easily bypassed. No tool helps with the TRANSITION.
- **Overlap with past builds**: None

### 15. YouTube Recommendation Quality
- **Source**: HN Ask (12 points, 4 comments)
- **Problem**: YouTube recommendations are repetitive and low quality.
- **Who**: YouTube users
- **Existing solutions**: Browser extensions to hide recommendations
- **Why inadequate**: Extensions exist. Not a web app problem. Small market.
- **Overlap with past builds**: None

### 16. Startup Idea Validation
- **Source**: Twitter (8 replies on "best way to validate"), IdeaProof.io
- **Problem**: Founders build before validating. Waste months on ideas nobody wants.
- **Who**: Aspiring entrepreneurs (millions)
- **Existing solutions**: IdeaProof, landing page builders, Google Trends
- **Why inadequate**: Existing tools are comprehensive. Not a clear gap.
- **Overlap with past builds**: None

### 17. AI Coding Agent Frustration
- **Source**: Twitter — "Codex is good but I've never been more frustrated with an agent"
- **Problem**: AI coding agents don't communicate well with humans. Need better UX.
- **Who**: Developers using AI tools
- **Existing solutions**: This IS the solution space. Meta-tools are emerging.
- **Why inadequate**: Too niche, developer-only audience
- **Overlap with past builds**: None

### 18. Zero Trust Security Pain for SMBs
- **Source**: Reddit r/startups (6 days ago)
- **Problem**: Small dev shops/IT consultancies struggle to implement Zero Trust for audits/compliance.
- **Who**: Small IT businesses
- **Existing solutions**: Enterprise security platforms
- **Why inadequate**: Too expensive/complex for small shops. But also very niche.
- **Overlap with past builds**: None

---

## Cross-Reference with Past Builds
- PromptShot v2: LLM comparison → No overlap
- GitScout: GitHub issues → No overlap
- NameDrill: Flashcards/learning → No overlap
- FlowPrompt: Teleprompter → No overlap
- QuoteCheck: Quote verification → No overlap
- PriceShift: SaaS price tracking → #6 (subscription fatigue) is adjacent, REJECTED
- PolicyPulse: ToS/privacy monitoring → #4 is different (cancellation difficulty, not policy text changes)

All non-rejected candidates are genuinely different from past builds.
