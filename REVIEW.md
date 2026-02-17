# Multi-Role Review — CancelScore

## 🎯 Product Review

**Core value prop in one sentence:** "Search any subscription, see how hard it is to cancel, and get step-by-step escape instructions — all from real user reports."

**Is this actually solving the problem?** Yes. The core pain is: "I want to cancel X but I don't know what I'm about to go through." CancelScore answers that question before you start.

**Concerns:**
1. **Cold start problem.** The product is only useful if there are reports and guides. Day 1 has zero. Solution: Seed the database with editorial cancellation guides for the top 100-200 most-subscribed services (Netflix, Spotify, Adobe, NYT, gym chains, etc.). This is manual work but creates immediate value.
2. **User motivation to contribute.** Why would someone submit a report after canceling? They're relieved it's over. Solution: Frame it as "help others avoid what you just went through" + gamification (contributor badges, report count). Also, prompt them BEFORE they cancel — "Before you go through cancellation, would you like to document it for others?"
3. **The Yelp problem.** Angry people are more motivated to review than happy ones. Scores will skew negative. Solution: The score algorithm uses structured data (steps, time, method) not just sentiment. A report that says "it was easy" but took 8 steps and 30 minutes will still get a low score based on the facts.

**Recommendation:** Add a "Before you cancel" CTA on service profiles — "Going to cancel? Report your experience as you go through it." This captures reports in real-time while the experience is fresh.

---

## 🐛 QA Review

**What's most likely to break?**

1. **Spam/fake reports.** Bad actors (or companies defending themselves) could flood a service with fake positive/negative reports.
   - Mitigation: Auth required. Rate limit (5 reports/day). Report flagging system. Score algorithm weights recent reports but doesn't let a single burst dominate. Minimum account age before reporting (24h).
   
2. **Stale cancellation guides.** Companies change their cancellation flows. Guide from 6 months ago might be wrong.
   - Mitigation: Show date on each guide. Allow users to flag guides as outdated. Prioritize recent reports in the display. Add "Was this guide accurate?" feedback button.

3. **Score manipulation.** Companies could create accounts to upvote favorable reports.
   - Mitigation: Score is based on structured data (steps, time), not just user rating. Upvotes affect report visibility, not the actual score.

4. **Legal/DMCA takedowns.** Companies might claim their cancellation process is proprietary.
   - Mitigation: Reports are factual user experiences (protected speech). We're not reverse-engineering software. Legal precedent: review sites are protected under CDA Section 230.

**Key edge cases:**
- Service doesn't exist in database → show "Suggest this service" form
- Service has <5 reports → show "Limited data" disclaimer, don't display score
- User tries to submit duplicate report for same service → block within 30-day window
- Report with wildly different data from others → flag for review but still include

---

## 🔧 Engineering Review

**Is this the simplest way to build it?**

Mostly yes. A few simplification opportunities:

1. **Skip the custom API server for MVP.** Use Supabase directly from the frontend with Row Level Security (RLS). Supabase's PostgREST gives you a full API automatically. This eliminates the Express server entirely for MVP.
   - Counter-argument: Score calculation needs server-side logic. Solution: Use Supabase Edge Functions for score recalculation (triggered on new report insert).

2. **Simplify score algorithm for V1.** The 5-dimension weighted score is correct long-term, but for MVP, use a simpler formula: `score = 10 - (steps_normalized * 0.4 + time_normalized * 0.4 + method_penalty * 0.2)`. Refine when you have data.

3. **Use Supabase's built-in full-text search** instead of custom tsvector management. It handles this natively.

4. **Image hosting for logos.** Don't build a logo upload system. Use Clearbit Logo API (`logo.clearbit.com/company.com`) to dynamically fetch company logos from their domain.

**Unnecessary complexity identified:**
- The `service_suggestions` table could be a simple Google Form for MVP. Don't build a review/approval system yet.
- The `user_subscriptions` watchlist is nice but not MVP-critical. Cut it from Sprint 1.

**Revised architecture for MVP:**
```
React (Vercel) → Supabase (DB + Auth + API via PostgREST + Edge Functions)
```
One deployment. No separate backend server. Dramatically simpler.

---

## 🎨 Design Review

**First-time user lands on this — do they understand what to do in 5 seconds?**

With current design: **Almost.** The search bar is clear. But the value prop needs to be instantly obvious.

**Recommendations:**

1. **Hero section must answer three questions immediately:**
   - What is this? → "Real cancellation data for every subscription"
   - Why should I care? → "Companies make it 5x harder to leave than to join"
   - What do I do? → Giant search bar: "Search any service..."

2. **Show don't tell.** Below the search bar, show 3-4 "worst offenders" cards with their scores. This instantly communicates what the app does and creates emotional engagement ("Oh yeah, I HATE canceling that one!").

3. **Score visualization.** The 1-10 score needs to be visceral, not just a number:
   - 9-10: Green 🟢 "Easy exit"
   - 7-8: Light green 🟢 "Mostly painless"
   - 5-6: Yellow 🟡 "Moderate friction"
   - 3-4: Orange 🟠 "Significant obstacles"
   - 1-2: Red 🔴 "Exit trap"
   
4. **Service profile page.** Lead with the score (big, colorful). Then the step-by-step guide. Then user reports. Don't bury the guide under reports.

5. **Mobile-first.** People Google "how to cancel X" on their phones. The service profile and guide must be perfectly readable on mobile.

---

## 🔒 Security Review

**Attack surface:**

1. **Authentication:** Supabase Auth handles this well. JWT tokens, bcrypt passwords, OAuth flows. ✅
   
2. **Report injection:** User-submitted text (comments) could contain XSS.
   - Mitigation: Sanitize all user input server-side. Use React's built-in XSS protection (JSX escapes by default). Store raw text, sanitize on render.

3. **Rate limiting:** Without it, someone could submit thousands of reports.
   - Mitigation: Supabase RLS + application-level rate limits. 5 reports/day, 50 votes/day. IP-based rate limiting on search (100 req/min).

4. **Data handling:** We store email addresses (via auth) and cancellation reports. No financial data, no passwords (Supabase handles), no PII beyond email.
   - Privacy policy: Be transparent about what we store. GDPR: Allow account + data deletion.

5. **API keys:** Supabase anon key is public (by design, protected by RLS). Service role key is server-side only (Edge Functions). No other API keys needed for MVP.
   - All keys in environment variables. ✅

6. **Abuse vectors:**
   - Companies creating fake positive reports → Structured data makes this hard (can't fake "1 step, 1 minute" when everyone else says "8 steps, 30 minutes")
   - Competitor smear campaigns → Reports are public + votable, community can flag
   - Scraping our database → Rate limit API, consider requiring auth for bulk access

**Verdict:** Low attack surface. Standard web app security practices apply. No financial data, no sensitive PII. Main risk is content abuse (spam/fake reports), mitigated by auth + rate limits + structured data scoring.

---

## Plan Revisions Based on Review

### Changes Made:

1. **Architecture simplified:** Drop custom Express server. Use Supabase (DB + Auth + PostgREST + Edge Functions) directly. One deployment to Vercel.

2. **Score algorithm simplified for V1:** Single formula based on steps + time + method. Refine to 5-dimension model after collecting real data.

3. **Cold start solution added:** Seed database with editorial guides for top 200 services before launch. Use Clearbit for logos.

4. **Cut from MVP:**
   - User subscription watchlist (V2)
   - Service suggestions approval system (use Google Form)
   - Email notifications (V2)
   - Compare feature (V2)

5. **Added to MVP:**
   - "Was this guide accurate?" feedback button
   - "Report as you cancel" prompt
   - Minimum 24h account age before reporting
   - Score requires ≥5 reports to display (show "Limited data" otherwise)

6. **Design updates:**
   - Hero section with search + worst offenders
   - Score color coding (green → red scale)
   - Mobile-first responsive design
   - Service profile leads with score + guide, then reports
