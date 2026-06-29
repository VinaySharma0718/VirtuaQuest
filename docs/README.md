# VirtuaQuest — Product Specification

**Product:** VirtuaQuest  
**Tagline:** Learn money. Simulate markets. Master decisions.  
**Version:** 1.0 (Internal Product Spec)  
**Status:** Pre-implementation blueprint

---

## About This Documentation

This folder contains the complete internal product specification for VirtuaQuest — an AI-powered financial education and investment simulation platform. The documentation is designed so that a team of engineers can build the platform without needing additional product decisions.

**Priority tags used throughout:**

| Tag | Meaning |
|-----|---------|
| `[MVP]` | Ships in the first releasable product (10 pillars + foundation) |
| `[P1]` | First post-MVP wave; requires MVP live |
| `[P2]` | Second post-MVP wave; requires P1 data and user base |
| `[Future]` | Long-term; requires mature platform and compliance review |

**No timelines or date estimates** appear in this specification. Build order is defined by dependency tiers in [13-BUILD_PRIORITIES.md](./13-BUILD_PRIORITIES.md).

---

## Reading Order for Engineers

### Start Here (Product Context)

1. [00-PRODUCT_VISION.md](./00-PRODUCT_VISION.md) — Mission, principles, personas, platform modes
2. [13-BUILD_PRIORITIES.md](./13-BUILD_PRIORITIES.md) — What to build first (MVP → P1 → P2 → Future)
3. [14-DEVELOPMENT_CHECKLIST.md](./14-DEVELOPMENT_CHECKLIST.md) — Trackable feature checklist

### Product & Features

4. [01-FEATURES.md](./01-FEATURES.md) — Exhaustive feature catalog with user stories and acceptance criteria
5. [02-LEARNING_GAMIFICATION.md](./02-LEARNING_GAMIFICATION.md) — Learning system, glossary, XP, badges, leaderboards
6. [03-COMPANY_MARKET_DATA.md](./03-COMPANY_MARKET_DATA.md) — Company pages, financial data, screeners, calendars
7. [04-TRADING_TOOLS.md](./04-TRADING_TOOLS.md) — Paper trading, charts, TA, portfolio analytics
8. [05-AI_SYSTEM.md](./05-AI_SYSTEM.md) — All 24 AI features, RAG, safety guardrails
9. [06-COMMUNITY_PERSONAL_FINANCE.md](./06-COMMUNITY_PERSONAL_FINANCE.md) — Social, competitions, personal finance

### Design

10. [07-UI_UX.md](./07-UI_UX.md) — Every screen, layouts, design system, accessibility

### Technical

11. [08-ARCHITECTURE.md](./08-ARCHITECTURE.md) — Stack, services, caching, Docker, CI/CD, deployment
12. [09-DATABASE.md](./09-DATABASE.md) — Full schema, ERD, indexes
13. [10-API.md](./10-API.md) — REST API, WebSocket events, auth, permissions
14. [11-SECURITY_COMPLIANCE.md](./11-SECURITY_COMPLIANCE.md) — COPPA, FERPA, RBAC, encryption

### Operations

15. [12-ADMIN.md](./12-ADMIN.md) — Admin panel capabilities

---

## Document Conventions

### User Stories

Format: *As a [persona], I want [action], so that [outcome].*

### Acceptance Criteria

Written as testable checkboxes:

- [ ] Criterion that QA can verify
- [ ] Another verifiable criterion

### Implementation Notes

Each feature section includes:

- **Priority:** `[MVP]`, `[P1]`, `[P2]`, or `[Future]`
- **Dependencies:** Other features or systems required
- **API endpoints:** Cross-reference to [10-API.md](./10-API.md)
- **Database tables:** Cross-reference to [09-DATABASE.md](./09-DATABASE.md)

### Cross-References

Documents link to each other using relative paths. When implementing a feature, follow links to related API, database, and UI specs.

---

## MVP Summary (Build First)

| # | Pillar |
|---|--------|
| 1 | User Authentication |
| 2 | Dashboard |
| 3 | Company Pages |
| 4 | Paper Trading |
| 5 | Portfolio |
| 6 | Charts |
| 7 | AI Chat |
| 8 | Learning Module |
| 9 | Leaderboards |
| 10 | Deployment |

See [13-BUILD_PRIORITIES.md](./13-BUILD_PRIORITIES.md) for full MVP scope and foundation items.

---

## Technology Stack (Summary)

| Layer | Choice |
|-------|--------|
| Frontend | Next.js 15 + React 19 + TypeScript |
| UI | Tailwind CSS + shadcn/ui + Radix |
| Charts | Lightweight Charts + Recharts |
| Backend | Next.js BFF + NestJS |
| AI | Python FastAPI |
| Database | PostgreSQL 16 |
| Cache | Redis 7 |
| Auth | Clerk or Supabase Auth |
| API Base | `https://api.virtuaquest.com/v1` |

Full details in [08-ARCHITECTURE.md](./08-ARCHITECTURE.md).

---

## Contact & Ownership

| Role | Responsibility |
|------|----------------|
| Product | Feature scope, priority tags, acceptance criteria |
| Engineering | Architecture, API, database, implementation |
| Design | UI/UX spec in [07-UI_UX.md](./07-UI_UX.md) |
| Compliance | Security spec in [11-SECURITY_COMPLIANCE.md](./11-SECURITY_COMPLIANCE.md) |
