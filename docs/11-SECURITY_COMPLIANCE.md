# VirtuaQuest — Security & Compliance

**Related docs:** [08-ARCHITECTURE.md](./08-ARCHITECTURE.md) · [10-API.md](./10-API.md) · [05-AI_SYSTEM.md](./05-AI_SYSTEM.md)

---

## 1. Security Overview

VirtuaQuest handles user accounts, educational financial data, and AI interactions. Security priorities:

1. Protect user credentials and PII
2. Prevent unauthorized access to accounts and admin functions
3. Ensure AI does not leak sensitive data or provide regulated investment advice
4. Comply with COPPA (children) and prepare for FERPA (classrooms)
5. Maintain audit trails for admin and moderation actions

---

## 2. Authentication

| Control | Implementation |
|---------|----------------|
| Provider | Clerk (recommended) or Supabase Auth |
| Password policy | Min 8 chars; breach detection via provider |
| Session | httpOnly secure cookies; JWT expiry 1h; refresh rotation |
| OAuth | Google, Apple MVP; Microsoft `[P1]` for schools |
| Email verification | Required before trading/AI |
| MFA | TOTP via provider `[P1]` |
| Brute force | Provider rate limiting + account lockout |

**No passwords stored in VirtuaQuest database** — delegated to auth provider.

---

## 3. Authorization (RBAC)

### Roles

| Role | Description |
|------|-------------|
| `student` | Default registered user |
| `teacher` | Educator; classroom features `[P2]` |
| `parent` | Linked to child accounts `[P1]` |
| `moderator` | Content moderation |
| `admin` | Full platform administration |

### Permission Matrix (Summary)

| Resource | student | parent | teacher | moderator | admin |
|----------|---------|--------|---------|-----------|-------|
| Own portfolio | CRUD | R (child) | CRUD | — | R |
| Own profile | CRUD | R (child) | CRUD | R | CRUD |
| Paper trade | Yes | — | Yes | — | Yes |
| AI chat | Yes | — | Yes | — | Yes |
| Admin panel | No | No | No | Partial | Full |
| Moderation queue | No | No | No | Yes | Yes |
| User ban/suspend | No | No | No | No | Yes |
| Course publish | No | No | No | No | Yes |

Enforcement: NestJS `@Roles()` guards + resource ownership checks (`user_id` match).

---

## 4. Data Protection

### 4.1 Encryption

| State | Standard |
|-------|----------|
| In transit | TLS 1.3 minimum |
| At rest (Postgres) | AES-256 (provider-managed) |
| At rest (R2) | AES-256 server-side encryption |
| Secrets | Platform secret manager; never in git |

### 4.2 PII Inventory

| Data | Classification | Retention |
|------|----------------|-----------|
| Email | PII | Account lifetime + 30 days post-delete |
| Display name, bio | PII | Account lifetime |
| Birth year | Sensitive (COPPA) | Account lifetime |
| Portfolio/trades | User data | Account lifetime |
| AI conversations | User data | 90 days |
| IP logs | Technical | 30 days |

### 4.3 AI PII Scrubbing

Before sending prompts to OpenAI:
- Strip email, phone, SSN patterns
- Do not include auth tokens or internal IDs
- Truncate portfolio data to symbols and percentages in summaries (not raw account IDs)

---

## 5. COPPA Compliance (Children Under 13)

VirtuaQuest targets users 12+. Users under 13 require special handling under COPPA (US).

### Rules

| Rule | Implementation |
|------|----------------|
| Age collection | Birth year at registration |
| Under 13 block | Block registration OR require parental consent flow |
| **Recommended:** Parental consent | Verifiable parent email + consent checkbox + limited account |
| Data minimization | No public profile, no social, no leaderboard for under-13 |
| No behavioral ads | No ad tracking; no third-party marketing pixels |
| Parent access | Parent can review/delete child data |

### Under-13 Account Restrictions

- Private profile only
- No followers/following
- No public journal
- No competitions
- AI rate limits reduced
- No email marketing

---

## 6. FERPA Awareness (Classrooms) `[P2]`

When Classroom Mode launches:

- Student educational records isolated per classroom
- Teachers see only their rosters
- No advertising targeted to students
- Data processing agreement (DPA) for school licenses
- Export limited to completion metrics (not behavioral profiling)

---

## 7. Regulatory Disclaimers

### Required Disclaimer Text

**Primary (all trading/financial pages):**
> VirtuaQuest is an educational simulation platform. All trading uses simulated money. Nothing on this platform constitutes financial, investment, tax, or legal advice. Consult a licensed professional before making real financial decisions.

**AI-specific:**
> AI responses are generated for educational purposes and may contain errors. They are not financial advice.

**Personal finance tools `[P2]`:**
> Calculators and planners are illustrative only. Results are estimates, not guarantees.

### Placement

- Footer on all authenticated pages (collapsible)
- Trading confirmation modal checkbox
- AI chat input area
- Sign-up terms acceptance
- Company page analysis tab

---

## 8. Application Security

| Threat | Mitigation |
|--------|------------|
| SQL injection | Prisma parameterized queries |
| XSS | React auto-escape; CSP headers; sanitize markdown |
| CSRF | SameSite cookies; CSRF tokens on mutations |
| IDOR | Ownership checks on all user resources |
| Rate limiting | Redis sliding window per IP/user |
| DDoS | Cloudflare/Vercel edge protection |
| Dependency vulnerabilities | Dependabot; npm audit in CI |
| Secret exposure | Git secret scanning; pre-commit hooks |

### Content Security Policy (CSP)

```
default-src 'self';
script-src 'self' 'unsafe-inline' https://clerk.virtuaquest.com;
connect-src 'self' https://api.virtuaquest.com wss://api.virtuaquest.com;
img-src 'self' data: https:;
style-src 'self' 'unsafe-inline';
```

---

## 9. AI Safety & Compliance

| Control | Detail |
|---------|--------|
| No investment advice | System prompts + output filter |
| Citation requirement | Summaries must link sources |
| Logging | 90-day retention for quality and safety review |
| Human review | Admin queue for flagged content `[P1]` |
| Age-appropriate | Beginner mode vocabulary limits |
| Opt-out | User can delete AI conversation history |

See [05-AI_SYSTEM.md](./05-AI_SYSTEM.md) for full AI guardrails.

---

## 10. Audit Logging

**Table:** `audit_logs`

**Logged actions:**
- Admin: user ban, role change, course publish, manual XP adjustment
- Moderator: content removal, report resolution
- User: data export request, account deletion
- System: failed login spikes (aggregated)

**Fields:** actor_id, action, target_type, target_id, metadata, ip, created_at

**Retention:** 2 years

---

## 11. Incident Response

| Severity | Example | Response |
|----------|---------|----------|
| P1 | Data breach, auth bypass | Immediate patch; user notification within 72h |
| P2 | AI advice filter failure | Disable feature flag; hotfix |
| P3 | Rate limit bypass | Patch within 48h |

**Contact:** security@virtuaquest.com (document in internal runbook)

---

## 12. Privacy Rights

| Right | Implementation |
|-------|----------------|
| Access | Settings → Download my data `[P1]` |
| Correction | Profile edit |
| Deletion | Settings → Delete account (30-day soft delete) |
| Portability | JSON export of profile, portfolio, learning progress |
| Opt-out leaderboard | Settings toggle `[P1]` |

### Data Export Contents

- User profile
- Portfolios, orders, transactions
- Learning progress, quiz scores
- AI conversation history (if requested)
- XP and achievements

---

## 13. Third-Party Processors

| Vendor | Purpose | Data Shared |
|--------|---------|-------------|
| Clerk | Auth | Email, name |
| OpenAI | AI | Scrubbed prompts |
| Finnhub / AV / FMP | Market data | Symbol queries only |
| Vercel | Hosting | Request logs |
| Sentry | Errors | Stack traces (PII scrubbed) |
| Cloudflare R2 | Storage | Avatars, media |

DPAs required before production for all processors handling PII.

---

## 14. Security Checklist (Pre-Launch MVP)

- [ ] TLS enforced; HSTS enabled
- [ ] Auth provider configured with email verification
- [ ] RBAC guards on all admin routes
- [ ] Rate limiting on auth and AI endpoints
- [ ] CSP headers configured
- [ ] Secrets in environment manager (not git)
- [ ] Dependency audit passing in CI
- [ ] Disclaimers on trading and AI pages
- [ ] COPPA age gate implemented
- [ ] Audit log for admin actions
- [ ] Sentry PII scrubbing enabled
- [ ] Database backups automated (daily)

---

## 15. Future Compliance

| Regulation | When | Action |
|------------|------|--------|
| SOC 2 Type II | Scale / enterprise sales | Formal audit |
| GDPR | EU users | DPA, cookie consent, data residency option |
| PCI DSS | If billing added | Stripe (PCI scope reduction) |
| SEC/FINRA | If real brokerage | Separate legal entity; not current scope |
