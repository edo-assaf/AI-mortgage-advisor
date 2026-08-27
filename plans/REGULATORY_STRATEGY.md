# Autonomous Mortgage Advisor — Regulatory Strategy

**Last Updated**: August 27, 2026
**Status**: 🟡 **PRE-POC** | One blocking legal opinion required before the friendly-file pilot

> ⚠️ **This document is not legal advice.** It is an engineering and business analysis of the regulatory surface, written to scope the legal work and to define which constraints must be enforced in code. Every item marked 🔴 requires a qualified Israeli lawyer before we act on it.

---

## 1. Why This Is a Separate Document

The business thesis rests on a regulatory observation: **mortgage advisory in Israel is unlicensed.** If that observation is wrong, incomplete, or becomes wrong, there is no business. That makes the regulatory surface the primary existential risk, not a compliance chore, and it deserves treatment at the same depth as the architecture.

Three things also make our position materially different from a human broker's, even though we perform the same function:

1. We operate at a volume and speed no individual can, which makes us visible to regulators and banks in a way a sole practitioner is not.
2. We process personal financial data at a scale that triggers obligations a sole practitioner escapes.
3. We present as a human advisor to lenders while being software, which is a defensible position but only if bounded precisely.

This document establishes the boundaries. Several of them are enforced in code, not policy — see §8.

---

## 2. Licensing Analysis

### 2.1 What is clearly unlicensed

**Mortgage advisory itself.** Israel regulates investment advice (Regulation of Investment Advice, Investment Marketing and Portfolio Management Law, 1995) and requires licensing for those activities. Mortgage advisory has no equivalent licensing regime. Thousands of independent advisors operate legally without a license, and the Mortgage Advisors Association participates in Bank of Israel consultations as a recognized industry body — which is itself evidence that the activity is recognized and unregulated.

**Acting as a borrower's agent** with a signed power of attorney is ordinary agency law and is exactly how human brokers operate.

### 2.2 🔴 The one question that must be answered before the pilot

**Does what we do constitute credit brokerage (תיווך אשראי) or credit-related activity requiring a license under the Supervision of Financial Services (Regulated Financial Services) Law, 2016?**

This is the single highest-consequence open legal question in the entire plan. The concern:

- We solicit credit offers from multiple lenders on a borrower's behalf, for a fee, as a business.
- We do this at commercial scale and systematically.
- The 2016 law licenses "provision of credit" and "credit brokerage" among regulated financial services, supervised by the Capital Market, Insurance and Savings Authority.

Arguments that we fall outside it: we do not provide credit; we do not receive compensation from lenders; our fee is paid by the borrower for advisory and administrative service; and thousands of human advisors do the identical thing without licenses, which strongly suggests the activity as practiced is outside the regime.

Arguments for caution: the "as practiced" defense relies on regulatory forbearance toward a fragmented cottage industry. A single, highly visible, high-volume automated entrant is a very different enforcement target than 2,000 sole practitioners. Precedent that has never been tested against someone doing 150 files a month is weaker precedent than it looks.

**Required action**: formal written legal opinion from Israeli counsel with financial-regulation practice, addressing:
- Credit brokerage licensing under the 2016 law as applied to our fee model and activity
- Whether receiving *any* compensation from a lender (which we do not plan to) would change the answer — this determines whether the lender-side SaaS in `BUSINESS_PLAN.md` §7 creates a licensing trigger
- Whether autonomous, non-human delivery of the advice changes the analysis
- Consumer protection exposure for automated financial advice
- Whether the insurance adjacency requires a separate entity

**Timing**: commission immediately. **Blocks the friendly-file pilot (Month 3), does not block POC development (Weeks 1–8).** POC runs entirely on simulated lenders and test data, so no regulated activity occurs.

**Budget**: ₪25,000–40,000. This is the highest-ROI spend in the plan.

### 2.3 Licensed adjacencies — known and deliberately deferred

| Activity | Regime | Our position |
|----------|--------|-------------|
| **Mortgage life & property insurance** | Insurance agent license (Supervision of Insurance Business Law) | 🔴 **Requires a license.** This is where human brokers earn their real margin, so it is the most valuable adjacency and the most clearly regulated. Path options: hire a licensed agent, revenue-share with an agency, or acquire a small agency. Deferred to Phase 3, gated on a decision. |
| **Investment advice** | Licensed (1995 law) | Hard boundary. We must never advise on whether to invest surplus funds versus prepay the mortgage in terms that constitute investment advice, even though borrowers will ask and competitors publish calculators for it. Our version presents the mortgage-side arithmetic only and explicitly declines the investment-side recommendation. Enforced as a topic guardrail on the agent. |
| **Provision of credit** | Licensed | We never lend. Architectural. |
| **Payment services / holding funds** | Licensed | We never touch client or lender funds. See §6. |
| **Tax advice** | Regulated in part | We compute purchase tax (מס רכישה) as a cost input. We do not advise on tax structuring. |
| **Legal services** | Bar-regulated | We flag contract discrepancies as factual mismatches against negotiated terms. We do not interpret legal effect or advise on legal rights. The distinction is real and must be maintained in the report language. |

---

## 3. Privacy — Amendment 13

### 3.1 Why this applies to us with unusual force

Amendment 13 to the Privacy Protection Law, 1981 was approved by the Knesset on 5 August 2024 and **entered into force on 14 August 2025**. It is the most significant reform to Israeli privacy law since 1981 and was explicitly designed to align Israel with GDPR-era standards, partly to preserve EU adequacy status.

It obliges appointment of a **Privacy Protection Officer (DPO)** for, among others, entities that carry out **systematic monitoring of individuals' behavior** and entities that **process specially sensitive information at significant scale**.

**We qualify on both grounds independently:**

- *Systematic monitoring*: the Privacy Protection Authority's guidance names credit-rating and insurance-underwriting systems as examples. We build behavioral and financial profiles of individuals continuously, and Mortgage Watch monitors clients' financial position for years.
- *Specially sensitive information at scale*: the amendment broadened "sensitive information" into "information of special sensitivity" and **explicitly added salary data**, alongside biometric data and professional personal assessments. Our core dataset is salary data, bank statements, tax filings, credit history, and our own assessments of a person's creditworthiness. There is no reading of this in which we fall outside.

### 3.2 Obligations

| Obligation | Our implementation | Timing |
|-----------|-------------------|--------|
| **Appoint a DPO** | Named individual meeting the statutory qualification requirements, with defined independence and reporting | Before the first real client file |
| Data Security Regulations, 2017 — **high security tier** | Full compliance: documented database definition, risk survey, penetration testing, access control, logging, incident response | Before pilot |
| Lawful basis & informed consent | Explicit, granular, plain-Hebrew consent at intake, separated by purpose (advisory, lender submission, credit bureau, monitoring) | POC (build it right the first time) |
| Purpose limitation | Data used only for stated purposes; no repurposing for model training without separate consent | POC |
| Data minimization | Source documents purged shortly after field extraction — we need the figures, not the scanned payslip | POC |
| Subject rights: access, correction, erasure | Self-service endpoints, not a manual process | Pilot |
| Breach notification | Documented procedure, defined thresholds, rehearsed | Before pilot |
| Processor agreements | Every third party touching personal data, **including LLM providers**, under a documented agreement with zero-retention terms | Before pilot |
| Cross-border transfer | LLM inference occurs outside Israel. Requires transfer basis analysis and disclosure. | 🔴 Confirm with counsel |

**Enforcement teeth**: Amendment 13 substantially expanded the Privacy Protection Authority's powers, including significant administrative monetary sanctions and expanded private rights of action for privacy violations. The pre-2025 practice of treating Israeli privacy law as low-risk is no longer sound.

### 3.3 🔴 The cross-border LLM question

Borrower salary data, bank statements, and tax filings will be transmitted to model providers whose inference runs outside Israel. This needs a specific answer from counsel on: the lawful transfer basis, whether consent alone suffices, what the privacy notice must disclose, and whether specially-sensitive-information status imposes additional constraints.

This has an architectural consequence worth planning for: if the answer is restrictive, we need self-hosted models for document extraction and intake — the two stages handling raw sensitive data — while frontier models continue to handle negotiation strategy on de-identified or derived data. Worth designing the boundary so that this substitution is possible without a rewrite. Noted as an open question in `TECHNICAL_ROADMAP.md`.

### 3.4 Immutability versus erasure

The immutable hash-chained action ledger (`TECHNICAL_ROADMAP.md` Decision 9) exists to make our conduct auditable and defensible. It directly tensions with the right to erasure.

**Resolution — crypto-shredding**: personal data is never stored inline in the ledger. Ledger entries reference encrypted payloads, each encrypted with a per-data-subject key. Erasure destroys the subject's key. The hash chain remains intact and verifiable, the record that *an action occurred at a time* survives, and the personal content becomes permanently unrecoverable.

🔴 Requires counsel confirmation that crypto-shredding satisfies erasure under Amendment 13. The GDPR position on this is broadly favorable; Israeli practice is less settled.

---

## 4. Credit Data

Under the Credit Data Law, 2016, the **Bank of Israel operates the credit data register** and all credit providers are obliged to report to it. Access to the raw register is not open. **Licensed credit bureaus** — private companies licensed by the Bank of Israel, notably BDI-Coface and Dun & Bradstreet — receive access and produce individual credit ratings.

Consumers have statutory rights: to inspect all data held about them **once annually at no cost** through licensed bureaus, and to demand correction of inaccuracies with escalation to the Bank of Israel's credit data unit.

**Our access path, in order of preference:**

1. **Borrower-obtained report, borrower-provided** (Phase 1). The borrower obtains their own report and shares it. Legally the cleanest possible route, uses their statutory free annual right, requires no license, and no relationship with a bureau. Cost: zero. Downside: friction in the intake flow and a stale-data risk. **This is the POC and pilot approach.**
2. **Consumer-consented bureau pull via a commercial agreement** (Phase 2). Standard arrangement used across Israeli fintech. Requires a bureau agreement and documented consent. Better UX, per-query cost, no license needed on our side.
3. **Becoming a licensed data user** (not planned). Disproportionate.

🔴 Confirm with counsel that route 2 requires no license on our side beyond documented consent, and confirm the permitted-use scope for a report obtained for mortgage advisory — specifically whether we may retain it, for how long, and whether we may use it in Mortgage Watch monitoring years later.

**Note on competitive context**: Walty's founder previously led the buildout of Israel's National Credit Registry at D&B and co-authored the operational model behind the Credit Data Law. They understand this terrain far better than we do. We should not attempt to compete on credit-data sophistication; route 1 or 2 with clean consent is sufficient for our product.

---

## 5. Presenting as a Human Advisor

This is the most consequential judgment call in the plan and it needs a precise line, because "act like a human" without a boundary drifts into fraud, and fraud here is criminal rather than merely embarrassing.

### 5.1 What we do, and why it is defensible

We communicate with lenders as **the borrower's authorized representative under a signed power of attorney**, under our company's brand identity, without proactively disclosing that the work is performed by software.

This is defensible on four grounds:

1. **The function is identical to a licensed-free human broker's.** Human brokers email and call banks as the borrower's agent under power of attorney every day. We do the same thing.
2. **The principal is real.** There is always an actual, identifiable borrower with real documents, real income, and a real signed mandate. Nothing about the underlying transaction is fictitious.
3. **No factual misrepresentation occurs.** Every statement we make about the borrower, the property, or the requested structure is true and provenance-backed.
4. **No Israeli law currently requires AI disclosure** in commercial correspondence. (The EU AI Act Article 50 would, for EU-facing systems. We are not EU-facing. See §5.5.)

### 5.2 The hard line — never crossed, enforced in code

| ❌ Prohibited | Why |
|--------------|-----|
| Fabricating a named individual human advisor ("I'm Yossi, your advisor") | Impersonation of a non-existent person. Moves the conduct from non-disclosure to affirmative deception about a material fact. |
| Claiming a license, certification, professional membership, or credential we do not hold | Straightforwardly fraudulent misrepresentation |
| Forging or auto-affixing a borrower's signature | Forgery. Criminal. |
| Asserting any fact about the borrower not resolvable to the validated profile with document provenance | Obtaining a benefit by deception (חוק העונשין). Also destroys the audit defense. |
| Denying being an automated system if directly and sincerely asked | The single fastest route to losing every bank relationship simultaneously, plus a media story that ends the company |
| Claiming to have performed an act we did not perform (a site visit, a meeting, a conversation) | Misrepresentation, and trivially disprovable |
| Impersonating the borrower themselves rather than acting as their agent | Identity fraud. Categorically different from agency. |

**Response protocol when directly asked whether this is AI**: acknowledge that the service is technology-driven, that we act as the borrower's authorized representative under a signed power of attorney, and offer to provide the mandate documentation. This is truthful, professional, and — critically — a *better* answer than a denial, because a bank's actual concern is whether the file is legitimate and the mandate is valid, not whether a human typed the email.

### 5.3 Why the boundary is architectural rather than a policy document

Policies drift. A prompt can be edited; a guardrail cannot be edited by accident. Three mechanisms in `TECHNICAL_ROADMAP.md`:

- **Factual assertion gate**: any outbound claim about the borrower's finances must resolve to a validated profile value with a provenance chain to a source document. Unresolvable claims block the send.
- **Identity constraint**: the agent has no name, credential, or license field available to it. Outbound identity is a fixed brand template, not a generated field. It cannot invent Yossi because Yossi is not representable.
- **Immutable ledger**: every outbound message is recorded before transmission. If we are ever challenged, we can produce the complete record, and the record is exculpatory rather than incriminating — which is only true if the boundary held on every single message.

The test to apply to every design decision: **would we be comfortable if this exact message, and every other message we have ever sent, appeared in a newspaper?** If yes, proceed. If not, the design is wrong regardless of whether it is technically legal.

### 5.4 Voice calls and recording

- **Recording**: Under the Secret Monitoring Law (חוק האזנת סתר), recording a conversation to which you are a party is generally permitted in Israel — one-party consent. So recording our own calls is lawful. But the *storage* of those recordings is fully subject to Amendment 13 and the Data Security Regulations. Treatment: encrypted, short retention, access-logged.
- **Notice**: we give a recording notice at call start regardless of whether strictly required. It is normal in Israeli commercial calls, costs us nothing, and removes an easy line of attack.
- **Synthetic voice**: 🔴 No current Israeli requirement to disclose that a voice is synthetic. Worth a specific question to counsel, and worth watching — this is the most likely place for near-term Israeli regulation, and a synthetic-voice disclosure rule would be far more disruptive to us than a text disclosure rule.

### 5.5 Regulatory trend risk

The direction of travel globally is toward mandatory AI disclosure. EU AI Act Article 50 requires disclosure when a person interacts with an AI system. Israel has historically imported EU privacy standards (Amendment 13 is a direct example) with a lag.

**Planning assumption: assume a Hebrew-market AI disclosure requirement arrives within 2–4 years, and build so that it is survivable rather than fatal.**

The mitigation is to make disclosure not matter. If our measurable advantage is a **−0.25% achieved rate** and a ₪2,490 price, then disclosure costs us a marketing preference, not a business. If our only advantage is that nobody knows, we have built a business on a secret with an expiry date. Practical steps: publish efficacy data so the value proposition stands independently; test disclosed positioning with a segment during Phase 2 to learn the real conversion cost before we are forced to; keep the human-relay fallback viable.

**This is a strategic ordering, not a moral one**: the position is defensible today, but a business whose viability depends on non-disclosure is fragile, and the plan should not require the position to hold forever.

### 5.6 Fleet of advisor identities (Phase 3)

Multiple advisor identities, each mapped to a **real registered legal entity or trading name** with genuine contact details, domain, phone, and address, optionally specialized by segment (foreign investor, refinance, complex credit). Each identity owns a **durable book of banker relationships**, so a given banker always deals with the same counterpart across many files.

Ordinary multi-brand commercial practice, and nothing about it is fabricated. The reason to build it is relationship leverage: a banker who has closed eleven clean files with the same counterpart over eight months behaves differently from one receiving a cold file. A single identity pushing 150 files a month across every institution reads as a firehose and weakens exactly the leverage we depend on. Stable assignment converts volume into relationship capital.

**Rules:**

- Every `advisor_identity` references a real registered entity. There is no field for a natural person's name, so a fabricated individual is not representable.
- **One identity per (institution, branch)** — a banker deals with exactly one of our brands, permanently.
- **One identity per (borrower, institution)** — a single brand owns each bank relationship for a given file.
- Cross-identity coordination is fully recorded in the ledger, so our conduct is reconstructable even though lenders see separate brands.

Both uniqueness rules are database constraints, not policy checks. See `TECHNICAL_ROADMAP.md` Decision 12.

**Intra-bank price discovery** is handled by selecting the right banker rather than by approaching several. Willingness to escalate a pricing exception internally is an individual trait, and material margin exceptions in Israeli banks generally escalate to a central pricing authority rather than resting on branch discretion — so *who* carries the file inside the bank matters more than which branch it enters. This is what `contact_memory` and `institution_policy` exist to capture. Competitive pressure is applied by naming a genuine competing offer, which gives the banker the internal justification they need to request the exception.

---

## 6. Deliberate Architectural Exclusions

Three constraints that cost us nothing and remove entire regulatory regimes:

| Constraint | Regime avoided |
|-----------|---------------|
| **We never hold, transfer, or access borrower funds, lender funds, or mortgage proceeds.** Our fee runs through a standard payment processor. | Payment services, e-money, and the substantial AML/CFT obligations attaching to entities in the funds flow |
| **We never lend, and never take a position in a loan.** | Provision of credit licensing; capital requirements |
| **We never receive compensation from lenders.** Borrower-paid only. | Conflict-of-interest disclosure regimes; and it preserves the honest claim that our only interest is the borrower's, which is also our best marketing line — Cantaio makes exactly this claim and it resonates |

The third has a cost: lender commissions are real money left on the table. Taking them would compromise the tender's integrity, likely trigger disclosure obligations, and possibly change the credit-brokerage analysis in §2.2. Not worth it.

---

## 7. Bank of Israel Directive 329 — Hard-Coded Constraints

These are the lending caps the compliance engine enforces. Non-compliant structures cannot leave the system.

| Constraint | Value | Notes |
|-----------|-------|-------|
| **LTV — sole dwelling** | ≤ 75% | |
| **LTV — replacement dwelling** | ≤ 70% | |
| **LTV — investment property / non-resident** | ≤ 50% | Foreign residents are automatically classified as investors |
| **LTV — all-purpose loan against residence** | ≤ 50% permanent, with a temporary relief to 70% subject to an absolute credit ceiling | 🔴 Confirm current relief status, ceiling amount, and expiry — this has changed more than once |
| **Variable-rate exposure** | ≤ 2/3 of total mix | Includes prime-linked and CPI-linked variable tracks. Raised from 1/3 to 2/3 in 2021. |
| **Fixed-rate minimum** | ≥ 1/3 of total mix | |
| **Payment-to-income (PTI)** | ≤ 50% of disposable income | Banks target 30–40% in practice due to risk-weighting penalties above 40%. **We optimize to the bank's practical preference, not the regulatory ceiling** — a structure at 48% PTI is legal and will be declined. |
| **Maximum term** | 30 years, fully amortizing | In force since April 2014 |
| **Prime rate definition** | BoI policy rate + 1.5% | Policy rate set 8× annually |
| **Subsidized-program property ceiling** | Elevated valuation ceiling for government-subsidized programs, with a minimum equity requirement | 🔴 Confirm current figures |
| **Risk-weighting thresholds** | RWA steps at LTV 45% / 60%, and PTI 40% / 50% | Not a cap, but it explains bank behavior and is essential to negotiation strategy — a structure just under a threshold is materially cheaper for the bank to write, which is leverage |

**Implementation requirements:**
- Every rule carries a citation to its source in the directive
- 100% branch coverage in tests, plus adversarial near-boundary generation in the eval suite
- Parameters are **configuration, not code** — the Bank of Israel changes these, sometimes with short notice, and a directive change must be a config deploy with a test run, not a code change
- A directive-change monitoring process with a named owner

🔴 **Before the POC**: the full parameter set must be confirmed against the current directive text with a mortgage professional. The values above are assembled from secondary sources and are the input to a system whose entire value depends on them being right.

---

## 8. Constraints Enforced in Code

Consolidating what this document requires the architecture to guarantee. Every item is a build-time or runtime enforcement, not a guideline:

| # | Constraint | Mechanism |
|---|-----------|-----------|
| 1 | No non-compliant structure reaches a client or lender | Compliance gate, not bypassable by configuration |
| 2 | No LLM-generated number in any outbound output | Typed Layer 1 bindings + CI check on outbound templates |
| 3 | No unprovenanced factual assertion about a borrower | Factual assertion gate in the guardrail service |
| 4 | No fabricated human identity or credential | Identity is a fixed brand template with no name/credential field the agent can populate |
| 5 | No irreversible action without a scoped signed borrower mandate | Mandate token check in the circuit breaker |
| 6 | No client funds in the system | No payment rails other than fee collection |
| 7 | Every action recorded before it takes effect | Immutable hash-chained ledger, write-ahead |
| 8 | Personal data erasable without breaking the audit chain | Crypto-shredding with per-subject keys |
| 9 | No investment advice | Topic guardrail on the agent |
| 10 | No borrower banking credentials, ever | No credential storage exists; no portal automation in scope |

---

## 9. Action Plan

### Immediate (before Week 1)
- [ ] 🔴 **Commission the legal opinion** (§2.2). ₪25–40K. Blocks the pilot, not the POC.
- [ ] 🔴 Confirm Directive 329 parameters with a mortgage professional (§7). **Blocks POC week 1.**
- [ ] Identify DPO candidates
- [ ] Obtain zero-retention terms from LLM providers

### Before the friendly-file pilot (Month 3)
- [ ] Legal opinion received and acted on
- [ ] DPO appointed
- [ ] Data Security Regulations high-tier compliance complete, with a documented risk survey
- [ ] Power of attorney template drafted by counsel
- [ ] Terms of service and privacy notice drafted by counsel, in Hebrew, plain-language
- [ ] Professional indemnity / E&O insurance bound
- [ ] Breach response procedure documented and rehearsed
- [ ] Processor agreements executed for every third party
- [ ] Cross-border transfer basis confirmed (§3.3)

### Before soft launch (Month 4)
- [ ] Consent flows reviewed by counsel
- [ ] Subject rights endpoints live and tested
- [ ] Crypto-shredding erasure verified as satisfying Amendment 13
- [ ] Complaint handling procedure

### Ongoing
- [ ] Directive 329 change monitoring, named owner, config-deploy path tested
- [ ] Privacy Protection Authority guidance monitoring
- [ ] AI disclosure regulation monitoring (Israel and EU)
- [ ] Annual penetration test and risk survey
- [ ] Quarterly review of this document

---

## 10. Regulatory Risk Summary

| Risk | Severity | Position |
|------|----------|----------|
| Credit brokerage licensing applies to us | 🔴 Existential | **Unresolved.** Blocking legal opinion commissioned. Mitigation if adverse: licensed-partner structure, or pursue the license. |
| Privacy breach with Amendment 13 sanctions | 🔴 High | Treated as P0 engineering. DPO mandatory. High-tier security compliance. |
| Financial error harms a client | 🔴 High | Deterministic core, independent recomputation, E&O insurance. `TECHNICAL_ROADMAP.md` Decisions 2 and 3. |
| Human-presentation position attacked | 🟠 High | Hard architectural boundary. Prepared public position. Publish efficacy data so the value proposition survives disclosure. |
| Mandatory AI disclosure introduced | 🟠 Medium-term | Assume it arrives in 2–4 years. Build so it costs marketing preference, not viability. |
| Directive 329 changes mid-file | 🟡 Medium | Parameters as config; grandfathering logic; change monitoring |
| Cross-border data transfer restricted | 🟠 Medium | Design the boundary so self-hosted models can substitute for the sensitive-data stages without a rewrite |
| Insurance adjacency needs a license | 🟡 Known | Deferred to Phase 3, gated on an explicit decision |

---

## 11. The Contrarian Framing

Worth stating, because it inverts how the regulatory work usually feels:

Every regulatory obligation in this document — audit trails, deterministic and citable compliance logic, provenance on every figure, immutable records, DPO, high-tier data security, E&O cover — is expensive for a sole practitioner and cheap for us. A human advisor cannot produce a machine-verifiable audit trail of why they recommended a mix. We can, as a byproduct of the architecture.

So if mortgage advisory in Israel is ever licensed or subjected to disclosure and audit requirements, the effect is to eliminate a large fraction of the 2,000 sole practitioners who are our real competition, and to leave standing the small number of operators who already built the infrastructure.

**The strategic conclusion: build the compliance infrastructure a licensing regime would demand, before it is demanded.** It is the cheapest insurance available against the plan's largest existential risk, and in the scenario where the risk materializes, it converts from insurance into a moat.

---

## Related Documents

- **`BUSINESS_PLAN.md`** — risks R3, R5, R6 map to §2, §3, §5 here
- **`PRODUCT_ROADMAP.md`** — capabilities 7, 23, 39 and the Phase 4 deferrals are gated by this document
- **`TECHNICAL_ROADMAP.md`** — Decisions 8, 9, 10 and §10 implement the constraints in §8 here

---

**Document Version**: 1.0
**Blocking items**: legal opinion on credit brokerage (§2.2) · Directive 329 parameter confirmation (§7)
