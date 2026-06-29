# VirtuaQuest — UI/UX Specification

**Related docs:** [00-PRODUCT_VISION.md](./00-PRODUCT_VISION.md) · [01-FEATURES.md](./01-FEATURES.md) · [07-UI_UX.md](./07-UI_UX.md)

---

## 1. Design Principles

1. **Clarity over density** — Beginner mode is spacious; Professional mode is information-rich
2. **Education visible** — Glossary, tooltips, and disclaimers integrated, not bolted on
3. **Semantic color** — Green/red only for P&L and market direction; brand uses neutral + accent
4. **Accessible by default** — WCAG 2.1 AA on all screens
5. **Responsive first** — Mobile, tablet, desktop layouts specified per screen

---

## 2. Design System

### 2.1 Typography

| Use | Font | Weight |
|-----|------|--------|
| UI text | Inter | 400, 500, 600, 700 |
| Numbers, tickers, tables | JetBrains Mono | 400, 500 |

**Scale:**
- Beginner mode: base 16px, headings +2px vs standard
- Student mode: base 15px
- Professional mode: base 14px

### 2.2 Color Tokens

| Token | Light | Dark | Use |
|-------|-------|------|-----|
| `--background` | #FFFFFF | #0A0A0B | Page bg |
| `--foreground` | #0A0A0B | #FAFAFA | Text |
| `--muted` | #F4F4F5 | #27272A | Secondary bg |
| `--accent` | #2563EB | #3B82F6 | Primary actions, links |
| `--positive` | #16A34A | #22C55E | Gains, up |
| `--negative` | #DC2626 | #EF4444 | Losses, down |
| `--border` | #E4E4E7 | #3F3F46 | Dividers |

**Rule:** Never use green/red as primary brand colors.

### 2.3 Spacing & Radius

- Grid: 4px base unit
- Card radius: 8px (Beginner: 12px)
- Button radius: 6px
- Max content width: 1280px (dashboard); 1440px (terminal)

### 2.4 Components (shadcn/ui)

Button, Input, Select, Dialog, Sheet, Tabs, Tooltip, Popover, Dropdown, Table, Card, Badge, Avatar, Progress, Toast, Command (search)

---

## 3. Global Layout

### 3.1 Desktop (>1024px)

```
┌─────────────────────────────────────────────────────────┐
│ Logo │ Nav Links │ Search │ 🔔 │ Theme │ Avatar ▼      │
├─────────────────────────────────────────────────────────┤
│                                                         │
│                    Main Content                         │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 3.2 Mobile (<768px)

```
┌─────────────────────────┐
│ ☰ │ Logo    │ 🔔 │ 👤  │
├─────────────────────────┤
│                         │
│     Main Content        │
│                         │
├─────────────────────────┤
│ 🏠 │ 📚 │ 📈 │ 💼 │ 🤖  │
└─────────────────────────┘
```

Bottom nav: Dashboard, Learn, Trade, Portfolio, AI Tutor

### 3.3 Tablet (768–1024px)

Collapsible sidebar nav; full bottom nav on portrait.

---

## 4. Screen Specifications

### 4.1 Landing Page `[MVP]`

**Route:** `/`

| Section | Content |
|---------|---------|
| Hero | Headline, tagline, CTA "Start Learning Free", hero illustration |
| Features | 3 columns: Learn, Simulate, Master |
| How it works | 4 steps: Sign up → Learn → Paper trade → Track progress |
| Audience | Cards: Students, Beginners, Teachers |
| CTA footer | Second sign-up button |
| Footer | Links, disclaimer, social |

**Acceptance:** LCP < 2.5s; responsive; no auth required.

---

### 4.2 Authentication Pages `[MVP]`

**Routes:** `/sign-up`, `/sign-in`, `/forgot-password`, `/verify-email`

| Element | Spec |
|---------|------|
| Layout | Centered card, max-width 400px |
| OAuth buttons | Google, Apple above divider |
| Age gate | Birth year dropdown on sign-up |
| Disclaimer | "Educational platform — not a brokerage" |

---

### 4.3 Dashboard `[MVP]`

**Route:** `/dashboard`

| Widget | Layout Desktop | Layout Mobile |
|--------|----------------|---------------|
| Portfolio summary | Top row, 4 stat cards | Stacked |
| Watchlist | Left column | Below stats |
| Recent trades | Center | Below watchlist |
| Market overview | Right column | Horizontal scroll |
| Learning progress | Banner below stats | Full width |
| Quick actions | FAB or button row: Trade, Learn, AI | Same |

**Empty state (new user):** Illustration + "Complete your first lesson" CTA

---

### 4.4 Company Page `[MVP]`

**Route:** `/company/[symbol]`

**Sticky header:**
- Symbol, name, price, change $/%, delayed badge
- Buttons: Watchlist, Trade, Ask AI

**Tabs (MVP):** Overview | Financials | Chart  
**Tabs (P1):** + Business, Valuation, Ownership, Events, News, Analysis, Timeline

**Overview layout:**
- Key stats grid (2×4)
- Description paragraph
- Embedded chart (1Y default)

---

### 4.5 Trading Screen `[MVP]`

**Route:** `/trade` or `/trade/[symbol]`

**Desktop:** 70% chart | 30% order panel  
**Mobile:** Chart top, order panel bottom sheet

**Order panel:**
- Symbol search/select
- Side toggle: Buy / Sell
- Order type: Market (MVP)
- Quantity input (supports decimal)
- Estimated total
- Submit → confirmation modal → journal prompt

**Banner:** "Simulated money — for education only"

---

### 4.6 Portfolio Page `[MVP]`

**Route:** `/portfolio`

| Section | Component |
|---------|-----------|
| Summary cards | Total value, cash, daily G/L, total return |
| Allocation | Donut chart |
| Holdings | Sortable table |
| Performance chart | `[P1]` Line chart |

---

### 4.7 Charts (Full-Screen) `[MVP]`

**Route:** `/charts/[symbol]`

- Full viewport chart
- Time range toolbar
- Chart type toggle: Line / Candle / `[P1]` OHLC
- Indicator picker `[P1]`
- Drawing tools `[P2]`

---

### 4.8 Research Terminal `[P1]`

**Route:** `/terminal`

- Dark theme default
- Resizable panel grid (react-grid-layout)
- Panels: Watchlist, Chart, Orders, News, Filings, AI
- Save layout to user preferences

---

### 4.9 Learning — Course Catalog `[MVP]`

**Route:** `/learn`

- Course cards with progress ring
- Skill tree view toggle `[P1]`
- Continue learning section at top

---

### 4.10 Learning — Lesson Reader `[MVP]`

**Route:** `/learn/courses/[courseId]/lessons/[lessonId]`

- Progress bar top
- Markdown content with glossary terms underlined
- Sidebar: module outline
- Bottom: Previous | Next | Take Quiz
- XP reward animation on complete

---

### 4.11 AI Chat `[MVP]`

**Route:** `/ai`

| Area | Spec |
|------|------|
| Sidebar | Thread list (collapsible mobile) |
| Main | Message bubbles: user right, AI left |
| Input | Textarea + send; mode selector above |
| Citations | Card below AI message |
| Disclaimer | Footer: "AI tutor — not financial advice" |

Slide-over variant: 400px width from right on company/trade pages.

---

### 4.12 Leaderboard `[MVP]`

**Route:** `/leaderboard`

- Tabs: Investors | Learners
- Scope filter: Global `[MVP]` | Friends, School `[P1]`
- Table: Rank, username, return % or XP, badge icons top 3
- User row highlighted if outside top 100

---

### 4.13 Profile `[MVP]`

**Route:** `/profile/[username]`

- Avatar, name, bio, level, title
- XP progress bar
- Badges grid
- Stats: courses complete, trades, streak `[P1]`
- Tabs: Achievements | Activity `[P1]` | Public portfolio `[P1]`

---

### 4.14 Settings `[MVP]`

**Route:** `/settings`

**Sections:** Profile, Experience mode, Theme, Notifications, Privacy, Security, Connected accounts `[P1]`, Data export `[P1]`, Delete account `[P1]`

---

### 4.15 Notifications `[MVP]`

**Route:** `/notifications` or dropdown from bell

- List with icon, title, body, time
- Mark read on click
- Empty state illustration

---

### 4.16 Screeners `[P1]`

**Route:** `/screeners`

- Filter builder left panel
- Results table right
- Save screen button
- AI explain button

---

### 4.17 Calendars & News `[P1]`

**Routes:** `/markets/calendar`, `/markets/news`

- Calendar: month grid + event list
- News: card feed with AI summary expand

---

### 4.18 Personal Finance Tools `[P2]`

**Routes:** `/finance/budget`, `/finance/goals`, `/tools/compound-interest`

- Wizard-style flows for budget setup
- Progress dashboards

---

### 4.19 Admin Panel `[P1]`

**Route:** `/admin/*`

- Sidebar: Users, Courses, Companies, News, Challenges, Leaderboards, Moderation, Analytics
- Role guard: admin/moderator only
- See [12-ADMIN.md](./12-ADMIN.md)

---

## 5. Experience Mode Variants

| Element | Beginner | Student | Professional |
|---------|----------|---------|--------------|
| Nav items | 5 max visible | Full | Full + Terminal |
| Chart default | Line | Candlestick | Candlestick + indicators |
| Stats shown | 4 key metrics | 8 metrics | 12+ metrics |
| AI default explain | Beginner | Standard | Expert option |
| Glossary | Auto all terms | On hover | On demand |
| Terminal access | Hidden | Hidden | Prominent |

---

## 6. Accessibility

| Requirement | Implementation |
|-------------|----------------|
| Color contrast | 4.5:1 minimum text |
| Focus indicators | Visible ring on all interactive elements |
| Screen readers | `aria-label` on icon buttons; live regions for price updates |
| Keyboard nav | Tab order logical; Escape closes modals |
| Skip link | "Skip to main content" first focusable |
| Reduced motion | `prefers-reduced-motion` disables chart animations |
| Font scaling | Supports browser zoom to 200% |

---

## 7. Keyboard Shortcuts

### Global `[MVP]`

| Key | Action |
|-----|--------|
| `/` | Focus search |
| `?` | Show shortcuts help |
| `Esc` | Close modal/panel |

### Professional Terminal `[P1]`

| Key | Action |
|-----|--------|
| `B` | Open buy panel |
| `S` | Open sell panel |
| `G` | Go to symbol |
| `1`–`5` | Switch watchlist |
| `Ctrl+K` | Command palette |

---

## 8. Dark Mode & Light Mode `[MVP]`

- Toggle: Settings + quick toggle in nav (sun/moon icon)
- Default: System preference; Professional terminal defaults dark
- Persist: Cookie + user settings DB
- Charts: Theme-aware colors via Lightweight Charts options

---

## 9. Responsive Breakpoints

| Name | Width | Layout changes |
|------|-------|----------------|
| `sm` | 640px | Single column |
| `md` | 768px | Bottom nav appears |
| `lg` | 1024px | Side nav, multi-column dashboard |
| `xl` | 1280px | Max content width |
| `2xl` | 1536px | Terminal multi-panel |

---

## 10. Motion & Feedback

| Interaction | Animation |
|-------------|-----------|
| Page transition | Fade 150ms |
| Modal | Scale + fade 200ms |
| Toast | Slide in from top |
| XP earned | +N float animation |
| Badge earned | Confetti (respect reduced motion) |
| Trade filled | Green pulse on portfolio value |

---

## 11. Error & Empty States

| State | Pattern |
|-------|---------|
| 404 | Friendly illustration + search CTA |
| API error | Retry button + support link |
| Empty portfolio | "Make your first trade" CTA |
| Empty watchlist | "Search for symbols" CTA |
| Stale data | Yellow banner with last updated time |

---

## 12. Legal & Disclaimer Placement

**Persistent (trading/financial pages):** Thin banner or footer strip  
**Text:** "VirtuaQuest is an educational simulation. Not financial advice. No real money invested."

**AI pages:** Below input area  
**Sign-up:** Checkbox acknowledgment

---

## 13. Figma / Design Handoff Notes

- Component library maps 1:1 to shadcn/ui
- Provide redline specs for spacing on dashboard and company page first
- Chart colors documented in tokens file
- Icon set: Lucide React
