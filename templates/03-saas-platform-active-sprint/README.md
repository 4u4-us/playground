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


---

## 🗺️ Interactive MindMap Graph (Generated 1:1 from `mindmap.json`)

> **Dynamic Architecture Decomposition:** The native Mermaid diagram below is generated programmatically from the `mindmap.json` data tree. Each branch represents an architecture epic, and terminal leaves represent actionable agile user stories and tasks.

```mermaid
mindmap
  root(("SaaS Platform & Active Sprint"))
    ["EPIC-1 Cloud Architecture & Infrastructure Foundation"]
      ("Provision multi-region VPC and Kubernetes clusters via Terraform • 6h")
      ("Configure managed PostgreSQL with read replicas and automated backups • 4h")
      ("Deploy distributed Redis caching and session layer • 3h")
      ("Build GitHub Actions CI/CD pipelines with preview environments • 4h")
    ["EPIC-2 Multi-Tenant Identity, RBAC & Security"]
      ("Implement SSO SAML 2.0 / OIDC and OAuth2 authentication • 5h")
      ("Enforce PostgreSQL Row-Level Security RLS for tenant isolation • 4h")
      ("Design granular RBAC permission matrix Owner, Admin, Editor, Viewer • 4h")
      ("Build Personal Access Token PAT and scoped API key manager • 3.5h")
    ["EPIC-3 Core API Gateway & Event-Driven Services"]
      ("Deploy versioned REST/GraphQL API Gateway with per-tenant rate limiting • 5h")
      ("Set up event-driven message broker and dead-letter queues • 4h")
      ("Build outbound webhook engine with HMAC signing and exponential backoff • 5h")
      ("Implement idempotent background worker fleet for async processing • 4h")
    ["EPIC-4 Next-Gen Web Dashboard & Design System 2.0"]
      ("Publish Design System 2.0 tokens and accessible UI component library • 6h")
      ("Implement real-time multiplayer state synchronization via WebSockets • 6h")
      ("Build customizable executive analytics dashboard and KPI widgets • 5h")
      ("Implement global Command Palette Cmd+K and keyboard shortcuts • 3h")
    ["EPIC-5 AI Copilot & Workflow Automation Layer"]
      ("Build encrypted BYOK vault for personal and team LLM API keys • 4h")
      ("Implement multi-LLM priority router with automatic failover • 4.5h")
      ("Build streaming AI Copilot sidebar with workspace context injection • 5h")
      ("Deploy vector embeddings and RAG indexing pipeline • 5h")
    ["EPIC-6 Billing, Subscriptions & Usage-Based Metering"]
      ("Configure Stripe products and pricing tiers Free, Developer, Pro, Enterprise • 2.5h")
      ("Build resilient Stripe webhook handler for subscription lifecycle events • 4h")
      ("Implement real-time usage metering and overage reporting • 4h")
      ("Integrate self-serve Billing Portal and seat-based proration UI • 3h")
    ["EPIC-7 Observability, QA & Performance Hardening"]
      ("Instrument OpenTelemetry distributed tracing and APM dashboards • 4h")
      ("Configure PagerDuty/Slack incident alerting and SLO burn-rate monitors • 2h")
      ("Write comprehensive Playwright E2E test suite for critical user flows • 6h")
      ("Execute k6 load and stress testing at 5x projected launch traffic • 4h")
    ["EPIC-8 Go-To-Market, Beta Rollout & Production Launch"]
      ("Launch zero-signup interactive sandbox demo environment • 4h")
      ("Publish v2.0 documentation hub, changelog, and migration guides • 5h")
      ("Onboard 50 design-partner accounts to Private Beta cohort • 3h")
      ("Launch public Status Page and SLA uptime reporting • 1.5h")
```

---

## 🌳 Epic Hierarchy & Status Flow (from `mindmap.json`)

> **Real-Time Agile Pipeline:** Nodes are color-coded based on live completion status (<span style="color:#059669">■</span> Done, <span style="color:#d97706">■</span> In Progress, <span style="color:#dc2626">■</span> To Do).

```mermaid
flowchart LR
    %% Theme & Node Styles
    classDef primary fill:#8b5cf6,stroke:#7c3aed,stroke-width:2.5px,color:#ffffff,font-weight:bold;
    classDef done fill:#059669,stroke:#047857,stroke-width:1.5px,color:#ffffff;
    classDef inprog fill:#d97706,stroke:#b45309,stroke-width:1.5px,color:#ffffff;
    classDef todo fill:#dc2626,stroke:#b91c1c,stroke-width:1.5px,color:#ffffff;

    Root["🚀 <b>SaaS Platform & Active Sprint</b><br/>#PRO-Architecture"]:::primary

    E_1["<b>#EPIC-1: Cloud Architecture & Infrastructure Foundation</b><br/>6 Tasks • 45h • 🟡 In Progress"]:::inprog
    Root --> E_1
    E_2["<b>#EPIC-2: Multi-Tenant Identity, RBAC & Security</b><br/>6 Tasks • 47h • 🔴 To Do"]:::todo
    Root --> E_2
    E_3["<b>#EPIC-3: Core API Gateway & Event-Driven Services</b><br/>6 Tasks • 51h • 🔴 To Do"]:::todo
    Root --> E_3
    E_4["<b>#EPIC-4: Next-Gen Web Dashboard & Design System 2.0</b><br/>6 Tasks • 55h • 🔴 To Do"]:::todo
    Root --> E_4
    E_5["<b>#EPIC-5: AI Copilot & Workflow Automation Layer</b><br/>6 Tasks • 52h • 🔴 To Do"]:::todo
    Root --> E_5
    E_6["<b>#EPIC-6: Billing, Subscriptions & Usage-Based Metering</b><br/>6 Tasks • 36h • 🔴 To Do"]:::todo
    Root --> E_6
    E_7["<b>#EPIC-7: Observability, QA & Performance Hardening</b><br/>6 Tasks • 46h • 🔴 To Do"]:::todo
    Root --> E_7
    E_8["<b>#EPIC-8: Go-To-Market, Beta Rollout & Production Launch</b><br/>6 Tasks • 43h • 🔴 To Do"]:::todo
    Root --> E_8
```

---

## ⏱️ Dynamic Agile Gantt Timeline (from `mindmap.json`)

> **Continuous Scheduling Engine:** Auto-projected task start dates, durations (derived from estimated hours), and critical execution paths.

```mermaid
gantt
    title SaaS Platform & Active Sprint — Execution Timeline
    dateFormat YYYY-MM-DD
    axisFormat %b %d

    section EPIC-1 Cloud Architecture & Infrastructure Foundation
    Provision multi-region VPC and Kubernetes clusters via Terraform :done, 2026-11-20, 1d
    Configure managed PostgreSQL with read replicas and automated backups :active, 2026-11-24, 1d
    Deploy distributed Redis caching and session layer :2026-11-26, 1d
    section EPIC-2 Multi-Tenant Identity, RBAC & Security
    Implement SSO SAML 2.0 / OIDC and OAuth2 authentication :2026-11-27, 1d
    Enforce PostgreSQL Row-Level Security RLS for tenant isolation :2026-12-01, 1d
    Design granular RBAC permission matrix Owner, Admin, Editor, Viewer :2026-12-04, 1d
    section EPIC-3 Core API Gateway & Event-Driven Services
    Deploy versioned REST/GraphQL API Gateway with per-tenant rate limiting :2026-12-07, 1d
    Set up event-driven message broker and dead-letter queues :2026-12-11, 1d
    Build outbound webhook engine with HMAC signing and exponential backoff :2026-12-16, 1d
    section EPIC-4 Next-Gen Web Dashboard & Design System 2.0
    Publish Design System 2.0 tokens and accessible UI component library :2026-12-14, 1d
    Implement real-time multiplayer state synchronization via WebSockets :2026-12-21, 1d
    Build customizable executive analytics dashboard and KPI widgets :2026-12-28, 1d
    section EPIC-5 AI Copilot & Workflow Automation Layer
    Build encrypted BYOK vault for personal and team LLM API keys :2026-12-28, 1d
    Implement multi-LLM priority router with automatic failover :2027-01-04, 1d
    Build streaming AI Copilot sidebar with workspace context injection :2027-01-08, 1d
    section EPIC-6 Billing, Subscriptions & Usage-Based Metering
    Configure Stripe products and pricing tiers Free, Developer, Pro, Enterprise :2027-01-06, 1d
    Build resilient Stripe webhook handler for subscription lifecycle events :2027-01-09, 1d
    Implement real-time usage metering and overage reporting :2027-01-13, 1d
    section EPIC-7 Observability, QA & Performance Hardening
    Instrument OpenTelemetry distributed tracing and APM dashboards :2027-01-18, 1d
    Configure PagerDuty/Slack incident alerting and SLO burn-rate monitors :2027-01-21, 1d
    Write comprehensive Playwright E2E test suite for critical user flows :2027-01-26, 1d
```

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
