# Israeli Regulatory Citations & Statutory References

**Date**: August 27, 2026 · **Companion to**: `plans/REGULATORY_STRATEGY.md`
**Purpose**: Primary-source legal authority, statutory citations, and regulatory baselines for the compliance engine and operational strategy.

> Sourced from primary legal texts, official circulars, and regulatory publications. Formal legal sign-off required prior to production pilot.

---

## 1. Licensing & Regulated Activities

### 1.1 Supervision of Financial Services (Regulated Financial Services) Law, 2016
- **Scope of Licensable Activities**: The statute establishes four distinct licensing categories:
  1. Service in a financial asset (§11א, §12(a))
  2. Provision of credit (§12(a))
  3. Deposit-and-credit services (Chapter ג׳2)
  4. Operating a peer-to-peer (P2P) credit-intermediation platform (§25יז, with unlicensed operation barred under §25יח)
- **Application to Mortgage Advisory**: No statutory category captures borrower-paid advisory, mortgage structuring, or borrower representation. The P2P platform licence covers matching marketplace operators executing credit transactions directly.
- **Mandatory Operating Boundaries**:
  - 100% borrower-paid fee structure
  - Zero remuneration, commission, or economic benefit from lenders
  - Zero custody, receipt, or handling of borrower or lender funds
  - Formal, signed power of attorney per customer file
- *Sources*: Supervision of Financial Services Law (Wikisource consolidated Hebrew text); [Supervisor Licensing Procedure](https://www.chamber.org.il/media/161364/th_2019-8223.pdf)

### 1.2 Regulation of Mortgage Advisory Bill (Pending)
- **Legislative Status**: Approved for first reading by the Knesset Economic Affairs Committee on **26 May 2026** (MKs Asher / Beliak). Readings two and three deferred to the next Knesset pending budget allocation.
- **Core Provisions**: Professional entry thresholds, statutory code of ethics, and administrative sanctions enforced by the Ministry of Justice (Registrar of Regulated Professions).
- *Source*: [Knesset Press Release, 26 May 2026](https://main.knesset.gov.il/News/PressReleases/Pages/press26052026n.aspx)

### 1.3 Prohibition on Lender Remuneration
- **Regulatory Posture**: Capital Market, Insurance and Savings Authority draft circular (2026) prohibiting non-bank lenders (credit providers, P2P platforms, deposit-and-credit institutions) from paying mortgage advisors commissions, marketing fees, conference sponsorships, or training stipends on real-estate-secured loans.
- **Scope**: Defines "representative" broadly to encompass any entity promoting or advising on a loan, mandating payment "by the customer only." Banks are independently barred under Banking Supervision rules.
- *Sources*: [Globes](https://www.globes.co.il/news/article.aspx?did=1001543816); [Calcalist](https://www.calcalist.co.il/market/article/rklhdeelmg)

### 1.4 Algorithmic Delivery & AI Policy
- **Robo-Advisory Scope**: Israel's robo-advisory constraints attach to the Regulation of Investment Advice, Investment Marketing and Portfolio Management Law, 1995, which does not govern mortgage advisory.
- **National Soft-Law Framework**: Governed by the Ministry of Innovation, Science and Technology and Ministry of Justice *Policy on AI Regulation and Ethics* (December 2023), supported by the National AI Program (May 2025) and National AI Strategic Plan (2026). Employs sectoral soft law with no horizontal licensing regime for automated consumer-finance decision tools.
- *Source*: [Israel AI Policy 2023](https://www.gov.il/BlobFolder/policy/ai_2023/en/Israels%20AI%20Policy%202023.pdf)

### 1.5 Reserved Legal Practice
- **Bar Association Law, 1961 §20**: Exclusively reserves specific commercial services to licensed attorneys:
  - Representation before courts, tribunals, and the **Land Registry (טאבו)** (§20(1), §20(2))
  - **Drafting documents of a legal character**, including contract negotiations (§20(3))
  - Legal advice and formal legal opinions (§20(4))
- **Application**: Platform limits operations to fact extraction, quantitative rule validation, and bank term comparison. Mortgage registration and title mechanics remain with the borrower's conveyancing counsel. (Note: February 2026 Notaries Law amendment removed the notarized POA requirement for bank mortgages, which represents a notarization procedural change rather than a modification of §20).
- *Source*: [Bar Association Law §20](https://fs.knesset.gov.il/4/law/4_lsr_209049.PDF)

---

## 2. Privacy & Data Protection

### 2.1 Protection of Privacy Law (Amendment 13)
- **Legislative Timeline**: Passed 5 August 2024; entered into force **14 August 2025**.
- **Information of Special Sensitivity**: Replaced former "sensitive information" definition and expressly added **salary data and financial activity**, alongside health, biometric, genetic, criminal, intimate life, and statutory confidential records.
- **Data Protection Officer (DPO) Mandatory Appointment (§17ב1)**: Required for entities whose primary activities include:
  1. Processing "information of special sensitivity" at significant scale (expressly capturing consumer financial services)
  2. Systematic monitoring of individuals at significant scale
  3. Data brokerage holding data on 10,000+ individuals
  4. Public authorities (§23)
- **Enforcement & Sanctions**: Direct administrative fines and Privacy Protection Authority (PPA) powers to obtain judicial orders halting processing or compelling data deletion (§23מט).
- *Sources*: [Ministry of Justice Amendment 13 Guide](https://maimonweb.com/wp-content/uploads/2025/09/tikun-13-guide.pdf); [PPA DPO Guidelines, Jan 2026](https://www.law.co.il/news/2026/01/22/israeli-ppa-publishes-guide-for-dpos/)

### 2.2 Data Security Regulations, 2017
- **Medium Security Level**: Applies to databases containing sensitive financial data with authorized users up to 100 and fewer than 100,000 data subjects. Requires:
  - Database Definition Document (Reg. 2)
  - Role-based physical and logical access controls (Regs. 7–9)
  - Access-control logging with **24-month retention** (Reg. 10(d))
  - Periodic internal or external security audit every **24 months** (Reg. 16)
- **High Security Level**: Triggers when holding sensitive data on **100,000+ data subjects** or with **more than 100 authorized users**. Adds mandatory risk surveys (Reg. 5(c)) and penetration tests (Reg. 5(d)) every **18 months**.

### 2.3 Cross-Border Data Transfers & EU Adequacy
- **Transfer of Data Abroad Regulations, 2001**: Unamended by Amendment 13. Reg. 2 sets lawful transfer grounds:
  - Express informed consent (Reg. 2(1))
  - Contractual undertaking to uphold Israeli protection standards (Reg. 2(4))
  - Transfers to EEA/Council of Europe convention signatories (Reg. 2(8))
  - Reg. 3 requires written data protection and onward-transfer commitments.
- **PPA Position (13 April 2026)**: Reads Reg. 2(4) restrictively, requiring recipients to contractually commit to purpose limitation, subject access/correction/deletion rights, confidentiality, and compliance with Data Security Regulations or ISO 27001.
- **EU Adequacy Status**: Maintained under European Commission review COM(2024) 7 final (January 2024), strengthened by Amendment 13 and EEA Data Regulations, 2023.
- *Sources*: [Gornitzky on PPA Cross-Border Opinion](https://www.gornitzky.co.il/30861/); [European Parliament E-000176/2025](https://www.europarl.europa.eu/doceo/document/E-10-2025-000176-ASW_EN.html)

---

## 3. Credit Data Law, 2016 & Register Operations

### 3.1 Registry & Bureau Architecture
- **National Credit Register**: Operated by the Bank of Israel since April 2019.
- **Licensed Bureaus**: BDI-Coface and D&B Israel (consumer application: Captain Credit).

### 3.2 Permitted Inquiries & Data Access (§27)
- **Lender-Exclusivity (§27)**: Direct requests for credit reports from licensed bureaus are legally restricted **exclusively to credit providers**, strictly for considering or executing a credit transaction with the subject.
- **Cumulative Preconditions (§26(a))**: Bureau extraction requires cumulative conditions (§26(a)(1) purpose limitation; §26(a)(4) customer consent). Consent alone cannot authorize non-lender API access.
- **Non-Lender Designation (§32)**: Ministerial authority to designate non-lenders as report recipients has never been enacted, and statutory authority narrows to individual consumers starting 26 July 2027.
- **Statutory Route (Route 1)**: Under §38(d), every individual is legally entitled to one free **data-concentration report (דוח ריכוז נתונים)** per calendar year directly from the Bank of Israel register. The platform operates exclusively on borrower-retrieved and user-uploaded reports.
- *Sources*: Credit Data Law, 2016 §§26–33, 38, 51, 109; [Bank of Israel Credit Data System](https://www.creditdata.org.il/); [State Comptroller Credit Register Audit](https://library.mevaker.gov.il/sites/DigitalLibrary/Pages/Reports/7692-3.aspx)

---

## 4. Agency, Recording & AI Transparency

### 4.1 Agency Law, 1965
- **Legal Representation (§§1–3)**: Written authorization empowers the agent to execute legal acts on behalf of the principal that directly bind and entitle the principal.
- **Mandate Boundaries**: Authorization includes preparing filings, gathering quotes, and negotiating tracks with lenders, while explicitly excluding binding loan agreement execution or disbursement authorizations.

### 4.2 Call Recording & Surveillance
- **Secret Monitoring Law, 1979**: §1 defines illegal wiretapping as recording without consent of *any* participant. Recording calls to which the platform/representative is an active party is fully lawful. §3 prohibits recording for unlawful or tortious purposes.
- **Standard Protocol**: Pre-call notification that calls may be recorded for service documentation and quality assurance.

### 4.3 Anti-Fraud & Deception Boundaries (Penal Law, 1977 §415)
- **Legal Test**: Obtaining a benefit by deception requires an affirmative false statement or concealment of a material fact subject to a disclosure duty.
- **Operational Rule**: All borrower financial assertions must map strictly to verified profile data. Operating as an institutional advisory desk is lawful; falsely claiming to be an individual human or denying automated assistance when directly asked constitutes prohibited affirmative misrepresentation.

### 4.4 AI Disclosure Standards
- **Domestic Recommendation**: Inter-agency Regulatory Workgroup on AI in the Financial Sector (Bank of Israel, ISA, CMA, MoJ, MoF, Competition Authority) December 2025 Final Report recommends mandatory disclosure when consumers interact with automated AI systems.
- **International Benchmark**: EU AI Act Article 50(1) chatbot transparency obligations in force since **2 August 2026**.
- *Sources*: [AI in Financial Sector Final Report, Dec 2025](https://www.gov.il/BlobFolder/reports/report_0013/he/AI%20in%20the%20Financial%20Sector%20-%20Final%20Report%20Dec2025.pdf); [EU AI Act Art. 50 Guidance](https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act)

---

## 5. Directive 329 & Banking Supervision Rules

### 5.1 Primary Directives & Circulars
- **Bank of Israel Proper Conduct of Banking Business Directive 329** (*Housing Loans*): Version 13, effective 30 June 2026, incorporating Circular **2840** (8 February 2026) and Circular **2852** (30 June 2026).
- **Directive 203 §72** (*Capital Measurement and Capital Adequacy - Credit Risk*): Rewritten by Circular **2816** (April 2025).

### 5.2 Hard Statutory & Underwriting Parameters

| Metric | Regulatory Value | Citation / Basis |
|---|---|---|
| **LTV — Sole Dwelling** | ≤ 75% | Directive 329 §2 |
| **LTV — Replacement Dwelling** | ≤ 70% | Directive 329 §2 |
| **LTV — Investment Dwelling** | ≤ 50% | Directive 329 §2 (includes foreign residents as "sole/replacement" definitions apply only to Israeli citizens) |
| **LTV — All-Purpose Loan** | ≤ 50% baseline; up to 70% if increment ≤ ₪200,000 | Directive 329 §10א (Circular 2840 permanent status; BoI Q&A 2.2, 8.2) |
| **Variable Rate Cap** | ≤ 66.66% (2/3 of total loan) | Directive 329 §7 (Circular 2647, effective 17 Jan 2021) |
| **Fixed Rate Floor** | ≥ 33.33% (1/3 of total loan) | Directive 329 §7 |
| **Payment-to-Income (PTI)** | ≤ 50% (**Absolute Prohibition**) | Directive 329 §5 ("bank shall not approve and shall not execute") |
| **Maximum Amortization Term** | 30 years | Directive 329 §8 (in force since 1 Sep 2013) |
| **Prime Rate Benchmark** | BoI Policy Rate + 1.50% | Fixed spread established 1994; 8 scheduled annual rate decisions |
| **Discounted Purchase Programs (Mehir LeMishtaken / Matara)** | LTV calculated on appraised value up to ₪2,100,000 | Directive 329 §4א (raised from ₪1.8M by Circular 2840, Feb 2026) |
| **Minimum Equity (Discounted Programs)** | ₪100,000 (or ₪60,000 with Accountant General grant) | Directive 329 §4א |
| **Risk Weights by LTV** | 35% (LTV ≤ 45%); 50% (45% < LTV ≤ 60%); 60% (LTV > 60%) | Directive 203 §72 (Circular 2816, April 2025; applies to loans ≤ ₪5M) |
| **Risk Weight Surcharge by PTI** | 100% risk weight across entire loan if PTI > 40% | Directive 329 §6 (drives commercial target PTI to 30–40%) |
| **Geographic Scope** | Limited strictly to dwellings in Israel | Directive 329 (Circular 2852) |

### 5.3 Early Repayment Rules
- **Governing Instrument**: Banking Order (Early Repayment of a Housing Loan), 2002 (repealed 1981 order under §13).
- **Fee Components (§3)**:
  - Operational handling fee capped at **₪60** (§3(1))
  - Notice fee: **0.1%** applied only if borrower provides less than 10 days' prior notice (§3(2))
  - Economic discounting fee: calculated under §3(3) and §3(4), of which the bank may charge **only the lower amount**
  - Average index change fee: applies to CPI-linked loans repaid between the 1st and 15th of the month (§5)
- **Statutory Repayment Discounts (§8(b))**:
  - 20% discount on economic fee for loans held between 3 and 5 years
  - 30% discount on economic fee for loans held for 5 years or longer
- *Sources*: [Directive 329 v13 / Circular 2852](https://www.boi.org.il/media/hjrlkrse/h2852.pdf); [Circular 2840](https://www.boi.org.il/media/ptciib1v/h2840.pdf); [Official BoI Q&A](https://boi.org.il/media/hhojvtdr/h2676.pdf); [Directive 203](https://boi.org.il/media/rixn0v3j/203_16.pdf); [Early Repayment Order](https://boi.org.il/media/qy5cow0l/116.pdf)

---

## 6. Consumer Protection & Tort Standards

### 6.1 Consumer Protection Law, 1981 §2
- **Misleading Representation**: Prohibits any act or omission likely to mislead a consumer regarding material loan terms, costs, or risks.
- **Standard**: Forbids marketing loan tracks as "guaranteed cheapest"; mandates clear descriptions of interest rate risk, linkage mechanics, and simulation assumptions.

### 6.2 Professional Negligence (Torts Ordinance §35)
- **Standard of Care**: Professional advisory requires reasonable care in financial computations, regulatory compliance, and track structuring.
- **Required Controls**:
  - Deterministic calculations for all amortizations, fees, and regulatory bounds
  - Algorithmic validation independent of LLM generative layers
  - Comprehensive Errors & Omissions (E&O) professional liability coverage (minimum ₪5,000,000 per claim)

### 6.3 Adjacent Regulated Boundaries
- **Insurance Brokerage**: Quoting and binding mortgage life/property insurance requires an insurance agent licence under the Supervision of Insurance Business Law, 1981.
- **Investment Advisory**: Making comparative recommendations between mortgage prepayment and capital-market investments is prohibited under the Regulation of Investment Advice Law, 1995. Calculations are confined to mortgage-side arithmetic.
