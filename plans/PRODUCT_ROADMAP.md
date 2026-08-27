# Autonomous Mortgage Advisor — Product Roadmap

**Last Updated**: August 27, 2026
**Status**: 🟡 **PRE-POC** | No code written. This document defines what gets built and in what order.

---

## Executive Summary

Build a system that performs every function of a human Israeli mortgage advisor without a human in the loop. The advisor's job decomposes into six capability areas — financial assessment, mix construction, bureaucracy and pre-approval, rate tenders and negotiation, execution support, and post-disbursement management. The POC must demonstrate that a machine can carry a file through the first four autonomously, because those four contain all the logic and all the communication that make the human expensive.

**Design principle — text-first, voice on escalation.** The default channel for both borrower and lender interaction is text. Voice exists because a banker will sometimes insist on a call, and a file must never stall on that. Voice is the last line of defense, not the primary interface. This shapes the architecture: the escalation boundary is designed in from week one, but the voice implementation lands in weeks 9–12.

**Design principle — the machine does not do arithmetic.** Every number that reaches a client or a lender is produced by a deterministic, tested financial engine. The LLM handles language, documents, judgment, strategy, and negotiation. It never computes a payment, never evaluates a regulatory cap, and never decides whether a structure is legal. This is the difference between a product and a liability.

**POC scope**: a single file carried autonomously from Hebrew conversational intake through document parsing, profiling, affordability, compliant mix optimization, a 5-lender simulated tender with multi-round negotiation, and a client-facing recommendation report — on test data, with a complete audit trail, in 8 weeks.

---

## Current Status

**⬜ Nothing built.** Planning phase.

**Blocking pre-work before Week 1:**
- [ ] Legal opinion on credit-brokerage licensing (`REGULATORY_STRATEGY.md` §2) — does not block POC coding, blocks the friendly-file pilot
- [ ] Collect and de-identify a corpus of real Israeli financial documents for the parsing test set: payslips (תלוש שכר), bank statements (תדפיס חשבון), Form 106, self-employed tax assessments (שומה), balance confirmations, purchase contracts. **Target: 60+ documents across 8+ formats.** This is the single most schedule-critical input to the POC and it has no engineering dependency, so it starts immediately and in parallel.
- [ ] Confirm Bank of Israel Directive 329 parameter set with a mortgage professional (the rules engine's correctness is only as good as this input)
- [ ] Baseline market rate data: BoI published average rates by track and month, for the achieved-rate-delta metric

---

## 1. Capability Inventory

Mapping the human advisor's job to system capabilities, with priority. **P0** = required for POC. **P1** = required for first real client. **P2** = required to scale. **P3** = later.

### 1.1 Financial Assessment & Strategic Planning

| # | Capability | Priority | Notes |
|---|-----------|----------|-------|
| 1 | Hebrew conversational intake producing a structured financial profile | **P0** | Text chat. Must handle the messy reality of how people describe income. |
| 2 | Document ingestion & parsing: payslips, bank statements, Form 106, tax assessments, balance confirmations, purchase contracts | **P0** | Hebrew RTL, scans, photos, mixed-quality PDFs |
| 3 | Cross-document validation & discrepancy detection | **P0** | Declared income vs. payslip vs. statement deposits. Catching these is a core advisor skill. |
| 4 | Affordability engine: max sustainable monthly payment, max loan, purchase budget | **P0** | Deterministic |
| 5 | Auxiliary cost modeling: purchase tax (מס רכישה), legal fees, agent fees, appraisal, renovation, registration fees, betterment levy | **P0** | Borrowers systematically underestimate these; modeling them is a visible trust win |
| 6 | Employment stability & income-quality assessment | **P1** | Tenure, contract type, sector risk, income volatility |
| 7 | Credit profile assessment | **P1** | Consumer-consented credit bureau report (see `REGULATORY_STRATEGY.md` §4) |
| 8 | Future financial alignment: savings-fund maturity (קרן השתלמות, פיצויים), anticipated career and family changes | **P1** | Drives track duration and prepayment planning — high-value advisory judgment |
| 9 | Self-employed income normalization | **P2** | Multi-year assessments, add-backs, seasonality |

### 1.2 Custom Mortgage Mix Construction

| # | Capability | Priority | Notes |
|---|-----------|----------|-------|
| 10 | Amortization engine: Spitzer (שפיצר), equal-principal (קרן שווה), balloon/grace (בלון/גרייס) | **P0** | Deterministic, exhaustively unit-tested |
| 11 | Full track model: fixed unlinked (קל"צ), fixed CPI-linked (ק"צ), prime, variable unlinked (מל"צ), variable CPI-linked (מ"צ) | **P0** | Including anchor reset mechanics and reset periodicity |
| 12 | Directive 329 compliance engine | **P0** | LTV 75/70/50%, variable ≤ 2/3, fixed ≥ 1/3, PTI ≤ 50%, term ≤ 30 years. **Hard gate — no non-compliant structure may leave the system.** |
| 13 | Multi-objective mix optimizer | **P0** | Minimize total cost subject to payment ceiling, volatility tolerance, and regulatory caps |
| 14 | Stress testing: rate shock, CPI shock, income shock, combined | **P0** | Every recommendation ships with its stress results |
| 15 | Prepayment penalty modeling (עמלת פירעון מוקדם) | **P1** | Capitalization, index, and operational components. Essential for refinance analysis. |
| 16 | Scenario alignment to expected life events | **P1** | Track durations aligned to known future liquidity |
| 17 | Refinance feasibility & break-even analysis | **P1** | Lead product for launch — see §3 Phase 1 |
| 18 | Early-payoff strategy optimizer | **P2** | Which track to pay down first, and when |

### 1.3 Bureaucracy & Pre-Approval

| # | Capability | Priority | Notes |
|---|-----------|----------|-------|
| 19 | Document checklist generation & autonomous chasing | **P0** | Per-lender requirements differ. Chasing a borrower for a missing document is pure advisor labor. |
| 20 | Lender-ready file package assembly | **P0** | Standardized submission package per lender format |
| 21 | Autonomous email communication with lenders, under borrower power of attorney | **P0** | Human-presenting, brand identity, factually accurate. Boundaries in `REGULATORY_STRATEGY.md` §5. |
| 22 | Pre-approval (אישור עקרוני) submission and tracking across lenders | **P0** | Simulated in POC, real in Phase 1 |
| 23 | Power of attorney generation & e-signature collection | **P1** | Legal prerequisite for any real file |
| 24 | Complex-credit case handling: low score, irregular income, non-standard employment | **P2** | The segment human advisors underserve and price highest |
| 25 | Non-bank lender coverage | **P2** | Insurance companies, credit funds, Magshimim-type lenders |

### 1.4 Rate Tenders & Negotiation

| # | Capability | Priority | Notes |
|---|-----------|----------|-------|
| 26 | Simultaneous multi-lender tender on an identical mix | **P0** | Identical mix is what makes offers comparable — a real advisor discipline |
| 27 | Structured offer parsing from unstructured lender replies | **P0** | Free-text email, attached PDFs, tables |
| 28 | Normalized offer comparison: total cost over life, not headline rate | **P0** | Headline-rate comparison is the mistake that costs borrowers the most |
| 29 | Multi-round autonomous negotiation | **P0** | Leverage competing offers, apply escalation tactics, know when to stop |
| 30 | Negotiation playbook & tactic library | **P0** | Encoded bargaining strategy: anchoring, competitive pressure, deadline use, walk-away framing |
| 31 | **Counterparty memory**: per-banker and per-branch behavioral model | **P0** | Responsiveness, concession patterns, tone preference, channel preference, what worked and what didn't. **The compounding asset.** |
| 32 | Market rate benchmarking against BoI published averages | **P1** | Powers both negotiation ("your offer is above market for this mix") and our public rate benchmark marketing asset |
| 33 | Voice escalation when a banker requires a call | **P1** | Weeks 9–12. Full-duplex Hebrew. |
| 34 | Macro context awareness: BoI rate path, CPI, market conditions | **P1** | Informs both advice and negotiation posture |

### 1.5 Execution & Signing Support

| # | Capability | Priority | Notes |
|---|-----------|----------|-------|
| 35 | Final contract verification against negotiated offer | **P1** | Clause and schedule diff. Catching a bank's "error" here is a headline advisor value. |
| 36 | Discrepancy detection & autonomous escalation | **P1** | Rate mismatch, term drift, undisclosed fees, changed linkage |
| 37 | Execution-step guidance: appraisal, title registration, cautionary note (הערת אזהרה), pledge registry | **P1** | Borrower-facing checklist + coordination |
| 38 | Appraiser coordination | **P2** | Scheduling, instruction, report intake |
| 39 | Mortgage life & property insurance tender | **P2** | **Requires insurance agent license** — see `BUSINESS_PLAN.md` §2.3 |
| 40 | Lawyer / title registry coordination | **P2** | |
| 41 | Disbursement tracking to seller or contractor | **P2** | |

### 1.6 Post-Disbursement & Refinancing

| # | Capability | Priority | Notes |
|---|-----------|----------|-------|
| 42 | Refinance monitoring engine with event-driven alerting | **P2** | Powers Mortgage Watch. Triggers on BoI rate moves, CPI prints, anchor reset dates. |
| 43 | Anchor-reset calendar per client loan | **P2** | The predictable, schedulable refinance trigger |
| 44 | Early-payoff advisory triggered by client liquidity events | **P2** | |
| 45 | Portfolio dashboard for the client's live mortgage | **P2** | |

### 1.7 Agent Harness (Cross-Cutting)

| # | Capability | Priority | Notes |
|---|-----------|----------|-------|
| 46 | Durable multi-week workflow state, restart-safe | **P0** | A file lives 4–8 weeks. Non-negotiable foundation. |
| 47 | Scheduled follow-ups and autonomous timer-driven action | **P0** | "No reply in 48h → follow up" is most of a broker's calendar |
| 48 | Email gateway: send, receive, thread, attach | **P0** | Dedicated domain, SPF/DKIM/DMARC, warmed sending reputation |
| 49 | Immutable action ledger | **P0** | Every autonomous action logged, replayable, defensible |
| 50 | Guardrails: irreversible-action circuit breaker, spend caps, escalation triggers | **P0** | Full autonomy needs *bounds*, not approvers |
| 51 | Simulated lender environment | **P0** | POC demo surface **and** the permanent eval/regression suite |
| 52 | Evaluation harness with golden files | **P0** | See §4 |
| 53 | Entity, episodic, and semantic memory | **P0** | Contacts, interaction history, market and tactical knowledge |
| 54 | Full-duplex Hebrew voice (STT + TTS + turn-taking) | **P1** | Weeks 9–12 |
| 55 | WhatsApp channel | **P2** | Dominant Israeli consumer channel; borrower-side first |
| 56 | Client web portal | **P2** | POC uses a minimal chat UI only |
| 57 | Lender portal automation | **P3** | Deferred deliberately — legally fraught, see `REGULATORY_STRATEGY.md` §7 |
| 58 | Open banking integration | **P3** | Explicitly out of scope until the regulatory path is clear |
| 59 | **Fleet of advisor identities**: multiple real registered brand entities | **P3** | Segment specialization (foreign investor, refinance, complex credit). Each identity is a real registered entity — see `REGULATORY_STRATEGY.md` §5.6. |
| 60 | **Persistent identity-to-banker assignment** | **P3** | Converts volume into relationship capital: a banker always deals with the same brand. Unique per (institution, branch) and per (borrower, institution). |
| 61 | Best-banker selection from learned institution policy | **P2** | Uses `contact_memory` + `institution_policy` to route a file to the banker most likely to escalate a pricing exception internally. Who carries the file inside the bank matters more than which branch it enters. |
| 62 | Competitive pressure in negotiation via named competing offers | **P1** | A genuine competing offer gives the banker the internal justification they need to request a pricing exception. |

---

## 2. The POC — 8 Weeks

### 2.1 Definition of Done

A single command starts a file. With **zero human intervention thereafter**, the system:

1. Conducts a Hebrew text conversation with a simulated borrower, extracting a complete financial profile and asking for what's missing
2. Ingests a folder of real (de-identified) Israeli financial documents, parses them, and reconciles them against the conversation — surfacing at least one planted discrepancy
3. Computes affordability, maximum loan, purchase budget, and the full auxiliary cost schedule
4. Generates 3 candidate compliant mixes, stress-tests each, and selects one with a written Hebrew rationale
5. Assembles a lender-ready file package and emails a tender to 5 simulated lenders
6. Parses their replies, normalizes offers to total-cost-over-life, and runs at least 2 negotiation rounds against each, using competing offers as leverage
7. Records what each simulated banker did into counterparty memory
8. Produces a Hebrew client report: offers compared, recommendation, reasoning, total cost, stress results, next steps
9. Survives being killed and restarted at any point without losing or duplicating work
10. Emits a complete audit trail from which every number and every sent message can be reconstructed

**Hard acceptance gates:**
- **Zero** Directive 329 violations across the full eval suite
- **Zero** LLM-generated numbers in any client- or lender-facing output — every figure traceable to the financial core
- 100% of outbound messages present in the immutable ledger
- Restart-safety demonstrated by killing the orchestrator mid-tender

### 2.2 Explicitly Out of Scope for the POC

Voice (weeks 9–12) · real banks · real clients · credit bureau · insurance · appraisal/legal coordination · refinance monitoring · WhatsApp · client portal beyond a bare chat UI · payment collection · non-bank lenders · open banking.

### 2.3 Week-by-Week

**Weeks 1–2 — Foundation & Financial Core**
- [ ] Repo, CI, Docker Compose local stack, Postgres + pgvector, Temporal, object storage
- [ ] **Financial core**: amortization (Spitzer, equal-principal, balloon/grace), all 5 track types with anchor-reset mechanics
- [ ] **Directive 329 rules engine** with 100% branch coverage
- [ ] Affordability + auxiliary cost model
- [ ] Immutable action ledger
- [ ] Golden-file test set: 20 hand-computed mortgage scenarios validated against an independent source

*Why first: everything downstream depends on these numbers being right, and this is the only part of the system with an objectively verifiable correct answer. If it slips, nothing else matters.*

**Weeks 2–3 — Document Pipeline** *(parallel with financial core)*
- [ ] Vision-model parsing for the 8 target Hebrew document types
- [ ] Hebrew RTL and mixed-script handling; scan and photo quality tolerance
- [ ] Structured extraction schemas per document type
- [ ] Cross-document reconciliation and discrepancy detection
- [ ] Accuracy measurement against the 60-document labeled corpus. **Target: > 95% field-level accuracy on gross/net income, > 98% on identity fields.**

**Weeks 3–4 — Agent Harness & Intake**
- [ ] Temporal workflow definitions for the file lifecycle
- [ ] Agent harness: tool registry, supervisor and specialist agents, structured outputs
- [ ] Guardrails: irreversible-action circuit breaker, spend caps, escalation triggers
- [ ] Hebrew conversational intake agent + minimal chat UI
- [ ] Entity / episodic / semantic memory schemas
- [ ] Restart-safety test: kill and resume at 10 different workflow points

**Week 4–5 — Mix Optimizer**
- [ ] Constraint-based multi-objective optimizer over the compliant solution space
- [ ] Stress-test battery (rate, CPI, income, combined)
- [ ] Candidate generation and selection with a natural-language rationale
- [ ] Verification pass: an independent second implementation recomputes every recommended mix and must agree to the agora

**Weeks 5–6 — Simulated Lender Environment**
- [ ] 5 simulated lenders with **distinct, documented policy profiles and personalities**: differing risk appetite, margin discretion, responsiveness, negotiation style, preferred channel, and one who insists on a phone call (to exercise the escalation boundary before voice exists)
- [ ] Email gateway: send, receive, thread, attach, against a local mail server
- [ ] Realistic reply generation, including free-text offers and PDF term sheets
- [ ] Scenario configuration for the eval suite

*Why this is P0 and not a testing convenience: this environment is how we will regression-test negotiation behavior forever. It is production infrastructure.*

**Weeks 6–7 — Tender & Negotiation**
- [ ] Tender orchestration: simultaneous submission, per-lender tracking, timers, follow-ups
- [ ] Offer parsing from unstructured replies into a normalized schema
- [ ] Total-cost-over-life comparison
- [ ] Negotiation agent + tactic playbook
- [ ] Counterparty memory writes and reads across rounds
- [ ] Escalation-boundary interface defined and stubbed for voice (the stub logs "would call" and continues via text)

**Weeks 7–8 — Reporting, Eval & Hardening**
- [ ] Hebrew client report generation
- [ ] Full evaluation harness (§4) running in CI
- [ ] End-to-end run on 10 varied golden files
- [ ] Audit trail completeness verification
- [ ] Observability: LLM tracing, cost per file, workflow dashboards
- [ ] Demo script and recorded walkthrough

### 2.4 Schedule Risk — Stated Plainly

Eight weeks for this scope with a small team is aggressive. The three most likely slip sources, in order:

1. **Hebrew document parsing.** Real Israeli payslips are visually chaotic and vary by payroll provider. If the 60-document corpus is not assembled in week 1, this slips and takes the POC with it. **Mitigation: corpus collection starts before week 1 and is the one task with no engineering dependency.**
2. **Negotiation quality.** Producing an agent that negotiates *well* rather than merely politely is open-ended research. **Mitigation: the acceptance bar for the POC is "completes multiple coherent rounds and uses competing offers as leverage," not "beats a human negotiator." Beating humans is a Phase 1 measurement, not a POC gate.**
3. **Optimizer scope creep.** The compliant mix space is large and the temptation to over-engineer is strong. **Mitigation: fixed candidate-generation budget; 3 candidates is the deliverable.**

**If the schedule must be cut, cut in this order**: (1) reduce simulated lenders from 5 to 3, (2) reduce negotiation to a single round, (3) narrow document types from 8 to 5. **Never cut**: the financial core, the compliance engine, the audit ledger, or restart safety. Those four are what make the thing legitimate rather than a demo.

---

## 3. Post-POC Phases

### Phase 0.5 — Voice Escalation (Weeks 9–12, parallel with pilot prep)

- [ ] Hebrew STT: streaming transcription tuned for telephony audio
- [ ] Hebrew TTS with a consistent, natural brand voice
- [ ] Turn-taking: VAD, barge-in, backchanneling, latency budget under 800ms perceived
- [ ] Telephony: Israeli DID, inbound and outbound, recording with lawful notice
- [ ] Voice negotiation agent sharing the text agent's memory and playbook
- [ ] Escalation trigger logic: when does text hand off to voice
- [ ] **Gate**: complete a full negotiation round by phone with the simulated banker who insists on calls

### Phase 1 — Friendly-File Pilot & Soft Launch (Months 3–6)

**The pilot is the highest-information experiment in the entire plan.** 5 real refinance files from personal network, executed autonomously against real banks, with a human observing who is empowered to abort but **not** to assist. Assisting destroys the measurement.

- [ ] Power of attorney generation and e-signature
- [ ] Real lender email channel with warmed sending domain
- [ ] Credit bureau integration (consumer-consented)
- [ ] Refinance feasibility & prepayment penalty modeling
- [ ] Final contract verification & discrepancy detection
- [ ] Execution-step guidance
- [ ] Employment stability and income-quality assessment
- [ ] Future financial alignment (savings-fund maturity)
- [ ] Fee collection
- [ ] Client portal v1
- [ ] Amendment 13 compliance implementation complete and audited

**Phase gate to soft launch**: 5 pilot files, ≥ 3 funded, zero compliance violations, zero incidents requiring reversal of an autonomous action.

### Phase 2 — Scale (Months 6–12)

- [ ] Standard purchase files (Month 6)
- [ ] Refinance monitoring engine + Mortgage Watch (Month 7)
- [ ] Channel partner tooling: referral tracking, partner dashboard (Month 7)
- [ ] Complex credit case handling (Month 10)
- [ ] Non-bank lender coverage
- [ ] Appraiser coordination
- [ ] WhatsApp borrower channel
- [ ] Anchor-reset calendar
- [ ] Early-payoff optimizer
- [ ] Public monthly rate benchmark (marketing asset built from tender data)

### Phase 3 — Adjacencies (Months 12–24)

- [ ] Insurance tender (**gated on license path**)
- [ ] Lawyer / title registry coordination
- [ ] Disbursement tracking
- [ ] Loan consolidation and all-purpose credit files
- [ ] Lender-side SaaS: file-structuring pipeline sold to non-bank lenders
- [ ] Self-employed income normalization at depth
- [ ] Fleet of advisor identities + persistent banker assignment

### Phase 4 — Deferred Pending Regulatory Clarity

- [ ] Lender portal automation with borrower-delegated credentials
- [ ] Open banking integration
- [ ] Geographic expansion

---

## 4. Evaluation Harness

The example project this methodology came from could rely on human judgment of output quality. We cannot — an autonomous system handling money needs measurable correctness, and it needs it in CI from week one. This section is a first-class deliverable, not test infrastructure.

| Eval | Method | Gate |
|------|--------|------|
| **Financial correctness** | 20+ hand-computed scenarios, independently verified | 100% exact to the agora |
| **Compliance** | Adversarial generation of near-boundary structures | **Zero** violations, permanently |
| **Document parsing** | 60-document labeled corpus | > 95% on income fields, > 98% on identity fields |
| **Discrepancy detection** | Planted inconsistencies across document sets | > 90% caught, < 5% false positive |
| **Optimizer quality** | Compare to brute-force optimal on a reduced solution space | Within 0.5% of optimal total cost |
| **Negotiation efficacy** | Simulated lenders with known concession curves; measure rate achieved vs. the known achievable floor | > 70% of available concession captured |
| **Offer parsing** | Labeled corpus of lender reply formats | > 95% field accuracy |
| **Workflow durability** | Chaos testing: kill at N points, verify no loss or duplication | 100% recovery |
| **Cost per file** | LLM tracing on every eval run | Within 2× of the model in `BUSINESS_PLAN.md` §3.1 |
| **Audit completeness** | Reconstruct every output and message from the ledger alone | 100% |

The negotiation eval is the interesting one. Simulated lenders with *known* concession curves mean we can measure how much of the theoretically available discount the agent actually captured — a ground truth that does not exist with real banks. This makes the simulated environment permanently more valuable for improving negotiation than production traffic is.

---

## 5. What Would Make Us Stop

Stating kill criteria in advance, because the sunk-cost pressure in Month 5 will be considerable:

1. **Banks systematically reject or filter the channel** and the human-relay fallback erodes the cost advantage below roughly 40%. The business becomes a normal brokerage with better software.
2. **Close rate stays under 25%** after 15 real files. Borrowers or banks will not complete a file without a human.
3. **A compliance violation or material financial error reaches a real client.** Stop, remediate, and re-examine whether full autonomy is the right target before resuming.
4. **Achieved-rate delta is worse than −0.10%.** If we cannot negotiate materially better than a borrower alone, the value proposition is convenience only, which does not support the price.
5. **A competitor ships genuine full autonomy first** and reaches meaningful volume. Reassess as an acquisition or partnership rather than a race.

---

## Related Documents

- **`BUSINESS_PLAN.md`** — market, pricing, unit economics, GTM
- **`TECHNICAL_ROADMAP.md`** — architecture and implementation detail for every capability above
- **`REGULATORY_STRATEGY.md`** — licensing, privacy, and the constraints that shape several P0/P1 items

---

**Document Version**: 1.0
