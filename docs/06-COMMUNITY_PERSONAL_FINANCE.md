# VirtuaQuest — Community, Competitions & Personal Finance

**Related docs:** [01-FEATURES.md](./01-FEATURES.md) · [02-LEARNING_GAMIFICATION.md](./02-LEARNING_GAMIFICATION.md) · [12-ADMIN.md](./12-ADMIN.md)

---

## 1. Social Features

### 1.1 Friends, Followers, Following `[P1]`

**Model:** Asymmetric follow (like Twitter) — not mutual friendship required.

| Action | Behavior |
|--------|----------|
| Follow | See public profile activity |
| Unfollow | Stop seeing activity |
| Block | Prevent all interaction `[P1]` |

**User Story:** *As a student, I want to follow classmates, so that I can see their learning progress and compete.*

**Acceptance Criteria:**
- [ ] Follow/unfollow from profile page
- [ ] Follower/following counts on profile
- [ ] List followers and following (paginated)
- [ ] Cannot follow private profiles without approval `[P2]`

**API:** `POST /users/:id/follow`, `DELETE /users/:id/follow`, `GET /users/:id/followers`  
**Tables:** `user_follows`

---

### 1.2 Public Portfolios & Sharing `[P1]`

**Privacy controls:**
- Portfolio visibility: Private (default) | Public (% return only) | Public (holdings visible)
- Never show dollar amounts on public leaderboard or shared portfolio by default

**Share flow:**
1. Enable public portfolio in settings
2. Generate share link `/profile/:username/portfolio`
3. Optional: hide specific symbols

**Acceptance Criteria:**
- [ ] Public view shows allocation % and return %, not dollar values
- [ ] Share button copies link
- [ ] User can revoke public access anytime

**Tables:** `user_profiles.portfolio_visibility`, `portfolios.is_public`

---

### 1.3 Activity Feed `[P1]`

**Events shown (respecting privacy):**
- Earned badge
- Completed course
- Joined competition
- Public journal entry (if opted in)

**Not shown:** Dollar amounts, exact trade quantities (unless user opts in)

**Route:** `/feed` — global and following tabs

**API:** `GET /feed?scope=following|global`  
**Tables:** `activity_events`

---

### 1.4 Comments & Likes `[P1]`

**Scope:** Lesson discussions, company page discussions (moderated), public journal entries.

**Acceptance Criteria:**
- [ ] Comment max 1000 chars
- [ ] Like/unlike toggle
- [ ] Report comment button
- [ ] AI moderation flag + human review queue

**Tables:** `comments`, `likes`, `reports`

---

### 1.5 Groups & Clubs `[P1]`

| Type | Description |
|------|-------------|
| Public group | Anyone can join |
| Private group | Invite or approval required |
| School club | Linked to school domain |
| University community | Linked to `.edu` domain `[P2]` |
| Private leagues | Invite-only competition group `[P2]` |

**Features:**
- Group name, description, avatar
- Member list, admin roles
- Group leaderboard (return % within group)
- Group chat or discussion board `[P2]`

**API:** `GET/POST /groups`, `POST /groups/:id/join`  
**Tables:** `groups`, `group_members`

---

## 2. Competitions

### 2.1 Daily Challenges `[P1]`

See [02-LEARNING_GAMIFICATION.md](./02-LEARNING_GAMIFICATION.md) — quiz/scenario based, not trading.

---

### 2.2 Weekly Challenges `[P1]`

**Types:**
- Learning: Complete 3 lessons
- Trading: Place 5 trades with journal entries
- Research: View 5 company pages and ask AI 3 questions

**Reward:** XP bonus + badge progress

---

### 2.3 Monthly Seasons `[P1]`

**Description:** Themed month-long seasons with cosmetic rewards and season leaderboard.

**Example themes:** "Dividend February", "Tech Titans March"

**Scoring:** Combined learning XP + risk-adjusted return (educational formula)

**Acceptance Criteria:**
- [ ] Season banner on dashboard
- [ ] Season-specific badge at completion
- [ ] Leaderboard resets each season; all-time preserved separately

**Tables:** `seasons`, `season_participants`, `season_scores`

---

### 2.4 Investment Tournaments `[P1]`

**Description:** Time-boxed paper trading competition with isolated portfolio.

**Rules:**
- Start: $100,000 simulated (fixed)
- Duration: 1 week default (configurable by admin)
- Allowed: US equities, `[P1]` ETFs
- Ranking: Total return % only
- Min 3 trades to qualify

**Flow:**
1. User joins tournament from `/competitions`
2. New portfolio scoped to `competition_id` created
3. Trade during window
4. End: rankings frozen, badges awarded to top 10

**Acceptance Criteria:**
- [ ] Cannot join after start time
- [ ] Separate portfolio from personal sim
- [ ] Live leaderboard during event
- [ ] Post-event AI Portfolio Review offered

**API:** `GET /competitions`, `POST /competitions/:id/join`, `GET /competitions/:id/leaderboard`  
**Tables:** `competitions`, `competition_participants`, `competition_portfolios`

---

### 2.5 School & University Competitions `[P2]`

**Description:** Institution-scoped tournaments verified by email domain.

**Requirements:**
- User verifies `@school.edu` email
- Admin creates school-wide competition
- School leaderboard + national leaderboard

**Tables:** `school_verifications`, `schools`, `universities`

---

### 2.6 AI Challenges `[P2]`

**Description:** Gamified scenarios — e.g., "AI presents a market scenario; user writes strategy; AI scores educational quality."

Not scored on return; scored on reasoning quality.

---

## 3. Public Investment Journals `[P1]`

**Description:** Opt-in public decision journal entries.

**Fields:** Title, rationale, symbol (optional), tags, public/private toggle

**Acceptance Criteria:**
- [ ] Markdown support
- [ ] Comments from followers
- [ ] AI Journal Coach can review privately before publish

**Tables:** `journal_entries` (extend with `is_public`, `title`)

---

## 4. AI Moderation `[P1]`

**Pipeline:**
1. User submits comment/post
2. AI classifier scores toxicity, spam, investment advice
3. Score > threshold → hold for human review
4. Auto-approve low-risk content

**Admin queue:** See [12-ADMIN.md](./12-ADMIN.md)

---

## 5. Classroom Mode `[Future/P2]`

Listed in user checklist as Future; product spec places at P2.

| Feature | Description |
|---------|-------------|
| Classroom Dashboard | Teacher creates class, invite code |
| Teacher Dashboard | Roster, assignments, progress heatmap |
| Assignments | Due dates, linked lessons/quizzes |
| Grade export | Completion % (FERPA-compliant) |

**Tables:** `classrooms`, `classroom_members`, `assignments`, `assignment_submissions`

---

## 6. Personal Finance Module

### 6.1 Compound Interest Calculator `[P1]`

**Route:** `/tools/compound-interest`

**Inputs:** Initial amount, monthly contribution, annual rate, years  
**Output:** Chart + final amount table  
**Educational:** Explain each input with glossary links

**No API persistence required** — client-side calculation MVP.

---

### 6.2 Budget Planner `[P2]`

| Feature | Description |
|---------|-------------|
| Templates | 50/30/20, zero-based, custom |
| Categories | Housing, food, transport, etc. |
| Monthly view | Budget vs actual |

**Tables:** `budgets`, `budget_categories`, `budget_entries`

---

### 6.3 Expense Tracker `[P2]`

- Manual entry: amount, category, date, note
- Recurring expenses
- Monthly summary chart
- Bank sync via Plaid `[Future]`

**Tables:** `expenses`

---

### 6.4 Savings Goals `[P2]`

- Goal name, target amount, target date
- Progress bar linked to manual "deposits"
- AI Savings Coach integration

**Tables:** `financial_goals`

---

### 6.5 Net Worth Tracker `[P2]`

**Manual entry:**
- Assets: cash, investments (external), property, other
- Liabilities: loans, credit cards, mortgage

**Dashboard widget:** Net worth over time chart

**Tables:** `net_worth_snapshots`, `assets`, `liabilities`

---

### 6.6 Debt Tracker `[P2]`

- Debt name, balance, APR, minimum payment
- Avalanche vs snowball educational comparison
- Payoff timeline calculator

---

### 6.7 Emergency Fund Tracker `[P2]`

- Target: 3–6 months expenses (user sets)
- Current saved amount
- Progress + educational tips

---

### 6.8 Retirement Planner `[P2]`

**Educational Monte Carlo simulation:**
- Inputs: age, retirement age, current savings, monthly contribution, expected return
- Output: probability of reaching goal, chart of projected growth
- Disclaimer: not financial advice

**AI Retirement Planner:** Explains 401k, IRA, compound growth concepts.

---

### 6.9 Financial Goals `[P2]`

Unified goals dashboard: home, education, retirement, custom — linked to savings goals and learning paths.

---

## 7. Community Guidelines

Displayed at signup and in footer:

1. Be respectful — no harassment or hate speech
2. Educational purpose — no pump-and-dump or manipulation
3. No personal financial advice to other users
4. Protect privacy — don't share others' personal info
5. Report violations — moderation team reviews within 48h

Violations: warning → temporary suspension → permanent ban (admin discretion).

---

## 8. Privacy Matrix

| Data | Default Visibility | User Control |
|------|-------------------|--------------|
| Profile | Public name + avatar | Private profile mode |
| Portfolio | Private | Public % or holdings |
| Journal | Private | Per-entry public toggle |
| Activity | Followers only | Disable in settings |
| Leaderboard | Username + return % | Opt out `[P1]` |

---

## 9. API Summary (Community & PF)

| Method | Endpoint | Priority |
|--------|----------|----------|
| POST | `/users/:id/follow` | P1 |
| GET | `/feed` | P1 |
| GET/POST | `/groups` | P1 |
| GET/POST | `/competitions` | P1 |
| POST | `/competitions/:id/join` | P1 |
| GET/POST | `/journal` | MVP (private) / P1 (public) |
| GET/POST | `/budgets` | P2 |
| GET/POST | `/expenses` | P2 |
| GET/POST | `/financial-goals` | P2 |

Full spec: [10-API.md](./10-API.md)
