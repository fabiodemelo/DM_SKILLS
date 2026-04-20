---
name: rfp-review
description: Analyze RFPs (Request for Proposal), RFQs, RFIs, SOWs, and tender documents from a C-level executive perspective. Produces a structured executive brief covering a 1-10 opportunity rating, bid/no-bid recommendation, financial analysis, resource requirements, risk assessment, compliance needs, and commercial terms. Triggers when the user shares RFP/RFQ/RFI/tender/proposal documents, pastes procurement text, asks for a bid/no-bid decision, or asks to "review this RFP", "analyze this proposal", "should we bid on this", "RFP analysis", or similar.
---

# Author
# Fabio DeMelo
# demelos.com - AI experts
# https://www.demelos.com

# RFP Review — Executive Analysis Framework

Analyze any Request for Proposal (RFP), RFQ, RFI, SOW, tender, or procurement document as if advising the CEO/owner who has 15 minutes to decide whether to pursue it.

## Purpose

Transform dense procurement documents into a crisp executive briefing that answers the only question that matters: **should we bid, and if so, how?**

## When to Use

Trigger whenever the user:
- Shares an RFP, RFQ, RFI, SOW, tender, or procurement document (PDF, Word, text)
- Pastes procurement text and asks for analysis
- Asks "should we bid?", "is this worth it?", "analyze this opportunity"
- Requests a bid/no-bid decision, proposal review, or tender evaluation
- Mentions government contracts, enterprise procurement, or sales pursuits

## Workflow

Execute these steps in order. Do not skip steps — each feeds the next.

### Step 1: Ingest & Inventory

Identify every document provided. If only one document is shared, ask whether there are attachments, amendments, or Q&A addenda. RFPs commonly include:
- Main RFP / Statement of Work (SOW)
- Pricing schedule / cost worksheet
- Technical specifications
- Terms and conditions / master services agreement
- Evaluation criteria / scoring rubric
- Past performance / experience forms
- Compliance matrices
- Amendments and Q&A responses

Confirm inventory with the user before proceeding. Missing documents (especially pricing schedules or T&Cs) should be flagged as blockers.

### Step 2: Extract the 30 Questions

Work through every question in `references/question-framework.md`. For each question:
- Provide a direct answer grounded in the document text
- Cite the RFP section / page where the answer was found
- If the document does not answer it, mark as **"Not specified — REQUEST CLARIFICATION"** and add it to the Q&A list

Do not skip questions. If a category does not apply (e.g., no certifications required), explicitly say so rather than omitting it.

### Step 3: Score the Opportunity

Apply the scoring rubric in `references/scoring-rubric.md` to generate three scores (1-10 each):
- **Opportunity Score** — Strategic fit, revenue potential, follow-on upside
- **Complexity Score** — Delivery difficulty, compliance burden, resource strain (LOWER is better)
- **Revenue Score** — TCV, margin, payment terms, cash flow

Then compute the composite rating:
```
Composite = (Opportunity x 0.4) + ((11 - Complexity) x 0.3) + (Revenue x 0.3)
```

Show the math. Do not just declare a number.

### Step 4: Flag Red Flags

Cross-check the RFP against `references/red-flags.md`. Any red flag found must be called out explicitly in the brief — do not bury them. Certain red flags (unlimited liability, IP handover, unbonded penalties, sanctioned entities) should trigger an automatic NO-BID recommendation unless the user explicitly overrides.

### Step 5: Produce the Executive Brief

Populate `assets/executive-brief-template.md` with the findings. Keep it to one screen where possible — a C-level reader should get the full picture in under 3 minutes.

Include at the top:
- Composite rating (1-10) with three sub-scores
- Bid / No-Bid / Conditional-Bid recommendation with one-sentence rationale
- Three biggest opportunities
- Three biggest risks
- Key deadline(s)

### Step 6: Produce the Scorecard

Populate `assets/scorecard-template.md` with the full 30-question breakdown. This is the appendix the CEO reads if they want to drill in.

### Step 7: Next Actions Checklist

End every analysis with a concrete action list:
- Questions to submit to the issuing authority (for the Q&A phase)
- Internal meetings needed (legal review, capacity planning, pricing sign-off)
- Partners / subcontractors to engage (if required)
- Go/no-go decision deadline (working backwards from proposal due date)

## Output Rules

- **Be specific, not generic.** "Require SOC 2 Type II" is useful. "Has compliance requirements" is not.
- **Cite sources.** Every financial number, deadline, and requirement must reference the document section.
- **Use dollar amounts and dates, not adjectives.** "Payment Net 90 on milestone acceptance" beats "payment terms are reasonable."
- **Flag assumptions.** If inferring from context, mark it clearly as "ASSUMPTION:"
- **Never hallucinate certifications, budget figures, or requirements.** If the RFP doesn't say it, don't make it up.
- **Respect confidentiality.** Treat the RFP as confidential.

## Bundled Resources

- `references/question-framework.md` — The 30-question C-level framework organized into 8 categories
- `references/scoring-rubric.md` — 1-10 scoring methodology for Opportunity / Complexity / Revenue with composite formula
- `references/red-flags.md` — Deal-breaker checklist and risk signals
- `assets/executive-brief-template.md` — One-page executive summary format
- `assets/scorecard-template.md` — Full 30-question structured scorecard

## Style

Write findings in flowing prose where possible, not fragmented bullet soup. C-level readers want reasoning, not just data points. Use tables for the scorecard, financials, and timeline — prose for the recommendation and rationale.
