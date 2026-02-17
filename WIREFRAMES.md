# UI/UX Wireframes — CancelScore

All screens are mobile-first. Desktop expands content areas but maintains the same hierarchy.

---

## Screen 1: Landing Page / Homepage

### Layout
```
┌────────────────────────────────────────────┐
│  🔴 CancelScore          [Sign In] [Sign Up]│
├────────────────────────────────────────────┤
│                                            │
│     Companies make it 5x harder to         │
│     leave than to join.                    │
│                                            │
│     We score them for it.                  │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │ 🔍 Search any service...             │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  Popular: Adobe · NYT · Planet Fitness ·   │
│           Comcast · SiriusXM               │
│                                            │
├────────────────────────────────────────────┤
│                                            │
│  🔴 HARDEST TO LEAVE         [See All →]   │
│                                            │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐   │
│  │ 🔴 2.1   │ │ 🔴 2.3   │ │ 🟠 2.8   │   │
│  │ SiriusXM │ │ Adobe CC │ │ NYT      │   │
│  │ 847 rpts │ │ 723 rpts │ │ 612 rpts │   │
│  │ ~45 min  │ │ ~35 min  │ │ ~25 min  │   │
│  │ Phone req│ │ 8 steps  │ │ 6 steps  │   │
│  └──────────┘ └──────────┘ └──────────┘   │
│                                            │
├────────────────────────────────────────────┤
│                                            │
│  🟢 EASIEST TO LEAVE        [See All →]    │
│                                            │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐   │
│  │ 🟢 9.8   │ │ 🟢 9.5   │ │ 🟢 9.3   │   │
│  │ Spotify  │ │ Netflix  │ │ Notion   │   │
│  │ 1.2K rpts│ │ 2.1K rpts│ │ 456 rpts │   │
│  │ ~2 min   │ │ ~2 min   │ │ ~1 min   │   │
│  │ 2 clicks │ │ 3 clicks │ │ 1 click  │   │
│  └──────────┘ └──────────┘ └──────────┘   │
│                                            │
├────────────────────────────────────────────┤
│                                            │
│  📊 HOW IT WORKS                           │
│                                            │
│  1. Search for any subscription service    │
│  2. See its CancelScore (1-10)             │
│  3. Follow step-by-step cancellation guide │
│  4. Report your experience to help others  │
│                                            │
├────────────────────────────────────────────┤
│                                            │
│  🗣️ RECENT REPORTS                         │
│                                            │
│  ┌─────────────────────────────────────┐   │
│  │ "Adobe took me 35 minutes and       │   │
│  │  4 discount offers before they'd    │   │
│  │  let me cancel." — Alex M. · 2d ago │   │
│  │  🔴 2/10 difficulty · 8 steps       │   │
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │ "Spotify was 2 clicks. Respect."    │   │
│  │  — Jamie K. · 5d ago               │   │
│  │  🟢 9/10 difficulty · 2 steps       │   │
│  └─────────────────────────────────────┘   │
│                                            │
├────────────────────────────────────────────┤
│                                            │
│  📈 THE PROBLEM                            │
│                                            │
│  38% of FTC fines relate to subscription   │
│  traps. The FTC's Click-to-Cancel rule     │
│  now requires canceling to be as easy as   │
│  signing up. But enforcement is slow.      │
│                                            │
│  CancelScore gives you the data to make    │
│  informed choices — and hold companies     │
│  accountable.                              │
│                                            │
│  [Browse All Rankings →]                   │
│                                            │
├────────────────────────────────────────────┤
│  CancelScore · About · Privacy · Contact   │
│  Data from 50,000+ real cancellation       │
│  reports by real users.                    │
└────────────────────────────────────────────┘
```

### UI Elements
- **Header**: Logo (red circle + "CancelScore"), Sign In / Sign Up buttons (right)
- **Hero**: Large text headline, subline, full-width search bar with autocomplete
- **Popular tags**: Clickable service name chips below search bar
- **Worst/Best cards**: Score badge (colored), service name, report count, avg time, primary method
- **How it works**: 4-step numbered list with icons
- **Recent reports**: Card with quote, author, score badge, step count
- **Problem section**: Stats + CTA to rankings
- **Footer**: Links, aggregate stat

### Interactions
- Type in search bar → instant dropdown with matching services (debounced, 300ms)
- Click service card → navigate to service profile
- Click "See All" → navigate to rankings page
- Click popular tag → pre-fill search and show results

---

## Screen 2: Search Results

### Layout
```
┌────────────────────────────────────────────┐
│  🔴 CancelScore          [Sign In] [Sign Up]│
├────────────────────────────────────────────┤
│  ┌──────────────────────────────────────┐  │
│  │ 🔍 adobe                        [×]  │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  3 results for "adobe"                     │
│                                            │
│  ┌─────────────────────────────────────┐   │
│  │ [logo] Adobe Creative Cloud         │   │
│  │        Software · Design Tools      │   │
│  │        🔴 2.3 · 847 reports         │   │
│  │        Avg: 8 steps · 35 min        │   │
│  │        ⚠️ Phone call may be required │   │
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │ [logo] Adobe Acrobat Pro            │   │
│  │        Software · PDF Tools         │   │
│  │        🟠 3.5 · 312 reports         │   │
│  │        Avg: 6 steps · 20 min        │   │
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │ [logo] Adobe Stock                  │   │
│  │        Media · Stock Photos         │   │
│  │        🟠 3.1 · 189 reports         │   │
│  │        Avg: 7 steps · 25 min        │   │
│  └─────────────────────────────────────┘   │
│                                            │
│  Don't see a service?                      │
│  [Suggest a service →]                     │
│                                            │
└────────────────────────────────────────────┘
```

### UI Elements
- **Search bar**: Persistent at top, shows current query with clear (×) button
- **Result count**: "3 results for 'adobe'"
- **Result cards**: Logo (from Clearbit), service name, category, score badge, report count, avg metrics, warnings
- **Suggest CTA**: Link at bottom for missing services

### Interactions
- Click result card → navigate to service profile
- Clear search → return to homepage
- Type new query → results update live

---

## Screen 3: Service Profile

### Layout
```
┌────────────────────────────────────────────┐
│  🔴 CancelScore          [Sign In] [Sign Up]│
├────────────────────────────────────────────┤
│                                            │
│  [← Back to results]                       │
│                                            │
│  [Adobe logo]                              │
│  Adobe Creative Cloud                      │
│  Software · Design Tools · adobe.com       │
│                                            │
│  ┌─────────────────────────────────────┐   │
│  │         🔴 CancelScore: 2.3         │   │
│  │         ══════════▓░░░░░░░░░░░      │   │
│  │         EXIT TRAP                    │   │
│  │                                     │   │
│  │  Steps: 2.0  Time: 1.5  Method: 3.0│   │
│  │  Transparency: 2.5  Retention: 2.0  │   │
│  │                                     │   │
│  │  Based on 847 reports               │   │
│  └─────────────────────────────────────┘   │
│                                            │
│  ⚠️ FTC COMPLIANCE: NON-COMPLIANT          │
│  Issues: More steps to cancel than subscribe│
│  Phone retention tactics reported           │
│                                            │
├─────────┬──────────┬───────────────────────┤
│ Guide   │ Reports  │ Alternatives           │
├─────────┴──────────┴───────────────────────┤
│                                            │
│  📋 HOW TO CANCEL (Step-by-step)           │
│                                            │
│  ⚠️ Before you cancel:                     │
│  □ Export your files from Creative Cloud    │
│  □ Download fonts you've used in projects  │
│  □ Check annual vs monthly (fee differs)   │
│                                            │
│  1. Go to account.adobe.com and sign in    │
│  2. Click "Plans" in the sidebar           │
│  3. Click "Cancel plan" next to your sub   │
│  4. Offered a discount — click "No thanks" │
│  5. Select a reason and click Continue     │
│  6. Early termination fee warning shown    │
│     → Click "Confirm"                      │
│  7. Click "Confirm cancellation"           │
│  8. Check email for confirmation           │
│                                            │
│  ⚠️ Warning: Annual plan has early          │
│  termination fee if canceled mid-cycle.    │
│                                            │
│  Was this guide accurate? [Yes] [No]       │
│  Last updated: Feb 15, 2026                │
│                                            │
│  ──────────────────────────────────────    │
│                                            │
│  Going to cancel? [Report your experience] │
│                                            │
├────────────────────────────────────────────┤
│                                            │
│  📊 QUICK STATS                            │
│                                            │
│  Average steps:     8                      │
│  Average time:      35 min                 │
│  Phone required:    42% of reports         │
│  Retention offers:  78% got one            │
│  Termination fee:   61% were charged       │
│  Avg fee:           $59.99                 │
│  Success rate:      94% eventually canceled│
│                                            │
├────────────────────────────────────────────┤
│                                            │
│  🗣️ USER REPORTS (847)    Sort: [Newest ▾] │
│                                            │
│  ┌─────────────────────────────────────┐   │
│  │ Alex M. · Feb 10, 2026 · via Web   │   │
│  │ 🔴 2/10 · 7 steps · 25 min         │   │
│  │                                     │   │
│  │ "Had to click through 4 discount    │   │
│  │  offers before they'd let me        │   │
│  │  cancel. The termination fee was    │   │
│  │  $59.99 which they don't mention    │   │
│  │  until the very last step."         │   │
│  │                                     │   │
│  │  ☐ Retention offer  ☐ Term fee      │   │
│  │  👍 42 helpful · 👎 3               │   │
│  └─────────────────────────────────────┘   │
│                                            │
│  ┌─────────────────────────────────────┐   │
│  │ Sam P. · Feb 8, 2026 · via Phone   │   │
│  │ 🔴 1/10 · 12 steps · 45 min        │   │
│  │                                     │   │
│  │ "Web cancellation didn't work (kept │   │
│  │  erroring). Had to call. Was on     │   │
│  │  hold for 20 min. Agent tried to    │   │
│  │  offer 3 different deals. Finally   │   │
│  │  canceled after 45 min total."      │   │
│  │                                     │   │
│  │  ☐ Retention offer  ☐ Phone req     │   │
│  │  👍 67 helpful · 👎 1               │   │
│  └─────────────────────────────────────┘   │
│                                            │
│  [Load more reports]                       │
│                                            │
└────────────────────────────────────────────┘
```

### Tab: Alternatives
```
│  🔄 EASIER ALTERNATIVES                    │
│                                            │
│  ┌─────────────────────────────────────┐   │
│  │ [logo] Canva Pro    🟢 8.5 · Easy   │   │
│  │        Design tool · $12.99/mo      │   │
│  │        2 clicks to cancel           │   │
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │ [logo] Figma Pro    🟢 9.1 · Easy   │   │
│  │        Design tool · $15/mo         │   │
│  │        1 click to cancel            │   │
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │ [logo] Affinity     🟢 9.8 · Easy   │   │
│  │        Design tool · One-time $69   │   │
│  │        No subscription to cancel!   │   │
│  └─────────────────────────────────────┘   │
```

### UI Elements
- **Back link**: Returns to search results
- **Service header**: Logo, name, category, website link
- **Score card**: Large score number, color bar, label, dimension breakdown, report count
- **FTC compliance badge**: Green/red with specific issues
- **Tab navigation**: Guide | Reports | Alternatives
- **Cancellation guide**: Pre-cancel checklist, numbered steps, warnings
- **Guide feedback**: "Was this accurate?" buttons
- **Report CTA**: Prominent button to submit report
- **Stats grid**: Key metrics at a glance
- **Report cards**: Author, date, method, score, step count, time, comment, vote buttons
- **Alternative cards**: Logo, name, score, price, cancel ease summary

### Interactions
- Click tabs → switch between Guide / Reports / Alternatives
- Click "Yes/No" on guide accuracy → submits feedback
- Click "Report your experience" → opens report form (or auth prompt)
- Click 👍/👎 on report → vote (requires auth)
- Click "Load more" → paginate reports
- Click alternative → navigate to that service's profile

---

## Screen 4: Submit Report Form (Auth Required)

### Layout
```
┌────────────────────────────────────────────┐
│  🔴 CancelScore                  [Profile] │
├────────────────────────────────────────────┤
│                                            │
│  📝 Report Your Cancellation Experience    │
│  for Adobe Creative Cloud                  │
│                                            │
│  Your report helps others know what to     │
│  expect. Only factual data — no personal   │
│  info needed.                              │
│                                            │
│  ──────────────────────────────────────    │
│                                            │
│  When did you cancel? *                    │
│  ┌──────────────────────┐                  │
│  │ Feb 17, 2026    [📅] │                  │
│  └──────────────────────┘                  │
│                                            │
│  How did you cancel? *                     │
│  ○ Website (self-serve)                    │
│  ○ Phone call                              │
│  ○ Live chat                               │
│  ○ Email                                   │
│  ○ In person                               │
│                                            │
│  How many steps/clicks did it take? *      │
│  ┌──────┐                                  │
│  │  7   │  (including login)               │
│  └──────┘                                  │
│                                            │
│  How long did it take? (minutes) *         │
│  ┌──────┐                                  │
│  │  25  │                                  │
│  └──────┘                                  │
│                                            │
│  Were you offered a discount to stay? *    │
│  ○ Yes  ○ No                               │
│                                            │
│  Did you accept the discount?              │
│  ○ Yes  ○ No                               │
│                                            │
│  Was there an early termination fee? *     │
│  ○ Yes  ○ No                               │
│                                            │
│  Fee amount ($)                            │
│  ┌──────┐                                  │
│  │ 59.99│                                  │
│  └──────┘                                  │
│                                            │
│  Did you successfully cancel? *            │
│  ○ Yes  ○ No (gave up)  ○ Not sure yet    │
│                                            │
│  Overall difficulty (1 = impossible,       │
│                       10 = effortless) *   │
│  [1] [2] [3] [4] [5] [6] [7] [8] [9] [10]│
│                                            │
│  Tell your story (optional)                │
│  ┌──────────────────────────────────────┐  │
│  │ Describe what happened...            │  │
│  │                                      │  │
│  │                                      │  │
│  └──────────────────────────────────────┘  │
│  500 characters max                        │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │        [Submit Report]               │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  Your report will be public. Your name     │
│  shows as first name + last initial.       │
│                                            │
└────────────────────────────────────────────┘
```

### UI Elements
- **Header**: Service name being reported on
- **Date picker**: Default to today
- **Radio groups**: Method, retention, fee, success
- **Number inputs**: Steps, time, fee amount
- **Difficulty scale**: 1-10 clickable buttons, color-coded (red→green)
- **Text area**: Optional comment, 500 char limit with counter
- **Submit button**: Primary CTA
- **Privacy note**: Explains display name format

### Interactions
- Conditional fields appear based on answers (fee amount appears only if "Yes" to termination fee)
- Difficulty buttons highlight on selection with color gradient
- Form validation on submit — required fields marked with *
- On success → redirect to service profile with "Report submitted!" toast

---

## Screen 5: Rankings Page

### Layout
```
┌────────────────────────────────────────────┐
│  🔴 CancelScore          [Sign In] [Sign Up]│
├────────────────────────────────────────────┤
│                                            │
│  📊 Subscription Rankings                  │
│                                            │
│  [🔴 Hardest] [🟢 Easiest] [📈 Most Reported]│
│                                            │
│  Category: [All ▾]                         │
│                                            │
│  ──────────────────────────────────────    │
│                                            │
│  🔴 HARDEST TO CANCEL                      │
│                                            │
│  #1  SiriusXM           🔴 2.1  847 rpts   │
│      Phone required · ~45 min avg          │
│                                            │
│  #2  Adobe Creative Cloud 🔴 2.3  723 rpts  │
│      8 steps · ~35 min avg                 │
│                                            │
│  #3  New York Times      🔴 2.8  612 rpts   │
│      Phone preferred · ~25 min avg         │
│                                            │
│  #4  Planet Fitness      🟠 3.0  891 rpts   │
│      In-person or certified mail only      │
│                                            │
│  #5  Comcast/Xfinity     🟠 3.2  1.4K rpts  │
│      Phone required · retention team       │
│                                            │
│  #6  LA Fitness          🟠 3.3  456 rpts   │
│      In-person only with 30 days notice    │
│                                            │
│  #7  Amazon Prime        🟠 3.5  2.3K rpts  │
│      6 steps · multiple "are you sure?"    │
│                                            │
│  #8  HelloFresh          🟠 3.7  567 rpts   │
│      Chat required · retention offers      │
│                                            │
│  ...                                       │
│                                            │
│  [Load more]                               │
│                                            │
└────────────────────────────────────────────┘
```

### UI Elements
- **Tab selector**: Hardest / Easiest / Most Reported
- **Category filter**: Dropdown (All, Software, Streaming, Fitness, News, Telecom, Food Delivery, etc.)
- **Ranked list**: Number, service name, score badge, report count, key detail

### Interactions
- Click tab → re-sort rankings
- Change category → filter results
- Click service name → navigate to service profile
- Infinite scroll or "Load more" pagination

---

## Screen 6: Auth Screens

### Sign Up
```
┌────────────────────────────────────────────┐
│  🔴 CancelScore                            │
├────────────────────────────────────────────┤
│                                            │
│  Create your account                       │
│  Help others cancel and track your own     │
│  subscriptions.                            │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │  [G] Continue with Google            │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  ─────────── or ───────────                │
│                                            │
│  Email *                                   │
│  ┌──────────────────────────────────────┐  │
│  │ you@example.com                      │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  Password *                                │
│  ┌──────────────────────────────────────┐  │
│  │ ••••••••                       [👁️]  │  │
│  └──────────────────────────────────────┘  │
│  Min 8 characters                          │
│                                            │
│  Display name *                            │
│  ┌──────────────────────────────────────┐  │
│  │ Alex M.                              │  │
│  └──────────────────────────────────────┘  │
│  Shown on your reports (first name +       │
│  last initial recommended)                 │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │        [Create Account]              │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  Already have an account? [Sign in]        │
│                                            │
└────────────────────────────────────────────┘
```

### Sign In
```
┌────────────────────────────────────────────┐
│  🔴 CancelScore                            │
├────────────────────────────────────────────┤
│                                            │
│  Welcome back                              │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │  [G] Continue with Google            │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  ─────────── or ───────────                │
│                                            │
│  Email *                                   │
│  ┌──────────────────────────────────────┐  │
│  │ you@example.com                      │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  Password *                                │
│  ┌──────────────────────────────────────┐  │
│  │ ••••••••                       [👁️]  │  │
│  └──────────────────────────────────────┘  │
│  [Forgot password?]                        │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │        [Sign In]                     │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  Don't have an account? [Sign up]          │
│                                            │
└────────────────────────────────────────────┘
```

---

## Screen 7: Error States

### Service Not Found
```
│  😔 Service not found                      │
│                                            │
│  We don't have data for "xyzservice" yet.  │
│                                            │
│  [Suggest this service]  [Search again]    │
```

### No Reports Yet
```
│  📊 CancelScore: —                         │
│  ┌─────────────────────────────────────┐   │
│  │  Limited data — fewer than 5 reports│   │
│  │  Be the first to share your         │   │
│  │  cancellation experience!           │   │
│  │                                     │   │
│  │  [Submit a Report]                  │   │
│  └─────────────────────────────────────┘   │
```

### Search No Results
```
│  No results for "qwerty123"                │
│                                            │
│  Try a different search, or suggest a      │
│  service we should add.                    │
│                                            │
│  [Suggest a service →]                     │
```

---

## Screen 8: Empty States

### Fresh Account — No Activity
```
│  👋 Welcome to CancelScore!                │
│                                            │
│  Start by searching for a service you're   │
│  thinking of canceling — or browse the     │
│  rankings to see who makes it hardest.     │
│                                            │
│  [🔍 Search services]  [📊 View rankings] │
```

---

## Screen 9: Loading States

### Search Loading
```
│  ┌──────────────────────────────────────┐  │
│  │ 🔍 adobe                     [⏳]    │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  ┌─────────────────────────────────────┐   │
│  │ ▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░│   │
│  │ ▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░░░│   │
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░│   │
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │ ▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░│   │
│  │ ▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░░░│   │
│  └─────────────────────────────────────┘   │
```
Skeleton loading cards matching result card shape.

### Service Profile Loading
Score card shows animated pulse. Tabs are visible but content shows skeleton.

---

## Mobile Layout Considerations

- **Search bar**: Full width, always accessible at top
- **Score card**: Stacks vertically on mobile (score number on top, breakdown below)
- **Tabs**: Full-width tab bar, horizontally scrollable if needed
- **Report cards**: Single column, full width
- **Rankings**: Single column list, no horizontal scroll
- **Report form**: All fields stack vertically, radio buttons become large tap targets
- **Bottom nav** (optional V2): Home | Search | Rankings | Profile

### Touch Targets
- All buttons minimum 44px height (Apple HIG)
- Radio buttons have large hit area (full row is clickable)
- Difficulty rating buttons: 40px × 40px minimum
- Swipeable between tabs on service profile

### Typography
- Headlines: 24px mobile, 32px desktop
- Body: 16px (never smaller on mobile)
- Score number: 48px bold
- Microcopy: 14px, muted color
