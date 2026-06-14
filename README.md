# B2B Compliance SaaS

A comprehensive B2B compliance management platform for processing XML and PDF documents with automated reconciliation rules, multi-tenant isolation, and Stripe-based subscription billing.

---

## Business Problem Solved

**The Challenge:** Enterprise compliance teams process thousands of financial documents (XML/PDF) monthly, manually reconciling transactions against complex business rules. This manual process is error-prone, time-consuming, and scales poorly as document volume grows.

**Our Solution:** B2B Compliance SaaS automates the entire compliance workflow:

1. **Massive File Processing:** Handles 500+ MB XML/PDF documents using streaming parsers with O(1) memory footprint, processing files that would crash traditional parsers.

2. **Automated Reconciliation:** Evaluates 170+ configurable reconciliation rules against parsed transactions, flagging violations by severity level with exact amount deltas.

3. **Multi-Tenant Security:** Row-Level Security (RLS) enforced at the database layer ensures complete data isolation between tenants—no cross-tenant data leakage possible.

4. **Real-Time Visibility:** React dashboard provides instant access to document status, violation summaries, and drill-down analytics with pagination.

5. **Subscription Management:** Full Stripe integration with trial periods, plan upgrades/downgrades with proration, webhooks for lifecycle events, and cancellation flows.

**Key Differentiators:**
- Streaming XML parser handles documents 10x larger than competitors
- Celery workers on separate queues (heavy/light) prevent dashboard blocking
- Repository pattern with mandatory tenant filtering at every query path
- Batch-inserted transactions with chunked commits for database efficiency

---

## Getting Started

### Prerequisites

- Python 3.11+
- PostgreSQL 14+
- Redis 6+
- Node.js 18+ (for dashboard)
- AWS S3 bucket (for document storage)
- Stripe account (for billing)

### Environment Setup

