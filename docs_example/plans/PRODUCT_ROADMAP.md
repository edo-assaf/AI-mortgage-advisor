# Roots Off - Bug Root Cause Analyzer: Product Roadmap

## Executive Summary

Transform the current Python-based JIRA root cause analyzer into a production-ready JIRA marketplace app targeting enterprise customers. The solution will help software teams identify bug patterns, prioritize technical debt, and generate actionable Epics/Stories to reduce bug volume.

**Current Status**: ✅ **MVP Core Features Complete** - Basic Forge app deployed to development environment with Issue Panel, Project Page, and Settings Page. Complete report generation functional. Jira write-back and Epic creation implemented. Ready for feature enrichment and production deployment.

**Architecture**: Implements a 7-phase pipeline (Ingestion, Triage, Deep Analysis, Aggregation, Reporting, Integration, Orchestration) to handle high volumes efficiently while providing deep, code-aware insights and seamless Jira integration.

**Target Market**: Enterprise (subscription-based pricing)
**Core Value**: Reduce bug volume by 20-40% through systematic root cause analysis, code-level deep dives, and targeted technical debt reduction.

---

## Current Status (January 21, 2026)

**🎉 Recent Completions (Latest Development Cycle):**
- **Orchestration Service**: Centralized scan lifecycle management and pipeline coordination implemented
- **Internal Concurrency**: Sub-second parallel message processing in Redis stream consumers
- **Service Standardization**: Unified microservice naming (underscores) across Terraform, scripts, and logs
- **Rate Limiting System**: Per-tenant, per-endpoint rate limiting with sliding window algorithm
- **Audit Logging**: Complete audit logging system with middleware and API endpoints for compliance
- **Staging Removal**: Streamlined cloud environments to Development and Production only
- **GDPR Compliance**: Audit log export and deletion endpoints implemented
- **Security Middleware Stack**: Security headers, error sanitization, Forge authentication, tenant context
- **Metrics & Monitoring**: Prometheus metrics middleware and `/metrics` endpoint
- **Scan History API**: List scans endpoint (`GET /api/v1/scans/`) with pagination and filtering

**✅ Completed:**
- Basic Forge app deployed to development environment
- Three core pages implemented: Issue Panel, Project Page, Settings Page with Jira Integration tab
- Complete report generation functional with primary/secondary category display
- Epic creation from insights working (async via Integration service)
- Jira write-back implemented (configurable RCA sync to tickets)
- Scan history tracking implemented with list endpoint (`GET /api/v1/scans/`) and UI component
- Credential management for JIRA and VCS providers
- Generic settings system with Redis caching
- JQL function for filtering by root cause category
- **Rate limiting middleware** (per-tenant, per-endpoint with sliding window algorithm)
- **Audit logging system** (middleware + API endpoints for query, export, deletion)
- **GDPR compliance endpoints** (audit log export and deletion)
- **Security headers middleware** (HSTS, CSP, X-Frame-Options, etc.)
- **Metrics middleware** and Prometheus endpoint
- **Forge authentication middleware** with JWKS validation
- **Tenant context middleware** for cloudId to tenant_id mapping

**⏳ Next Priorities:**
- Enrich existing pages with additional features (charts, filters, export)
- ~~Implement data caching for fetched data~~ ✅ Settings caching implemented, API caching implemented
- ~~Enhance data reflection in JIRA (beyond epic creation)~~ ✅ Jira write-back implemented
- Add comparison and trend visualization for analysis history
- Expand configuration options (link traversal, LLM settings, timeouts)
- Source data caching (JIRA ticket data, VCS diffs, HTML reports)

---

## 1. Feature List (Prioritized for Enterprise Market)

### Phase 1: MVP (Minimum Viable Product) - Core Functionality

**P0 - Critical (Must Have for Launch)**
1.  **High Volume Ingestion & Triage**
    -   Fetch tickets in bulk (JIRA Fetcher)
    -   Store raw data efficiently (Redis/S3)
    -   **Smart Triage Agent**: Group duplicates and similar issues using hashing and small LLMs to reduce analysis volume (Phase 2 of pipeline).
    -   Identify "Representative Tickets" for deep analysis.

2.  **Deep Agentic Analysis (RCA)**
    -   **RCA Agent**: LangChain-based service performing deep analysis on representative tickets.
    -   **Code Repository Integration**: Connect to GitHub/GitLab/Bitbucket/Perforce to analyze actual code alongside JIRA tickets.
    -   Root cause extraction with code context.

3.  **JIRA App Integration** ✅ **IMPLEMENTED**
    -   ✅ Forge app with JIRA UI integration (deployed to development environment)
    -   ✅ **Issue Panel**: Displays RCA analysis for individual Bug tickets
    -   ✅ **Project Page**: Scan trigger with JQL input, progress tracking, results display, epic creation, scan history
    -   ✅ **Settings Page**: Credential management for JIRA and all VCS providers + Jira Integration settings
    -   ✅ Results display within JIRA (not external)
    -   ✅ **Jira Write-back**: Reflect RCA data directly in JIRA issues (custom fields + comments, configurable per tenant)
    -   ✅ **JQL Function**: `rootCauseCategory("Logic Error")` for filtering tickets by unified category

4.  **Semantic Aggregation & Reporting** ✅ **IMPLEMENTED**
    -   ✅ **Vector DB Storage**: Store RCA embeddings (PostgreSQL + pgvector)
    -   ✅ **Insight Generator**: Cluster RCA patterns semantically to find systemic issues
    -   ✅ **Category Unification**: LLM-based category consolidation (primary/secondary categories)
    -   ✅ **Complete Report Generation**: Full HTML reports with unified categories, remediation plans, and insights
    -   ✅ Generate actionable Epics from clustered insights (async via Integration service)
    -   ✅ **Jira Write-back**: Automatically sync RCA results to tickets (configurable per tenant)

5.  **Security & Authentication**
    -   OAuth 2.0 with JIRA & Code Hosts (GitHub/GitLab)
    -   User permission checks
    -   Secure credential storage

**P1 - High Priority (Launch + 1 month)**
6.  **Multi-Tenant Support** ✅ **IMPLEMENTED**
    -   ✅ Tenant isolation (Forge product context)
    -   ✅ Per-tenant configuration (credentials management)
    -   ⏳ Usage tracking per tenant - Rate limiting implemented, detailed usage tracking pending

7.  **Analysis History** ✅ **PARTIALLY IMPLEMENTED**
    -   ✅ View past analysis runs (scan history component on project page)
    -   ✅ List scans endpoint (`GET /api/v1/scans/`) with pagination and filtering
    -   ⏳ Compare analyses over time
    -   ⏳ Trend visualization

8.  **Notification System**
    -   Email notifications on completion
    -   JIRA notifications for generated Epics
    -   Slack/Teams integration (webhooks)

9.  **Configuration UI** ✅ **PARTIALLY IMPLEMENTED**
    -   ✅ Credential management UI (JIRA and VCS providers)
    -   ✅ Test connection functionality for all services
    -   ✅ **Jira Integration tab**: Configure write-back settings (custom field, comments)
    -   ⏳ Customize link traversal rules
    -   ⏳ LLM model selection (if multiple supported)
    -   ⏳ Timeout settings
    -   ⏳ Category customization

10. **Data Caching** ✅ **PARTIALLY IMPLEMENTED**
    -   ✅ Cache scan status (Redis, 10s TTL with invalidation)
    -   ✅ Cache ticket RCA results (Redis, 10min TTL with invalidation)
    -   ✅ Cache tenant settings (Redis Hash, 1h TTL with immediate updates)
    -   ⏳ Cache fetched JIRA ticket data (source data)
    -   ⏳ Cache VCS repository data (code diffs)
    -   ⏳ Cache HTML reports

**P2 - Medium Priority (Launch + 3 months)**
11. **Advanced Analytics**
    -   Bug trend analysis (bugs over time by category)
    -   Impact scoring (which root causes affect most bugs)
    -   Cost estimation (engineering hours saved)
    -   ROI calculator

12. **Smart Filtering & Grouping**
    -   Group bugs by component/module
    -   Filter by confidence score
    -   Filter by root cause source (explicit vs inferred)
    -   Component-level insights

13. **Collaboration Features**
    -   Share analysis reports
    -   Comment on root causes
    -   Mark root causes as "fixed" (track resolution)
    -   Team dashboards

14. **API Access**
    -   REST API for programmatic access
    -   Webhook support for integrations
    -   Export API for external tools

**P3 - Nice to Have (Launch + 6 months)**
15. **AI-Powered Insights**
    -   Predictive analysis (which areas likely to have bugs)
    -   Similar bug detection
    -   Automated follow-up suggestions

16. **Integration Ecosystem**
    -   CI/CD integration (prevent regressions)
    -   ServiceNow integration
    -   Custom webhook integrations

17. **Advanced Visualizations**
    -   Sankey diagrams (bug flow)
    -   Heatmaps (bug density by component)
    -   Timeline views
    -   Interactive dashboards

18. **Compliance & Audit** ✅ **COMPLETED**
    -   ✅ Audit logging middleware (publishes events to Redis Streams)
    -   ✅ Audit log API endpoints (`GET /api/v1/audit/logs`, export, deletion)
    -   ✅ GDPR compliance endpoints (audit log export and deletion)
    -   ✅ User data export API (`GET /api/v1/users/{user_id}/data-export`)
    -   ✅ User data deletion API (`DELETE /api/v1/users/{user_id}/data` with 30-day grace period)
    -   ✅ pgvector 0.8.1 updated
    -   ⏳ Data retention policies (implementation pending)
    -   ⏳ SOC 2 readiness documentation (pending)

---

## 2. Roadmap / To Do List Until Publication

### Phase 0: Pre-Development (Weeks 1-2)
- [ ] **Market Research**
  - Analyze competitor apps (BugHerd, LinearB, etc.)
  - Survey potential customers (10-20 interviews)
  - Define unique value proposition
  - Pricing strategy finalization

- [x] **Technical Architecture Design** ✅ **COMPLETED**
  - 5-phase pipeline (Ingestion, Triage, Analysis, Aggregation, Reporting) finalized
  - Vector DB provider and Redis strategy selected
  - API architecture designed
  - Security architecture for Code Access reviewed

- [x] **Atlassian Developer Account Setup** ✅ **COMPLETED**
  - Registered as Atlassian Marketplace vendor
  - Forge development environment set up
  - Marketplace requirements reviewed
  - Security review preparation completed

### Phase 1: Core Development (Weeks 3-8) ✅ **COMPLETED**
- [x] **Forge App Foundation** ✅ **COMPLETED**
  - [x] Set up Forge project structure ✅
  - [x] Implement OAuth 2.0 authentication ✅
  - [x] Create JIRA UI modules (issue panel, project page, settings page) ✅
  - [x] Basic navigation and routing ✅
  - [x] App deployed to development environment ✅

- [x] **Backend API** ✅ **COMPLETED**
  - [x] Asynchronous scan processing
  - [x] Scan status polling and reporting
  - [ ] Notification hook integration (pending)

- [x] **Backend Pipeline** ✅ **COMPLETED**
  - [x] Ingestion: JIRA ticket fetching and storage
  - [x] Triage: Duplicate detection and grouping
  - [x] Analysis: Deep RCA with code repository integration
  - [x] Aggregation: Pattern clustering and insight generation

- [x] **Code Repository Integration** ✅ **COMPLETED**
  - [x] GitHub, GitLab, Bitbucket, Perforce connectors
  - [x] Code analysis tools integrated

- [x] **Epic/Story Generation**
  - [x] Epic creation from clustered insights ✅
  - [ ] Story creation from remediation plans
  - [x] User approval workflow ✅

### Phase 2: UI/UX Development (Weeks 9-12) ✅ **COMPLETED** → **Enhancement Phase**
- [x] **Analysis UI** ✅ **COMPLETED**
  - [x] JQL input and validation ✅
  - [x] Analysis configuration panel (basic) ✅
  - [x] Progress indicator ✅
  - [x] Results display (summary + details) ✅

- [x] **Report Visualization** ✅ **COMPLETED**
  - [x] Complete report generation (HTML reports) ✅
  - [x] Root cause details view ✅
  - [x] Ticket list with RCA information ✅
  - [ ] Charts (category distribution, confidence scores) - ⏳ Enhancement opportunity
  - [ ] Export functionality (CSV/JSON) - ⏳ Enhancement opportunity

- [x] **Epic/Story UI** ✅ **COMPLETED**
  - [x] Epic creation interface ✅
  - [x] Preview generated Epics ✅
  - [ ] Bulk creation interface - ⏳ Enhancement opportunity
  - [x] Approval workflow UI (explicit API call required) ✅
  - [ ] Link visualization - ⏳ Enhancement opportunity
  - [ ] Story creation from remediation plans - ⏳ Pending

- [x] **Settings/Configuration** ✅ **PARTIALLY COMPLETED**
  - [x] Credential management UI ✅
  - [x] Test connection functionality ✅
  - [x] **Jira Integration tab**: Write-back settings (custom field ID, comment toggle) ✅
  - [ ] Link traversal configuration - ⏳ Next priority
  - [ ] LLM settings - ⏳ Next priority
  - [ ] Notification preferences - ⏳ Next priority
  - [ ] User permissions - ⏳ Next priority

**Completed in Enhancement Phase:**
- ✅ Generic settings system with Redis caching
- ✅ Jira write-back capability (configurable per tenant)
- ✅ Primary/secondary category system for better filtering
- ✅ JQL function for root cause category filtering
- ✅ Integration service for async Epic creation and Jira sync

**Next Phase: Feature Enrichment**
- Enhance existing pages with advanced features (charts, filters, bulk operations)
- Add comparison and trend visualization for analysis history
- Expand configuration options (link traversal, LLM settings)

### Phase 3: Enterprise Features (Weeks 13-16)
- [x] **Multi-Tenant Support** ✅ **COMPLETED**
  - [x] Tenant isolation implemented
  - [x] Per-tenant configuration and credentials management
  - [ ] Usage tracking and limits (Rate limiting implemented, detailed tracking pending)

- [x] **Security Hardening** ✅ **COMPLETED**
  - [x] Security headers middleware implemented ✅
  - [x] HTTPS/WAF enabled ✅
  - [x] Secrets Manager integration ✅
  - [x] Rate limiting middleware (per-tenant, per-endpoint) ✅
  - [x] Forge authentication middleware with JWKS validation ✅
  - [x] Tenant context middleware ✅
  - [x] Error sanitization middleware ✅
  - [x] Database encryption at rest ✅ **COMPLETED** - RDS encryption verified
  - [x] S3/Redis encryption at rest ✅ **COMPLETED** - Both enabled in Terraform

- [x] **Performance Optimization** ✅ **PARTIALLY COMPLETED**
  - [x] Rate limiting ✅
  - [x] Async processing optimization ✅
  - [x] **Caching strategy** ✅ **IMPLEMENTED** - API response caching
  - [x] **Circuit breakers** ✅ **IMPLEMENTED** - Fully integrated with LLM, VCS, and Jira clients
  - [ ] Source data caching - ⏳ **NEXT PRIORITY**
  - [ ] Connection pool tuning - ⏳ **PENDING**

- [x] **Monitoring & Logging** ✅ **COMPLETED**
  - [x] Prometheus/Grafana configured ✅
  - [x] Self-hosted PostHog (error tracking & analytics) ✅
  - [x] Audit logging middleware ✅
  - [x] Audit log API endpoints (query, export, deletion) ✅
  - [x] Metrics middleware and Prometheus endpoint ✅
  - [x] Rate limiting with Prometheus metrics ✅
  - [x] Smoke tests for production validation ✅


### Phase 4: Testing & QA (Weeks 17-20)
- [ ] **Unit Testing**
  - Test coverage >80%
  - Mock JIRA API responses
  - Mock LLM responses
  - Edge case testing

- [ ] **Integration Testing**
  - [x] End-to-end analysis flows (Smoke tests passing)
  - [ ] Multi-tenant scenarios
  - [ ] Error recovery
  - [ ] Performance testing

- [ ] **User Acceptance Testing**
  - Beta testing with 3-5 customers
  - Feedback collection
  - Bug fixes
  - UX improvements

- [ ] **Security Testing**
  - Security audit
  - Vulnerability scanning
  - Compliance review

### Phase 5: Marketplace Preparation (Weeks 21-24)
- [ ] **Documentation**
  - User guide
  - Admin guide
  - API documentation
  - Video tutorials

- [ ] **Marketing Materials**
  - App listing description
  - Screenshots (5-10)
  - Demo video
  - Case studies (if available)

- [ ] **Marketplace Submission**
  - Complete vendor profile
  - Submit app for review
  - Address review feedback
  - Security review compliance

- [ ] **Launch Preparation**
  - Pricing page setup
  - Support system (Zendesk/Intercom)
  - Onboarding flow
  - Customer success materials

### Phase 6: Launch (Week 25+)
- [ ] **Soft Launch**
  - Limited release to early adopters
  - Monitor metrics
  - Quick bug fixes
  - Gather feedback

- [ ] **Public Launch**
  - Marketplace publication
  - Marketing campaign
  - Customer onboarding
  - Support ramp-up

---

## 3. Technical Plan

For detailed architecture, technology stack, and migration plans, please refer to the [Technical Roadmap](TECHNICAL_ROADMAP.md).
