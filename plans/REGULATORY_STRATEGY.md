# Autonomous Mortgage Advisor — Regulatory Strategy

**Last Updated**: August 27, 2026 · **Version**: 2.0
**Status**: 🟢 **No licensing blocker.** Core operational boundaries defined; six open items tracked for formal counsel sign-off.

> ⚠️ **Not legal advice.** Positions marked settled are verified against primary statutory sources and regulatory circulars (citations in `REGULATORY_ANSWERS.md`). Formal legal sign-off required prior to production pilot. 🔴 items require specialist counsel input.

---

## 1. Executive Summary & Legal Baseline

**No licensing requirement today.** The Supervision of Financial Services (Regulated Financial Services) Law, 2016 contains no licensable activity governing mortgage advisory or loan brokerage. The four statutory licence types (service in a financial asset, provision of credit under §12(a), deposit-and-credit services under Chapter ג׳2, and operating a P2P credit-intermediation platform under §25יז) do not apply to borrower-paid advisory and representation. Mortgage advisory is currently unlicensed, though a licensing bill passed committee for first reading in May 2026 (§9.1).

**Credit data access is strictly Route 1 (Borrower-Shared).** Under §27 of the Credit Data Law, 2016, only a licensed credit provider may request a credit report directly from a bureau. Non-lenders cannot query credit bureaus via API, even with borrower consent. The only lawful mechanism is for the borrower to retrieve their statutory free data-concentration report (§38(d)) and upload it to the platform.

**Lender remuneration is legally barred.** The Capital Market, Insurance and Savings Authority draft circular bars non-bank lenders from paying mortgage advisors commissions, marketing fees, or economic benefits on housing loans, matching existing banking restrictions. The business operates strictly on a 100% borrower-paid fee model, eliminating structural conflicts of interest.

**Strict compliance engine specifications.** All loan structure evaluations are bound to Bank of Israel Directive 329 (version 13, circulars 2840 and 2852), Directive 203 §72 risk weights (circular 2816), and the Banking Order (Early Repayment of a Housing Loan), 2002. Compliance logic is deterministic, immutable, and version-aware.

---

## 2. Settled Positions

| Regulatory Area | Settled Position |
|---|---|
| **Credit-brokerage licensing (2016 Law)** | **Not required** — no such licensable activity exists. Conditions: 100% borrower-paid, zero lender compensation, zero custody of funds, signed power of attorney per file. |
| **Autonomous delivery of advice** | **No additional regime.** Robo-advisory rules attach to the 1995 Investment Advice Law; mortgage advisory sits outside it. Israeli AI policy is soft-law and sectoral. |
| **Mortgage advisory profession** | **Unlicensed today**; legislative bill pending first/second/third readings (§9.1). |
| **Lender compensation** | **Prohibited** by regulatory draft circulars and Banking Supervision rules; 100% borrower-funded. |
| **Investment advice** | Hard boundary under 1995 Law. Mortgage-side arithmetic only; explicitly decline invest-vs-prepay recommendations. Topic guardrail enforced. |
| **Tax advice** | Statutory purchase-tax brackets computed purely as a cost input, with an explicit disclaimer. No tax structuring. |
| **Legal services** | Bar Association Law §20 confirmed — representation before the Land Registry (טאבו) and drafting documents of legal character remain reserved to lawyers. Platform diffs facts against negotiated terms without interpreting legal effect. |
| **DPO appointment** | **Mandatory** under Privacy Law §17ב1(4) (processing specially sensitive information at significant scale, which includes salary and financial data). |
| **Erasure vs. immutable ledger** | Israeli law has **no GDPR Art. 17-style general erasure right**. §14 covers inaccurate, incomplete, or outdated data; broader deletion applies under EEA Data Regulations, 2023 or contract terms. |
| **Call recording** | **Lawful** — one-party consent under Secret Monitoring Law, 1979 §1. Standard pre-call transparency notice provided. |
| **Synthetic voice disclosure** | **No statute today**; recommended by December 2025 inter-agency report on AI in financial services (§9.6). |
| **Agency under power of attorney** | Governed by Agency Law, 1965 §§1–3. Mandate authorizes negotiation and filing while strictly excluding binding loan execution and disbursement. |
| **Advisor-identity fleet** | Standard multi-brand practice under Registration of Business Names Ordinance. Every brand is a registered entity with genuine contact details and no natural-person name field. |
| **Directive 329 parameters** | **Confirmed** against version 13, effective 30 June 2026. See §7. |

---

## 3. Privacy — Amendment 13

In force since 14 August 2025. Processing financial and salary data at scale triggers the statutory Data Protection Officer (DPO) requirement under §17ב1(4).

**Data security tiering.** Under the Data Security Regulations, 2017, the **high** tier applies to databases holding sensitive data on 100,000+ data subjects or with more than 100 authorized users. At pilot and Year-1 scale, the platform falls under the **medium tier** (holding sensitive financial records under First Schedule type). Medium tier requires a formal Database Definition Document (Reg. 2), role-based access control, access logging with 24-month retention (Reg. 10(d)), and biennial audits (Reg. 16). High tier adds mandatory 18-month risk surveys and penetration testing (Regs. 5(c)–(d)). The platform is architected to high-tier security standards from inception.

| Obligation | Implementation | Timing |
|---|---|---|
| **DPO** | External certified privacy counsel, reporting directly to CEO | Before first live file |
| **Data Security Regulations** | Compliance to determined tier (architected to high tier) | Before pilot |
| **Granular Consent** | Plain-Hebrew, unbundled by purpose: advisory, lender submission, cross-border processing | POC |
| **Purpose Limitation & Minimization** | No secondary processing without consent; raw documents purged post-extraction | POC |
| **Subject Rights** | Self-service access, correction, and deletion endpoints | Pilot |
| **Breach Notification** | Documented, thresholded, and rehearsed response procedure | Before pilot |
| **Processor Agreements** | Enterprise DPAs with all sub-processors (including zero-retention LLM terms) | Before pilot |
| **Cross-Border Transfers** | Informed consent (Reg. 2(1)) + written security undertaking (Reg. 3) | 🔴 §9.4 |

Amendment 13 strengthened regulatory enforcement, introducing administrative fines and empowering the Privacy Protection Authority (PPA) to seek judicial orders compelling data deletion or cessation of processing (§23מט).

**Cross-border LLM transfers.** Governed by the Transfer of Data Abroad Regulations, 2001. Lawful grounds include express consent (Reg. 2(1)), contractual undertakings to maintain Israeli standards (Reg. 2(4)), and transfers to EEA/convention signatories (Reg. 2(8)), alongside Reg. 3 written security undertakings. A PPA opinion of 13 April 2026 interprets Reg. 2(4) restrictively, requiring recipients to commit to purpose limitation, subject rights, confidentiality, and Data Security Regulations / ISO 27001 compliance.

*Architectural Treatment*: Enterprise DPAs with zero-retention terms, unbundled consent naming destination jurisdictions, and de-identification with surrogate subject IDs before third-party LLM invocation. Document extraction touching raw data is decoupled to enable self-hosted models if cross-border restrictions tighten.

---

## 4. Credit Data — Route 1 Architecture

Only two licensed bureaus operate in Israel: BDI-Coface and D&B Israel. Direct API querying by non-lenders is legally prohibited under §27 of the Credit Data Law, 2016. Customer consent cannot override this statutory lender-only restriction.

The compliant operating model is **Route 1**: the borrower retrieves their own report and uploads it to the platform. Under §38(d), every individual is entitled to one free statutory **data-concentration report (דוח ריכוז נתונים)** annually from the Bank of Israel register.

- **Intake Flow**: Step-by-step guided retrieval flow, direct deep-links to the government credit register portal, and automated PDF upload and parsing.
- **Mortgage Watch**: Refinance monitoring operates on macro triggers (BoI rate decisions, CPI prints, scheduled anchor resets) combined with borrower-initiated profile updates, without automated recurring bureau pulls.
- **Statutory Framework**: §19 governs register reporting, §26(a)(1) enforces purpose limitation, §27 restricts bureau reports to lenders, §30 provides customer release opt-out, §31 sets lender notification duties, §33 governs credit indications, §38(d) establishes free annual concentration reports, and §51 enforces non-discrimination in scoring models.

---

## 5. Advisory Identity & Operational Transparency

The platform communicates with lending institutions as **the borrower's authorized representative under a signed power of attorney**, operating under registered corporate trading names.

Under Penal Law, 1977 §415, obtaining a benefit by deception requires an affirmative false statement or concealment of a material fact subject to a disclosure duty. Operating as an institutional advisory desk representing an actual borrower under a valid mandate is fully lawful.

### 5.1 Absolute Operational Boundaries

| Prohibited Action | Legal / Compliance Basis |
|---|---|
| Fabricating a named natural person advisor ("I'm Yossi") | Impersonation of a non-existent individual; constitutes affirmative misrepresentation |
| Claiming a licence or credential not held | Fraudulent misrepresentation |
| Forging or auto-affixing a borrower's signature | Criminal forgery |
| Asserting borrower facts not verified in profile provenance | Obtaining a benefit by deception; invalidates audit trail |
| Denying automated system operation when directly asked | Affirmative misrepresentation |
| Claiming physical acts not performed (meetings, site visits) | Factual misrepresentation |
| Impersonating the borrower directly | Identity fraud (distinct from lawful agency under POA) |

**Protocol When Asked Directly Regarding Automation**: Acknowledge that the service utilizes advanced algorithmic technology, affirm representation of the borrower under an executed power of attorney, and provide mandate documentation upon request. If an institution requires human dialogue, escalate seamlessly to human desk relay.

### 5.2 Architectural Enforcement
- **Factual Assertion Gate**: Every outbound claim resolves deterministically to a verified profile field with provenance before message transmission.
- **Identity Constraints**: Identity templates contain no natural-person name or credential fields.
- **Immutable Write-Ahead Ledger**: Every interaction and outbound message is logged with cryptographic hash-chaining prior to delivery.

### 5.3 Multi-Brand Identity Management
Multiple trading brands operating under registered business names with verified contact information, phone numbers, and domains. Governed by database constraints:
- One identity per (institution, branch)
- One identity per (borrower, institution)
- Full cross-brand coordination recorded in the central immutable ledger

---

## 6. Architectural Exclusions & Scope Limits

| Constraint | Regulatory Regime Avoided |
|---|---|
| **Zero Custody of Client Funds**: Never receive, hold, or transfer borrower funds, loan proceeds, or down payments. Fees collected via standard credit card processing. | Payment Services Law, E-Money Regulations, and statutory AML/CFT fund-handling obligations |
| **No Principal Lending**: Never lend, underwrite, or hold loan positions. | Provision-of-Credit Licensing (§12(a)) and statutory capital adequacy mandates |
| **Zero Lender Remuneration**: 100% borrower-paid fee structure. | Mandatory under Capital Market Authority draft circulars and Banking Supervision rules |

---

## 7. Directive 329 — Hard-Coded Compliance Parameters

Verified against **Directive 329 (version 13, effective 30 June 2026)**, Circulars **2840** and **2852**, Directive **203 §72** (Circular 2816), and the Banking Order (Early Repayment of a Housing Loan), 2002.

| Metric | Statutory Limit | Regulatory Citation & Underwriting Notes |
|---|---|---|
| **LTV — Sole Dwelling** | ≤ 75% | Directive 329 §2 |
| **LTV — Replacement Dwelling** | ≤ 70% | Directive 329 §2 |
| **LTV — Investment Dwelling** | ≤ 50% | Directive 329 §2. Foreign residents fall under investment tier as sole/replacement definitions apply only to Israeli citizens. |
| **LTV — All-Purpose Loan** | ≤ 50% baseline; up to 70% if increment ≤ ₪200,000 | Directive 329 §10א (Permanent status via Circular 2840; BoI Q&A 2.2, 8.2) |
| **Variable Rate Share** | ≤ 66.66% (2/3 of loan) | Directive 329 §7 (Circular 2647) |
| **Fixed Rate Share** | ≥ 33.33% (1/3 of loan) | Directive 329 §7 |
| **Payment-to-Income (PTI)** | ≤ 50% (**Absolute Prohibition**) | Directive 329 §5 (Bank shall not approve or execute above 50%) |
| **Maximum Loan Term** | 30 years | Directive 329 §8 (in force since 1 Sep 2013) |
| **Prime Rate Benchmark** | BoI Policy Rate + 1.50% | Fixed spread since 1994; 8 scheduled annual rate decisions |
| **Discounted Programs (Mehir LeMishtaken)** | LTV on appraisal up to ₪2,100,000 | Directive 329 §4א (Circular 2840) |
| **Minimum Equity (Discounted Programs)** | ₪100,000 (₪60,000 with AG grant) | Directive 329 §4א |
| **Risk Weight by LTV** | 35% (≤ 45%), 50% (45–60%), 60% (> 60%) | Directive 203 §72 (Circular 2816, loans ≤ ₪5M) |
| **Risk Weight by PTI** | 100% risk weight across loan if PTI > 40% | Directive 329 §6 (Underwriting preference drives practical targets to 30–40%) |
| **Geographic Scope** | Dwellings in Israel only | Directive 329 (Circular 2852) |

### 7.1 Capital Optimization & Negotiation Leverage
- Crossing below 60% LTV drops the lender risk weight from 60% to 50%; crossing below 45% drops it to 35%.
- Crossing below 40% PTI removes the 100% risk weight penalty — the single largest capital saving for the bank, used by the agent to negotiate rate concessions.

### 7.2 Early Repayment Calculations
- **Governing Law**: Banking Order (Early Repayment of a Housing Loan), 2002.
- **Fee Rules**:
  - Operational fee capped at **₪60** (§3(1))
  - Notice fee: **0.1%** charged only if borrower gives less than 10 days' notice (§3(2))
  - Economic discounting fee: charged at the **lower** of §3(3) and §3(4) calculations
  - Index adjustment fee: applies to CPI loans repaid between the 1st and 15th of the month (§5)
  - Statutory discounts (§8(b)): **20%** for loans held 3 to <5 years; **30%** for loans held ≥ 5 years

---

## 8. Compliance Controls Enforced in Architecture

| # | Control Requirement | Architectural Enforcement Mechanism |
|---|---|---|
| 1 | Zero non-compliant loan structures | Deterministic Layer 1 compliance validation gate |
| 2 | Zero LLM arithmetic generation | Strictly typed calculations and deterministic template binding |
| 3 | Provable factual assertions | Profile provenance validator in outbound guardrail service |
| 4 | No fabricated human identities | Static brand templates with no natural-person name fields |
| 5 | Mandate-scoped actions only | Cryptographic mandate token check in execution circuit breaker |
| 6 | Zero client funds in custody | Zero payment rails other than fee checkout processing |
| 7 | Full pre-execution audit trail | Append-only, hash-chained ledger written ahead of external calls |
| 8 | Privacy-compliant erasure | Per-subject encryption keys supporting crypto-shredding |
| 9 | No investment structuring | Topic boundary guardrail blocking investment comparisons |
| 10 | Zero banking credential storage | No credential intake or direct bank portal automation |
| 11 | Zero lender revenue | Fee collection strictly limited to borrower payment rails |
| 12 | No direct bureau API pulls | Ingestion restricted to borrower-uploaded data files |

---

## 9. Open Gaps & Counsel Verification Items

### 9.1 🔴 Mortgage Advisory Licensing Bill (Pending)
Knesset Economic Affairs Committee approved the bill for first reading on 26 May 2026. Establishes professional standards, registration under the Ministry of Justice, and administrative penalties.
- **Action**: Obtain bill text and committee protocols; evaluate whether licensing conditions attach exclusively to natural persons or accommodate corporate/algorithmic platforms, and determine requirements for a licensed supervisory officer.

### 9.2 🔴 Lender-Side SaaS Scope Under Remuneration Prohibition
CMA draft circular broadly prohibits lender payments to mortgage representatives.
- **Action**: Confirm with counsel whether licensing file-structuring software to lenders requires separate corporate entity structuring and strict operational separation.

### 9.3 🔴 Insurance Adjacency Licensing
Brokering mortgage life and property insurance requires an insurance agency licence under the Supervision of Insurance Business Law, 1981.
- **Action**: Deferred to Phase 3; evaluate partnership vs. licensed agency acquisition model prior to Phase 3 launch.

### 9.4 🔴 Crypto-Shredding & Cross-Border LLM Transfers
- **Crypto-Shredding**: Confirm that cryptographic key destruction meets statutory anonymization/deletion standards under the Privacy Protection Law and PPA DPO Guidelines.
- **Cross-Border Transfers**: Confirm adequacy of consent (Reg. 2(1)) and Reg. 3 undertakings for sensitive financial data under the 13 April 2026 PPA opinion.

### 9.5 🟡 Formal Data Security Tier Determination
Formalize security tiering (medium tier baseline at current scale, building to high tier standards) as the primary deliverable for incoming DPO counsel.

### 9.6 🟠 AI Transparency & Disclosure Roadmap
Inter-agency Regulatory Workgroup on AI in the Financial Sector December 2025 report recommends mandatory AI disclosure in consumer financial interactions.
- **Strategy**: Position the platform so that efficacy (measurable interest savings) and radical cost advantages stand on their own merit regardless of mandatory disclosure timelines.

### 9.7 🟡 Confirmatory Legal Opinion (₪12–20K)
Commission formal legal opinion covering 2016 Law licensing status, POA mandate template, privacy terms of service, and cross-border consent documentation.

### 9.8 Consumer Protection & Professional Liability
- **Consumer Protection Law §2**: Framing strictly limited to *optimized solutions under stated inputs and market parameters*; zero claims of "guaranteed lowest cost."
- **Torts Ordinance §35**: Deterministic computation engine, algorithmic verification, and Errors & Omissions (E&O) professional liability insurance (minimum ₪5,000,000 per claim).

---

## 10. Implementation & Action Plan

**Immediate (Pre-POC)**
- [ ] 🔴 Obtain mortgage advisory bill text and committee protocols; assess corporate licensability (§9.1)
- [ ] 🔴 Commission confirmatory legal opinion (§9.7)
- [ ] Identify DPO counsel candidates; obtain zero-retention enterprise terms from LLM providers
- [ ] Integrate borrower-retrieved credit workflow (Route 1) into product architecture (§4)
- [ ] Load Directive 329 parameter specifications into configuration files with version and effective dates (§7)

**Before Production Pilot (Month 3)**
- [ ] DPO appointed; formal security tier determination recorded (§9.5)
- [ ] Power of attorney template drafted by legal counsel, excluding binding execution and disbursement
- [ ] Terms of service and privacy notices completed in plain Hebrew
- [ ] E&O insurance bound (≥ ₪5,000,000 per claim)
- [ ] Incident response and breach notification procedures documented and tested
- [ ] Cross-border transfer documentation finalized (§9.4)
- [ ] Borrower-side credit-report retrieval flow built and usability tested (§4)

**Before Soft Launch (Month 4)**
- [ ] Consent flows and privacy endpoints operational
- [ ] Crypto-shredding and data lifecycle management active (§9.4)
- [ ] Disclosed AI positioning test protocol prepared for Phase 2 (§9.6)

**Ongoing Governance**
- [ ] Directive 329 circular monitoring with tested configuration-deployment pipeline
- [ ] Mortgage advisory bill legislative tracking
- [ ] Credit Data Law §32 regulation monitoring
- [ ] PPA and AI regulatory updates monitoring; biennial security audits and annual penetration tests
- [ ] Quarterly review of regulatory strategy and compliance baselines

---

## 11. Risk Summary

| Risk | Severity | Regulatory Baseline & Mitigation |
|---|---|---|
| **Mortgage advisory licensing enacted** | 🔴 High | Bill passed committee 26 May 2026. Design compliance architecture to exceed expected standards; prepare licensed supervisory officer path. |
| **Credit brokerage licensing applies** | 🟢 Resolved | No statutory licence category exists under 2016 Law. Confirmatory legal opinion only. |
| **Direct bureau API access unavailable** | 🟡 Product Constraint | Route 1 (borrower upload) is the permanent lawful route under §27. Intake flow designed for friction-free user retrieval. |
| **Privacy breach / regulatory sanctions** | 🔴 High | P0 architectural priority. DPO appointed; high-tier security architecture implemented from inception. |
| **Financial computation error** | 🔴 High | Deterministic financial calculation engine, independent verification layer, and ₪5M E&O insurance. |
| **Mandatory AI disclosure enacted** | 🟠 12–24 Months | Expected per Dec 2025 report. Value proposition anchored on measurable rate savings and superior execution rather than anonymity. |
| **Multi-brand identity challenge** | 🟠 High | Strict factual assertion gates, registered business names, full ledger transparency. |
| **Cross-border transfer restrictions** | 🟠 Medium | Architecture decouples raw document extraction to allow local self-hosted models if necessary. |
| **Directive 329 amendments** | 🟡 Medium | Versioned configuration rules with date-aware evaluation and grandfathering support. |
| **Lender-side SaaS restriction** | 🟡 Phase 3 | Strict corporate entity separation and legal counsel sign-off prior to lender software release. |
| **Insurance adjacency licensing** | 🟡 Known | Requires insurance agency licence; deferred to Phase 3. |

---

## 12. Strategic Advantage of Regulatory Rigor

The regulatory compliance requirements in Israeli consumer finance — immutable audit logging, deterministic calculation proof, complete factual provenance, professional liability coverage, and strict data protection — impose substantial overhead on traditional individual brokers.

By building these compliance standards directly into the automated software architecture, the platform transforms regulatory requirements into an operational moat. The system maintains continuous, machine-verifiable compliance records at negligible marginal cost per file, providing institutional-grade auditability that manual advisory practices cannot match.

---

## Related Documents

- **`REGULATORY_ANSWERS.md`** — Statutory citations, circulars, and primary-source references
- **`BUSINESS_PLAN.md`** — Business model, market sizing, and regulatory risk mappings
- **`PRODUCT_ROADMAP.md`** — Product features and compliance workflow stages
- **`TECHNICAL_ROADMAP.md`** — Architectural enforcement, ledger implementation, and security controls
