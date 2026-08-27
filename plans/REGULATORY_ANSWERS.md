# Regulatory Verification Log — Settled Findings & Citations

**Date**: August 27, 2026 · **Companion to**: `plans/REGULATORY_STRATEGY.md`
**Purpose**: primary-source backing for every position marked *settled* in the strategy document, plus a record of what the earlier draft got wrong. Open questions live in `REGULATORY_STRATEGY.md` §9 and are not repeated here.

> Sourced from primary legal texts and reputable secondary reporting, not from counsel. Sign-off still required before the pilot.

---

## 1. Corrections to the Previous Draft

The items below were stated incorrectly in the first version of these plans. Anything built on them needs rechecking.

| # | Previously stated | Correct position |
|---|---|---|
| 1 | The 2016 law licenses "credit brokerage"; our exemption rests on regulatory forbearance | **No such licensable activity exists.** Licence types: service in a financial asset, provision of credit, deposit-and-credit services, operating a P2P credit-intermediation platform. None captures borrower-paid advisory or referral work. |
| 2 | Licence requirement at §11; P2P definition at §25B | **§12(a)** is the licence requirement; **§25יז** defines the platform (§25יח prohibits operating one unlicensed). §11א holds only the Chapter ג׳ definitions. |
| 3 | Route 2 — consented bureau API pull, ₪15–30/query, needs only a data-user contract plus consent | **Not lawful.** §27 confines report requests to credit providers; §26(a) conditions are cumulative; the §32 power to designate non-lender recipients has never been exercised and narrows from 26 July 2027. Route 1 only. |
| 4 | Credit Data Law purpose limitation at §31, retention at §19/§33 | **§26(a)(1)** is purpose limitation. §19 = reporting into the register; §30 = customer opt-out from release; §31 = credit provider's post-receipt notification duty; §33 = credit indication; §51 = non-discrimination in bureau models. |
| 5 | Data Security high tier at 10,000 subjects or 100 users | **100,000 subjects, or more than 100 authorized users.** At pilot scale we are medium tier. |
| 6 | Amendment 13 strengthens a right to erasure | **No GDPR Art. 17-style general erasure right exists in Israeli law.** §14 covers incorrect, incomplete, unclear, or outdated data. The broad "no longer necessary" right sits in the EEA Data Regulations, 2023. Amendment 13's contribution was the enforcement layer. |
| 7 | 100,000 is a DPO trigger | It is the **database-notification** threshold for specially sensitive information. DPO triggers (§17ב1): public bodies; data brokers at 10,000+; systematic monitoring at significant scale; specially sensitive information at significant scale. |
| 8 | PTI 50% is a regulatory ceiling with a 30–38% practical target | **§5 is a flat prohibition** — a bank shall not approve or execute above 50%. Separately, **§6 imposes a 100% risk weight above 40% PTI**, which is what actually drives the 30–40% practical target. |
| 9 | Risk weights 35 / 50 / 75% at LTV 45 / 60 / 75% | **35% up to 45%; 50% above 45% to 60%; 60% above 60%** (Directive 203 §72, circular 2816, April 2025). 75% was never an LTV breakpoint. Reduced weights require loan ≤ ₪5M. |
| 10 | All-purpose loan at 70% is a temporary wartime relief order | **Permanent** as §10א since circular 2840, 8 February 2026. Increment above 50% still capped at ₪200,000. |
| 11 | Discounted-program ceiling ₪1,800,000 (₪1,500,000 older vintage); minimum equity ₪100,000 or 10%; loan ≤ 90% of contract price | **₪2,100,000** since circular 2840, February 2026. **No ₪1.5M vintage.** Minimum equity **₪100,000, or ₪60,000 with an Accountant General grant — no percentage alternative.** The 90% figure is commercial marketing arithmetic, not a rule. |
| 12 | Early repayment discounts 10% after 3 years, 20% after 5 | **§8(b): 20% at three-to-under-five years, 30% at five years or more.** The 10/20/30/40% ladder in §8(a) applies only to supplementary loans for directed-loan eligibles. |
| 13 | Early repayment governed by the 2002 "עמלות פירעון מוקדם" order | Correct instrument is the **Banking Order (Early Repayment of a Housing Loan), 2002**; the 1981 fees order was repealed by its §13. |
| 14 | 0.1% fee if 10 days' notice "is not submitted" | Applies where the borrower gave **less than** ten days' notice. Also: of the two discounting fees (§3(3), §3(4)) the bank may charge **only the lower**. |
| 15 | Maximum 30-year term in force since April 2014 | Supervisor's measures of August 2013, **effective 1 September 2013**, codified when Directive 329 was first published **15 July 2014**. |
| 16 | Variable-rate cap reformed December 2020 | Decided 14 Dec 2020 (circular 2647), **effective 17 Jan 2021** for new loans and 28 Feb 2021 for refinancing. The reform removed a separate 2011 cap on the **prime track specifically**; the one-third-fixed structure predates it. |
| 17 | Foreign residents "automatically categorized as investors" as a distinct rule | Not a separate category — "sole" and "replacement" dwelling are defined only for Israeli citizens, so a foreign-resident purchase is necessarily an investment dwelling. Same 50% outcome, different mechanism. |
| 18 | Mandatory AI disclosure projected 2026–2028 | The **December 2025 inter-agency report on AI in the Financial Sector already recommends it** and directs regulators to update their rules. 12–24 months, not 2–4 years. |
| 19 | Amendment 13 amended the cross-border transfer rules | It did not. The 2001 regulations stand. The relevant developments are the **EEA Data Regulations, 2023** and a **PPA opinion of 13 April 2026** reading Reg. 2(4) restrictively. |
| 20 | "Credit marketer" is a legal classification risk | No such licensing regime exists. The real constraint is the **2026 Capital Market Authority draft circular prohibiting lender remuneration to mortgage advisors entirely**. |

---

## 2. Licensing

**No credit-brokerage licence.** Supervision of Financial Services (Regulated Financial Services) Law, 2016. Licence types: service in a financial asset; provision of credit (§12(a)); deposit-and-credit services (Chapter ג׳2); operating a P2P credit-intermediation platform (§25יז, prohibition §25יח). The platform licence covers running an online marketplace that matches lenders and borrowers and operates the transactions — not human or algorithmic advisory. Conditions to maintain the position: borrower-paid only, zero lender compensation, zero custody of funds, express signed power of attorney per file.
*Sources*: full law text (Wikisource, "חוק הפיקוח על שירותים פיננסיים (שירותים פיננסיים מוסדרים)"); [supervisor's licensing procedure](https://www.chamber.org.il/media/161364/th_2019-8223.pdf)

**Mortgage advisory is unlicensed today, and a bill is moving.** The Knesset Economic Affairs Committee approved the Regulation of Mortgage Advisory Bill (MKs Asher / Beliak) for first reading on **26 May 2026** — professional threshold conditions, binding ethics rules, and financial-sanction enforcement, supervised by the Ministry of Justice as registrar of regulated professions rather than by a new council. Readings two and three were deferred to the next Knesset pending agreement on implementation budget.
*Source*: [Knesset press release, 26 May 2026](https://main.knesset.gov.il/News/PressReleases/Pages/press26052026n.aspx)

**Lender remuneration is being prohibited outright.** A 2026 Capital Market, Insurance and Savings Authority draft circular would bar non-bank lenders (credit-provision, P2P-platform, and deposit-and-credit licensees) from paying mortgage advisors any commission, conference funding, advertising, training, or other benefit of economic value on housing loans, reverse mortgages, or any real-estate-secured loan. "Representative" is defined broadly to catch anyone acting as a mortgage advisor to promote a transaction, expressly to prevent relabelling; advisors may be paid "by the customer only." The Authority signalled a parallel rule for institutional bodies. Banks are already barred under Banking Supervision rules.
*Sources*: [Globes](https://www.globes.co.il/news/article.aspx?did=1001543816); [Calcalist](https://www.calcalist.co.il/market/article/rklhdeelmg)

**Algorithmic delivery.** Israeli robo-advisory constraints attach to the 1995 Investment Advice Law, which does not reach mortgage advisory. The national framework is the December 2023 Ministry of Innovation / Ministry of Justice **Policy on AI Regulation and Ethics** — non-binding, sector-led soft law, with an AI Policy Coordination Center, a May 2025 National AI Program, and a 2026 National AI Strategic Plan continuing that posture. No horizontal licensing requirement for algorithmic decision support in consumer finance.
*Source*: [Israel's AI Policy 2023](https://www.gov.il/BlobFolder/policy/ai_2023/en/Israels%20AI%20Policy%202023.pdf)

**Reserved legal work.** Bar Association Law, 1961 §20 reserves to lawyers, when done as a business or for consideration: representation before courts, tribunals and arbitrators; representation and action before the **Land Registry**, Execution Office, Companies Registrar, and tax authorities; **drafting documents of a legal character** for another, including negotiation toward such a document; and legal advice and opinions. Mortgage registration and title work falls inside limbs (2) and (3). A February 2026 amendment to the Notaries Law abolished the notarized-POA requirement for registering a mortgage in favour of a banking corporation — a notarization formality only, not a change to the reservation.
*Source*: [Bar Association Law §20](https://fs.knesset.gov.il/4/law/4_lsr_209049.PDF)

---

## 3. Privacy

**Amendment 13** passed second and third readings **5 August 2024**; in force **14 August 2025**.

**DPO duty (§17ב1)** — four triggers: public bodies under §23; data brokers holding data on 10,000+ people; entities whose main activity involves systematic monitoring of individuals at significant scale; entities whose main activity involves processing specially sensitive information at significant scale (expressly aimed at banks, insurers, HMOs, and financial companies). Enforcement deferred to 31 October 2025; the PPA published its final opinion on the duty on 15 July 2026 and a DPO guide in January 2026. Separately, databases holding specially sensitive information on **100,000+** people must be notified to the PPA.

**"Information of special sensitivity"** — Amendment 13 replaced "sensitive information" and **expressly added salary data and financial activity**, alongside intimate and family life, sexual orientation, medical and mental-health data, genetic data, biometric identifiers, political opinions and religious belief, personality and psychological assessments, criminal record, ethnic origin, location and communications data, and information subject to a statutory duty of confidentiality.

**Data Security Regulations, 2017** — the **high** level applies to a First Schedule item 1(1) or 1(3) database holding data on **100,000+** people, or having **more than 100 authorized users**. The **medium** level applies to First Schedule types, including any database containing a listed sensitive category (which captures financial data), with a carve-out down to basic level where the sensitive data concerns only the controller's own employees or suppliers for business-management purposes, or where authorized users number ten or fewer. High level adds a risk survey (Reg. 5(c)) and penetration testing (Reg. 5(d)) at least every 18 months. Medium and high both require internal or external audit every 24 months (Reg. 16). Access-control log retention is 24 months (Reg. 10(d)). Reg. 2 requires the database definition document.

**Cross-border transfer** — Transfer of Data Abroad Regulations, 2001, unamended by Amendment 13. Reg. 1 sets a general adequacy test. Reg. 2 lists eight alternative grounds: **consent (2(1))**; vital interest where consent is unobtainable (2(2)); transfer to an entity under the transferor's control with secured protection (2(3)); **contractual undertaking to observe Israeli holding-and-use conditions (2(4))**; lawfully published data (2(5)); public safety (2(6)); required by Israeli law (2(7)); and **Council of Europe convention states, states receiving EC-member data on the same terms, or gazetted states (2(8))** — which is how EU/EEA transfers are reached, there being no Israeli adequacy whitelist. Reg. 3 additionally requires a written undertaking on security and onward transfer. A **PPA opinion of 13 April 2026** reads Reg. 2(4) restrictively, requiring the recipient to commit contractually to purpose limitation, access/correction/deletion rights, confidentiality, and either the Data Security Regulations or ISO 27001. The **EEA Data Regulations, 2023** impose additional obligations on data received from the EEA.

**EU adequacy** — Israel's 2011 adequacy decision was reviewed in the Commission's January 2024 report (COM(2024) 7 final) and left unmodified; Amendment 13 and the 2023 EEA regulations formed part of the strengthening negotiated during that review. Civil-society and MEP pressure to reassess (April 2024, January 2025, June 2025) has not produced a formal review. Intact, but politically exposed.

**Deletion and crypto-shredding** — no general erasure right (correction 6). The PPA's January 2026 DPO guide requires, at end of information life, that data be **deleted or anonymized (התממה)**, which is the doctrinal hook for a crypto-shredding argument, but no published guidance addresses key destruction by name. Open — `REGULATORY_STRATEGY.md` §9.4.

**Enforcement** — administrative fines (e.g. ILS 15,000 for failing to notify a refusal to correct or delete) and a PPA power to seek a court order to stop processing or delete data (§23מט).

*Sources*: Data Security Regulations and Transfer Abroad Regulations (Wikisource, Hebrew consolidated texts); [Ministry of Justice Amendment 13 guide](https://maimonweb.com/wp-content/uploads/2025/09/tikun-13-guide.pdf); [coverage of the PPA DPO guide, Jan 2026](https://www.law.co.il/news/2026/01/22/israeli-ppa-publishes-guide-for-dpos/); [Gornitzky on the April 2026 opinion](https://www.gornitzky.co.il/30861/); [European Parliament answer E-000176/2025](https://www.europarl.europa.eu/doceo/document/E-10-2025-000176-ASW_EN.html)

---

## 4. Credit Data

The Bank of Israel has operated the credit data register since April 2019. Five entities received preliminary approvals; **only BDI-Coface and D&B Israel (consumer app: Captain Credit) actually operate as licensed bureaus.**

**Who may receive a report** — §27 permits **only a credit provider** to request a credit report from a bureau, and only to enter into a credit transaction with that customer or ensure compliance with its terms. §26(a) makes those conditions cumulative preconditions for the bureau to receive data from the register at all; customer consent is §26(a)(4), one condition among several rather than a standalone basis. §32 allows the Minister of Justice, with the Governor's agreement and Finance Committee approval, to designate non-lenders as eligible recipients subject to consent — **no such regulations have been made**, and from 26 July 2027 the power narrows to individual customers. §33 credit indication is likewise confined to credit providers, extending from 26 January 2027 only to assignees of consumer payment obligations. **A bureau API serving a non-lender advisor would put the bureau in breach.**

**The lawful route** — §38(d): the data-concentration report is provided to the customer free of charge once a year on request, with the Bank of Israel obliged to notify customers of the right annually and the register manager to notify within 30 days of starting collection (§109). Further reports in the same year carry a fee. From 26 January 2027 the free entitlement is limited to individual customers.

*Sources*: Credit Data Law §§26–33, 38, 51, 109 (Wikisource consolidated text); [official PDF, creditdata.org.il](https://www.creditdata.org.il/); [State Comptroller report on the credit data register](https://library.mevaker.gov.il/sites/DigitalLibrary/Pages/Reports/7692-3.aspx)

---

## 5. Agency, Recording & AI Disclosure

**Agency Law, 1965** — §1 defines agency as authorization to perform legal acts in the name or place of the principal; §2 makes the agent's act bind and entitle the principal directly; §3(a) creates agency by written authorization. The POA must authorize submitting applications and documentation to banking and non-banking lenders, negotiating rates, tracks, terms and fees, and receiving preliminary approvals, draft schedules and term sheets — and must **expressly exclude authority to execute binding loan agreements or disburse funds**.

**Recording** — Secret Monitoring Law, 1979 §1 defines secret monitoring as listening or recording without the consent of **any** party, so recording a call you are party to is lawful with no duty to inform the other side. §3 nevertheless prohibits recording by a party where the purpose is to commit an offence, cause malicious harm, or improperly reveal intimate matters, and lawful recording can still attract civil privacy liability depending on subsequent use. Treatment: opening notice in Hebrew that the call is recorded for service and quality assurance, encrypted storage with short retention, and access logged in the ledger.

**Deception boundary** — Penal Law §415 (obtaining a benefit by deception) requires an affirmative false statement or concealment of a fact one is obliged to disclose. Absent a disclosure duty, operating as a named corporate desk while every substantive statement about the borrower, income and property is true is not deception. **Denying being an automated system when directly asked is affirmative misrepresentation.**

**AI disclosure** — no binding Israeli statute and no horizontal AI act as of 2026. The **December 2025 final report of the inter-agency Regulatory Workgroup on AI in the Financial Sector** (Ministry of Justice, Ministry of Finance, Competition Authority, Securities Authority, Capital Market Authority, Bank of Israel) **recommends mandatory notification that a chatbot interaction is with an AI system rather than a human**, especially where users might mistakenly believe otherwise, and directs regulators to update their rules. Treat as an expected supervisory requirement. **EU AI Act Article 50(1)** chatbot disclosure has applied since **2 August 2026**; the AI Omnibus (Regulation (EU) 2026/1744) granted only a narrow transition to 2 December 2026 for the provider-side machine-readable marking duty under Art. 50(2), and the high-risk regime — which includes credit scoring under Annex III — slipped to 2 December 2027.

**Multi-brand structuring** — every brand must be a registered company or registered business name under the Registration of Business Names Ordinance, with real phone numbers, registered addresses, and verified domains. Outbound identity is always an institutional desk, never a natural person; the `advisor_identity` entity has no first- or last-name field.

*Sources*: [AI in the Financial Sector final report, Dec 2025](https://www.gov.il/BlobFolder/reports/report_0013/he/AI%20in%20the%20Financial%20Sector%20-%20Final%20Report%20Dec2025.pdf); [Commission FAQ on AI Act Art. 50](https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act); [Goodwin on Art. 50 entry into force](https://www.goodwinlaw.com/en/insights/publications/2026/08/alerts-technology-dpc-eu-ai-act-transparency-obligations-now-in-force)

---

## 6. Directive 329 & Pricing Mechanics

Verified against **Directive 329 version 13, effective 30 June 2026** (circulars **2840** of 8 February 2026 and **2852** of 30 June 2026), **Directive 203 §72** as rewritten by **circular 2816** (April 2025), the official Q&A, and the early-repayment order. The full parameter table with section citations lives in `REGULATORY_STRATEGY.md` §7 and is the single source of truth for the compliance engine; it is not duplicated here.

Two scope notes that affect rule evaluation:
- Circular 2852 narrowed "housing loan" to **dwellings in Israel**.
- A transitional provision defers part of the circular 2840 amendment, including Q&A 5.1, to **1 October 2026** — so rules must be evaluated against the version in force at the file's date.

Prime is the Bank of Israel policy rate plus 1.5%, a spread unchanged since 1994, with eight scheduled Monetary Committee decisions per year.

*Sources*: [Directive 329 v13 / circular 2852](https://www.boi.org.il/media/hjrlkrse/h2852.pdf); [circular 2840](https://www.boi.org.il/media/ptciib1v/h2840.pdf); [official Q&A](https://boi.org.il/media/hhojvtdr/h2676.pdf); [Directive 203](https://boi.org.il/media/rixn0v3j/203_16.pdf); [early repayment order](https://boi.org.il/media/qy5cow0l/116.pdf); [BoI rate decision dates](https://www.boi.org.il/roles/monetary-policy/interest-rate-dates)

---

## 7. Consumer Protection & Tort

**Consumer Protection Law, 1981 §2** prohibits any act or omission likely to mislead a consumer on a material matter. Describing a mix as "the absolute cheapest" when it carries unmodelled risk is exposure. Required framing: *optimized models based on current parameters and specific borrower inputs*, never guaranteed lowest cost.

**Torts Ordinance §35** — an objectively flawed recommendation (miscalculated linkage, a Directive 329 violation, a misstated prepayment penalty) is actionable negligence for direct financial harm. Mitigations: a deterministic financial core with zero arithmetic delegated to LLMs; independent algorithmic recomputation before any client-facing output; and **E&O cover of at least ₪5,000,000 per claim**, endorsed for algorithmic financial analysis.

**Adjacency boundaries** — insurance broking requires an agent licence under the Supervision of Insurance Business Law, 1981 (Phase 3, licensed-partner path). Investment advice under the 1995 law is a hard prohibition: asked whether to invest savings or prepay, the agent produces mortgage-side arithmetic only and declines the investment recommendation. Tax is limited to deterministic statutory purchase-tax brackets with a disclaimer.

---

*This log is the regulatory specification baseline for Layer 1 compliance modules. Open questions are tracked only in `REGULATORY_STRATEGY.md` §9.*
