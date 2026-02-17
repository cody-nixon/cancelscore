# Technical Plan — CancelScore

## Product Name
**CancelScore**

## Tagline
"The exit difficulty score for every subscription."

## Alternative Taglines
- "Know before you subscribe. Leave when you want."
- "Rate how hard it is to cancel anything."
- "Subscription transparency, powered by real users."

---

## User Stories

### Must Have (MVP)
1. **As a consumer**, I want to search for a service by name and see how hard it is to cancel, so I can know what I'm getting into before signing up.
2. **As a consumer**, I want to read step-by-step cancellation instructions for a specific service, so I can cancel efficiently without wasting time.
3. **As a user who just canceled something**, I want to submit my cancellation experience (how many steps, how long, obstacles), so others can benefit from my experience.
4. **As a comparison shopper**, I want to compare cancellation scores between two similar services, so I can choose the one that won't trap me.
5. **As a visitor**, I want to browse the worst offenders list, so I can see which companies are most hostile to customers trying to leave.

### Should Have
6. **As a registered user**, I want to save my active subscriptions and get alerts when their cancellation process gets flagged, so I'm never caught off guard.
7. **As a user**, I want to see FTC Click-to-Cancel compliance status for services, so I can report non-compliant companies.
8. **As a user**, I want to upvote/downvote cancellation reports for accuracy, so the best instructions rise to the top.

### Nice to Have
9. **As a business**, I want to claim my company profile and show that we comply with Click-to-Cancel, so we stand out as customer-friendly.
10. **As a journalist**, I want to query cancellation data by category/industry, so I can write informed articles about dark pattern trends.

---

## Tech Stack

| Layer | Choice | Rationale |
|-------|--------|-----------|
| **Frontend** | React + Vite + TypeScript | Fast, modern, component-based. Per LESSONS.md: always JS/TS stack. |
| **UI Library** | shadcn/ui + Tailwind CSS | Per LESSONS.md: always use shadcn/ui. |
| **Backend** | Node.js (Express) + TypeScript | Simple, proven, same language as frontend. |
| **Database** | PostgreSQL (via Supabase) | Relational data with full-text search. Supabase gives free tier + auth. |
| **Auth** | Supabase Auth | Email/password + Google OAuth. Per LESSONS.md: both are required. |
| **Search** | PostgreSQL full-text search + trigram | Services are searchable by name; pg_trgm handles fuzzy matching. |
| **Hosting** | Vercel (frontend) + Railway (API) | Free tiers for MVP. Easy deploy. |
| **ORM** | Drizzle ORM | Type-safe, lightweight, great DX with TypeScript. |

---

## Architecture Overview

```
┌─────────────────────────────────────────────────┐
│                   Client (React)                 │
│  ┌───────────┐  ┌──────────┐  ┌───────────────┐│
│  │ Search Bar │  │ Service  │  │ Submit Report ││
│  │ + Results  │  │ Profile  │  │    Form       ││
│  └───────────┘  └──────────┘  └───────────────┘│
└───────────────────────┬─────────────────────────┘
                        │ HTTPS
┌───────────────────────▼─────────────────────────┐
│              API Server (Express)                │
│  ┌────────┐ ┌──────────┐ ┌────────────────────┐│
│  │ /search│ │ /services│ │ /reports (CRUD)     ││
│  └────────┘ └──────────┘ └────────────────────┘│
│  ┌────────────────┐ ┌──────────────────────────┐│
│  │ Score Engine   │ │ Auth Middleware           ││
│  │ (aggregation)  │ │ (Supabase JWT verify)    ││
│  └────────────────┘ └──────────────────────────┘│
└───────────────────────┬─────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────┐
│          PostgreSQL (Supabase)                    │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────┐│
│  │ services │ │ reports  │ │ users            ││
│  │ + scores │ │          │ │ + subscriptions  ││
│  └──────────┘ └──────────┘ └──────────────────┘│
└─────────────────────────────────────────────────┘
```

---

## API Design

### Public Endpoints (no auth required)

#### `GET /api/search?q={query}&category={category}&page={page}`
Search for services by name.
```json
// Response
{
  "results": [
    {
      "id": "uuid",
      "name": "Adobe Creative Cloud",
      "category": "Software",
      "logo_url": "https://...",
      "cancel_score": 2.3,
      "total_reports": 847,
      "cancellation_method": "phone_and_web",
      "avg_time_minutes": 35,
      "avg_steps": 8,
      "ftc_compliant": false
    }
  ],
  "total": 1250,
  "page": 1,
  "per_page": 20
}
```

#### `GET /api/services/:slug`
Get full service profile with cancellation details.
```json
// Response
{
  "id": "uuid",
  "name": "Adobe Creative Cloud",
  "slug": "adobe-creative-cloud",
  "category": "Software",
  "subcategory": "Design Tools",
  "website": "https://adobe.com",
  "logo_url": "https://...",
  "cancel_score": 2.3,
  "total_reports": 847,
  "score_breakdown": {
    "steps_score": 2.0,
    "time_score": 1.5,
    "method_score": 3.0,
    "transparency_score": 2.5,
    "retention_tactics_score": 2.0
  },
  "stats": {
    "avg_time_minutes": 35,
    "avg_steps": 8,
    "methods_reported": ["web", "phone", "chat"],
    "requires_phone_call": true,
    "has_retention_team": true,
    "early_termination_fee": true,
    "data_export_available": true
  },
  "cancellation_guide": {
    "steps": [
      {"order": 1, "text": "Go to account.adobe.com and sign in"},
      {"order": 2, "text": "Click 'Plans' in the sidebar"},
      {"order": 3, "text": "Click 'Cancel plan' next to your subscription"},
      {"order": 4, "text": "You'll be offered a discount — click 'No thanks'"},
      {"order": 5, "text": "You'll be asked why — select a reason and click Continue"},
      {"order": 6, "text": "You'll see an early termination fee warning — click 'Confirm'"},
      {"order": 7, "text": "Finally click 'Confirm cancellation'"},
      {"order": 8, "text": "Check email for cancellation confirmation"}
    ],
    "warnings": [
      "Annual plan requires early termination fee if canceled before renewal date",
      "Retention team may offer discounts — decline if you truly want to cancel"
    ],
    "pre_cancel_checklist": [
      "Export your files from Creative Cloud before canceling",
      "Download any fonts you've used in projects",
      "Check if you have annual vs monthly plan (fee implications)"
    ]
  },
  "ftc_compliance": {
    "compliant": false,
    "issues": ["Requires more steps to cancel than to subscribe", "Phone call retention tactics"],
    "last_checked": "2026-02-15"
  },
  "alternatives": [
    {"name": "Canva", "slug": "canva", "cancel_score": 8.5},
    {"name": "Figma", "slug": "figma", "cancel_score": 9.1},
    {"name": "Affinity", "slug": "affinity", "cancel_score": 9.8}
  ],
  "recent_reports": [...],
  "created_at": "2025-01-15T00:00:00Z",
  "updated_at": "2026-02-17T00:00:00Z"
}
```

#### `GET /api/services/:slug/reports?page={page}&sort={sort}`
Get cancellation reports for a service.
```json
// Response
{
  "reports": [
    {
      "id": "uuid",
      "service_id": "uuid",
      "user_display_name": "Alex M.",
      "cancel_date": "2026-02-10",
      "method_used": "web",
      "total_steps": 7,
      "time_minutes": 25,
      "had_retention_offer": true,
      "accepted_retention": false,
      "had_termination_fee": true,
      "fee_amount": 59.99,
      "difficulty_rating": 2,
      "comment": "Had to click through 4 discount offers before they'd let me cancel. Took 25 min.",
      "helpful_votes": 42,
      "created_at": "2026-02-10T14:30:00Z"
    }
  ],
  "total": 847,
  "page": 1
}
```

#### `GET /api/rankings?sort={worst|best|most_reported}&category={category}`
Get ranked lists.
```json
// Response
{
  "rankings": [
    { "rank": 1, "name": "Adobe Creative Cloud", "slug": "...", "cancel_score": 2.3, "total_reports": 847 },
    { "rank": 2, "name": "New York Times", "slug": "...", "cancel_score": 2.8, "total_reports": 523 }
  ]
}
```

### Authenticated Endpoints

#### `POST /api/reports` (auth required)
Submit a cancellation report.
```json
// Request
{
  "service_id": "uuid",
  "method_used": "web",
  "total_steps": 7,
  "time_minutes": 25,
  "had_retention_offer": true,
  "accepted_retention": false,
  "had_termination_fee": true,
  "fee_amount": 59.99,
  "difficulty_rating": 2,
  "comment": "Had to click through 4 discount offers..."
}
// Response: 201 Created with report object
```

#### `POST /api/reports/:id/vote` (auth required)
Vote on report helpfulness.
```json
// Request
{ "vote": "up" | "down" }
```

#### `GET /api/me/subscriptions` (auth required)
Get user's tracked subscriptions.

#### `POST /api/me/subscriptions` (auth required)
Add a subscription to watchlist.
```json
// Request
{ "service_id": "uuid" }
```

#### `POST /api/services/suggest` (auth required)
Suggest a new service to be added.
```json
// Request
{
  "name": "ServiceName",
  "website": "https://...",
  "category": "Software"
}
```

---

## Data Model

### `services`
```sql
CREATE TABLE services (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  slug VARCHAR(255) NOT NULL UNIQUE,
  website VARCHAR(500),
  logo_url VARCHAR(500),
  category VARCHAR(100) NOT NULL,
  subcategory VARCHAR(100),
  description TEXT,
  
  -- Computed/cached scores (updated on new reports)
  cancel_score DECIMAL(3,1) DEFAULT 5.0,  -- 1 (impossible) to 10 (instant)
  total_reports INTEGER DEFAULT 0,
  avg_time_minutes DECIMAL(6,1),
  avg_steps DECIMAL(4,1),
  
  -- Editorial fields
  cancellation_guide JSONB,       -- Step-by-step instructions
  pre_cancel_checklist JSONB,     -- Things to do before canceling
  warnings JSONB,                 -- Important warnings
  
  -- Metadata
  requires_phone_call BOOLEAN DEFAULT false,
  has_retention_team BOOLEAN DEFAULT false,
  has_early_termination_fee BOOLEAN DEFAULT false,
  has_data_export BOOLEAN DEFAULT false,
  ftc_compliant BOOLEAN,
  ftc_issues JSONB,
  
  -- Search
  search_vector TSVECTOR,
  
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_services_slug ON services(slug);
CREATE INDEX idx_services_category ON services(category);
CREATE INDEX idx_services_cancel_score ON services(cancel_score);
CREATE INDEX idx_services_search ON services USING GIN(search_vector);
CREATE INDEX idx_services_name_trgm ON services USING GIN(name gin_trgm_ops);
```

### `cancellation_reports`
```sql
CREATE TABLE cancellation_reports (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  service_id UUID NOT NULL REFERENCES services(id),
  user_id UUID NOT NULL REFERENCES auth.users(id),
  
  -- Report data
  cancel_date DATE NOT NULL,
  method_used VARCHAR(50) NOT NULL,  -- 'web', 'phone', 'chat', 'email', 'in_person'
  total_steps INTEGER NOT NULL CHECK (total_steps >= 1),
  time_minutes INTEGER NOT NULL CHECK (time_minutes >= 1),
  difficulty_rating INTEGER NOT NULL CHECK (difficulty_rating BETWEEN 1 AND 10),
  
  -- Experience details
  had_retention_offer BOOLEAN DEFAULT false,
  accepted_retention BOOLEAN DEFAULT false,
  had_termination_fee BOOLEAN DEFAULT false,
  fee_amount DECIMAL(10,2),
  fee_currency VARCHAR(3) DEFAULT 'USD',
  successfully_canceled BOOLEAN DEFAULT true,
  
  -- Freeform
  comment TEXT,
  
  -- Community
  helpful_votes INTEGER DEFAULT 0,
  unhelpful_votes INTEGER DEFAULT 0,
  
  -- Moderation
  status VARCHAR(20) DEFAULT 'published',  -- 'published', 'flagged', 'removed'
  
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_reports_service ON cancellation_reports(service_id);
CREATE INDEX idx_reports_user ON cancellation_reports(user_id);
CREATE INDEX idx_reports_date ON cancellation_reports(cancel_date DESC);
```

### `report_votes`
```sql
CREATE TABLE report_votes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  report_id UUID NOT NULL REFERENCES cancellation_reports(id),
  user_id UUID NOT NULL REFERENCES auth.users(id),
  vote VARCHAR(10) NOT NULL CHECK (vote IN ('up', 'down')),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(report_id, user_id)
);
```

### `user_subscriptions` (watchlist)
```sql
CREATE TABLE user_subscriptions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES auth.users(id),
  service_id UUID NOT NULL REFERENCES services(id),
  added_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(user_id, service_id)
);
```

### `service_suggestions`
```sql
CREATE TABLE service_suggestions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id),
  name VARCHAR(255) NOT NULL,
  website VARCHAR(500),
  category VARCHAR(100),
  status VARCHAR(20) DEFAULT 'pending',  -- 'pending', 'approved', 'rejected'
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## Score Algorithm

CancelScore is calculated from 5 dimensions, each scored 1-10:

1. **Steps Score** (weight: 25%): Fewer steps = higher score
   - 1 step = 10, 2-3 steps = 8, 4-5 steps = 6, 6-8 steps = 4, 9+ steps = 2

2. **Time Score** (weight: 25%): Less time = higher score
   - <2 min = 10, 2-5 min = 8, 5-15 min = 6, 15-30 min = 4, 30+ min = 2

3. **Method Score** (weight: 20%): Self-serve = highest
   - Web self-serve = 10, Chat = 7, Email = 6, Phone required = 3, In-person = 1

4. **Transparency Score** (weight: 15%): Clear process = higher
   - Based on: retention offer frequency, hidden fee reports, unclear UI reports

5. **Retention Tactics Score** (weight: 15%): Less aggressive = higher
   - No retention = 10, Discount offer only = 7, Multiple offers = 4, Aggressive retention team = 2

**Final Score** = Weighted average across all reports, with recency bias (newer reports weighted more).

---

## Auth Flow

### Email + Password
1. User clicks "Sign Up" → enters email + password
2. Supabase sends verification email
3. User clicks link → verified → logged in
4. JWT stored in httpOnly cookie
5. Password reset via Supabase's built-in flow

### Google OAuth
1. User clicks "Sign in with Google"
2. Redirect to Google consent screen (Supabase handles OAuth)
3. On callback, Supabase creates/links account
4. JWT returned → stored in httpOnly cookie
5. Edge case: Google email matches existing email account → auto-link

### Auth Rules
- Browsing/searching: No auth required
- Submitting reports: Auth required
- Voting on reports: Auth required
- Managing watchlist: Auth required
- Rate limiting: 5 reports per day per user, 50 votes per day

---

## UI Flow

1. **Landing** → User sees search bar + worst offenders + recent reports feed
2. **Search** → Type service name → instant results with scores
3. **Service Profile** → Full score breakdown + cancellation guide + reports
4. **Submit Report** → (after auth) Fill out structured form about cancellation experience
5. **Rankings** → Browse worst/best by category
6. **My Subscriptions** → (after auth) Watchlist of services you use
7. **Compare** → Side-by-side comparison of two services

---

## What We're NOT Building (Scope Cuts)

- ❌ Automated cancellation service (too complex, liability issues)
- ❌ Integration with bank/payment APIs (scope creep)
- ❌ Mobile app (web-first, responsive)
- ❌ AI-generated cancellation guides (start with editorial/crowdsourced)
- ❌ Browser extension (future feature, not MVP)
- ❌ Business dashboard for companies to manage profiles (V2)
- ❌ Email notification system (V2 — watchlist alerts)
- ❌ API for third parties (V2)
- ❌ Dark pattern screenshot archive (cool but hard to moderate)
