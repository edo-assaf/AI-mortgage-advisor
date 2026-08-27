# Autonomous Mortgage Advisor — Regulatory Strategy

**Last Updated**: August 27, 2026 · **Version**: 2.0
**Status**: 🟢 **No licensing blocker.** Two product-shaping constraints found; six gaps open.

> ⚠️ **Not legal advice.** Positions marked settled are verified against primary sources (citations in `REGULATORY_ANSWERS.md`) and still need counsel sign-off before the pilot. 🔴 items are unresolved.

---

## 1. What the August 2026 Verification Changed

**Closed — the credit-brokerage blocker.** The Supervision of Financial Services (Regulated Financial Services) Law, 2016 has **no licensable activity resembling credit brokerage**. Its licence types are service in a financial asset, provision of credit (§12(a)), deposit-and-credit services, and operating a P2P credit-intermediation *platform* (§25יז). Borrower-paid advisory and representation fits none. The forbearance worry was misconceived — there is no category to be enforced into. The legal opinion drops from existential to confirmatory, ₪25–40K to ₪12–20K.

**Broken — credit bureau Route 2 does not exist.** Under §27 of the Credit Data Law only a **credit provider** may request a credit report, for a credit transaction it is itself party to. Borrower consent is a cumulative precondition (§26(a)(4)), not a standalone basis, and the §32 power to designate non-lender recipients has never been exercised. A bureau API pull would put the bureau in breach. **Route 1 — the borrower obtains their own report and shares it — is the only lawful route, permanently, not a Phase 1 expedient.** See §4.

**Broken — the unlicensed thesis has a legislative expiry date.** A Regulation of Mortgage Advisory Bill was approved by the Knesset Economic Affairs Committee **for first reading on 26 May 2026**, with second and third readings deferred to the next Knesset. R3 in `BUSINESS_PLAN.md` moves from speculative to scheduled. See §9.1.

**Tailwind — borrower-paid-only is mandated, not chosen.** A 2026 Capital Market Authority draft circular prohibits non-bank lenders from paying mortgage advisors *any* remuneration on housing loans — commission, conference funding, advertising, training, or any benefit of economic value — with a deliberately broad "representative" definition to prevent relabelling, stating advisors may be paid "by the customer only." Banks are already barred. Our §6 exclusion is now the law rather than a differentiator, and it constrains lender-side SaaS (§9.2).

**Corrected numbers.** Directive 329 changed twice in 2026. Risk weights, the discounted-program ceiling, early-repayment discounts, and the PTI rule's legal character were all wrong in the prior revision. See §7.

---

## 2. Settled Positions

| Question | Position |
|---|---|
| Credit-brokerage licence, 2016 law | **Not required** — no such licensable activity. Conditions: borrower-paid only, no lender compensation, no custody of funds, signed mandate per file. |
| Autonomous delivery of advice | **No additional regime.** Robo-advisory rules attach to the 1995 Investment Advice Law; mortgage advisory sits outside it. Israeli AI policy is soft-law and sectoral. |
| Mortgage advisory as a profession | **Unlicensed today**; bill pending (§9.1). |
| Lender compensation | **Prohibited**, not merely avoided. |
| Investment advice | Hard boundary. Mortgage-side arithmetic only; explicitly decline the invest-vs-prepay recommendation. Topic guardrail. |
| Tax advice | Statutory purchase-tax brackets computed as a cost input, with a disclaimer. No structuring advice. |
| Legal services | Bar Association Law §20 confirmed — representation before the Land Registry and drafting documents of legal character are reserved. We diff facts against negotiated terms; we never interpret legal effect. |
| DPO appointment | **Mandatory** under §17ב1(4), specially sensitive information at significant scale. Salary and financial-activity data is expressly within "information of special sensitivity." |
| Erasure vs. immutable ledger | Israeli law has **no GDPR Art. 17-style general erasure right**. §14 covers inaccurate, incomplete, or outdated data; the broad right sits in the EEA Data Regulations, 2023. Far lower tension than assumed. Crypto-shredding still unconfirmed (§9.4). |
| Call recording | **Lawful** — one-party consent, §1 Secret Monitoring Law, 1979 (§3 bars recording for an unlawful purpose). We give notice anyway. |
| Synthetic voice disclosure | **No statute today**, but recommended by a December 2025 inter-agency report (§9.6). |
| Agency under power of attorney | Ordinary agency law (Agency Law, 1965 §§1–3). A bank cannot arbitrarily refuse a valid POA. The mandate must exclude executing binding loan agreements and disbursing funds. |
| Advisor-identity fleet | Ordinary multi-brand practice, provided every identity is a real registered entity with genuine contact details and no natural-person name field. |
| Directive 329 parameters | **Confirmed** against version 13, effective 30 June 2026. See §7. |

---

## 3. Privacy — Amendment 13

In force 14 August 2025. We trigger the DPO duty on the specially-sensitive-at-scale ground independently of the monitoring ground; enforcement began 31 October 2025, and the PPA published its final opinion on the duty on 15 July 2026.

**Security tier — corrected.** The high tier begins at **100,000 data subjects or more than 100 authorized users**, not 10,000. At pilot and Year-1 scale we are **medium tier**, which does not carry the 18-month risk survey and penetration test (high-tier only, Regs. 5(c)–(d)). Medium still requires the database definition document, access control, logging with 24-month retention (Reg. 10(d)), and audit every 24 months (Reg. 16). Engineering does not change — we build to high tier because it is cheap for us and it is the tier we grow into — but the compliance calendar and cost do. Formal determination is the DPO's first deliverable.

| Obligation | Implementation | Timing |
|---|---|---|
| DPO | External certified privacy counsel, ₪3–5K/month, reporting to CEO — not the CTO or architect | Before first real file |
| Data Security Regs | Tier formally determined, then compliance to it (building to high) | Before pilot |
| Granular consent | Plain-Hebrew, unbundled by purpose: advisory, lender submission, cross-border processing, monitoring | POC |
| Purpose limitation, minimization | No repurposing without separate consent; source documents purged after field extraction | POC |
| Subject rights | Self-service access, correction, deletion endpoints | Pilot |
| Breach notification | Documented, thresholded, rehearsed | Before pilot |
| Processor agreements | Every third party incl. LLM providers, zero-retention terms | Before pilot |
| Cross-border basis | Consent (Reg. 2(1)) + Reg. 3 written undertaking | 🔴 §9.4 |

Amendment 13's main contribution was enforcement: administrative fines and a PPA power to obtain a court order to stop processing or delete data (§23מט). Treating Israeli privacy law as low-risk is no longer sound.

**Cross-border LLM transfers.** Governed by the Transfer of Data Abroad Regulations, 2001 (unamended by Amendment 13). Consent is Reg. 2(1); Reg. 2(4) permits a contractual undertaking to observe Israeli holding-and-use conditions; Reg. 2(8) reaches EU/EEA states; Reg. 3 requires a separate written undertaking on security and onward transfer. A **PPA opinion of 13 April 2026 reads Reg. 2(4) restrictively**, requiring the recipient to commit contractually to purpose limitation, subject rights, confidentiality, and either the Data Security Regulations or ISO 27001 — raising the bar for US-hosted inference.

Treatment: enterprise DPAs with zero-retention and no-training terms; explicit unbundled consent naming the destinations; de-identification with surrogate subject IDs before any third-party call, extracted figures held only in the Israeli database. **Architectural consequence, now more likely to be needed than not: keep the boundary such that self-hosted models can substitute for document extraction and intake — the two stages touching raw sensitive data — while frontier models handle negotiation strategy on derived data.**

---

## 4. Credit Data — Route 1 Only

Only two bureaus operate: BDI-Coface and D&B Israel. **We cannot pull reports** (§1). Consent does not cure it. The prior plan's "Phase 2 bureau API pull at ₪15–30/query" is unavailable and is removed from the roadmap and unit economics.

The lawful route is the borrower obtaining their own report — the free annual data-concentration report under §38(d), or a consumer report from a bureau — and sharing it. Zero cost, with intake friction and stale-data risk we design around rather than engineer away.

- Intake must make borrower-side retrieval as frictionless as possible (guided flow, deep links, upload-and-parse), because it is permanent.
- Mortgage Watch cannot rely on periodic pulls at all: macro triggers only — BoI rate decisions, CPI prints, scheduled anchor-reset dates — plus borrower-initiated updates.
- Watch §32: if regulations designating non-lender recipients are ever made, bureau integration becomes available. Until then it is not a roadmap item.
- Citation corrections: §19 is reporting *into* the register, §30 the customer's opt-out from release, §31 the credit provider's post-receipt notification duty, §33 credit indication. Purpose limitation is §26(a)(1).

Walty's founder built the National Credit Registry at D&B and co-authored this law's operational model. We do not compete on credit-data sophistication.

---

## 5. Presenting as a Human Advisor

We communicate with lenders as **the borrower's authorized representative under a signed power of attorney**, under our company brand, without proactively disclosing that the work is performed by software. Defensible because the function is identical to a human broker's, the principal is real, no factual misrepresentation occurs, and no Israeli law currently requires disclosure.

The sharpest framing is criminal: under §415 of the Penal Law, deception requires an affirmative false statement or concealment of a fact one is *obliged* to disclose. Absent a disclosure duty, operating as a named corporate desk is not deception. The moment a duty exists (§9.6) the analysis inverts — which is why the plan must not depend on non-disclosure.

### 5.1 The hard line — enforced in code, never crossed

| ❌ Prohibited | Why |
|---|---|
| Fabricating a named individual advisor ("I'm Yossi") | Impersonation of a non-existent person; moves non-disclosure into affirmative deception |
| Claiming a licence, certification, or credential we lack | Fraudulent misrepresentation |
| Forging or auto-affixing a borrower's signature | Forgery. Criminal. |
| Asserting a fact about the borrower not resolvable to the validated profile with provenance | Obtaining a benefit by deception; destroys the audit defence |
| Denying being an automated system if directly and sincerely asked | Affirmative misrepresentation, and the fastest route to losing every bank relationship at once |
| Claiming to have performed an act we did not (a visit, a meeting) | Misrepresentation, trivially disprovable |
| Impersonating the borrower rather than acting as their agent | Identity fraud. Categorically different from agency. |

**When asked directly whether this is AI**: acknowledge the service is technology-driven, state that we act as the borrower's authorized representative under a signed power of attorney, and offer the mandate documentation. Truthful, and a *better* answer than a denial — the bank's real concern is whether the file and mandate are legitimate, not who typed the email. If the banker declines to deal with an automated representative, terminate gracefully and escalate to human relay or email.

### 5.2 Architectural, not policy

Policies drift; a prompt can be edited, a guardrail cannot be edited by accident. Three mechanisms in `TECHNICAL_ROADMAP.md`: a **factual assertion gate** (every outbound claim resolves to a validated profile value with provenance, or the send blocks); an **identity constraint** (no name, credential, or licence field exists for the agent to populate — it cannot invent Yossi because Yossi is not representable); and the **immutable ledger** (every outbound message recorded before transmission, so the record is exculpatory rather than incriminating — true only if the boundary held on every message).

The test for every design decision: **would we be comfortable if this exact message, and every other message we have sent, appeared in a newspaper?**

### 5.3 Advisor identity fleet (Phase 3)

Multiple identities, each a real registered entity or trading name with genuine domain, phone, and address, optionally specialized by segment. The reason is relationship leverage: a banker who has closed eleven clean files with the same counterpart behaves differently from one receiving a cold file, and a single identity pushing 150 files a month across every institution reads as a firehose.

Rules, all database constraints rather than policy checks (`TECHNICAL_ROADMAP.md` Decision 12): every identity references a real registered entity with **no natural-person name field**; **one identity per (institution, branch)**; **one identity per (borrower, institution)**; all cross-identity coordination recorded in the ledger.

Intra-bank price discovery works by selecting the right banker, not by approaching several — material margin exceptions escalate to a central pricing authority rather than resting on branch discretion, so *who* carries the file matters more than which branch it enters. That is what `contact_memory` and `institution_policy` capture.

---

## 6. Deliberate Architectural Exclusions

| Constraint | Regime avoided |
|---|---|
| **Never hold, transfer, or access borrower funds, lender funds, or proceeds.** Fee via standard payment processor. | Payment services, e-money, and the AML/CFT obligations attaching to entities in the funds flow |
| **Never lend, never take a position in a loan.** | Provision-of-credit licensing (§12(a)); capital requirements |
| **Never receive compensation from lenders.** Borrower-paid only. | Now legally required (§1). Preserves the honest claim that our only interest is the borrower's — also our best marketing line. |

The third no longer carries an opportunity cost worth debating: lender commissions on housing loans are being prohibited outright.

---

## 7. Directive 329 — Hard-Coded Constraints

Verified against **Directive 329 version 13, effective 30 June 2026** (circulars 2840 of 8 Feb 2026 and 2852 of 30 Jun 2026), **Directive 203 §72 as rewritten by circular 2816** (April 2025), and the official Q&A.

| Constraint | Value | Notes |
|---|---|---|
| LTV — sole dwelling | ≤ 75% | §2 |
| LTV — replacement dwelling | ≤ 70% | §2 |
| LTV — investment dwelling | ≤ 50% | §2. Foreign residents are not a separate category: "sole" and "replacement" are defined only for Israeli citizens, so a foreign-resident purchase is necessarily an investment dwelling. |
| LTV — all-purpose loan against residence | ≤ 50% baseline; **70% where the increment above 50% ≤ ₪200,000** | Baseline from the Q&A (2.2, 8.2), not the directive body. The 70% option is **§10א and now permanent** — circular 2840 converted the wartime temporary order. No expiry. |
| Variable-rate exposure | ≤ 66.66% | §7. Decided 14 Dec 2020, circular 2647; effective for new loans 17 Jan 2021, refinancing 28 Feb 2021. The change *removed* a separate 1/3 cap on the prime track (2011); the 1/3-fixed structure predates it. |
| Fixed-rate minimum | ≥ 1/3 | §7, corollary |
| Payment-to-income | ≤ 50% — **flat prohibition** | §5: a bank "shall not approve and shall not execute" above 50% PTI. Hard block, not a soft target. Distinct from the capital penalty below. |
| Maximum term | 30 years, fully amortizing | §8. From the Supervisor's Aug 2013 measures, effective 1 Sep 2013, codified when 329 was published 15 Jul 2014. **Not April 2014.** |
| Prime rate | BoI policy rate + 1.5% | Spread unchanged since 1994. Eight scheduled Monetary Committee decisions per year. |
| Discounted-price programs | LTV on appraised market value, not discounted contract price, capped at **₪2,100,000** | §4א. Raised from ₪1.8M by circular 2840, Feb 2026. **No ₪1.5M vintage exists.** |
| Minimum buyer equity, those programs | **₪100,000**, or ₪60,000 with an Accountant General grant | **No 10%-of-price alternative.** The "90% of contract price" figure in commercial material is an arithmetic by-product, not a rule. |
| Risk weight by LTV | 35% up to 45%; 50% above 45% to 60%; **60% above 60%** | Directive 203 §72. **The 75% top tier is outdated.** Reduced weights require the loan not exceed **₪5 million**. |
| Risk weight by PTI | **100% whenever PTI exceeds 40%**, overriding the reduced §72 weights | Directive 329 §6. Why banks target 30–40%, and why a legal 48% PTI structure gets declined. **We optimize to the bank's practical preference, not the regulatory ceiling.** |
| Scope | Housing loans narrowed to **dwellings in Israel** | Circular 2852 |

**Negotiation leverage, recalibrated**: crossing below 60% LTV moves the bank from a 60% to a 50% risk weight; below 45%, to 35%. Crossing below 40% PTI removes a 100% weight — by far the largest step and the highest-value optimization target. A file at 61% LTV where ₪15,000 of equity reaches 59.9% gives the agent a quantified capital saving to trade for a rate concession.

**Early repayment fees** — Banking Order (Early Repayment of a Housing Loan), 2002; the 1981 עמלות order was repealed by §13. §3 components: operational fee capped at **₪60**; **0.1%** of the repaid amount where the borrower gave *less than* ten days' notice (we always give notice, eliminating it); and the discounting fee under §3(3) and §3(4), of which the bank may charge **only the lower**. §5 index fee: CPI-linked loans repaid between the 1st and 15th, at half the average monthly index change over the last twelve indices. Statutory discounts under **§8(b): 20% at three-to-under-five years, 30% at five years or more** — *not* 10%/20%. The 10/20/30/40% one-to-four-year ladder in §8(a) applies only to supplementary loans for those eligible for a directed loan.

**Implementation**: every rule cites directive, section, and circular version; 100% branch coverage plus adversarial near-boundary generation; parameters are **configuration, not code**; directive-change monitoring with a named owner and a tested config-deploy path; and **date-aware rule evaluation**, because a transitional provision defers part of circular 2840 (including Q&A 5.1) to **1 October 2026**.

---

## 8. Constraints Enforced in Code

| # | Constraint | Mechanism |
|---|---|---|
| 1 | No non-compliant structure reaches a client or lender | Compliance gate, not bypassable by configuration |
| 2 | No LLM-generated number in any outbound output | Typed Layer 1 bindings + CI check on outbound templates |
| 3 | No unprovenanced factual assertion about a borrower | Factual assertion gate in the guardrail service |
| 4 | No fabricated human identity or credential | Identity is a fixed brand template with no name/credential field |
| 5 | No irreversible action without a scoped signed mandate | Mandate token check in the circuit breaker |
| 6 | No client funds in the system | No payment rails other than fee collection |
| 7 | Every action recorded before it takes effect | Immutable hash-chained ledger, write-ahead |
| 8 | Personal data erasable without breaking the audit chain | Crypto-shredding with per-subject keys |
| 9 | No investment advice | Topic guardrail on the agent |
| 10 | No borrower banking credentials, ever | No credential storage; no portal automation in scope |
| 11 | **No lender-sourced revenue on any advisory file** | No lender-payable rail in billing; enforced at the schema level |
| 12 | **No credit-bureau API pull** | Credit data enters only by borrower upload; no bureau client in the codebase |

---

## 9. Open Gaps

### 9.1 🔴 Mortgage advisory licensing bill — the new principal risk
Approved for first reading 26 May 2026; readings two and three deferred to the next Knesset subject to implementation-budget agreement. Threshold conditions, ethics rules, and financial sanctions, supervised by the Ministry of Justice as registrar of regulated professions.

**Need to know**: whether threshold conditions attach to a natural person (examination, training hours, indemnity cover) or admit a corporate/algorithmic provider; whether a licensed employee can carry files a machine prepares; grandfathering for incumbents. **Action**: obtain the bill text and committee protocol; counsel assessment of licensability under it. Highest-value legal spend now. Does not block the POC or pilot — it cannot become law before the next Knesset sits — but it determines Phase 3 structure and whether to hire a licensable human early.

### 9.2 🔴 Does the lender-remuneration ban catch lender-side SaaS?
The draft circular's "representative" definition is broad and covers "any benefit of economic value." Selling file-structuring software to a non-bank lender is plainly a technology sale, but if that lender also funds files we advised on, the fee is arguably remuneration to a representative. **Question**: is a separate legal entity with strict data and revenue separation sufficient, or is the ban entity-agnostic? Blocks `BUSINESS_PLAN.md` §7 only. Until answered: **reject all lender revenue**, and if lender-side SaaS ships, it processes only the lender's own direct inbound files, in a separate entity.

### 9.3 🔴 Insurance adjacency
Unchanged. Mortgage life and property insurance requires an insurance agent licence and is where human brokers earn their real margin. Options: hire a licensed agent, revenue-share with an agency, or acquire a small agency. Deferred to Phase 3 on an explicit decision. Do not quote or bind before then.

### 9.4 🔴 Crypto-shredding, and cross-border transfer of sensitive data
Two questions to the same privacy counsel.

*Crypto-shredding*: no PPA guidance addresses it by name. The doctrinal hook is the PPA's January 2026 DPO guide, which accepts **anonymization (התממה) as an alternative to deletion** at end of information life — irreversible key destruction leaving undecryptable ciphertext is arguably anonymization, but that is our inference. Severity is lower than previously assessed because there is no general erasure right (§2); the exposure is a §14 request or an EEA-origin file, not a routine deletion demand.

*Cross-border*: whether consent under Reg. 2(1) plus a Reg. 2(4)/Reg. 3 undertaking suffices for information of special sensitivity after the 13 April 2026 PPA opinion, or whether the sensitive-data stages must be self-hosted. Blocks nothing in the POC; determines whether the self-hosted extraction path moves from contingency to requirement.

### 9.5 🟡 Formal security-tier determination
Medium or high, per §3. Affects the compliance calendar and cost, not the engineering. The DPO's first deliverable.

### 9.6 🟠 AI disclosure — nearer than assumed
No enacted statute, but the **December 2025 final report of the inter-agency Regulatory Workgroup on AI in the Financial Sector** (Ministry of Justice, Ministry of Finance, Competition Authority, Securities Authority, Capital Market Authority, Bank of Israel) **recommends mandatory notification that an interaction is with an AI system rather than a human**, and directs regulators to update their rules. Treat as an expected supervisory requirement on a **12–24-month horizon**, not the 2–4-year trend previously assumed. For context, EU AI Act Article 50(1) chatbot disclosure has been in force since 2 August 2026.

Architecture unchanged; only the urgency of making disclosure survivable. Now on the Phase 2 critical path rather than optional: publish efficacy data so the value proposition stands independently of who performs the work; test disclosed positioning with a real segment to price the conversion cost before we are forced to; keep human relay viable. If our measurable advantage is a **−0.25% achieved rate** at a ₪2,490 fee, disclosure costs a marketing preference. If our only advantage is that nobody knows, we built a business on a secret with an expiry date.

### 9.7 🟡 Confirmatory legal opinion — ₪12–20K
Confirm the §2 licensing conclusion; review the POA template and its exclusion of binding execution; review consent flows and the privacy notice; answer §9.2 and §9.4. Commission immediately. It no longer blocks the pilot on its own, but the POA template and consent flows do.

### 9.8 Consumer protection and tort — mitigated, retained because it drives engineering
Under §2 of the Consumer Protection Law, 1981, no output may claim a mix is "the cheapest" or a guaranteed outcome; framing is *optimized under stated parameters and borrower inputs*. Under §35 of the Torts Ordinance an objectively flawed recommendation is actionable negligence. Mitigations: deterministic financial core with zero arithmetic delegated to LLMs (Decision 2), independent recomputation before any client-facing output (Decision 3), and **E&O cover of at least ₪5,000,000 per claim**, endorsed for algorithmic financial analysis.

---

## 10. Action Plan

**Immediate**
- [ ] 🔴 Obtain the mortgage advisory bill text and committee protocol; counsel assessment (§9.1)
- [ ] 🔴 Commission the narrowed legal opinion, ₪12–20K (§9.7)
- [ ] Identify DPO candidates; obtain zero-retention terms from LLM providers
- [ ] Remove the bureau-pull assumption from roadmap and unit economics (§4)
- [ ] Load the corrected Directive 329 parameter set into config with version and effective dates (§7)

**Before the friendly-file pilot (Month 3)**
- [ ] DPO appointed; security tier determined (§9.5); Data Security Regs compliance to that tier
- [ ] POA template drafted by counsel, with the execution/disbursement exclusion
- [ ] Terms of service and privacy notice in plain Hebrew, drafted by counsel
- [ ] E&O insurance bound, ≥ ₪5M per claim
- [ ] Breach response procedure documented and rehearsed; processor agreements executed
- [ ] Cross-border transfer basis confirmed (§9.4)
- [ ] Borrower-side credit-report retrieval flow built and usability-tested (§4)

**Before soft launch (Month 4)**
- [ ] Consent flows reviewed by counsel; subject rights endpoints live
- [ ] Crypto-shredding position confirmed (§9.4); complaint handling procedure
- [ ] Disclosed-positioning test designed for Phase 2 (§9.6)

**Ongoing**
- [ ] Directive 329 change monitoring, named owner, tested config-deploy path, date-aware evaluation
- [ ] Mortgage advisory bill tracking through the next Knesset
- [ ] Credit Data Law §32 regulations watch — would unlock bureau access
- [ ] PPA guidance and AI-disclosure monitoring (Israel and EU); annual PT and risk survey
- [ ] Quarterly review of this document

---

## 11. Risk Summary

| Risk | Severity | Position |
|---|---|---|
| Mortgage advisory becomes licensed | 🔴 High | **Bill passed committee 26 May 2026.** Assess licensability under it; consider hiring a licensable human early; build what a regime would demand. |
| Credit brokerage licensing applies | 🟢 Resolved | No such licensable activity. Confirmatory opinion only. |
| Bureau access unavailable | 🟡 Product constraint | Route 1 only, permanently. Design intake around it; watch §32. |
| Privacy breach, Amendment 13 sanctions | 🔴 High | P0 engineering. DPO mandatory. Build to high tier regardless of determined tier. |
| Financial error harms a client | 🔴 High | Deterministic core, independent recomputation, E&O cover. |
| Mandatory AI disclosure introduced | 🟠 12–24 months | Recommended by the Dec 2025 financial-sector report. Make disclosure cost marketing preference, not viability. |
| Human-presentation position attacked | 🟠 High | Hard architectural boundary; prepared public position; published efficacy data. |
| Cross-border transfer restricted | 🟠 Medium | Keep the boundary so self-hosted models can substitute without a rewrite. |
| Directive 329 changes mid-file | 🟡 Medium | Changed twice in 2026. Config parameters, date-aware evaluation, grandfathering. |
| Lender-side SaaS caught by the ban | 🟡 Phase 3 | Separate entity, no advisory-file overlap, counsel confirmation first. |
| Insurance adjacency needs a licence | 🟡 Known | Deferred to Phase 3 on an explicit decision. |

---

## 12. The Contrarian Framing

Every obligation here — audit trails, deterministic and citable compliance logic, provenance on every figure, immutable records, a DPO, tiered data security, E&O cover — is expensive for a sole practitioner and cheap for us. A human advisor cannot produce a machine-verifiable audit trail of why they recommended a mix; we can, as a byproduct of the architecture.

This has stopped being hypothetical. A licensing bill cleared committee in May 2026, and a regulator has proposed banning the lender commissions a meaningful share of the 2,000 sole practitioners quietly rely on. Both fall hardest on exactly our competition. **Build the compliance infrastructure a licensing regime would demand, before it is demanded** — the cheapest insurance against the plan's largest risk, and in the scenario where it materializes it converts from insurance into a moat.

---

## Related Documents

- **`REGULATORY_ANSWERS.md`** — verified findings with primary-source citations
- **`BUSINESS_PLAN.md`** — R3, R5, R6 map to §9.1, §3, §5
- **`PRODUCT_ROADMAP.md`** — capability 7 constrained by §4; capability 39 by §9.3
- **`TECHNICAL_ROADMAP.md`** — Decisions 8, 9, 10, 12 implement §8

**Blocking items**: none for the POC · POA template and consent flows block the pilot · §9.1 determines Phase 3 structure
