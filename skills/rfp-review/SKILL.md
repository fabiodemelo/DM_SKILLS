---
name: rfp-review
description: Analyze RFPs (Request for Proposal), RFQs, RFIs, SOWs, and tender documents from a C-level executive perspective. Integrates with the Alta Apps API to fetch RFP data, post analysis notes, update fields, and attach documents. Produces a structured executive brief covering a 1-10 opportunity rating, bid/no-bid recommendation, financial analysis, resource requirements, risk assessment, compliance needs, and commercial terms. Triggers when the user shares RFP/RFQ/RFI/tender/proposal documents, pastes procurement text, asks for a bid/no-bid decision, asks to "review this RFP", "analyze this proposal", "should we bid on this", "RFP analysis", or says "analyze RFP [ID]".
---

# Author
# Fabio DeMelo
# demelos.com - AI experts
# https://www.demelos.com

# RFP Review — Executive Analysis Framework + Alta Apps API Integration

Analyze any Request for Proposal (RFP), RFQ, RFI, SOW, tender, or procurement document as if advising the CEO/owner who has 15 minutes to decide whether to pursue it. When working with the Alta Apps system, fetch RFP data directly from the API, run the analysis, and post a structured note back to the record — always with user confirmation before writing anything.

---

## Alta Apps API

### Credentials

| Field | Value |
|-------|-------|
| Base URL | `https://apps.altajan.com/admin/api/v1/` |
| Header | `X-API-Key: YOUR_PERSONAL_API_KEY` |
| Content-Type | `application/json` (for POST/PUT) |

> **How to get your API key:**
> 1. Log in to Alta Apps at `https://apps.altajan.com/admin/`
> 2. Click your name in the top-right corner
> 3. Select **Settings & API Key**
> 4. Click **Generate Personal API Key**
> 5. Copy the key immediately — it is only shown once
> 6. Paste it into your Claude skill configuration or environment variable
>
> Each user has their own personal key. All API calls are logged with your name, IP address, and timestamp. Never share your key or commit it to a repository. If compromised, revoke it from the same Settings page and generate a new one.

### Endpoints

#### List RFPs
```
GET https://apps.altajan.com/admin/api/v1/rfps.php
```
Optional query params: `status`, `priority`, `bid_decision`, `region`, `search`, `due_date_from`, `due_date_to`, `limit`, `offset`, `sort_col`, `sort_dir`

Filter `status=open` returns all non-closed RFPs (`new` + `active`).

#### Get Single RFP
```
GET https://apps.altajan.com/admin/api/v1/rfps.php?id={rfp_id}
```

#### Create RFP
```
POST https://apps.altajan.com/admin/api/v1/rfps.php
```
Required: `title`, `client_name`. All other fields optional.

#### Update RFP (any field)
```
PUT https://apps.altajan.com/admin/api/v1/rfps.php?id={rfp_id}
```
Send only the fields to update. Full writable field list below.

#### Post Note to RFP
```
POST https://apps.altajan.com/admin/api/v1/rfp_notes.php
Content-Type: application/json

{
  "rfp_id":    <id>,
  "summary":   "<one-line headline>",
  "note":      "<full analysis text>",
  "author":    "Claude",
  "note_date": "<today YYYY-MM-DD>"
}
```
Success returns HTTP 201 with `{ "success": true, "id": <note_id> }`.

#### List Notes for an RFP
```
GET https://apps.altajan.com/admin/api/v1/rfp_notes.php?rfp_id={rfp_id}
```

#### Attach Document to RFP
```
POST https://apps.altajan.com/admin/api/v1/rfp_documents.php
Content-Type: application/json

{
  "rfp_id": <id>,
  "url":    "<https://... public URL of the document>",
  "name":   "<optional filename override>",
  "notes":  "<optional description>"
}
```
Downloads from URL, uploads to storage, deduplicates automatically. HTTPS only, 50 MB max.

#### List Documents for an RFP
```
GET https://apps.altajan.com/admin/api/v1/rfp_documents.php?rfp_id={rfp_id}
```

### Writable RFP Fields (PUT/POST)

**String fields:**
`title`, `rfp_number`, `client_name`, `client_department`, `client_contact_name`, `client_contact_email`, `client_contact_phone`, `client_location`, `agency_department`, `naics_code`, `region`, `state`, `source_url`, `rfp_link`, `description`, `summary`, `scope_summary`, `scope_of_work`, `services_required`, `services_areas`, `industry_vertical`, `delivery_location`, `contract_duration`, `complexity_level`, `pricing_type`, `payment_terms`, `budget_currency`, `competitors`, `incumbent_contractor`, `win_evaluation_criteria`, `mandatory_requirements`, `past_performance_reqs`, `certifications_required`, `compliance_requirements`, `key_requirements`, `terms_conditions_url`, `proposal_manager`, `technical_lead`, `capture_manager`, `estimator`, `notes`, `internal_notes`, `go_no_go_reason`, `bid_decision`, `bid_decision_reason`, `pursue_decision`, `bid_submitted_by`, `status`, `priority`, `tags`, `category`, `due_timezone`

**Date fields** (`YYYY-MM-DD`):
`due_date`, `pre_proposal_date`, `award_date`, `posted_date`, `open_date`, `pre_bid_meeting_date`, `site_visit_date`, `response_date`, `bid_submission_date`, `start_date`, `end_date`

**Time field** (`HH:MM:SS`):
`due_time`

**Integer fields:**
`source_id`, `contract_duration_months`, `estimated_hours`, `win_probability`, `internal_assigned_to`, `multiple_builds`, `pre_bid_meeting_required`, `site_visit_required`, `response_required`, `bonding_required`, `insurance_required`, `prequalification_required`, `is_favorite`

**Decimal fields:**
`estimated_budget`, `contract_value`, `contract_value_max`, `our_bid_amount`, `bid_amount`, `bid_margin`, `bid_score`, `award_value`, `insurance_amount`, `bid_submission_amount`, `estimated_profit_margin`

### Valid Enum Values

| Field | Values |
|-------|--------|
| `status` | `new`, `active`, `closed` |
| `priority` | `low`, `medium`, `high`, `critical` |
| `bid_decision` | `no_decision`, `New`, `Reviewing`, `Not a Good Fit`, `Not Qualified`, `Bidding`, `Submitted`, `Missed Deadline`, `Won`, `Lost`, `Needs Attention`, `Needs Price`, `Needs Approval (Fabio/Angelica)` |
| `pursue_decision` | `undecided`, `pursue`, `no_pursue` |

### Error Handling

| HTTP | Meaning | Action |
|------|---------|--------|
| 201 | Created | Confirm with ID and link |
| 200 | OK | Proceed |
| 400 | Bad input | Report exact error, ask user to correct |
| 404 | Not found | Ask user to verify ID |
| 403 | Permission denied | Report API key issue |
| 500 | Server error | Report and stop |

---

## API-Integrated Workflow (when working with Alta Apps)

Use this workflow when the user says "analyze RFP [ID]", "review our RFPs", or references the Alta Apps system.

### Step 1 — Fetch RFP Data

If the user provides an RFP ID:
```
GET https://apps.altajan.com/admin/api/v1/rfps.php?id={id}
```

If no ID provided, fetch the open list:
```
GET https://apps.altajan.com/admin/api/v1/rfps.php?status=open&sort_col=due_date&sort_dir=asc&limit=50
```
Present a summary table (ID, title, due date, bid_decision) and ask which RFP to analyze.

Also fetch existing notes to avoid duplicate analysis:
```
GET https://apps.altajan.com/admin/api/v1/rfp_notes.php?rfp_id={id}
```

### Step 2 — Run the Analysis

Execute the full Executive Analysis Framework (Steps 1–7 below) against the RFP data.

If the RFP has an `rfp_link` or `source_url`, attempt to fetch the document for deeper analysis. If it's a PDF URL, it can be attached via the documents API.

### Step 3 — Ask Confirmation Before Posting

After completing the analysis, ask BOTH questions before writing anything:

**Question 1:**
> "I have completed the RFP analysis for RFP #[ID] — [Title]. Are you ready for me to post the summary note to the RFP record? (Yes / No)"

If **No**: stop. Do not post. Offer to adjust the analysis or update specific fields instead.

If **Yes**: proceed to Question 2.

**Question 2:**
> "Please confirm the RFP ID where I should post this note — is it [ID]?"

Wait for explicit confirmation. Never assume.

### Step 4 — Post the Note

```
POST https://apps.altajan.com/admin/api/v1/rfp_notes.php

{
  "rfp_id":    <confirmed id>,
  "summary":   "<recommendation + one-line rationale>",
  "note":      "<full structured analysis>",
  "author":    "Claude",
  "note_date": "<today YYYY-MM-DD>"
}
```

### Step 5 — Optionally Update RFP Fields

After posting the note, offer to update RFP fields based on the analysis findings, for example:

```
PUT https://apps.altajan.com/admin/api/v1/rfps.php?id={id}

{
  "bid_decision": "Reviewing",
  "priority": "high",
  "win_probability": 65,
  "scope_summary": "..."
}
```

Always confirm field updates with the user before executing.

### Step 6 — Confirm to User

After a successful note post, reply:
> "Note posted successfully to RFP #[ID] — [Title].
> Note ID: [id]
> View it at: https://apps.altajan.com/admin/rfp_edit.php?id=[ID]"

### API Rules

- **NEVER post a note without explicit user confirmation.**
- **NEVER assume the RFP ID** — always confirm with the user.
- **NEVER post more than one note per RFP per session** without asking again.
- If the API returns a duplicate note warning, inform the user and stop.
- Author must always be `"Claude"`.
- Always include today's date in `note_date`.

---

## Note Format Template

The `note` body must follow this structure:

```
RECOMMENDATION: [BID / NO BID / CONDITIONAL BID / NEEDS REVIEW]
COMPOSITE SCORE: [X.X / 10]

SCOPE:
[2-3 sentences describing what is being procured]

KEY DATES:
- Due Date: [date] [time] [timezone]
- Pre-Bid Meeting: [date or N/A]
- Site Visit: [date or N/A]
- Response Deadline: [date or N/A]

VALUE:
[Estimated contract value or budget range]
Win Probability: [X%]

REASONING:
[Why this is or isn't a good fit for Alta Jan]

TOP 3 OPPORTUNITIES:
1. [opportunity]
2. [opportunity]
3. [opportunity]

TOP 3 RISKS:
1. [risk]
2. [risk]
3. [risk]

ACTION ITEMS:
- [Item 1 with owner and deadline]
- [Item 2]
- [Item 3]
```

---

## Executive Analysis Framework

### Purpose

Transform dense procurement documents into a crisp executive briefing that answers the only question that matters: **should we bid, and if so, how?**

### When to Use

Trigger whenever the user:
- Shares an RFP, RFQ, RFI, SOW, tender, or procurement document (PDF, Word, text)
- Pastes procurement text and asks for analysis
- Asks "should we bid?", "is this worth it?", "analyze this opportunity"
- Requests a bid/no-bid decision, proposal review, or tender evaluation
- Mentions government contracts, enterprise procurement, or sales pursuits
- Says "analyze RFP [ID]" (use API workflow above)

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

---

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
