# Specification: B2B Compliance SaaS

## 1. Project Overview

**Project:** B2B Compliance SaaS — XML/PDF Processing + Rules Engine
**GitHub Repo:** https://github.com/9KMan/JOB-20260614072336-000095
**Lead:** https://www.upwork.com/jobs/~022065867000701624462
**Tier:** EXPERT
**Budget:** EUR 4,000 (fixed-price, 6 milestones)
**Timeline:** 6 milestones

## 2. Technical Stack

- **Backend:** Python + FastAPI
- **Frontend:** React + TypeScript
- **Database:** PostgreSQL
- **Queue:** Celery + Redis (separate heavy/light queues)
- **Storage:** AWS S3
- **Auth:** Supabase or Auth0 (behind adapter module)
- **Payments:** Stripe (trial, plans, proration, webhooks, cancellation)
- **Hosting:** AWS or Azure

## 3. Architecture

### Core Processing Pipeline
```
Upload → FastAPI → S3 → Celery Worker → Streaming Parser → Rules Engine → PostgreSQL
                                                                              ↓
                                                              React Dashboard ← REST API
                                                                              ↓
                                                                  Stripe Billing
```

### Key Architecture Decisions
- **Streaming XML Parser (lxml iterparse):** O(1) memory, handles 500+ MB files per document type
- **Two distinct XML formats** per document type (documented in spec workbooks)
- **PDF parsing:** Extract embedded XML, route to same rules engine
- **Celery heavy queue:** Large file parsing (500+ MB)
- **Celery light queue:** Dashboard queries (prevents large jobs blocking UI)
- **170 reconciliation rules:** Rule evaluator classes with configurable thresholds
- **Multi-tenant PostgreSQL:** RLS + repository pattern with mandatory tenant filter
- **Stripe billing:** Trial → plan → proration → webhooks → cancellation

## 4. Data Model

### Core Entities
- **Tenant** — client account with data isolation
- **Document** — uploaded XML/PDF with tenant_id, type, period, version
- **Transaction** — parsed rows from XML, batch-inserted with chunked commits
- **RuleViolation** — document IDs, rule ID, amount delta, severity, tenant-scoped
- **Job** — Celery job tracking with parse_cursor for resumable parsing
- **Subscription** — Stripe customer + subscription state

### Tenant Isolation
- RLS at DB layer: `USING (tenant_id = current_setting('app.current_tenant')::uuid)`
- Repository pattern: tenant_id mandatory on every query path
- FastAPI dependency injection: `Depends(get_current_tenant)` on every route

## 5. Project Structure

```
app/
├── api/v1/
│   ├── documents.py     # upload, status, list
│   ├── rules.py         # rule config, thresholds
│   ├── violations.py    # drill-down results
│   ├── subscriptions.py # Stripe webhook handler
│   └── exports.py       # Excel/PDF export
├── core/
│   ├── auth.py          # Auth0/Supabase adapter
│   ├── db.py            # connection + RLS session
│   └── config.py        # env vars
├── models/             # SQLAlchemy models + RLS policies
├── repositories/       # tenant-scoped data access
├── services/
│   ├── parser.py        # streaming XML/PDF parsers
│   ├── rules_engine.py  # 170 reconciliation rule evaluators
│   └── celery_app.py   # Celery config, heavy/light queues
├── worker/
│   └── tasks.py         # parse_document, apply_rules
└── dashboard/
    ├── components/      # React dashboard components
    └── pages/           # upload, results, analytics
```

## 6. Milestones

| # | Milestone | Key Deliverable |
|---|---|---|
| M1 | Core API + Auth | FastAPI skeleton, Auth adapter, tenant model |
| M2 | **Payment Gate** | Live 500+ MB XML iterparse + memory profile proof |
| M3 | XML/PDF Parsing | Both XML formats, PDF extraction, S3 integration |
| M4 | Rules Engine | 170 rules, configurable thresholds, violation records |
| M5 | Dashboard + Exports | React dashboard, pagination, Excel/PDF export |
| M6 | Stripe + Polish | Subscriptions, webhooks, 70% coverage, static analysis |

## 7. Acceptance Criteria

- [ ] Milestone 2 memory gate: 500+ MB XML parses with <200MB resident memory
- [ ] All 170 reconciliation rules implemented and individually testable
- [ ] Multi-tenant RLS enforced at DB layer — no cross-tenant data leakage
- [ ] Stripe trial → paid flow end-to-end
- [ ] 70% test coverage on business logic
- [ ] Static analysis: zero critical findings
