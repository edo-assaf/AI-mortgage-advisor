# Autonomous Mortgage Advisor — Business Plan

**Last Updated**: August 27, 2026
**Status**: 🟡 **PRE-POC** | Planning phase, no code written
**Market**: Israel (Hebrew-first)
**Currency**: All revenue and pricing in ILS (₪). Cloud/LLM costs in USD, converted at ₪3.30/$.

---

## Executive Summary

**Product**: A fully autonomous AI mortgage advisor for the Israeli market. It replaces the human mortgage broker end-to-end — profiling the borrower, constructing an optimal loan-track mix, preparing the document file, running a competitive tender across all lenders, negotiating rates, verifying the final contract, and monitoring for refinancing opportunities for the life of the loan.

**Why Israel, why now**: Mortgage advisory in Israel requires **no license** (unlike investment advice, which is licensed under the Regulation of Investment Advice Law), and the 2016 Regulated Financial Services Law contains no licensable activity that captures it. A licensing bill cleared committee in May 2026, so this window is open but not indefinitely — see `REGULATORY_STRATEGY.md` §9.1. Human advisors charge ₪3,000–₪14,000 per file for work that is overwhelmingly document handling, structured optimization, and repetitive multi-party negotiation — all of which are now automatable. The market is large (₪105.7B in new mortgages in 2025) and the advisory layer is fragmented across thousands of sole practitioners.

**Technology**: A durable agent harness (Temporal-orchestrated, multi-week stateful workflows) wrapping a **deterministic financial core**. The LLM never performs arithmetic or evaluates regulatory compliance; it handles language, documents, judgment, and negotiation. A simulated-lender environment serves as both the POC demonstration surface and the permanent regression suite.

**Business Model**: Flat fee per closed file, paid on success, priced at roughly **40–50% of the human advisor rate**, plus a low-cost recurring refinance-monitoring subscription.

| Product | Price | Human advisor benchmark |
|---------|-------|------------------------|
| Instant analysis & mix recommendation | **Free** | ₪500–1,500 (single consultation) |
| Refinance file (מיחזור), end-to-end | **₪1,490** | ₪3,000–6,000 |
| Standard purchase file (salaried, single property) | **₪2,490** | ₪4,500–6,000 |
| Complex file (self-employed, multi-borrower, investment, non-resident) | **₪3,990** | ₪6,000–14,000 |
| Mortgage Watch (refinance monitoring) | **₪190/year** | Not offered |

**Key Metrics (First 18 Months Post-POC):**
- Phase 1 (Months 1–6): 25 closed files/month, ₪56K MRR
- Phase 2 (Months 7–18): 150 closed files/month, ₪358K MRR (₪4.3M ARR)
- Gross margin ~95% (marginal cost per file ≈ ₪53 modeled, under ₪265 even at a 5× overrun)
- Cost of goods per file is **~1/30th** of a human advisor's cost to serve
- Target CAC: ₪250–450 blended
- Attach rate on Mortgage Watch: > 60% of closed files

**Competitive Advantage — the one that matters:**

Every incumbent in this space (Walty, Cantaio, Mimi AI, Finwiz) is an **AI-assisted human advisory business**. Their marketing leads with the algorithm; their delivery leads with "our team of expert advisors." That means their cost to serve is a salaried human, which floors their price at roughly the human rate and caps their throughput at roughly the human rate.

We remove the human from the loop entirely. That is not a feature difference, it is a **cost-structure difference**, and it is the only thing that lets us charge ₪2,490 profitably while running 150 files a month with a team of four. Every other differentiator in this plan is downstream of that one.

---

## 1. Market Analysis

### 1.1 Market Size (2025 actuals)

| Metric | Value | Source |
|--------|-------|--------|
| New mortgage volume, 2025 | **₪105.7B** (+13.4% YoY) | Bank of Israel |
| New mortgage volume, 2024 | ₪93.2B | Bank of Israel |
| New mortgage volume, 2023 (trough) | ₪71.1B | Bank of Israel |
| New mortgage volume, 2022 (peak) | ₪117.6B | Bank of Israel |
| Refinancing volume (מיחזורים), 2025 | **~₪36B** | Bank of Israel |
| Average mortgage size | **₪1.03M** | Bank of Israel |
| Average mortgage term | **~27 years** (up from 22 in 2019) | Yad2 / BoI |
| Apartment transactions, 2025 | ~85,000–91,000 (20-year low, −11% YoY) | Chief Economist / CBS |
| Unsold new apartments | ~86,000 (all-time high) | CBS |

### 1.2 File Volume and Addressable Revenue

Volume divided by average loan size gives roughly **103,000 mortgage files per year**, which we split as:

| Segment | Est. files/year | Our price | Segment value |
|---------|----------------|-----------|---------------|
| Purchase — standard (salaried) | ~45,000 | ₪2,490 | ₪112M |
| Purchase — complex | ~22,000 | ₪3,990 | ₪88M |
| Refinance | ~35,000 | ₪1,490 | ₪52M |
| **Total addressable (TAM)** | **~102,000** | — | **~₪252M/yr** |

Industry practitioners estimate **40–50% of files currently use a paid advisor**. That advised slice is our directly serviceable market:

- **SAM (advised files today)**: ~46,000 files/yr ≈ **₪113M/yr**
- **Expansion opportunity**: our price point makes advice economic for the 55,000+ files/yr that currently go unadvised — borrowers who negotiate alone and, per advisor estimates, leave ₪50,000–200,000 on the table over the loan's life.
- **Recurring layer**: Mortgage Watch at ₪190/yr against a 27-year average term. At 150 files/month with 60% attach, this compounds to **~₪209K/yr of recurring revenue by end of Year 2** and keeps growing with no churn pressure from the underlying transaction.

**Honest constraint**: Israel-only, this is a **~₪250M/yr ceiling**. A business capturing an impressive 10% of TAM tops out around ₪25M/yr revenue. This is a genuinely profitable company but not a large one on domestic mortgage fees alone. See §7 for the three expansion vectors that break the ceiling.

### 1.3 Structural Tailwinds

1. **Affordability stress is the product's best salesman.** Average term stretched from 22 to 27 years since 2019 purely to compress monthly payments. Payment-to-income capacity is eroding and arrears are rising. Optimization has never mattered more per borrower.
2. **Refinancing is structurally about to spike.** Bank of Israel analysis shows refinancing typically occurs 40–50 months after origination, timed to variable-rate anchor resets. The 2020–2021 origination peak — during which ~20% of new loans were CPI-linked variable with 5-year anchor resets — reaches its reset window across 2025–2026. Refinance is our cheapest, fastest, lowest-risk file type and its volume is arriving on a schedule.
3. **Developers are desperate.** ~86,000 unsold new units and a 34% collapse in free-market developer sales. Developers are running aggressive 20/80 financing promotions and will happily route buyers to anyone who can get a mortgage approved. This is a high-volume B2B2C channel available on favorable terms right now.
4. **Regulatory transparency reform.** The Bank of Israel's mortgage transparency and competition reform standardized how banks present offers, which materially lowers the cost of automated cross-lender comparison — the reform was designed to help consumers compare, and it helps machines compare even more.

### 1.4 Competitive Landscape

| Competitor | Model | Positioning | Our advantage |
|-----------|-------|-------------|---------------|
| **Walty** (2018, TLV, ~5 employees) | AI advisory + digital acquisition, B2C + B2B lender partnerships. Founder previously built Israel's National Credit Registry at D&B and co-authored the operational model behind the Credit Data Law. | Claims category market leader | Deep credit-data expertise and lender relationships we lack. But still human-in-the-loop advisory; 8 years in with ~5 staff suggests throughput is people-bound. |
| **Cantaio** | Algorithmic multi-bank tender + negotiation, app-based. Team of ex-senior bankers, ex-Finance Ministry, ex-8200. | "Our algorithm negotiates across all banks and non-bank lenders" | Closest to our thesis and the most credible threat. But delivery is explicitly "our team combines experience with our technology" — humans run the negotiation. Their pricing sits at ₪6,000–12,000, i.e. human-rate. |
| **Mimi AI** | Free AI mortgage analysis: optimal mix, purchasing power, refinance feasibility, Directive 329-compliant calculations. | Free top-of-funnel, monetized via human advisors | Validates our free-analysis funnel and confirms the calculation layer is table stakes, not a moat. Their conversion still requires human advisors. |
| **Finwiz** | Content + calculator suite (mix simulator, max-mortgage, early-repayment fee, life insurance comparison) + consulting. Strong English-language/foreign-investor niche. | SEO and tools authority | Dominant organic content position we must respect and route around. Tools are free lead-gen for human consulting. |
| **~2,000+ independent advisors** | Sole practitioners, ₪3,000–14,000/file | Relationship and trust | Structurally cannot match our price or throughput. Also our best referral partners for files we choose not to serve. |

**What this means strategically:**

The calculation engine is **not** a moat — Mimi AI gives it away free. The moat candidates, in order of durability:

1. **Autonomous execution of the bureaucratic and negotiation middle.** Nobody automates document chasing, multi-round lender negotiation, appraisal/legal/insurance coordination, and contract verification. This is 80% of the labor and 100% of what makes the human expensive.
2. **Counterparty memory.** A per-banker behavioral model — who responds fast, who concedes under competitive pressure, who needs a phone call, which branch has discretion on margins — compounds with every file and cannot be bought. This is the asset that gets better than a human advisor rather than merely cheaper, because no individual human works 150 files a month across every lender.
3. **Cost structure.** Enables a price no competitor can match without abandoning their delivery model.

**What this also means honestly**: we are late, not first. Walty has 8 years and credit-registry expertise. Cantaio has banking insiders. We win on architecture and price, not on being novel.

---

## 2. Business Model & Pricing

### 2.1 Pricing Rationale

Human advisor benchmarks from the Israeli market:

| Service | Market rate |
|---------|-------------|
| Refinance file, standard | ₪3,000–6,000 |
| Refinance file, complex | ₪5,000–10,000 |
| Purchase file, simple | ₪3,000–4,500 |
| Purchase file, standard | ₪4,500–6,000 |
| Purchase file, complex | ₪6,000–8,000 |
| All-purpose / loan consolidation | ₪8,000–14,000 |
| Percentage-based alternative | 0.3%–1.0% of loan |

We price at **40–50% of the standard human rate**. This is deliberate and specific:

- **Not 10%.** A ₪300 price signals a calculator, not an advisor entrusted with the largest financial decision of a household's life. Trust is the binding constraint in this category, and price is a trust signal.
- **Not 90%.** At near-parity, the customer has no reason to accept a non-human, and we forfeit the only advantage we structurally own.
- **40–50% clears the "obviously worth trying" bar** while preserving > 90% gross margin and leaving room to discount through channel partners without going underwater.

### 2.2 Success-Based Fee Structure

Fee is charged **only on a closed, funded mortgage**. This matters more than it appears:

- It is the strongest possible trust device for an unlicensed, AI-delivered financial service. We are paid if and only if the client gets a mortgage.
- It aligns with how Israeli borrowers already expect advisors to be paid.
- It makes the free upfront analysis a genuine gift rather than a bait-and-switch.
- Our marginal cost per file (₪53 modeled, and non-closing files cost roughly 40% of that because they die before the expensive negotiation and verification stages) is low enough that we can absorb a high proportion of non-closing files. At a 45% close rate, fully-loaded direct cost per *closed* file is still under ₪100.

**Payment mechanics**: fee due at the point the lender issues final approval, collected before disbursement. We never touch client funds, mortgage proceeds, or lender funds at any point — a deliberate constraint that keeps us out of payment-services and AML licensing regimes entirely (see `REGULATORY_STRATEGY.md` §6).

### 2.3 Revenue Expansion (post-POC)

| Stream | Economics | Requirement |
|--------|-----------|-------------|
| **Mortgage Watch** subscription | ₪190/yr, ~₪8/yr marginal cost | Refinance monitoring engine (Phase 2) |
| **Mortgage life & property insurance** | ₪800–2,500 commission per file — this is where human brokers make their real margin | **Requires an insurance agent license.** Options: hire a licensed agent, partner for revenue share, or acquire a small agency. Flagged as the highest-value licensed adjacency. |
| **Refinance execution** on watched loans | ₪1,490 per event, ~1.5 events per loan lifetime | Nothing new — reuses the core product |
| **Lender-side SaaS** | Sell the underwriting-file-preparation layer to non-bank lenders drowning in unstructured borrower files | Phase 3, and **legally uncertain**: a 2026 Capital Market Authority draft circular bars non-bank lenders from paying mortgage advisors any benefit of economic value on housing loans. Requires a separate entity and counsel clearance — `REGULATORY_STRATEGY.md` §9.2. |

---

## 3. Unit Economics

### 3.1 Marginal Cost Per File

Modeled on a full autonomous file: ~40 pages of document parsing, ~60-turn Hebrew intake conversation, 5-lender tender at 3 negotiation rounds, contract verification, and 2–4 voice escalations.

| Cost component | Estimate (USD) | Notes |
|----------------|---------------|-------|
| Document parsing (vision model, ~40 pages) | $0.50 | Hebrew payslips, statements, tax forms |
| Intake conversation (~60 turns, prompt-cached) | $1.20 | Small/mid model |
| Mix optimization | $0.05 | **Deterministic solver** — near-zero LLM cost by design |
| Tender + negotiation (45 LLM calls, large context) | $4.00 | Mixed frontier/mini tiering |
| Contract verification & discrepancy detection | $1.50 | Frontier model, high stakes |
| Voice escalation (3 calls × 8 min) | $3.00 | STT ~$0.03/hr (self-hosted Hebrew Whisper), TTS self-hosted, realtime reasoning dominates |
| Telephony (Israeli DID + minutes) | $0.60 | |
| **Subtotal** | **$10.85** | |
| **+40% buffer** | **$15.20** | ≈ **₪50** |

| Scenario | Cost/file | Gross margin on ₪2,490 |
|----------|-----------|----------------------|
| Modeled | ₪50 | 98% |
| 3× worse than modeled | ₪150 | 94% |
| 5× worse than modeled | ₪250 | 90% |
| 10× worse (pathological) | ₪500 | 80% |

The margin is robust to being badly wrong about token consumption. This is the entire economic thesis: **an autonomous file costs roughly ₪50–250 to serve; a human file costs ₪1,500–3,000 in labor.**

### 3.2 Phase 1 — Months 1–6 Post-POC (25 closed files/month)

**Revenue mix:**

| Product | Files/mo | Price | Revenue/mo |
|---------|----------|-------|-----------|
| Refinance | 12 | ₪1,490 | ₪17,880 |
| Purchase, standard | 9 | ₪2,490 | ₪22,410 |
| Purchase, complex | 4 | ₪3,990 | ₪15,960 |
| Mortgage Watch (60% attach, annualized/12) | 15 | ₪190/yr | ₪238 |
| **Total** | **25** | avg ₪2,250 | **₪56,488** |

**Costs:**

| Line | Monthly |
|------|---------|
| Cloud infrastructure (see `TECHNICAL_ROADMAP.md`) | ₪2,970 (~$900) |
| LLM + voice (25 closed @ ₪53 + ~31 non-closing @ ₪21) | ₪1,980 |
| Team (4 people, below-market founder salaries) | ₪80,000 |
| Legal & compliance (retainer, DPO, E&O insurance) | ₪9,000 |
| Marketing & channel referral fees | ₪11,000 |
| Tools, telephony, domains, misc | ₪2,500 |
| **Total** | **₪107,450** |

**Phase 1 P&L**: −₪50,962/month. **Deliberately loss-making** — Phase 1 buys evidence (close rate, achieved-rate delta vs. market, bank tolerance of an autonomous counterparty), not profit. Contribution margin per closed file is already ₪2,171 before channel referral fees.

### 3.3 Phase 2 — Months 7–18 (150 closed files/month)

**Revenue mix:**

| Product | Files/mo | Price | Revenue/mo |
|---------|----------|-------|-----------|
| Refinance | 70 | ₪1,490 | ₪104,300 |
| Purchase, standard | 55 | ₪2,490 | ₪136,950 |
| Purchase, complex | 25 | ₪3,990 | ₪99,750 |
| Mortgage Watch (cumulative ~1,100 subs) | — | ₪190/yr | ₪17,417 |
| **Total** | **150** | avg ₪2,274 | **₪358,417** |

**Costs:**

| Line | Monthly |
|------|---------|
| Cloud infrastructure | ₪7,260 (~$2,200) |
| LLM + voice (150 closed @ ₪53 + ~185 non-closing @ ₪21) | ₪11,840 |
| Team (7 people) | ₪175,000 |
| Legal & compliance | ₪14,000 |
| Marketing & channel referral fees (₪350 × 150) | ₪52,500 |
| Tools, telephony, misc | ₪6,000 |
| **Total** | **₪266,600** |

**Phase 2 P&L:**
- Revenue: **₪358,417/month**
- Costs: ₪266,600/month
- **Net profit: ₪91,817/month (26% net margin)**
- **Gross margin (revenue less LLM/voice/infra): 95%**
- **Annualized: ₪4.30M revenue, ₪1.10M net profit**

The gap between 95% gross and 26% net is almost entirely headcount. That gap is the growth lever: files/month can roughly triple against the Phase 2 team before headcount must move again, because the marginal file consumes machine time rather than human time.

### 3.4 Break-Even

| Input | Value |
|-------|-------|
| Fixed monthly cost (Phase 1 team + compliance + tools + infra) | ₪94,470 |
| Average fee per closed file | ₪2,250 |
| Less direct cost (₪53) and allocated non-closing files (₪26) | −₪79 |
| Less channel referral fee | −₪350 |
| **Net contribution per closed file** | **₪1,821** |

Break-even at **~52 closed files/month** — reachable in Month 7–9 on the Phase 2 trajectory.

---

## 4. Go-To-Market

### 4.1 Positioning

The AI-native framing is a liability with a borrower deciding how to finance ₪1M+, and a liability with a mortgage banker deciding whether to take your file seriously. Positioning leads with **outcome and price**, not technology.

- **Headline**: "משכנתא שעברה מכרז בכל הבנקים. בלי לצאת מהבית. ₪2,490." *(A mortgage that went to tender at every bank. Without leaving home. ₪2,490.)*
- **Sub**: Full analysis free. You pay only if you get the mortgage.
- **Proof points**: rate achieved vs. the Bank of Israel published average for the same mix; total interest saved over the loan's life; days from intake to approval.

The "how" is described as *our system* and *our process*. Not deceptive, and not the lead. See `REGULATORY_STRATEGY.md` §5 for the precise boundary between positioning and misrepresentation, which is a hard architectural constraint, not a guideline.

### 4.2 Acquisition Channels

#### Channel 1: Free Instant Analysis + Organic Content (target 35% of files)
- **Mechanic**: Free tool answering the three questions every Israeli borrower actually has — *how much can I get*, *what's the right mix for me*, *is refinancing worth it*. Delivers a real, Directive 329-compliant answer with a full amortization schedule, then offers to execute.
- **Tactics**: Hebrew SEO on high-intent long-tail terms; a public Israeli mortgage-rate benchmark published monthly from our own tender data (a data asset no competitor can replicate and the single best link-earning and PR asset available to us); calculator suite.
- **CAC**: ~₪120 blended (content production amortized)
- **Note**: Finwiz and Mimi AI hold strong positions here. We compete on the benchmark data asset, not on out-writing them.

#### Channel 2: Real Estate Agent & Lawyer Referrals (target 30% of files)
- **Mechanic**: The highest-intent moment in the Israeli market is signing a purchase contract. Agents and real estate lawyers are present at that moment and are already informally asked "who should I use for the mortgage?"
- **Tactics**: ₪400 referral fee per closed file; co-branded landing pages; a partner dashboard showing their clients' file status (agents want to know the deal won't die at financing).
- **CAC**: ₪400 + ~₪80 partner management = **₪480**
- **Why it works**: agents are paid on closing, so a financing partner that reliably closes is worth more to them than the referral fee.

#### Channel 3: Developer & Contractor Partnerships (target 20% of files)
- **Mechanic**: ~86,000 unsold units and a 34% collapse in free-market developer sales. Developers with 20/80 promotions need buyers who can actually secure financing at the drawdown date.
- **Tactics**: Embedded financing-qualification widget in developer sales funnels; batch pre-qualification for entire projects; per-project revenue share.
- **CAC**: ~₪250 (volume deals, no per-file fee)
- **Why now**: this channel is available on unusually favorable terms specifically because the market is bad. It will get more expensive when the market recovers.

#### Channel 4: Community & Word-of-Mouth (target 10% of files)
- **Mechanic**: Israeli mortgage discussion is concentrated in large Facebook groups, WhatsApp communities, and forums. Participation is helpful-first, never promotional.
- **Tactics**: Publish the monthly rate benchmark into communities; answer questions substantively; referral incentive (₪300 credit both sides).
- **CAC**: ~₪60

#### Channel 5: Paid Search & Social (target 5% of files, scale lever)
- Held deliberately small in Phase 1. Mortgage keywords in Israel are expensive and Walty is an established bidder. Reserved as a growth lever once the close rate and achieved-rate advantage are proven with real numbers, at which point paid CAC becomes an arithmetic decision rather than a bet.
- **CAC**: ₪600–1,200 (estimated)

**Blended CAC target: ₪250–450.** Against a ₪2,274 average fee plus refinance and subscription follow-on, LTV:CAC lands in the 6–10× range.

### 4.3 Launch Sequence

| Stage | Timing | Goal |
|-------|--------|------|
| **POC complete** | Week 8 | Autonomous file executed end-to-end against simulated lenders on test data |
| **Friendly-file pilot** | Months 3–4 | 5 real files, all refinance, from personal network. **Gated on the POA template and consent flows in `REGULATORY_STRATEGY.md` §10**, not on a licensing answer — that question is closed. Fully autonomous execution against real banks, with a human observing and empowered to abort but not to assist. This is the single highest-information experiment in the plan. |
| **Soft launch** | Month 4 | Refinance only, 10 files/month, waitlisted. Refinance first: no purchase deadline, so a slow or failed file costs the client an opportunity rather than their home. |
| **Purchase files** | Month 6 | Add standard purchase after the refinance close rate stabilizes |
| **Channel activation** | Month 7 | Agent/lawyer and developer partnerships go live |
| **Complex files** | Month 10 | Self-employed, multi-borrower, investment |

**The gating discipline**: we do not add a segment until the prior segment's close rate exceeds 40% and zero compliance violations have been recorded. Reputational failure in this category is not recoverable — a single well-publicized case of an AI mishandling someone's home financing ends the company.

### 4.4 Retention

For a per-transaction business, retention means the *next* transaction:

- **Mortgage Watch** turns a one-time fee into a 27-year relationship, with the refinance monitoring engine generating a warm, high-intent event roughly every 40–50 months.
- **Early-payoff advisory** at bonus season and inheritance/savings-maturity events.
- **The file is the switching cost.** We hold the structured, validated financial profile. The second transaction costs the client almost nothing in effort and costs us almost nothing to serve.
- **Referral loop**: mortgage decisions are discussed intensely among peers at the same life stage. NPS is the highest-leverage metric in the business.

---

## 5. Key Performance Indicators

### Product Efficacy (the metrics that determine whether the business is real)
- **Achieved rate delta**: weighted-average rate obtained vs. the Bank of Israel published average for the same mix and month. **Target: −0.25% or better.** This single number is the product.
- **Total interest saved** over loan life vs. the borrower's first bank offer. Target: > ₪80,000 average.
- **Close rate** (signed engagement → funded loan). Target: > 45% Phase 1, > 60% Phase 2.
- **Autonomy rate**: files completed with zero human intervention. Target: > 80% Phase 1, > 95% Phase 2.
- **Time to approval-in-principle**. Target: < 6 days.
- **Compliance violation rate**: proposed structures breaching Directive 329. **Target: exactly zero, permanently.**

### Acquisition
Free-analysis completions; analysis → engagement conversion; CAC by channel; partner-sourced share; organic traffic and rate-benchmark citations.

### Financial
MRR/ARR; average fee per closed file; contribution margin per file; LLM+voice cost per file; Mortgage Watch attach rate; blended LTV:CAC.

### Operational & Risk
Voice escalation rate (how often text-only fails); lender responsiveness by institution and banker; document parse accuracy on Hebrew financial documents; discrepancy-detection catch rate on final contracts; **incidents where an autonomous action required reversal.**

---

## 6. Risk Analysis

### R1 — Banks refuse to deal with an autonomous counterparty 🔴 **Existential**
Banks may detect and block automated outreach, or refuse files from a non-human intermediary once the pattern is recognized.
**Mitigation**: Every file is submitted under a **signed borrower power of attorney** — legally identical to how human brokers operate, and the borrower is always a real, verifiable person with real documents. No inbound automation against bank systems and no credential-based portal access in Phase 1; all interaction runs over ordinary email and phone, the same channels human brokers use. Communications are rate-limited and paced to human norms. Fallback ladder: (1) voice escalation, (2) a human employee relays the file while the agent still does all preparation and analysis — this preserves ~70% of the cost advantage, (3) partnership with a licensed brokerage as the interface layer.
**Leading indicator to watch**: response-rate decay by institution over time. If a bank's response rate to us drops materially below baseline, we are being filtered.

### R2 — A financial error causes real client harm 🔴 **Existential**
A miscalculated affordability figure, a missed regulatory cap, or a misread contract clause harms a client materially. In this category, the reputational damage is unbounded and arrives faster than the legal damage.
**Mitigation**: **The LLM never computes and never adjudicates compliance.** All arithmetic runs in a deterministic, unit-tested financial core; all regulatory checks run in a rules engine with 100% branch coverage against Directive 329. Every recommendation carries a machine-generated rationale and a full audit trail. Professional indemnity / E&O insurance from day one, budgeted in Phase 1. Independent recomputation of every recommendation by a second implementation before it reaches a client. See `TECHNICAL_ROADMAP.md` §Key Decisions #2.

### R3 — Regulatory change introduces licensing 🔴 **High — now scheduled rather than speculative**
The credit-brokerage worry is closed: the 2016 Regulated Financial Services Law contains no licensable activity that captures borrower-paid advisory. But a **Regulation of Mortgage Advisory Bill was approved for first reading on 26 May 2026**, with readings two and three deferred to the next Knesset, and the December 2025 inter-agency report on AI in the financial sector recommends mandatory AI-interaction disclosure. Both are live.
**Mitigation**: See `REGULATORY_STRATEGY.md` §9.1 and §9.6. Determine early whether the bill's threshold conditions can be satisfied by a corporate provider or require a licensed natural person — if the latter, hire one before it is mandatory rather than after. Build the compliance and audit infrastructure a licensing regime would demand *before* it is demanded, so a licensing event eliminates unlicensed competitors rather than us. Maintain a licensed-partner relationship as a standing fallback.

### R4 — Capitalized competitor moves first 🟠 **High**
Walty or Cantaio removes their human layer, or a bank launches a free equivalent to foreclose the category.
**Mitigation**: Speed on the autonomous execution layer, which is the hard part and where we start from the same line. The counterparty-memory asset compounds from file one, so time-in-market is directly defensive. Banks are structurally conflicted — a bank cannot run a tender against itself, which is the entire product.

### R5 — Privacy breach 🟠 **High**
We concentrate the most sensitive financial data that exists about a household: income, statements, tax filings, credit history, identity documents.
**Mitigation**: Amendment 13 to the Privacy Protection Law (in force since 14 Aug 2025) applies to us on two independent grounds and carries significant administrative fines. Treated as a P0 engineering requirement, not a compliance checkbox: encryption at rest and in transit, field-level encryption for identity and income data, strict retention limits with automated purge, DPO appointed, and the 2017 Data Security Regulations built to the high tier even though our formal classification is medium until 100,000 data subjects. Detail in `REGULATORY_STRATEGY.md` §3.

### R6 — The "presenting as human" position backfires 🟠 **High**
A bank, journalist, or regulator frames the company as having deceived banks and borrowers.
**Mitigation**: A hard architectural boundary, enforced in code rather than policy. We operate under a company brand identity with documented borrower authorization; we never fabricate a named licensed individual, never forge a signature, and never assert a false fact about a borrower or a property. The Phase 3 fleet of advisor identities operates entirely within this boundary: every identity is a real registered entity, and identity has no natural-person name field. See `REGULATORY_STRATEGY.md` §5.3. Every outbound communication is logged immutably and is defensible on its face. Prepared public position: *we are a technology company acting as the borrower's authorized agent, and every factual statement we have ever made to a lender is true and on the record.* That position survives scrutiny; anything weaker does not. See `REGULATORY_STRATEGY.md` §5.

### R7 — Market freeze 🟡 **Medium**
Transactions already sit at a 20-year low. A rate shock, war escalation, or credit tightening could compress volume further.
**Mitigation**: Refinance is counter-cyclical to purchase volume and is our lead product — refinancing rose in 2025 while transactions fell 11%. Loan consolidation and all-purpose files are also counter-cyclical. Cost structure is variable and near-zero at idle.

### R8 — Trust barrier 🟡 **Medium**
Borrowers refuse to delegate the largest financial decision of their lives to a system.
**Mitigation**: Free analysis first — the product proves itself before any commitment. Success-only fee. Radical transparency in reasoning: every recommendation explains itself in plain Hebrew, with the alternatives considered and why they lost. Refinance-first launch targets the lowest-anxiety, most numerically-motivated segment.

### R9 — Hebrew voice quality is inadequate 🟡 **Medium**
Full-duplex Hebrew conversation with a banker fails on latency, accent, or naturalness, and the escalation path collapses.
**Mitigation**: Voice is explicitly the **last line of defense**, not the primary channel — the design assumption is that text handles the overwhelming majority of interactions. Voice is scheduled as a parallel spike (weeks 9–12), not on the POC critical path. If quality proves inadequate, a human handles the small residue of calls at a marginal cost that barely dents the model.

---

## 7. Breaking the ₪250M Ceiling

The domestic mortgage-fee market caps this business. Three vectors, in order of proximity:

0. **Segment-specialized brands (Year 2, low cost)**. The same harness behind several real registered brands — a foreign-investor brand, a refinance brand, a complex-credit brand — each with its own positioning, content, and channel mix. This is ordinary multi-brand practice, it lets us occupy several SEO and channel niches simultaneously rather than compromising one brand's message across segments, and the marginal cost is registration plus a landing page. Also delivers the relationship-continuity benefit described in `PRODUCT_ROADMAP.md` #60. Not a market-size expansion so much as a share-capture mechanism, which is why it sits at zero.

1. **Adjacent Israeli products with the same harness (Year 2)**. Mortgage life and property insurance is the immediate one — it is where human brokers earn their real margin and it requires an insurance agent license. Then loan consolidation, all-purpose credit against property, and commercial mortgage. Each reuses the same document parsing, financial core, tender, and negotiation machinery. Realistically **2–3× the addressable market** for incremental engineering.

2. **Lender-side SaaS (Year 2–3)**. Non-bank lenders and smaller institutions receive unstructured borrower files and underwrite them manually. We will have the best borrower-file-structuring pipeline in the market as a byproduct, and selling it back is a higher-multiple recurring revenue line. **Discount this option until cleared**: a 2026 draft circular bars non-bank lenders from paying mortgage advisors any benefit of economic value on housing loans, and the "representative" definition is drafted broadly to prevent relabelling. Viable only in a separate entity processing the lender's own inbound files, and only after counsel confirms the ban is not entity-agnostic (`REGULATORY_STRATEGY.md` §9.2).

3. **Geographic expansion (Year 3+)**. The harness is market-agnostic; the financial core, regulatory rules, language, and lender integrations are market-specific. The realistic first target is another market with an unlicensed or lightly-licensed brokerage layer, a bank-concentrated lending market, and a language where we have an edge. This is a rebuild of roughly 40% of the system per market — the agent harness, workflow engine, document pipeline, memory, and negotiation machinery all carry over.

---

## 8. Resourcing & Funding

**Posture**: Small self-funded team (4 people), targeting revenue before an external raise.

**Team (Phase 1)**: founder/product, two engineers (one on the agent harness and communications, one on the financial core and document pipeline), one part-time ops/compliance lead who also handles the friendly-file pilot supervision.

**Phase 1 capital requirement**: ~₪107K/month against ₪56K revenue, so **~₪51K/month burn for 6 months ≈ ₪306K**, plus a ₪150K pre-revenue POC period (8 weeks of team plus legal opinion plus infrastructure). **Total to break-even: roughly ₪600–700K.**

**Phase 2 becomes self-funding** at ~52 closed files/month.

**Whether to raise**: the case for staying self-funded is that the cost structure permits it and the constraint is trust and evidence rather than capital. The case for raising is R4 — if Walty or Cantaio removes their human layer, this becomes a land-grab and capital decides it. Recommended trigger: **raise if and only if the friendly-file pilot shows a close rate above 50% and an achieved-rate delta better than −0.25%.** Those two numbers turn a bet into an arithmetic problem, and that is the moment capital is worth its dilution.

---

## 9. Milestones

| Milestone | Timing | Success criteria |
|-----------|--------|-----------------|
| **M0 — POC complete** | Week 8 | One autonomous file, intake to recommendation, against 5 simulated lenders on test data. Zero compliance violations. Full audit trail. See `PRODUCT_ROADMAP.md` §POC. |
| **M1 — Voice escalation** | Week 12 | Full-duplex Hebrew call with a simulated banker, handled autonomously end-to-end |
| **M2 — First real close** | Month 4 | One real refinance funded, executed autonomously against real banks |
| **M3 — Efficacy proven** | Month 6 | 10 closed files; achieved-rate delta ≤ −0.25%; close rate > 40% |
| **M4 — Break-even** | Month 9 | 52+ closed files/month |
| **M5 — Scaled** | Month 18 | 150 closed files/month; > 95% autonomy rate; ₪4.3M ARR |

---

## Related Documents

- **`PRODUCT_ROADMAP.md`** — capability prioritization and the 8-week POC plan
- **`TECHNICAL_ROADMAP.md`** — agent harness architecture, financial core, voice stack, cost model
- **`REGULATORY_STRATEGY.md`** — licensing analysis, privacy obligations, and the hard boundaries on human presentation
- **`REGULATORY_ANSWERS.md`** — verified regulatory findings with primary-source citations

---

**Document Version**: 1.0
**Open items requiring decision**: insurance-license path (hire vs. partner vs. acquire); whether to hire a licensable mortgage advisor ahead of the pending licensing bill (`REGULATORY_STRATEGY.md` §9.1); raise-vs-bootstrap trigger confirmation.
