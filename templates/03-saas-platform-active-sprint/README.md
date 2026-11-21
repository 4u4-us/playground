# <p align="center"><img src="https://api.iconify.design/lucide/layers.svg?color=%230ea5e9" width="40" height="40" align="center" /> Modern B2B SaaS Platform — Production Architecture</p>

<p align="center">
  <strong>Multi-Tenant Microservices, AI Reasoning Engine, Next.js 15 App Router & Stripe Metered Billing</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Project_Key-%23PRO--3-0ea5e9?style=for-the-badge" alt="PRO-3" />
  <img src="https://img.shields.io/badge/Frontend-Next.js_15_App_Router-000000?style=for-the-badge&logo=next.js" alt="Next.js" />
  <img src="https://img.shields.io/badge/Language-TypeScript_5.6-3178c6?style=for-the-badge&logo=typescript" alt="TS" />
  <img src="https://img.shields.io/badge/Database-PostgreSQL_%2B_pgvector-336791?style=for-the-badge&logo=postgresql" alt="Postgres" />
  <img src="https://img.shields.io/badge/Billing-Stripe_Subscriptions-635bff?style=for-the-badge&logo=stripe" alt="Stripe" />
  <img src="https://img.shields.io/badge/AI_Engine-Gemini_1.5_Pro-8b5cf6?style=for-the-badge&logo=google" alt="AI" />
</p>

---

## ⚡ Architectural Principles & System Design

This project represents the core architecture of a high-growth **B2B SaaS application**. Built for horizontal scalability, sub-second latency, and strict enterprise multi-tenancy:

* **Zero-Leakage Multi-Tenancy:** Hardened Row-Level Security (RLS) on PostgreSQL enforced at the database kernel level.
* **Hybrid Rendering:** React Server Components (RSC) for maximum SEO and sub-100ms first-contentful paint, combined with client-side interactive canvas components.
* **Vector Semantic Search:** High-performance similarity searches via `pgvector` enabling AI contextual memory and rag pipelines.
* **Metered Credit System:** Token-bucket rate limiting backed by Upstash Redis with real-time usage synchronization to Stripe Billing v3.

---

## 🏛️ End-to-End Cloud Topology

```mermaid
flowchart TD
    subgraph Clients ["💻 Client Surface"]
        Web["Next.js Web Client (Desktop / Mobile)"]
        CLI["Developer CLI & Public REST APIs"]
    end

    subgraph EdgeLayer ["🌐 Edge Routing, Security & Auth"]
        Edge["Vercel Edge Network / Cloudflare Workers"]
        AuthService["Clerk / Supabase Authentication (JWT)"]
        RateLimiter["Upstash Redis (Token-Bucket Limiter)"]
    end

    subgraph ApplicationCore ["⚡ Next.js 15 App Core Tier"]
        RSC["React Server Components (SSR / RSC)"]
        ServerActions["Server Actions (Mutations & Graph Engine)"]
        AIOrchestrator["AI Orchestrator (Prompt Pipeline & Structured Outputs)"]
        BillingEngine["Stripe Usage Metering & Webhook Dispatcher"]
    end

    subgraph DataTier ["🗄️ Persistence & Storage"]
        Postgres[("PostgreSQL 16 Cluster
(Multi-Tenant with RLS)")]
        VectorStore[("pgvector Extension
(HNSW Index for Embeddings)")]
        RedisCache[("Redis 7.2 Cluster
(Distributed Lock & Ephemeral State)")]
        S3Files[("Cloudflare R2 / S3
(Encrypted Project Attachments)")]
    end

    Web & CLI --> Edge
    Edge --> AuthService --> RateLimiter
    RateLimiter --> RSC & ServerActions
    ServerActions --> AIOrchestrator & BillingEngine
    ServerActions & AIOrchestrator --> Postgres & VectorStore
    ServerActions --> RedisCache & S3Files
```

---

## 🤖 Contextual AI Inference & Usage Metering Flow

```mermaid
sequenceDiagram
    autonumber
    actor Dev as 👩‍💻 Software Architect
    participant Client as 4u4.us Web App
    participant Server as Server Action (App Core)
    participant Redis as Redis Quota Gate
    participant DB as Postgres + pgvector
    participant AI as Gemini 1.5 Pro API
    participant Stripe as Stripe Metering API

    Dev->>Client: 1. Prompt: "Expand Architecture with Auth0, Redis & Stripe"
    Client->>Server: 2. executeAiNodeExpansion(projectId, prompt)
    Server->>Redis: 3. Verify organization token balance (Token-Bucket)
    Redis-->>Server: 4. Quota approved (Remaining: 84 tokens)
    Server->>DB: 5. Query contextual embeddings: SELECT ... ORDER BY embedding <=> query_vec LIMIT 5
    DB-->>Server: 6. Return existing project nodes, epics and dependencies
    Server->>AI: 7. POST /v1beta/models/gemini-1.5-pro:generateContent (with Structured Schema)
    AI-->>Server: 8. Return JSON payload (validated nodes, coordinates, estimates, descriptions)
    Server->>DB: 9. Atomically commit new nodes & edges in PostgreSQL transaction
    Server->>Stripe: 10. Report 1 token consumption to Stripe Metered Billing endpoint
    Server-->>Client: 11. Stream visual node updates to interactive React Flow canvas
```

---

## 🗂️ Active Sprints & Engineering Roadmap

### Sprint 1: Cloud Foundation, Database Schema & Authentication (Nov 20 – Dec 14, 2026)
* **Goal:** Deliver production-ready multi-tenant infrastructure, tenant isolation, and user authentication.

| Task Key | Technical Deliverable | Stack | Estimate | Status |
|----------|-----------------------|-------|----------|--------|
| [#TSK-S1](https://github.com/4u4-us/playground/issues) | Setup Next.js 15 Monorepo with Turborepo & Tailwind | Next.js / TS | 10h | Done |
| [#TSK-S2](https://github.com/4u4-us/playground/issues) | Configure PostgreSQL with RLS & Drizzle ORM Schema | Postgres / Drizzle | 14h | Done |
| [#TSK-S3](https://github.com/4u4-us/playground/issues) | Multi-Tenant Organization Switcher & JWT Middleware | Clerk / Next.js | 12h | Done |
| [#TSK-S4](https://github.com/4u4-us/playground/issues) | Upstash Redis Distributed Rate Limiting Configuration | Upstash / Redis | 8h | In Progress |
| [#TSK-S5](https://github.com/4u4-us/playground/issues) | GitHub Actions CI/CD Pipeline with Automated E2E | Playwright | 10h | In Progress |

---

### Sprint 2: Core APIs, Design System & AI Reasoning Engine (Dec 15, 2026 – Jan 18, 2027)
* **Goal:** Deploy the interactive architecture canvas, vector embeddings search, and Gemini reasoning engine.

| Task Key | Technical Deliverable | Stack | Estimate | Status |
|----------|-----------------------|-------|----------|--------|
| [#TSK-S6](https://github.com/4u4-us/playground/issues) | Interactive React Flow Canvas with Custom Node Renderers | React Flow / Canvas | 16h | In Progress |
| [#TSK-S7](https://github.com/4u4-us/playground/issues) | Integrate Gemini 1.5 Pro with Zod Schema Validation | Google GenAI SDK | 14h | In Progress |
| [#TSK-S8](https://github.com/4u4-us/playground/issues) | Setup pgvector HNSW Index for Architecture Semantic Search | pgvector | 10h | To Do |
| [#TSK-S9](https://github.com/4u4-us/playground/issues) | Real-Time Collaborative WebSockets with Yjs | Yjs / WebSockets | 18h | To Do |
| [#TSK-S10](https://github.com/4u4-us/playground/issues) | REST API v1 Endpoints (/api/v1/projects, /api/v1/nodes) | Next.js Route Handlers | 12h | To Do |

---

### Sprint 3: Monetization, Hardening & Enterprise Launch (Jan 19 – Feb 19, 2027)
* **Goal:** Deploy Stripe customer portal, usage-based invoicing, team RBAC, and SOC 2 security compliance.

| Task Key | Technical Deliverable | Stack | Estimate | Status |
|----------|-----------------------|-------|----------|--------|
| [#TSK-S11](https://github.com/4u4-us/playground/issues) | Stripe Billing v3 Subscriptions & Webhook Processing | Stripe SDK | 14h | To Do |
| [#TSK-S12](https://github.com/4u4-us/playground/issues) | Usage-Based Invoicing & Real-Time Token Tracking UI | React / Server Actions | 10h | To Do |
| [#TSK-S13](https://github.com/4u4-us/playground/issues) | Team RBAC Permissions (Owner, Admin, Member, Viewer) | Drizzle / Postgres RLS | 12h | To Do |
| [#TSK-S14](https://github.com/4u4-us/playground/issues) | Audit Log Streaming & Security Event Tracking | Datadog / OpenTelemetry | 8h | To Do |

---

## 🔒 Multi-Tenant Database Schema with Row-Level Security (RLS)

```sql
-- Enable Row Level Security (RLS) on primary tables
ALTER TABLE "projects" ENABLE ROW LEVEL SECURITY;
ALTER TABLE "nodes" ENABLE ROW LEVEL SECURITY;

-- Enforce strict organization isolation
CREATE POLICY tenant_project_isolation ON "projects"
  FOR ALL
  USING (organization_id = current_setting('app.current_org_id', true));

CREATE POLICY tenant_node_isolation ON "nodes"
  FOR ALL
  USING (organization_id = current_setting('app.current_org_id', true));
```

---
*Generated automatically by [4u4.us](https://4u4.us) — Visual Agile for Modern Engineering Armies.*
