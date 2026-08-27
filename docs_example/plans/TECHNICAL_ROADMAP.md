# Roots Off - Bug Root Cause Analyzer: Technical Roadmap

**Last Updated**: January 21, 2026
**Status**: ✅ **BETA-READY** | Production deployment pending final validation

## Current Status (December 2025)

**🎉 Recent Completions (Latest Development Cycle):**
- **Orchestration Service**: Centralized scan lifecycle management and pipeline coordination
- **Internal Concurrency**: Sub-second parallel message processing in Redis stream consumers
- **Service Standardization**: Unified microservice naming (underscores) across Terraform, scripts, and logs
- **Rate Limiting**: Per-tenant, per-endpoint rate limiting with sliding window algorithm
- **Audit Logging**: Complete audit logging system with middleware and API endpoints
- **Deployment Scripts**: Hardened release and smoke-test scripts for multi-service environments
- **Staging Removal**: Streamlined cloud environments to Development and Production only
- **Langfuse Tracing**: Enhanced safety, automatic thread propagation, and async context support
- **LLM Quota/API Error Clean Stop**: Proactive detection of quota exhaustion/invalid API keys across all services, triggering immediate scan failure with descriptive guidance shown in the Forge UI.

**✅ Implemented Components:**
- Backend pipeline (7-phase architecture: Ingestion → Triage → Analysis → Aggregation → Reporting → Integration → Orchestration)
- Forge app UI (Issue Panel, Project Page, Settings Page with Jira Integration)
- VCS integration (GitHub, GitLab, Bitbucket, Perforce)
- Multi-tenant architecture with tenant isolation
- Credential management (HashiCorp Vault + AWS Secrets Manager)
- Generic settings system with Redis caching
- Complete report generation (HTML + JSON)
- Epic creation from insights (async via Integration service)
- Jira write-back (configurable RCA sync to tickets)
- **Rate limiting middleware** (per-tenant, per-endpoint with sliding window algorithm)
- **Audit logging system** (middleware + API endpoints)
- **GDPR compliance endpoints** (audit log export and deletion + user data management)
- **Security headers middleware** (HSTS, CSP, X-Frame-Options, etc.)
- **Metrics middleware** and Prometheus endpoint
- **Forge authentication middleware** with JWKS validation
- **Tenant context middleware** for cloudId mapping
- **Scan history API** (`GET /api/v1/scans/` with pagination)

**🚀 Deployment Status:**
- Development environment: ✅ Deployed and functional
- Production environment: ⏳ **READY** - Pending manual terraform apply and smoke tests

**⏳ Priority Enhancements:**
- **Caching layer** - ✅ Implemented for API endpoints (scan status, ticket RCA, settings)
- **Source data caching** - Cache JIRA ticket data, VCS diffs, HTML reports
- **On-prem VCS Agent** - Support for Perforce/VCS behind firewalls (beta phase)
- **Advanced UI features** - Charts, filters, bulk operations, trend visualization
- **Monitoring expansion** - Prometheus/Grafana (configured), PostHog (configured)

---

## Architecture Overview

The system follows a 6-phase pipeline designed for scale and deep analysis coverage:

1.  **Phase 1: Ingestion** (High Volume Fetching & Queueing)
2.  **Phase 2: Triage** (Classification & Filtering - Optional, can bypass)
3.  **Phase 3: Deep Analysis** (Root Cause Analysis with VCS Integration)
4.  **Phase 4: Aggregation** (Embedding, Clustering & Insight Generation)
5.  **Phase 5: Reporting** (HTML Report Generation & Storage)
6.  **Phase 6: Integration** (Jira Sync & Epic Creation)
7.  **Phase 7: Orchestration** (Scan Lifecycle & Completion Detection)

```mermaid
flowchart LR
    %% Color Legend for Components
    classDef entry fill:#4CAF50,stroke:#388E3C,stroke-width:2px,color:#fff
    classDef service fill:#00BCD4,stroke:#0097A7,stroke-width:2px,color:#000
    classDef agent fill:#9C27B0,stroke:#7B1FA2,stroke-width:2px,color:#fff
    classDef external fill:#FF9800,stroke:#F57C00,stroke-width:2px,color:#000
    classDef storage fill:#2196F3,stroke:#1976D2,stroke-width:2px,color:#fff
    classDef output fill:#FFEB3B,stroke:#FBC02D,stroke-width:2px,color:#000

    %% PHASE 1: Entry Point
    API[🌐 API Gateway<br/>FastAPI + Kong]:::entry

    %% PHASE 2: Ingestion
    ING[📥 Ingestion Service<br/>Fetches tickets]:::service
    JIRA[(Jira API)]:::external

    %% PHASE 3: Triage (Optional)
    TRI[🔍 Triage Service<br/>LLM Classification]:::service

    %% PHASE 4: Deep Analysis
    RCA[🔬 RCA Agent<br/>Root Cause Analysis]:::agent
    VCS[(VCS APIs<br/>GitHub/GitLab/etc)]:::external

    %% PHASE 5: Aggregation
    AGG[🧮 Aggregation Service<br/>Embeddings & Clustering]:::service
    INS[💡 Insight Agent<br/>Pattern Detection]:::agent

    %% PHASE 6: Reporting
    RPT[📊 Report Generator<br/>HTML Creation]:::service

    %% PHASE 7: Integration (Async)
    INT[🔄 Integration Service<br/>Jira Write-back]:::service
    EPIC[📋 Epic Service<br/>Epic Creation]:::agent
    SYNC[🔗 Jira Sync<br/>Update Tickets]:::agent

    %% Storage & Infrastructure
    DB[(PostgreSQL<br/>+ pgvector)]:::storage
    S3[(S3/MinIO<br/>Reports)]:::storage

    %% Main Pipeline Flow (Left to Right)
    API -->|1. Scan Request| ING
    ING -->|Fetch Tickets| JIRA
    ING -->|Redis Stream:<br/>ingestion_queue| TRI
    ING -.->|Optional Bypass<br/>BYPASS_TRIAGE=true| RCA

    TRI -->|Redis Stream:<br/>rca_queue| RCA
    RCA -->|Fetch Code Diffs| VCS
    RCA -->|Stores RCA Results| DB

    RCA -->|Redis Stream:<br/>aggregation_queue| AGG
    AGG -->|Generate Embeddings| DB
    AGG --> INS
    INS -->|Stores Insights| DB

    INS -->|Redis Stream:<br/>reports_queue| RPT
    RPT -->|Upload HTML| S3
    RPT -->|Store Metadata| DB

    %% Async Integration Flow
    INS -.->|Redis Stream:<br/>integration_queue| INT
    API -.->|Epic Request<br/>Async| INT
    INT --> EPIC
    INT --> SYNC
    EPIC -->|Create Epics| JIRA
    SYNC -->|Update Fields<br/>& Comments| JIRA

    %% Visual Grouping with Subgraphs
    subgraph Phase1[" 1️⃣ INGESTION "]
        ING
        JIRA
    end

    subgraph Phase2[" 2️⃣ TRIAGE (Optional) "]
        TRI
    end

    subgraph Phase3[" 3️⃣ DEEP ANALYSIS "]
        RCA
        VCS
    end

    subgraph Phase4[" 4️⃣ AGGREGATION "]
        AGG
        INS
    end

    subgraph Phase5[" 5️⃣ REPORTING "]
        RPT
    end

    subgraph Phase6[" 6️⃣ INTEGRATION (Async) "]
        INT
        EPIC
        SYNC
    end

    subgraph Phase7[" 7️⃣ ORCHESTRATION "]
        ORC[🎭 Orchestration Service<br/>Lifecycle Management]:::service
    end

    subgraph Storage[" 💾 DATA LAYER "]
        DB
        S3
    end
```

**Key:**
- **Solid lines** (→): Main pipeline flow via Redis Streams
- **Dotted lines** (⇢): Optional/bypass paths or async operations
- **Cylinders**: External APIs and data stores
- **Colors**: Green=Entry, Cyan=Services, Purple=AI Agents, Orange=External APIs, Blue=Storage, Yellow=Output

**Infrastructure Components:**
- **Kong Gateway**: API routing, auth, rate limiting (port 8000) - Local dev only
- **Application Load Balancer**: HTTPS, WAF, routing in production
- **Redis Streams**: Message queue between all services
- **MinIO (S3)**: Raw ticket JSON + HTML reports
- **PostgreSQL + pgvector**: Scans, RCAs, embeddings, insights
- **Langfuse**: LLM observability and cost tracking (self-hosted on ops server)
- **HashiCorp Vault**: Production secrets management with AWS KMS auto-unseal

## Deployment Architecture

### Local Development
```
docker-compose.yml (app services) + docker-compose.infra.yml (infrastructure)
├── 9 Application Services (api, ingestor, triage, deep_analysis, aggregation, reporting, integration, audit_worker, orchestration)
├── PostgreSQL (Docker)
├── Redis (Docker)
├── MinIO (S3 alternative)
├── Vault (secrets)
├── Kong (API gateway)
├── LangFuse + Prometheus + Grafana (monitoring)
```

### Production (AWS)
```
ECS Fargate (app services) + Terraform-managed infrastructure
├── 9 ECS Services (2 tasks each, auto-scaling ready)
├── RDS PostgreSQL (multi-AZ, encrypted)
├── ElastiCache Redis (multi-node, encrypted)
├── S3 Buckets (encrypted)
├── HashiCorp Vault (ops server, AWS KMS auto-unseal)
├── Application Load Balancer + WAF (replaces Kong)
├── Ops Server (t3.xlarge, Ubuntu 22.04)
    └── LangFuse + Prometheus + Grafana + Vault (Docker Compose)
```

**Services & Scaling (Load Test Optimized):**

> See [Load Testing Guide](../LOAD_TESTING.md) for methodology and how to run tests.

| Service | Local Replicas | CPU Limit | Memory Limit | CPU Reserve | Memory Reserve | Notes |
|---------|---------------|-----------|--------------|-------------|----------------|-------|
| API | 1 | 1.0 | 512M | 0.5 | 256M | Exposed via ALB |
| Ingestor | 2 | 1.0 | 512M | 0.5 | 256M | Background worker |
| Triage | 2 | 1.0 | 512M | 0.5 | 256M | Optional (BYPASS_TRIAGE) |
| Deep Analysis | 12 | 1.5 | 768M | 1.0 | 512M | LLM-heavy, scaled for throughput |
| Aggregation | 5 | 0.75 | 512M | 0.5 | 256M | Embedding + clustering |
| Reporting | 1 | 0.5 | 384M | 0.25 | 192M | HTML generation |
| Integration | 1 | 0.5 | 384M | 0.25 | 192M | Jira write-back |
| Audit Worker | 1 | 0.25 | 256M | 0.125 | 128M | S3 log archival |
| Orchestration | 1 | 0.5 | 384M | 0.25 | 192M | Lifecycle management |

**Total**: 9 service types, 27 local containers

**Production ECS**: 9 services × 2 tasks = 18 ECS tasks (auto-scaling configured)

## Technology Stack

**Frontend (Forge App - Implemented ✅)**
-   **Framework**: Atlassian Forge (UI Kit v3 - `@forge/react`)
-   **Components**:
    -   Issue Panel - Displays RCA for individual Bug tickets
    -   Project Page - Scan trigger, progress tracking, results display, epic creation, scan history
    -   Settings Page - Credential management for JIRA and VCS providers
-   **Authentication**: Forge OAuth JWT with JWKS validation
-   **API Integration**: Secure external fetch to backend API (`https://api.rootsoff.com`)
-   **Deployment**: Development environment active, staging available

**Backend Pipeline (Microservices - All Implemented ✅)**
-   **API Gateway**: **Kong Gateway** (local) / **Application Load Balancer + WAF** (production)
-   **API Service**: **FastAPI** (Python) - `src/api/` - Scan orchestration, status polling, report retrieval
-   **Ingestor Service**: Python worker - `src/ingestion/` - High throughput Jira fetching
-   **Triage Service**: Python worker - `src/triage/` - LLM classification (optional, can bypass)
-   **Deep Analysis Service**: Python worker - `src/deep_analysis/` - Root cause analysis with VCS integration
-   **Aggregation Service**: Python worker - `src/aggregation/` - Embeddings, clustering, insight generation
-   **Reporting Service**: Python worker - `src/reporting/` - HTML report generation
-   **Integration Service**: Python worker - `src/integration/` - Jira sync and Epic creation (async)
-   **Orchestration Service**: Python worker - `src/orchestration/` - Pipeline coordination and completion detection

**LLM Configuration**
-   **Primary Model**: GPT-5.1-nano ($0.05/1M input, $0.40/1M output)
-   **Triage**: GPT-5.1-nano (bypassed by default with BYPASS_TRIAGE=true)
-   **Deep Analysis**: GPT-5.1-nano
-   **Insight Generation**: GPT-5.1-nano
-   **Embeddings**: OpenAI text-embedding-ada-002 (1536 dimensions)
-   **Observability**: **Langfuse** for LLM tracing and cost monitoring (self-hosted)

**VCS Integration (Implemented ✅)**
-   **GitHub**: REST API via `GITHUB_TOKEN`
-   **GitLab**: REST API via `GITLAB_URL` + `GITLAB_TOKEN`
-   **Bitbucket Server**: REST API via `BITBUCKET_URL` + `BITBUCKET_TOKEN`
-   **Perforce**: HTTP REST API via `P4_PORT`, `P4_USER`, `P4_PASSWD`
-   **On-Prem Agent** (Beta): Containerized agent for VCS behind firewalls (see Agent Plan section)
-   Factory pattern in `src/deep_analysis/vcs/` supports adding new providers

**Data Storage (All Implemented ✅)**
-   **Message Queue**: **Redis Streams** - Inter-service communication with consumer groups
-   **Object Storage**: **MinIO** (local) / **S3** (production) - Raw ticket JSON + HTML reports
-   **Database**: **PostgreSQL 16 + pgvector** - Scans, RCAs, embeddings, insights, report metadata

**Multi-Tenant Architecture (Implemented ✅)**
-   **Tenant Isolation**: All data access filtered by `tenant_id` (extracted from Forge JWT tokens via `cloudId` mapping)
-   **Authentication**: Forge OAuth JWT validation with JWKS caching (5-minute TTL) via `ForgeAuthMiddleware`
-   **Tenant Mapping**: `TenantMappingService` maps Atlassian `cloudId` to internal `tenant_id`
-   **Tenant Context Middleware**: Extracts tenant_id from request state and maps cloudId when needed
-   **Credential Management**: Per-tenant credentials stored in HashiCorp Vault with encryption
-   **Rate Limiting**: Per-tenant, per-endpoint rate limiting using sliding window algorithm (`RateLimitMiddleware`)
-   **Audit Logging**: All tenant actions logged with tenant context for compliance (`AuditLogMiddleware`)

**Security & Infrastructure (Implemented ✅)**
-   **HTTPS/TLS**: ACM certificate with automatic renewal
-   **Security Headers**: HSTS, CSP, X-Frame-Options, X-Content-Type-Options, X-XSS-Protection, Referrer-Policy (`SecurityHeadersMiddleware`)
-   **Error Sanitization**: Error messages sanitized to prevent information leakage (`ErrorSanitizationMiddleware`)
-   **WAF**: AWS WAF Web ACL protecting ALB endpoints
-   **Network Security**: VPC with private subnets, security groups, VPC endpoints
-   **Secrets Management**: HashiCorp Vault (production) with AWS KMS auto-unseal; fallback to AWS Secrets Manager for specific integrations
-   **Data Encryption**: ✅ At-rest encryption for RDS, S3, and Redis verified in Terraform

**Monitoring & Observability (Implemented ✅)**
-   **Langfuse**: LLM call tracing, token usage, cost monitoring (self-hosted with dedicated Postgres on ops server)
-   **Langfuse Tracing**: Enhanced with `ThreadingInstrumentor` and safer client calls to prevent "No active span" warnings.
-   **Prometheus/Grafana**: Metrics collection and visualization (deployed on ops server)
-   **Metrics Middleware**: Request metrics, rate limit metrics, error tracking (`MetricsMiddleware`)
-   **Prometheus Endpoint**: `/metrics` endpoint exposing Prometheus-formatted metrics
-   **CloudWatch**: Logs and metrics for all ECS services
-   **PostHog**: Product analytics and error tracking (configured, optional)
-   **Audit Logging**: Non-blocking audit event publishing to Redis Streams (`AuditLogMiddleware`)

**Caching Strategy (Implemented ✅)**
-   **Scan Status**: Redis cache with 10-second TTL (for frequent polling from Forge app)
-   **Ticket RCA**: Redis cache with 10-minute TTL (for Issue Panel loads)
-   **Tenant Settings**: Redis Hash cache with 1-hour TTL (read-through cache for settings)
-   **Cache Invalidation**: Event-driven invalidation when:
    - Scan status changes (running → generating_insights → completed/failed)
    - New RCA analysis completes for a ticket
    - Settings are updated (immediate cache update)
-   **Multi-tenant Isolation**: Cache keys include tenant ID for complete isolation
-   **Configuration**: `CACHE_ENABLED`, `CACHE_SCAN_STATUS_TTL`, `CACHE_TICKET_RCA_TTL` in `.env`

**Settings Management (Implemented ✅)**
-   **Generic Key-Value Store**: PostgreSQL table with dot-notation keys (e.g., `jira.sync.enabled`)
-   **Redis Caching**: Read-through cache with automatic invalidation on updates
-   **Jira Integration Settings**:
    - `jira.sync.enabled` - Enable/disable automatic write-back to Jira
    - `jira.sync.custom_field_id` - Custom field ID for root cause category
    - `jira.sync.add_comment` - Add RCA comment to tickets
-   **Future-Proof**: Easily extensible for new settings without schema changes

**Category System (Implemented ✅)**
-   **Primary Categories**: Unified, clean categories for filtering and JQL (e.g., "Logic Error")
-   **Secondary Categories**: Detailed LLM-generated categories for context (e.g., "Missing guard condition")
-   **Database Storage**: Both stored in `rca_results` table with separate columns
-   **Bulk Update**: After LLM unification, primary categories are updated via bulk SQL operation
-   **Report Display**: Primary category shown in filters/dropdowns, secondary shown in detail view
-   **JQL Integration**: Forge JQL function queries against primary categories

**Integration Service (Implemented ✅)**
-   **Async Processing**: Epic creation and Jira sync run asynchronously via Redis Streams
-   **Jira Write-back**: Configurable per-tenant feature to sync RCA results to tickets
    - Update custom fields with root cause category
    - Add formatted comments with RCA details
    - Settings managed via UI in Forge Settings Page
-   **Epic Creation**: Moved from API to async service for better scalability
-   **Idempotency**: Both operations are idempotent (safe to retry)

## Key Technical Decisions

1.  **Pipeline Architecture**
    -   **Decision**: 6-Phase Microservices Pipeline with Redis Streams.
    -   **Reason**: Decouples ingestion from analysis. Each phase can scale independently. Consumer groups enable reliable message processing with acknowledgment.
    -   **Status**: ✅ Fully implemented with graceful shutdown and restart recovery.

2.  **LLM Integration**
    -   **Decision**: Direct LLM calls with Pydantic structured outputs (no LangChain agents).
    -   **Reason**: Simpler, more predictable orchestration. Structured outputs via `response_format` provide type-safe RCA generation.
    -   **Status**: ✅ Implemented via `LLMFactory` supporting OpenAI and Gemini with automatic Langfuse tracing.

3.  **VCS Integration**
    -   **Decision**: Direct Code Repository Integration via Factory Pattern + On-Prem Agent for firewalled VCS.
    -   **Providers**: GitHub, GitLab, Bitbucket Server, Perforce (HTTP REST APIs + on-prem agent).
    -   **Reason**: RCA without code context is superficial. Fetching actual diffs from PRs/CLs provides concrete evidence.
    -   **Status**: ✅ Implemented in `src/deep_analysis/vcs/` with automatic reference extraction from Jira tickets.

4.  **Database Strategy**
    -   **Decision**: PostgreSQL + pgvector for unified storage.
    -   **Reason**: Single database for scans, RCAs, embeddings, insights, and report metadata. pgvector extension enables semantic similarity search on RCA embeddings.
    -   **Status**: ✅ Implemented with 1536-dimension vectors (OpenAI ada-002).

5.  **Message Queue Strategy**
    -   **Decision**: Redis Streams with Consumer Groups.
    -   **Reason**: Reliable message delivery with acknowledgments, automatic load balancing, pending message recovery on restart.
    -   **Status**: ✅ All services use `StreamConsumerMixin` for consistent stream handling.

6.  **Triage Bypass**
    -   **Decision**: Optional triage phase via `BYPASS_TRIAGE=true` (default).
    -   **Reason**: For curated JQL queries where all tickets are known to be valid bugs, triage adds latency without value.
    -   **Status**: ✅ Implemented with conditional queue routing.

7.  **Secrets Management**
    -   **Decision**: HashiCorp Vault as primary secrets store with AWS KMS auto-unseal.
    -   **Reason**: Enterprise-grade secrets management with encryption-as-a-service, audit logging, and rotation support.
    -   **Status**: ✅ Production-ready with documented setup procedures (`docs/VAULT_PROD_SETUP.md`).

## Redis Streams Data Flow

The pipeline uses Redis Streams with Consumer Groups for reliable message passing:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  rca:commands   │────▶│ ingestion_queue │────▶│   rca_queue     │
│   (API → Ing)   │     │  (Ing → Triage) │     │ (Triage → RCA)  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                              │ BYPASS_TRIAGE=true
                              └──────────────────────────────┐
                                                             ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  reports_queue  │◀────│aggregation_queue│◀────│   rca_queue     │
│ (Agg → Report)  │     │  (RCA → Agg)    │     │ (Triage → RCA)  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │
        │                       └──────────────────┐
        └──────────────────────────────────┐       │
                                           ▼       ▼
                                ┌─────────────────────────┐
                                │  integration_queue      │
                                │ (Agg/API → Integration) │
                                └─────────────────────────┘
```

**Stream Names:**
- `rca:commands` - Scan trigger commands from API
- `jira_ingestion_queue` - Tickets pending triage
- `jira_rca_queue` - Tickets pending deep analysis
- `jira_aggregation_queue` - RCAs pending embedding/insight generation
- `jira_reports_queue` - Scans pending HTML report generation
- `jira_integration_queue` - Tasks for Jira sync and Epic creation

## VCS On-Prem Agent Plan

**Goal**: Support customers with on-prem VCS (Perforce, self-hosted GitLab, etc.) behind firewalls without opening inbound ports.

**Architecture Summary:**
- Customer runs a small containerized Agent on-prem (pulls jobs, runs licensed CLI e.g., Perforce Helix CLI)
- Agent long-polls a secure job queue (SQS/Redis over TLS) for tasks (ticket IDs, file paths, revision specs)
- Agent performs local `p4 sync` or VCS fetch, sanitizes outputs, uploads artifacts to customer-owned S3 or main service via HTTPS with mutual TLS
- Agent authenticates with cloud service using short-lived credentials (customer-provisioned)
- Agent is configured to limit data exfiltration (whitelist paths, size limits, rate limits)

**Implementation Plan (Beta → Production):**
1. **Prototype** (2 weeks): Container with minimal `p4` client + SQS polling + upload to S3
2. **Harden** (2-3 weeks): Input sanitization, rate limits, size quotas, secure logging, credential rotation
3. **Beta Test** (2 weeks): Deploy to 1 customer, exercise Perforce syncs, validate latency & security
4. **Production**: Multi-tenant support, UI for agent provisioning, health checks & monitoring

**Security Constraints:**
- Sanitize VCS outputs (prevent code injection, PII leakage)
- Restrict arbitrary shell execution
- Use least privilege roles for S3 upload
- Require explicit customer opt-in
- Audit all agent operations

**Reference**: Stub code in `docs/TODO` (Perforce agent mock). Will be adapted into `on_prem_agent/` repo or `src/on_prem_agent/` module.

**Status**: Design complete, implementation scheduled for beta phase.

## Usage Assumptions & Cost Breakdown

### Pricing Tiers (December 2025)

| Plan       | Price/Month | Tickets/Month |
|------------|-------------|---------------|
| Trial      | Free        | 50            |
| Starter    | $99         | 350           |
| Mid Level  | $190        | 1,000         |
| Senior     | $290        | 2,000         |
| Enterprise | $390        | 5,000         |

### LLM Costs (GPT 5.1 nano)

**Model**: GPT-5.1-nano
**Pricing**: $0.05/1M input tokens, $0.40/1M output tokens

**Per-Ticket Cost (with 20% buffer):**
- Average tokens/ticket: ~10,000 (70% input / 30% output)
- Input: 7,000 tokens → $0.05 × 0.007 = $0.00035
- Output: 3,000 tokens → $0.40 × 0.003 = $0.00120
- Subtotal: $0.00155
- **With 20% buffer: ~$0.002/ticket**

---

### Phase 1: 5 Customers (Lowest Cost Setup)

**Revenue (conservative mix):**

| Tier        | Count | Revenue/mo | Tickets/mo |
|-------------|-------|------------|------------|
| Starter     | 2     | $198       | 700        |
| Mid Level   | 2     | $380       | 2,000      |
| Senior      | 1     | $290       | 2,000      |
| **Total**   | **5** | **$868**   | **4,700**  |

**Infrastructure (Minimal - 9 tasks):**

| Component | Spec | Cost/mo | Notes |
|-----------|------|---------|-------|
| EC2 ops   | t3.xlarge | $121 | Required for monitoring stack (9GB RAM) |
| ECS (9 tasks) | mixed CPU/mem | $95 | api(2), ingestor(2), deep(2), agg(1), reporting(1), int(1) |
| RDS       | db.t3.small, single-AZ, 20GB | $26 | 5 customers, light metadata |
| Redis     | cache.t4g.micro, 1 node | $12 | Queue/cache for low volume |
| S3        | ~1GB | $3 | Raw tickets + reports |
| ALB       | minimal traffic | $18 | Load balancing |
| NAT       | t3.nano instance | $5 | Cost-effective for Phase 1 |
| CloudWatch | logs & metrics | $5 | Basic monitoring |
| **Subtotal** | | **$285** | |
| **+4% buffer** | | **$296** | |

**LLM Costs**: 4,700 tickets × $0.002 = **$9/month**

**Phase 1 P&L:**
- Revenue: $868/month
- Infrastructure: $285/month
- LLM: $9/month
- **Net Profit: $574/month (66% margin)**

---

### Phase 2: 50 Customers (Scalable Setup)

**Revenue:**

| Tier        | Count | Revenue/mo | Tickets/mo |
|-------------|-------|------------|------------|
| Starter     | 25    | $2,475     | 8,750      |
| Mid Level   | 15    | $2,850     | 15,000     |
| Senior      | 7     | $2,030     | 14,000     |
| Enterprise  | 3     | $1,170     | 15,000     |
| **Total**   | **50**| **$8,525** | **52,750** |

**ECS Task Scaling (Local → Production):**

| Service | Local Replicas | Phase 2 Tasks | CPU (millicores) | Memory (MB) | Notes |
|---------|---------------|---------------|------------------|-------------|-------|
| api | 1 | 2 | 500 | 512 | Load balancing |
| ingestor | 2 | 2 | 500 | 512 | Ticket fetching |
| triage | 2 | 0 | 500 | 512 | Disabled (BYPASS_TRIAGE) |
| deep-analysis | 12 | 4 | 1000 | 768 | LLM bottleneck |
| aggregation | 5 | 3 | 500 | 512 | Clustering |
| reporting | 1 | 2 | 250 | 384 | Redundancy |
| integration | 1 | 2 | 250 | 384 | Jira sync |
| **Total** | **24** | **15** | | | |

**Why fewer ECS tasks**: Fargate auto-scales; local uses high parallelism for dev speed.

**Infrastructure (Scaled):**

| Component | Spec | Cost/mo | Scale-up from Phase 1 |
|-----------|------|---------|----------------------|
| EC2 ops   | t3.xlarge | $121 | No change (monitoring fixed cost) |
| ECS (15 tasks) | mixed CPU/mem | $277 | +6 tasks for scale |
| RDS       | db.t3.medium, multi-AZ, 100GB | $71 | Multi-AZ HA, larger instance |
| Redis     | cache.t4g.small × 2 | $47 | Cluster for reliability |
| S3        | ~10GB | $10 | More data storage |
| ALB       | moderate traffic | $25 | More health checks |
| NAT Gateway | production-grade | $33 | Better throughput vs instance |
| CloudWatch | increased usage | $15 | Detailed metrics |
| **Subtotal** | | **$599** | |

**LLM Costs**: 52,750 tickets × $0.002 = **$106/month**

**Phase 2 P&L:**
**Before Marketplace Fees:**
- Gross Revenue: $8,525/month
- Infrastructure: $599/month
- LLM: $106/month
- **Gross Profit: $7,820/month (92% margin)**

**After Atlassian Marketplace (15%):**
- Marketplace Fee: -$1,279/month
- Net Revenue: $7,246/month
- **Net Profit: $6,542/month (90% margin)**
- **Annual Profit: $78,504**

**Note**: Actual costs tracked via Langfuse. Query `http://ops-server:3000` for real per-scan metrics.

## API Endpoints

**Production/Staging**: All endpoints are exposed at `https://api.rootsoff.com`
**Local Development**: All endpoints are exposed via Kong Gateway at `http://localhost:8000`

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/v1/scans/` | Trigger a new scan with JQL query |
| `GET` | `/api/v1/scans/` | List scans with pagination and optional project_key filter |
| `GET` | `/api/v1/scans/{scan_id}` | Get scan status (pending/running/completed/failed) |
| `GET` | `/api/v1/scans/{scan_id}/report` | Get JSON report with insights and Epic suggestions |
| `GET` | `/api/v1/scans/{scan_id}/report/html` | Get HTML report metadata with presigned S3 URL |
| `POST` | `/api/v1/scans/{scan_id}/insights/{insight_id}/epic` | Queue Epic creation from an insight (async) |
| `POST` | `/api/v1/credentials/jira` | Store/update JIRA credentials (encrypted) |
| `POST` | `/api/v1/credentials/vcs` | Store/update VCS credentials (encrypted) |
| `GET` | `/api/v1/credentials` | List configured credential services |
| `GET` | `/api/v1/credentials/status` | Get validation status of all credentials |
| `DELETE` | `/api/v1/credentials/{service_type}` | Delete credentials for a service |
| `GET` | `/api/v1/settings/jira-sync` | Get Jira sync configuration for tenant |
| `PUT` | `/api/v1/settings/jira-sync` | Update Jira sync configuration (write-back settings) |
| `GET` | `/api/v1/settings/generic` | List all tenant settings as key-value pairs |
| `PUT` | `/api/v1/settings/generic/{key}` | Update a specific setting by key |
| `GET` | `/api/v1/tickets/by-root-cause/{category}` | Get tickets matching a unified root cause category (for JQL) |
| `GET` | `/api/v1/users/{user_id}/data-export` | Export all user data (GDPR compliance) |
| `DELETE` | `/api/v1/users/{user_id}/data` | Delete user data with 30-day grace period (GDPR) |
| `GET` | `/api/v1/audit/logs` | Query audit logs (tenant-scoped) |
| `GET` | `/api/v1/audit/logs/export` | Export audit logs as JSON (GDPR compliance) |
| `DELETE` | `/api/v1/audit/logs` | Delete audit logs (GDPR compliance) |
| `GET` | `/health` | Health check endpoint |
| `GET` | `/metrics` | Prometheus metrics endpoint |

**Request Example:**
```bash
curl -X POST http://localhost:8000/api/v1/scans/ \
  -H "Content-Type: application/json" \
  -d '{"jql": "project = PROJ AND type = Bug AND created >= -7d", "max_tickets": 50}'
```

## Forge App Module

**UI Components:**

1.  **Issue Panel** (`forge-app/src/frontend/issue-panel/`)
    -   Displays RCA analysis for individual Bug tickets
    -   Shows root cause category, confidence score, technical domain
    -   Displays detailed cause and remediation plan
    -   Shows similar tickets
    -   Links to full HTML report
    -   Uses `useProductContext()` for issue key/ID

2.  **App Page** (`forge-app/src/frontend/app-page/`)
    -   Scan trigger form with JQL input and max tickets validation
    -   Progress tracking with automatic polling
    -   Results display with summary, top root causes, epic suggestions
    -   **Epic creation UI**: Implemented with success/error feedback
    -   **Scan history component**: Lists past scans with status badges, report links, pagination
    -   VCS configuration status warnings

3.  **Settings Page** (`forge-app/src/frontend/settings-page/`)
    -   JIRA credentials form (server URL, email, API token)
    -   VCS credentials for GitHub, GitLab, Bitbucket, Perforce
    -   Test connection functionality for all services
    -   Tabbed interface for service selection
    -   Jira sync settings (enable/disable write-back, custom field configuration)

**Architecture Best Practices:**
-   **Security**: CSP configured (`unsafe-inline/eval` removed), no custom JS execution (`render: native`)
-   **API Client**: Uses relative URLs (`/api/v1/...`) resolved via manifest
-   **Context**: Uses `useProductContext` for issue/project awareness
-   **State**: Async context loading with proper loading states

**Purpose**: Provides in-Jira UI for triggering scans, viewing RCA results, managing credentials, and creating epics from insights.

## Running the Application

**Local Development (with real LLM calls):**
```bash
# Copy and configure environment
cp env.example .env
# Edit .env with your credentials

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f
```

**Testing (with mocked LLM responses):**
```bash
docker-compose -f docker-compose-mock.yml up -d
```

**Forge App Development:**
```bash
cd forge-app

# Local development with hot reload
forge tunnel

# Deploy to development environment
forge deploy --environment development

# Deploy to staging environment
forge deploy --environment staging

# Install in Jira instance
forge install --site YOUR_SITE --product jira
```

## Deployment Infrastructure

**AWS Infrastructure (Terraform):**
-   **Compute**: ECS Fargate with auto-scaling
-   **Load Balancing**: Application Load Balancer with HTTPS and WAF
-   **Database**: RDS PostgreSQL 16 with pgvector, multi-AZ, encrypted at rest
-   **Cache**: ElastiCache Redis cluster, multi-node, encrypted at rest and in transit
-   **Storage**: S3 buckets for reports and raw data, encrypted with SSE-S3 (AES-256)
-   **Networking**: VPC with public/private subnets, NAT gateway, VPC endpoints
-   **DNS**: GoDaddy DNS (CNAME `api.rootsoff.com` → ALB); Route 53 migration optional
-   **Monitoring**: CloudWatch logs and metrics, Langfuse/Prometheus/Grafana on ops server
-   **Secrets**: HashiCorp Vault on ops server with AWS KMS auto-unseal

**Environments:**
-   **Development**: Forge development environment + local Docker Compose
-   **Production**: `https://api.rootsoff.com` - **READY** for deployment (pending manual approval)

**Testing:**
-   **Unit Tests**: 92 tests passing
-   **API Coverage**: 71%
-   **Integration Tests**: End-to-end flow coverage
-   **Smoke Tests**: Comprehensive deployment validation (`scripts/smoke_tests.sh`, `scripts/smoke-test-ecs.sh`)

## Production Deployment Guide for Beginners

**This guide assumes no prior AWS experience.** We provide automated scripts to handle most of the complexity.

### Overview

Production deployment involves:
1. **Prerequisites** (~30 minutes): Set up AWS resources using automated scripts
2. **Deployment** (~20 minutes): Run Terraform to create infrastructure
3. **Post-Deployment** (~30 minutes): Configure monitoring and run smoke tests
4. **Total Time**: ~90 minutes for first deployment

### Important Notes

- **Cost**: Production infrastructure costs ~$700-900/month for beta phase
- **Region**: All resources are deployed to `us-east-1` (US East - N. Virginia)
- **Domain**: You'll need `api.rootsoff.com` configured in your DNS provider (GoDaddy)
- **Vault**: We use HashiCorp Vault for secrets management instead of AWS Secrets Manager

---

## Step 1: Install Required Tools

**Install AWS CLI** (if not already installed):
```bash
# macOS
brew install awscli

# Or download from: https://aws.amazon.com/cli/
```

**Install Terraform** (if not already installed):
```bash
# macOS
brew tap hashicorp/tap
brew install hashicorp/tap/terraform

# Or download from: https://www.terraform.io/downloads
```

**Configure AWS credentials**:
```bash
aws configure
# You'll be prompted for:
# - AWS Access Key ID: (get from AWS Console → IAM → Users → Security Credentials)
# - AWS Secret Access Key: (from same location)
# - Default region: us-east-1
# - Default output format: json
```

**How to get AWS credentials**:
1. Log in to AWS Console: https://console.aws.amazon.com
2. Go to IAM → Users → Your username
3. Click "Security credentials" tab
4. Click "Create access key"
5. Choose "CLI" as use case
6. Copy the Access Key ID and Secret Access Key

---

## Step 2: Run Automated Prerequisites Setup

We've created scripts to automate most AWS setup. Run:

```bash
# From project root directory
cd /Users/edo.m/Documents/Repos/Jira-rc-analyzer

# Run pre-deployment validation
./scripts/deployment/pre-deployment-check.sh

# Run automated AWS setup (creates SSH key, certificate, state bucket)
./scripts/deployment/setup-aws-prerequisites.sh
```

**What these scripts do**:
- ✅ Create SSH key pair for accessing the ops server
- ✅ Request SSL certificate for `api.rootsoff.com`
- ✅ Create S3 bucket for Terraform state (with encryption and versioning)
- ✅ Validate your AWS configuration

**If certificate validation is required**:
- **DNS validation** (recommended): The script will show you a CNAME record to add to GoDaddy
- **Email validation**: Check your email for a validation link from AWS

**To add DNS record in GoDaddy**:
1. Log in to GoDaddy
2. Go to "My Products" → DNS
3. Add a new CNAME record with values provided by the script
4. Wait 5-30 minutes for validation

**Check certificate status**:
```bash
# Replace with your actual certificate ARN (script output)
aws acm describe-certificate --certificate-arn arn:aws:acm:us-east-1:XXXXX:certificate/XXXXX --region us-east-1
# Look for Status: ISSUED
```

---

## Step 3: Configure Production Variables

Update the file `infrastructure/terraform/production.tfvars`:

```bash
# Open in your editor
nano infrastructure/terraform/production.tfvars
# or
code infrastructure/terraform/production.tfvars
```

**Update these values**:
1. **certificate_arn**: Paste the ARN from Step 2 output
2. **ops_allowed_cidrs**: Change from `["0.0.0.0/0"]` to your organization's IP ranges
   - Find your IP: `curl ifconfig.me`
   - Use format: `["YOUR_IP/32"]` (or CIDR range if multiple IPs)

**Other values to verify**:
- `ops_ami_id`: Should be `ami-0557a15b87f6559cf` (Ubuntu 22.04 for us-east-1)
- `environment`: Should be `production`
- `region`: Should be `us-east-1`

---

## Step 4: Uncomment Terraform Backend

Edit `infrastructure/terraform/main.tf` and uncomment lines 22-27:

```hcl
terraform {
  backend "s3" {
    bucket         = "jira-rc-analyzer-terraform-state-production"
    key            = "terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
  }
}
```

**Why?** This tells Terraform to store its state in S3 instead of locally. Critical for production.

---

## Step 5: Run Pre-Deployment Validation

```bash
./scripts/deployment/pre-deployment-check.sh
```

**This checks**:
- ✅ AWS CLI is installed and configured
- ✅ Terraform is installed
- ✅ Certificate exists and is validated
- ✅ SSH key pair exists
- ✅ Terraform state bucket exists
- ✅ Terraform configuration is valid

**Don't proceed unless all checks pass!**

---

## Step 6: Deploy Infrastructure with Terraform

```bash
cd infrastructure/terraform

# Initialize Terraform (migrates state to S3)
terraform init -migrate-state
# When prompted "Do you want to copy existing state to the new backend?", type: yes

# Validate configuration
terraform validate
# Should output: Success! The configuration is valid.

# Preview what will be created (dry-run)
terraform plan -var-file=production.tfvars -out=production.plan

# Review the output carefully - you should see:
# - ~50-60 resources to be created
# - VPC with 3 availability zones
# - RDS PostgreSQL (encrypted)
# - ElastiCache Redis (encrypted)
# - S3 buckets (encrypted)
# - ECS cluster with 7 services
# - Application Load Balancer
# - Ops server (EC2 instance)
```

**If the plan looks good, apply it**:
```bash
terraform apply production.plan
# This takes 15-20 minutes
# Don't interrupt! Grab a coffee ☕
```

**Save the outputs**:
```bash
# After apply completes, note these values:
terraform output
# Save: ALB DNS name, Ops server IP, RDS endpoint, Redis endpoint
```

---

## Step 7: Post-Deployment Verification

```bash
cd /Users/edo.m/Documents/Repos/Jira-rc-analyzer

# Run automated verification
./scripts/deployment/post-deployment-verify.sh
```

**This verifies**:
- ✅ All encryption is enabled (RDS, Redis, S3)
- ✅ All 7 ECS services are running
- ✅ API health endpoint is accessible
- ✅ ALB target groups are healthy

---

## Step 8: Configure DNS

**In GoDaddy (or your DNS provider)**:

1. Log in to GoDaddy
2. Go to "My Products" → DNS for rootsoff.com
3. Add/Update CNAME record:
   - **Type**: CNAME
   - **Name**: api
   - **Value**: `<ALB DNS name from terraform output>`
   - **TTL**: 600 (10 minutes)
4. Save

**Wait 5-10 minutes for DNS propagation**, then test:
```bash
dig api.rootsoff.com
# Should show your ALB DNS name

curl https://api.rootsoff.com/health
# Should return: {"status":"healthy"}
```

---

## Step 9: Set Up Ops Server (Monitoring Stack)

The ops server hosts Vault, Prometheus, Grafana, and Langfuse.

```bash
# Get ops server IP from terraform output
OPS_IP=$(cd infrastructure/terraform && terraform output -raw ops_server_public_ip)

# SSH to ops server
ssh ubuntu@$OPS_IP -i ~/jira-rc-analyzer-production-key.pem
```

**On the ops server**:

```bash
# The setup script is already on the server
~/setup-ops-server.sh

# Clone repository
sudo mkdir -p /opt/ops
sudo chown ubuntu:ubuntu /opt/ops
cd /opt/ops
git clone https://github.com/YOUR_ORG/Jira-rc-analyzer.git .

# Create .env file for monitoring stack
cat > .env << 'EOF'
# Grafana
GF_SECURITY_ADMIN_PASSWORD=CHANGE_ME_STRONG_PASSWORD

# LangFuse
LANGFUSE_SECRET_KEY=GENERATE_RANDOM_KEY_32_CHARS
LANGFUSE_PUBLIC_KEY=pk_production_GENERATE_RANDOM

# Postgres for Langfuse
POSTGRES_PASSWORD=GENERATE_RANDOM_PASSWORD

# Vault (will be auto-unsealed with AWS KMS)
VAULT_ADDR=http://localhost:8200
EOF

# Generate secure passwords (recommended)
openssl rand -base64 32  # Use for LANGFUSE_SECRET_KEY
openssl rand -base64 24  # Use for POSTGRES_PASSWORD
openssl rand -hex 16     # Use for LANGFUSE_PUBLIC_KEY (prefix with pk_production_)

# Edit .env with generated values
nano .env

# Start monitoring stack
docker-compose -f docker-compose.infra.yml up -d

# Check all services are running
docker-compose -f docker-compose.infra.yml ps
```

---

## Step 10: Initialize and Configure Vault

**See detailed guide**: `docs/VAULT_PROD_SETUP.md`

**Quick steps** (on ops server):

```bash
# Wait for Vault to start
sleep 10

# Initialize Vault
docker exec jira-rc-vault vault operator init -format=json > /tmp/vault-init.json

# Save recovery keys securely!
cat /tmp/vault-init.json
# Copy the entire output to a secure password manager (1Password, LastPass, etc.)

# Get root token
export VAULT_TOKEN=$(jq -r .root_token /tmp/vault-init.json)

# Enable secrets engine
docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault secrets enable -path=secret kv-v2

# Enable transit engine for encryption
docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault secrets enable transit
docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault write -f transit/keys/jira-credentials

# Populate production secrets
docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault kv put secret/jira-rc-analyzer/production/database \
  db_password="GENERATE_STRONG_PASSWORD"

docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault kv put secret/jira-rc-analyzer/production/redis \
  redis_password="GENERATE_STRONG_PASSWORD"

docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault kv put secret/jira-rc-analyzer/production/llm \
  openai_api_key="sk-YOUR_OPENAI_KEY"

docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault kv put secret/jira-rc-analyzer/production/encryption \
  encryption_master_key="$(openssl rand -base64 32)"

docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault kv put secret/jira-rc-analyzer/production/jira \
  jira_api_token="YOUR_JIRA_TOKEN"

docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault kv put secret/jira-rc-analyzer/production/langfuse \
  langfuse_secret_key="YOUR_LANGFUSE_SECRET" \
  langfuse_public_key="YOUR_LANGFUSE_PUBLIC"

# Optional: PostHog analytics
docker exec -e VAULT_TOKEN=$VAULT_TOKEN jira-rc-vault vault kv put secret/jira-rc-analyzer/production/posthog \
  posthog_api_key="YOUR_POSTHOG_KEY"

# Securely delete init file
shred -u /tmp/vault-init.json
```

**Important**: Store the Vault recovery keys and root token in a secure password manager!

---

## Step 11: Run Database Migrations

```bash
# From ops server (or locally with VPN)
cd /opt/ops

# Install Python dependencies
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Set database URL (get from terraform output)
export DATABASE_URL="postgresql://admin:PASSWORD@RDS_ENDPOINT:5432/jira_analyzer"

# Run migrations
alembic upgrade head

# Verify pgvector extension
psql $DATABASE_URL -c "SELECT * FROM pg_extension WHERE extname = 'vector';"
```

---

## Step 12: Run Smoke Tests

```bash
# From your local machine
cd /Users/edo.m/Documents/Repos/Jira-rc-analyzer

# Run comprehensive smoke tests
./scripts/smoke-test-ecs.sh production

# Should show:
# ✓ Health endpoint OK
# ✓ Metrics endpoint OK
# ✓ Database connectivity OK
# ✓ Redis connectivity OK
# ✓ S3 connectivity OK
# ✓ All ECS services healthy
```

---

## Step 13: Access Monitoring Dashboards

**Grafana** (Metrics & Dashboards):
```
http://<OPS_IP>:3001
Username: admin
Password: (from .env file)
```

**Prometheus** (Metrics Database):
```
http://<OPS_IP>:9090
```

**Langfuse** (LLM Observability):
```
http://<OPS_IP>:3000
```

**AlertManager** (Alerts):
```
http://<OPS_IP>:9093
```

---

## Rollback Plan (If Something Goes Wrong)

**During Terraform apply**:
```bash
# Press Ctrl+C to stop
# Then review state:
cd infrastructure/terraform
terraform show

# Destroy specific resource if needed:
terraform destroy -target=module.ecs.aws_ecs_service.api

# Or rollback everything (LAST RESORT):
terraform destroy -var-file=production.tfvars
```

**After successful deployment**:
1. Keep staging environment as fallback
2. Update DNS to point back to staging
3. Document issues
4. Fix and redeploy
5. **Never run `terraform destroy` without team approval**

**Data protection**:
- RDS has automated backups (30-day retention)
- S3 has versioning enabled
- Terraform state has versioning

---

## Troubleshooting

**"Certificate not validated"**:
- Check DNS records in GoDaddy match ACM requirements
- Wait 30 minutes and check again
- Resend validation email if using email validation

**"Terraform state locked"**:
- Another process is using Terraform
- Wait for it to complete or force-unlock:
  ```bash
  terraform force-unlock LOCK_ID
  ```

**"ECS tasks not starting"**:
- Check CloudWatch logs: AWS Console → CloudWatch → Log Groups → `/ecs/production/SERVICE_NAME`
- Verify Vault is accessible from ECS tasks
- Check IAM role policies

**"API not accessible"**:
- Verify DNS propagation: `dig api.rootsoff.com`
- Check ALB health: AWS Console → EC2 → Load Balancers
- Check target group health
- Verify security groups allow HTTPS (443)

**"Vault connection failed"**:
- SSH to ops server
- Check Vault status: `docker exec jira-rc-vault vault status`
- Restart if needed: `docker-compose -f docker-compose.infra.yml restart vault`

---

## Cost Estimation

**Beta Phase** (~$850-1,050/month):
- ECS Fargate (14 tasks): $200-300
- RDS PostgreSQL (db.t3.medium, multi-AZ): $150-200
- ElastiCache Redis (cache.t4g.small × 2): $80-100
- ALB + Data Transfer: $50-80
- S3 Storage: $5-10
- Ops Server (t3.xlarge): $150
- NAT Gateway: $50
- LLM costs (GPT-5.1-nano): ~$155 for 100k tickets

**Production Scale** (500k tickets/month: ~$1,850-2,200/month):
- Infrastructure: $1,050-1,400
- LLM costs: $775
- Embeddings: $25

---

## Next Steps After Deployment

1. ✅ Deploy Forge app to production environment
2. ✅ Install Forge app in customer Jira instances
3. ✅ Monitor first 24 hours (Grafana, Langfuse, CloudWatch)
4. ✅ Verify cache hit rates (target: >80% scan status, >60% ticket RCA)
5. ✅ Set up CloudWatch alarms for critical metrics
6. ✅ Enable automated backup verification
7. ✅ Document any production-specific configurations

---

## Related Documentation

- **`scripts/deployment/README.md`** - Deployment scripts reference guide
- **`docs/VAULT_PROD_SETUP.md`** - Vault initialization and configuration
- **`docs/CIRCUIT_BREAKER.md`** - Circuit breaker implementation
- **`docs/DATABASE_MIGRATIONS.md`** - Alembic migration guide
- **`docs/CACHING.md`** - Redis caching strategy
- **`docs/SECURITY.md`** - Security model
- **`docs/MONITORING.md`** - Prometheus/Grafana setup
- **`scripts/smoke_tests.sh`** - Local smoke tests
- **`scripts/smoke-test-ecs.sh`** - ECS smoke tests for production

## Technical To-Do List (Prioritized)

### Critical (Pre-Production / Immediate)
1. ✅ **Create Automated Deployment Scripts** (COMPLETED)
   - ✅ Pre-deployment validation script
   - ✅ AWS prerequisites setup script
   - ✅ Terraform backend setup script
   - ✅ Post-deployment verification script

2. ✅ **Consolidate Deployment Documentation** (COMPLETED)
   - ✅ Merged PRODUCTION_DEPLOYMENT.md into TECHNICAL_ROADMAP.md
   - ✅ Added beginner-friendly step-by-step guide
   - ✅ Included troubleshooting section
   - ✅ Added cost estimates and rollback plan

3. **Execute Production Deployment** (READY TO START)
   - [ ] Run automated prerequisites setup
   - [ ] Configure production.tfvars with certificate ARN
   - [ ] Run pre-deployment validation
   - [ ] Execute `terraform apply` with production.tfvars
   - [ ] Monitor deployment (15-20 minutes)
   - [ ] Verify all resources created successfully

4. **Post-Deployment Setup** (AFTER DEPLOYMENT)
   - [ ] Configure DNS (CNAME api.rootsoff.com → ALB)
   - [ ] SSH to ops server and deploy monitoring stack
   - [ ] Initialize Vault and populate secrets
   - [ ] Run database migrations
   - [ ] Execute smoke tests
   - [ ] Verify all 7 ECS services healthy
   - [ ] Test API endpoints (health, metrics)
   - [ ] Access monitoring dashboards (Grafana, Prometheus, Langfuse)

5. **Secrets Management** (PART OF STEP 4)
   - [ ] Initialize Vault with AWS KMS auto-unseal
   - [ ] Securely store Vault recovery keys
   - [ ] Populate all production secrets in Vault
   - [ ] Validate ECS services can access Vault secrets
   - [ ] Document emergency access procedures

### ✅ Already Completed (Verified December 2025)
- ✅ **Rate limiting middleware** (`src/api/middleware/rate_limit.py`)
- ✅ **Circuit breakers** (`src/common/circuit_breaker.py`) - LLM, VCS, Jira
- ✅ **Audit logging** (`src/api/middleware/audit_log.py`)
- ✅ **PostHog telemetry** (`src/common/telemetry.py`)
- ✅ **LangFuse integration** (`src/common/llm_factory.py`)
- ✅ **Vault secrets** (`src/common/vault_client.py`)
- ✅ **On-prem VCS agent** (`src/on_prem_agent/` - 11 files)
- ✅ **Settings cache** (`src/common/settings_manager.py`)
- ✅ **Metrics middleware** (`src/common/metrics.py`)
- ✅ **pgvector 0.8.1** updated

### Phase 1 (First 5 Customers)

6. **Gather Langfuse Observations for Cost Metrics**
   - Query observations endpoint for per-scan token usage
   - Calculate actual cost per ticket vs estimates
   - Instrument any missing LLM call traces
   - Set up cost alerts and quotas

7. **Validate Rate-Limiting Thresholds**
   - Monitor per-tenant rate limit hits
   - Adjust thresholds based on beta usage patterns
   - Implement quota warnings for tenants
   - Prevent runaway LLM costs

8. **Monitor First 24 Hours Post-Deployment**
   - Check Prometheus/Grafana dashboards continuously
   - Review Langfuse traces for LLM call patterns
   - Monitor PostHog events (if enabled)
   - Check CloudWatch logs for errors
   - Verify cache hit rates meet targets (>80% scan status, >60% ticket RCA)

### Medium Priority
9. **Connection Pool Tuning**
   - Monitor PostgreSQL connection pool usage
   - Tune pool size based on actual load
   - Optimize Redis connection pooling
   - Set up alerts for pool exhaustion

10. **Monitoring Dashboard Refinements**
    - Create custom Grafana dashboards for business metrics
    - Add alerts for anomalous patterns
    - Set up PagerDuty/Slack integrations
    - Document dashboard usage for ops team

11. **Resilience Enhancements**
    - Evaluate Redis cluster setup
    - Implement circuit breaker metrics dashboards
    - Add retry logic for transient failures
    - Document disaster recovery procedures

### Low Priority
12. **UI Feature Enrichment**
    - Add charts and visualizations to Project Page
    - Implement bulk operations for scans
    - Add trend visualization over time
    - Export functionality for reports

13. **Data Retention Policies**
    - Implement configurable data retention (e.g., 90 days)
    - Add optional deletion of raw tickets after scan completion
    - Opt-in for storing raw reports vs metadata only
    - GDPR compliance for data retention

## Success Metrics

### Technical Metrics
-   Analysis completion rate (target: >95%)
-   Pipeline throughput target: tickets/hour (define baseline during beta)
-   VCS diff fetch success rate (target: >99%)
-   LLM call latency p95 (track via Langfuse)
-   Embedding generation time p95
-   Cache hit rate: scan status >80%, ticket RCA >60%
-   Circuit breaker trip rate (target: <1% of requests)
-   API response time: p50 <100ms, p95 <500ms, p99 <1000ms

### Business Metrics
-   Bug volume reduction after Epic implementation
-   Actionable Epic acceptance rate (target: >70%)
-   Cost per ticket analyzed (track via Langfuse)
-   Time saved vs manual RCA (estimate 30-60 min/ticket)
-   User adoption rate (Forge app installs)
-   Scan frequency per tenant

### Quality Metrics
-   Inconclusive rate (target: <20%)
-   False positive rate for Epic suggestions (target: <10%)
-   User satisfaction score (NPS) - collect during beta
-   RCA accuracy (feedback from developers)

## Production Readiness Checklist

### Security ✅ **COMPLETE**
- [x] Authentication (JWT validation via ForgeAuthMiddleware)
- [x] Authorization (tenant isolation)
- [x] Rate limiting (per-tenant, per-endpoint with sliding window)
- [x] Error sanitization middleware
- [x] Credential encryption (Vault transit engine)
- [x] Audit logging middleware
- [x] Security headers middleware (HSTS, CSP, etc.)
- [x] WAF rules active on ALB
- [x] Vault secrets management configured
- [x] HTTPS enforcement via ACM
- [x] Tenant context middleware
- [x] Metrics middleware
- [x] Database encryption at rest (RDS) ✅
- [x] S3/Redis encryption at rest ✅
- [x] Circuit breakers implemented and tested ✅

### Compliance ✅ **COMPLETE**
- [x] Tenant data isolation (multi-tenant architecture)
- [x] Audit logging middleware (publishes to Redis Streams)
- [x] Audit log API endpoints (query, export, deletion)
- [x] GDPR: Audit log export/deletion endpoints
- [x] GDPR: User data export API (`GET /api/v1/users/{user_id}/data-export`) ✅
- [x] GDPR: User data deletion API (`DELETE /api/v1/users/{user_id}/data` with 30-day grace) ✅
- [ ] GDPR: Data retention policies (implementation pending - beta phase)
- [ ] HIPAA: Gap analysis and implementation (post-beta if required)

### Deployment ✅ **READY FOR PRODUCTION**
- [x] Terraform infrastructure defined and validated
- [x] CI/CD pipelines configured (GitHub Actions)
- [x] Staging deployment removed (streamlined architecture)
- [x] HTTPS/DNS configured (`api.rootsoff.com`)
- [x] Forge app deployed to development
- [x] Production tfvars created with encryption settings ✅
- [x] Deployment checklist documented (this file + PRODUCTION_DEPLOYMENT.md) ✅
- [x] Smoke tests script created (`scripts/smoke_tests.sh`, `scripts/smoke-test-ecs.sh`) ✅
- [x] Database migration tooling (Alembic) ✅
- [x] Vault production setup documented (`docs/VAULT_PROD_SETUP.md`) ✅
- [ ] Production deployment executed (pending manual `terraform apply`)
- [ ] Smoke tests passing for all 9 services in production

## Appendix A: Forge Development Reference

**Local Development (Recommended):**
```bash
cd forge-app
forge tunnel  # Start local tunnel - changes reflect immediately!
```

**Deploy & Install:**
```bash
# Login
forge login

# Deploy
forge deploy --environment development
forge deploy --environment staging

# Install
forge install --site YOUR_SITE --product jira
```

**Logs:**
```bash
forge logs --environment development
```

## Appendix B: Troubleshooting Guide

**"Failed to fetch" in Forge app**
- Check: Is `api.rootsoff.com` resolving? (`dig api.rootsoff.com`)
- Check: Is HTTPS certificate valid?
- Check: Is ALB target group healthy?
- Check: Are all ECS tasks running?

**"React is not defined" error**
- Fix: Ensure `import React from 'react';` is in all JSX files. React dependency required even for UI Kit v3 JSX.

**Forge app not appearing in Jira**
- Check: Apps → Manage apps → Connected apps
- Verify: App is installed and enabled
- Verify: App is deployed to correct environment (`forge deploy --environment development`)

**ECS tasks fail to start**
- Check CloudWatch logs: `/ecs/production/<service-name>`
- Verify Vault secrets are accessible (check IAM policies)
- Check task definition environment variables

**Vault connection issues**
- Verify Vault is running on ops server: `docker ps | grep vault`
- Check Vault status: `docker exec jira-rc-vault vault status`
- Verify ECS tasks have correct `VAULT_URL` environment variable
- Check security groups allow ECS → Ops Server on Vault port

**High LLM costs**
- Check Langfuse for per-tenant usage: `http://<ops-ip>:3000`
- Review rate limiting: Are limits being hit?
- Check for runaway scan loops in CloudWatch logs
- Verify BYPASS_TRIAGE is enabled (reduces costs)

## Appendix C: Related Documentation

- `scripts/deployment/README.md` - Deployment scripts guide (beginner-friendly)
- `docs/VAULT_PROD_SETUP.md` - Vault initialization and configuration for production
- `docs/CIRCUIT_BREAKER.md` - Circuit breaker implementation details
- `docs/DATABASE_MIGRATIONS.md` - Alembic migration guide
- `docs/CACHING.md` - Redis caching strategy and configuration
- `docs/SECURITY.md` - Security model and best practices
- `docs/MONITORING.md` - Prometheus/Grafana setup and dashboards
- `scripts/smoke_tests.sh` - Smoke test suite for API validation
- `scripts/smoke-test-ecs.sh` - ECS-specific smoke tests

---

**Last Updated**: January 21, 2026
**Document Version**: 2.0 (Consolidated with deployment readiness)
**Prepared By**: Development Team
