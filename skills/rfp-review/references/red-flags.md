# RFP Red Flags — Deal-Breakers and Risk Signals

Cross-check every RFP against this list. Flag every match in the executive brief. Items marked **AUTO NO-BID** should trigger an automatic no-bid recommendation unless the user explicitly overrides.

---

## Tier 1: Automatic No-Bid Triggers (AUTO NO-BID)

These are contractual or structural issues that threaten company solvency or legal standing. Do not proceed without explicit override.

- **Unlimited liability.** No cap, or cap at 10x+ fees for indirect/consequential damages.
- **Unlimited indemnification.** Broad "hold harmless" without reciprocity or mutual caps.
- **Full IP assignment of background/pre-existing IP.** Buyer takes ownership of tools, frameworks, or methodologies we use across clients.
- **Unbonded liquidated damages with no cap.** Daily penalties with no ceiling.
- **Sanctioned entity or jurisdiction.** Buyer or delivery location is on OFAC, EU, or UN sanctions lists.
- **Required kickbacks, referral fees, or non-standard "facilitation" payments.** Possible FCPA / anti-bribery violation.
- **Mandatory non-compete preventing us from serving other clients in the industry.**
- **Contract governed by a jurisdiction we cannot operate in** (e.g., certain ITAR/export-controlled contexts without the required clearance).
- **No right to cure on termination for default.** Buyer can terminate without notice for any perceived breach.

---

## Tier 2: Strong Warning Signals

Don't auto-kill the deal, but these require explicit executive sign-off and likely price adjustment.

- **Liability cap below fees.** E.g., cap at 50% of fees — means we're eating cost if something goes wrong.
- **Net-120+ payment terms** without advance or milestone billing.
- **Retainage exceeding 10%** held until final acceptance with no milestones.
- **Acceptance criteria not defined.** "Subject to buyer's satisfaction" with no objective standard.
- **One-sided IP infringement indemnity.** We indemnify buyer for our IP; buyer doesn't reciprocate for their inputs.
- **Exclusivity or first-refusal clauses** that block future opportunities.
- **Compliance requirements we don't currently hold** (FedRAMP, CMMC L3, ISO 27001) with insufficient time to achieve certification.
- **Required teaming with a specific subcontractor** where we have no existing relationship.
- **Sole-source sole-decision authority** — no right to dispute, arbitrate, or escalate.
- **Audit rights exceeding 2 years post-termination** or granting unannounced on-site access.
- **Pricing schedule not provided** — buyer wants our number without disclosing budget envelope.
- **Evaluation criteria undisclosed or vague.** "Best value" with no scoring rubric.
- **Amendments issued inside 5 business days of proposal due date** without deadline extension.

---

## Tier 3: Yellow Flags

Common but worth noting in the brief. Negotiate in final terms if we win.

- **Unilateral change orders.** Buyer can modify scope at their discretion.
- **Key personnel clauses** requiring specific named individuals for project duration.
- **Non-solicitation of buyer employees** beyond contract term.
- **Most-favored-customer pricing.** We guarantee buyer our lowest price to any customer.
- **Audit of financial records** beyond the specific engagement.
- **Press release / publicity restrictions** prohibiting us from naming buyer as a client.
- **Buyer-provided tools/infrastructure** that create dependency or lock-in.
- **Ambiguous data ownership** in AI/ML engagements (training data, derived models).
- **Geographic restrictions on personnel** (e.g., U.S. persons only) without clear rationale.
- **Mandatory SLA credits without cap** on a specific metric.

---

## Industry-Specific Red Flags

### Government (Federal / State / Local)
- CMMC level we don't hold
- Required SAM.gov registration we haven't completed
- Buy American / Trade Agreements Act constraints
- FAR/DFARS flowdown clauses we can't comply with
- Set-aside we don't qualify for (8(a), HUBZone, SDVOSB)

### Healthcare
- HIPAA Business Associate Agreement with broad liability
- PHI handling requirements without commensurate pricing
- FDA validation scope creep

### Financial Services
- SOC 2 Type II without grace period
- Regulatory examination rights (OCC, FDIC, Fed)
- Model risk management (SR 11-7) obligations

### International
- Data residency in jurisdictions we don't operate in
- GDPR Article 28 processor terms
- Local employment law compliance for staff augmentation

---

## How to Use

For each flag found:
1. Quote the RFP language verbatim (with section reference)
2. Classify the tier
3. State the implication in one sentence
4. Recommend negotiation position or no-bid

Example:

> **Tier 1 — AUTO NO-BID**
> Section 8.2: "Contractor shall be liable for all direct, indirect, consequential, and punitive damages without limitation."
> Implication: Unlimited liability exposure. A single failure could bankrupt the company.
> Recommendation: No-bid unless liability is capped at 1-2x annual fees with mutual carve-outs for IP, confidentiality, and gross negligence.
