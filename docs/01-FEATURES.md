# VirtuaQuest — Feature Catalog

**Related docs:** [00-PRODUCT_VISION.md](./00-PRODUCT_VISION.md) · [14-DEVELOPMENT_CHECKLIST.md](./14-DEVELOPMENT_CHECKLIST.md) · [10-API.md](./10-API.md) · [09-DATABASE.md](./09-DATABASE.md)

Each feature includes: **Priority | User Story | Acceptance Criteria | Dependencies**

---

## 1. Foundation — Core Platform

### 1.1 Landing Page `[MVP]`

**Description:** Marketing homepage at `virtuaquest.com` with hero, feature highlights, mode selector preview, and sign-up CTA.

**User Story:** *As a prospective user, I want to understand what VirtuaQuest offers, so that I can decide to sign up.*

**Acceptance Criteria:**
- [ ] Hero section with tagline and primary CTA (Sign Up Free)
- [ ] Three feature pillars: Learn, Simulate, Master
- [ ] Social proof section (testimonials or stats placeholder)
- [ ] Footer with Terms, Privacy, educational disclaimer
- [ ] Responsive on mobile, tablet, desktop
- [ ] Lighthouse accessibility score ≥ 90

**Dependencies:** None  
**API:** None (static/SSR page)  
**Tables:** None

---

### 1.2 Authentication `[MVP]`

**Description:** Full auth flow via Clerk or Supabase Auth.

| Sub-feature | Priority |
|-------------|----------|
| User Registration | `[MVP]` |
| Login | `[MVP]` |
| Logout | `[MVP]` |
| Forgot Password | `[MVP]` |
| Email Verification | `[MVP]` |
| OAuth (Google, Apple) | `[MVP]` |
| OAuth (Microsoft — schools) | `[P1]` |
| Two-Factor Authentication | `[P1]` |

**User Story:** *As a new user, I want to create an account securely, so that I can save my progress and portfolio.*

**Acceptance Criteria:**
- [ ] Email/password registration with validation
- [ ] Email verification required before full access
- [ ] Login persists session (httpOnly cookie or secure token)
- [ ] Logout clears session on client and server
- [ ] Forgot password sends reset link (expires 1 hour)
- [ ] OAuth providers work on web
- [ ] Age gate at registration (birth year); under-13 triggers COPPA flow per [11-SECURITY_COMPLIANCE.md](./11-SECURITY_COMPLIANCE.md)

**Dependencies:** Auth provider (Clerk/Supabase)  
**API:** `POST /auth/register`, `POST /auth/login`, `POST /auth/logout`, `POST /auth/forgot-password`, `POST /auth/verify-email`  
**Tables:** `users`, `user_profiles`

---

### 1.3 Global Navigation & Search `[MVP]`

**Description:** Persistent top nav with logo, main sections, search bar, notifications, profile menu.

**User Story:** *As a logged-in user, I want to navigate anywhere and search symbols quickly.*

**Acceptance Criteria:**
- [ ] Nav items: Dashboard, Learn, Trade, Portfolio, Leaderboard, AI Tutor
- [ ] Symbol search with autocomplete (debounced 300ms)
- [ ] Search returns companies, ETFs; courses in P1
- [ ] Keyboard shortcut `/` focuses search (Professional mode)
- [ ] Mobile: hamburger menu + bottom nav

**Dependencies:** Search index (Typesense/Meilisearch), market symbol master  
**API:** `GET /search?q=`  
**Tables:** `companies`

---

### 1.4 User Profile `[MVP]` / Extended `[P1]`

| Field | Priority |
|-------|----------|
| Avatar, Bio, Country | `[MVP]` |
| Experience Level, Financial Goals | `[MVP]` |
| University, School | `[P1]` |
| Skills, Interests | `[P1]` |
| Investment Style, Risk Profile | `[P1]` |
| XP, Level, Achievements, Badges | `[MVP]` basic / `[P1]` full |
| Public Portfolio, Activity Feed | `[P1]` |
| Followers, Following | `[P1]` |

**User Story:** *As a user, I want a profile that reflects my learning journey, so that I can track progress and share optionally.*

**Acceptance Criteria:**
- [ ] Upload avatar (max 2MB, JPG/PNG) to R2
- [ ] Edit bio (max 500 chars), country (ISO dropdown)
- [ ] Experience level: Beginner / Student / Professional
- [ ] Financial goals: multi-select (retirement, home, education, wealth building)
- [ ] XP and level displayed on profile
- [ ] At least 5 MVP badges visible
- [ ] Privacy toggle: public profile on/off `[P1]`

**API:** `GET/PATCH /users/me`, `GET /users/:id/public`  
**Tables:** `users`, `user_profiles`, `user_achievements`, `xp_events`

---

### 1.5 User Settings `[MVP]`

| Setting | Priority |
|---------|----------|
| Theme (dark/light/system) | `[MVP]` |
| Notifications preferences | `[MVP]` |
| Privacy | `[MVP]` |
| Password change | `[MVP]` |
| Two-Factor Authentication | `[P1]` |
| Connected Accounts | `[P1]` |
| Data Export | `[P1]` |

**Acceptance Criteria:**
- [ ] Theme persists across sessions
- [ ] Notification toggles: in-app, email (email P1)
- [ ] Change password requires current password
- [ ] Delete account flow with confirmation `[P1]`

**API:** `GET/PATCH /users/me/settings`  
**Tables:** `user_settings`

---

### 1.6 Notification Center `[MVP]`

**Description:** In-app notification bell with unread count and list.

**Acceptance Criteria:**
- [ ] Bell icon with badge count
- [ ] Notifications: trade filled, lesson complete, badge earned, leaderboard change
- [ ] Mark as read (single and all)
- [ ] Real-time via WebSocket `notification:new`

**API:** `GET /notifications`, `PATCH /notifications/:id/read`  
**Tables:** `notifications`

---

### 1.7 Dark Mode / Light Mode `[MVP]`

**Acceptance Criteria:**
- [ ] Toggle in settings and quick toggle in nav
- [ ] All screens support both themes
- [ ] Charts respect theme (dark chart on dark mode)
- [ ] No flash of wrong theme on load (SSR theme cookie)

---

### 1.8 Responsive Design & Accessibility `[MVP]`

**Acceptance Criteria:**
- [ ] Breakpoints: mobile <768px, tablet 768–1024px, desktop >1024px
- [ ] WCAG 2.1 AA: color contrast, focus indicators, alt text
- [ ] Screen reader labels on all financial numbers
- [ ] Reduced motion option disables animations
- [ ] Skip to main content link

---

### 1.9 Multi-language Support `[Future]`

**Description:** i18n for UI strings. MVP is English only.

**Acceptance Criteria (Future):**
- [ ] next-intl or similar
- [ ] Priority languages: English, Spanish, Hindi

---

## 2. Dashboard `[MVP]`

**Description:** Home screen after login showing portfolio summary, learning progress, market overview, and quick actions.

| Widget | Priority |
|--------|----------|
| Portfolio Value, Cash Balance, Daily G/L, Total Return | `[MVP]` |
| Watchlist | `[MVP]` |
| Recent Trades | `[MVP]` |
| Market Overview (S&P, NASDAQ, Dow) | `[MVP]` |
| Learning Progress | `[MVP]` |
| News Feed | `[P1]` |
| Economic Events | `[P1]` |
| AI Insights | `[P1]` |
| Portfolio Health Score, Risk Score | `[P1]` |
| Current Challenges | `[P1]` |
| Goals Progress | `[P1]` |

**User Story:** *As a returning user, I want a single dashboard showing my progress and portfolio, so that I know what to do next.*

**Acceptance Criteria:**
- [ ] Portfolio value updates on page load and via WebSocket
- [ ] Daily G/L shows $ and % with semantic green/red
- [ ] Watchlist shows up to 10 symbols with price and change
- [ ] Recent trades: last 5 with symbol, side, quantity, time
- [ ] Market overview: 3 major indices with sparklines
- [ ] Learning progress: current course % complete + CTA
- [ ] Widget layout responsive (stack on mobile)
- [ ] Empty states for new users with guided CTAs

**API:** `GET /dashboard` (aggregated BFF endpoint)  
**Tables:** `portfolios`, `positions`, `transactions`, `watchlists`, `enrollments`

---

## 3. Paper Trading `[MVP]`

| Feature | Priority |
|---------|----------|
| Virtual Wallet | `[MVP]` |
| Starting Capital Selection | `[MVP]` |
| Buy/Sell Stocks | `[MVP]` |
| Fractional Shares | `[MVP]` |
| Market Orders | `[MVP]` |
| Limit Orders | `[P1]` |
| Stop Orders | `[P1]` |
| Transaction History | `[MVP]` |
| Order History | `[MVP]` |
| Trade Confirmation | `[MVP]` |
| Portfolio History | `[P1]` |
| Real-Time Portfolio Updates | `[MVP]` |

**User Story:** *As a learner, I want to buy and sell stocks with simulated money, so that I can practice without risk.*

**Acceptance Criteria:**
- [ ] Default starting capital $100,000 (selectable: $10K, $50K, $100K, $500K at creation)
- [ ] Market order executes at last available quote (delayed OK)
- [ ] Fractional shares to 4 decimal places
- [ ] Insufficient funds rejected with clear error
- [ ] Trade confirmation modal shows symbol, qty, price, total, fees ($0 simulated)
- [ ] Decision journal prompt on orders >$1,000 simulated value
- [ ] Order/transaction history paginated, filterable by symbol and date
- [ ] WebSocket pushes portfolio update within 2s of fill
- [ ] "Simulated money" banner on all trading screens

**Business Rules:**
- No margin, no short selling in MVP
- Market hours: orders queue with educational message if outside hours (still fill at next quote for simplicity in MVP)
- One primary portfolio per user at MVP

**API:** `POST /portfolios/:id/orders`, `GET /portfolios/:id/orders`, `GET /portfolios/:id/transactions`  
**Tables:** `portfolios`, `orders`, `transactions`, `positions`, `journal_entries`

See [04-TRADING_TOOLS.md](./04-TRADING_TOOLS.md) for full trading rules.

---

## 4. Portfolio `[MVP]`

| Feature | Priority |
|---------|----------|
| Holdings | `[MVP]` |
| Allocation (basic) | `[MVP]` |
| Profit/Loss | `[MVP]` |
| Unrealized/Realized Gain | `[MVP]` |
| Sector/Country/Asset Allocation | `[P1]` |
| Dividend History | `[P1]` |
| Portfolio Timeline | `[P1]` |
| Portfolio Performance | `[P1]` |
| Benchmark Comparison | `[P1]` |
| Portfolio Replay | `[P2]` |
| Portfolio Heatmap | `[P2]` |

**Acceptance Criteria:**
- [ ] Holdings table: symbol, name, qty, avg cost, current price, market value, unrealized G/L
- [ ] Allocation donut chart by holding
- [ ] Total portfolio value, cash, invested
- [ ] Realized G/L in separate section
- [ ] Export CSV `[P1]`

**API:** `GET /portfolios/:id`, `GET /portfolios/:id/positions`, `GET /portfolios/:id/performance`  
**Tables:** `portfolios`, `positions`, `transactions`

---

## 5. Company Pages `[MVP]` / Extended `[P1]`

See [03-COMPANY_MARKET_DATA.md](./03-COMPANY_MARKET_DATA.md) for full company page specification.

**MVP tabs:** Overview, Financials (annual), Chart  
**P1 tabs:** Business, Valuation, Ownership, Events, News, Analysis, Timeline

---

## 6. Charts `[MVP]`

| Chart Type | Priority |
|------------|----------|
| Line | `[MVP]` |
| Candlestick | `[MVP]` |
| Volume | `[MVP]` |
| Interactive (zoom, pan) | `[MVP]` |
| OHLC | `[P1]` |
| Portfolio / Performance / Comparison | `[P1]` |
| Sector Chart | `[P1]` |
| Drawing Tools | `[P2]` |

See [04-TRADING_TOOLS.md](./04-TRADING_TOOLS.md).

---

## 7. AI Chat `[MVP]`

| Feature | Priority |
|---------|----------|
| AI Chat Assistant | `[MVP]` |
| AI Financial Tutor | `[MVP]` |
| Explain Like I'm a Beginner | `[MVP]` |
| All other AI features (21) | `[P1]` / `[P2]` |

See [05-AI_SYSTEM.md](./05-AI_SYSTEM.md) for all 24 AI features.

**MVP Acceptance Criteria:**
- [ ] Chat UI with thread history
- [ ] Tutor mode answers concept questions
- [ ] Refuses buy/sell recommendations
- [ ] Beginner-mode vocabulary when user is in Beginner experience level
- [ ] Rate limit: 20 messages/hour free tier

**API:** `POST /ai/chat`, `GET /ai/conversations`  
**Tables:** `ai_conversations`, `ai_messages`

---

## 8. Learning Module `[MVP]`

| Feature | Priority |
|---------|----------|
| Courses, Lessons, Quizzes | `[MVP]` |
| Definitions / Finance Dictionary | `[MVP]` |
| Progress Tracking | `[MVP]` |
| Learning Paths | `[P1]` |
| Flashcards | `[P1]` |
| Interactive Exercises | `[P1]` |
| Skill Trees | `[P1]` |
| Certificates | `[P1]` |

See [02-LEARNING_GAMIFICATION.md](./02-LEARNING_GAMIFICATION.md).

**MVP Acceptance Criteria:**
- [ ] At least 3 courses seeded: Personal Finance 101, Intro to Stocks, Risk & Diversification
- [ ] Lesson reader with inline glossary tooltips
- [ ] Quiz after each module (pass ≥ 70%)
- [ ] Progress bar per course on dashboard
- [ ] Completing Lesson 1 of Intro to Stocks unlocks paper trading

---

## 9. Leaderboards `[MVP]`

| Leaderboard | Priority |
|-------------|----------|
| Global | `[MVP]` |
| Best Investors (return %) | `[MVP]` |
| Best Learners (XP) | `[MVP]` |
| Friends, Weekly, Monthly, All-Time | `[P1]` |
| Country, State, School, University | `[P1]` |
| Most Consistent, Lowest Risk, Highest Growth | `[P2]` |

**Acceptance Criteria:**
- [ ] Rankings by simulated portfolio return % (not dollar amounts)
- [ ] Best Learners ranked by total XP
- [ ] Top 100 displayed; user's rank shown if outside top 100
- [ ] Refreshed every 5 minutes (materialized view)

**API:** `GET /leaderboards?type=investors|learners&scope=global`  
**Tables:** `leaderboard_snapshots`, `xp_events`

---

## 10. Deployment `[MVP]`

**Acceptance Criteria:**
- [ ] Production environment on Vercel (frontend) + Railway/Fly.io (backend)
- [ ] Staging environment mirrors production
- [ ] CI/CD: lint, typecheck, test on PR; deploy on merge to main
- [ ] Environment variables managed securely
- [ ] Health check endpoints: `/health`, `/ready`
- [ ] Error tracking (Sentry or similar)
- [ ] Uptime monitoring on API and data providers

See [08-ARCHITECTURE.md](./08-ARCHITECTURE.md).

---

## 11. Feature Index by Priority

### `[MVP]` — Full List

Authentication, Landing, Nav, Search, Profile (basic), Settings, Notifications, Dark/Light mode, Responsive, Accessibility, Dashboard (core widgets), Paper Trading (market orders), Portfolio (core), Company Pages (overview + financials), Charts (line/candle/volume), AI Chat (tutor), Learning (courses/quizzes/dictionary), Leaderboards (global), Deployment

### `[P1]` — Representative List

Limit/stop orders, full profile/social, news, screeners, calendars, 20+ AI features, competitions, admin panel, full gamification, research terminal, benchmark comparison, 2FA, data export

### `[P2]` — Representative List

Advanced TA, portfolio replay/heatmap, personal finance module, school competitions, conference calls, drawing tools

### `[Future]` — Representative List

Mobile/desktop apps, broker integrations, options/crypto simulators, voice AI, multi-language, classroom dashboard

Full checklist: [14-DEVELOPMENT_CHECKLIST.md](./14-DEVELOPMENT_CHECKLIST.md)
