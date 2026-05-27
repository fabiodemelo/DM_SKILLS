---
name: rfp-review
description: Analyze RFPs (Request for Proposal), RFQs, RFIs, SOWs, and tender documents from a C-level executive perspective. Integrates with the Alta Apps API to fetch RFP data, post analysis notes, update fields, attach documents, extract labor specs + cost, estimate profit/revenue against max-budget caps, validate source URLs, and update bid decisions. Produces a structured executive brief covering a 1-10 opportunity rating, bid/no-bid recommendation, financial analysis, resource requirements, risk assessment, compliance needs, and commercial terms. Always verifies the active RFP ID before any read or write. Triggers when the user shares RFP/RFQ/RFI/tender/proposal documents, pastes procurement text, asks for a bid/no-bid decision, asks to "review this RFP", "analyze this proposal", "should we bid on this", "RFP analysis", says "analyze RFP [ID]", or runs any `run [ID]`, `run exec`, `run docs`, `run labor cost`, `run profit`, `run revenue`, `run update [field] to [value]`, `run web update`, `run link`, `run decision [value]`, or `run help` command.
---

# Author
# Fabio DeMelo
# demelos.com - AI experts
# https://www.demelos.com

# RFP Review — Executive Analysis Framework + Alta Apps API Integration

Analyze any Request for Proposal (RFP), RFQ, RFI, SOW, tender, or procurement document as if advising the CEO/owner who has 15 minutes to decide whether to pursue it. When working with the Alta Apps system, fetch RFP data directly from the API, run the analysis, and post a structured note back to the record — always with user confirmation before writing anything.

---

## Installation & First-Run Setup

When this skill is first invoked — or when `ALTA_API_KEY` is not yet set in the current session — Claude MUST run the setup sequence below before doing anything else. Do not skip it, even if the user immediately asks to analyze an RFP.

### Setup Sequence

**Step 1 — Detect missing key**

Check whether `ALTA_API_KEY` has been set in this session. If not, display:

---

> **Alta Apps RFP Skill — First-Time Setup**
>
> This skill connects to your Alta Apps portal to fetch RFPs, post analysis notes, and update records.
>
> **You need a personal API key to continue.**
>
> **How to get your key:**
> 1. Log in to Alta Apps → `https://apps.altajan.com/admin/`
> 2. Click your name in the top-right corner
> 3. Select **Settings & API Key**
> 4. Click **Generate Personal API Key**
> 5. Copy the key — it is only shown once
>
> Please paste your API key here to continue:

---

**Step 2 — Receive and validate the key**

When the user pastes a key:
- Store it as `ALTA_API_KEY` for this session
- Make a test call: `GET https://apps.altajan.com/admin/api/v1/rfps.php?limit=1`  
  with header `X-API-Key: <pasted key>`
- If HTTP 200 → confirm: `"API key accepted. Connected to Alta Apps as [key name]. Ready to analyze RFPs."`
- If HTTP 401 → reply: `"That key was rejected (invalid or inactive). Please verify the key in Settings & API Key and try again."`
- If HTTP 403 → reply: `"Key authenticated but missing RFP read permissions. Ask your administrator to update your key's permissions."`
- Do NOT proceed until a valid key is confirmed.

**Step 3 — Resume the user's original request**

Once the key is confirmed, immediately continue with whatever the user originally asked (e.g., "analyze RFP 373").

### Key Storage Rules

- Store the key in memory for this session only as `ALTA_API_KEY`
- Never write the key to a file, commit, or log
- Never display the full key back to the user after setup
- If the session ends and restarts, run setup again
- If the user types `reset api key` or `change api key`, clear `ALTA_API_KEY` and re-run setup

---

## RFP ID Verification Protocol

**Every command and every action that touches an RFP MUST verify the RFP ID first. No exceptions.**

### Rules

1. **No RFP ID in current context** → ask the user:
   > "Give me the RFP ID."

   Wait for the reply. Never guess. Never auto-pick the most recent RFP.

2. **An RFP ID is already active in this session** → confirm before any new action:
   > "Are we still working on RFP #[ID] — [Title]? (Yes / No / different ID)"

   - **Yes** → proceed
   - **No** → ask for the new ID
   - **Different ID supplied** → fetch it, echo the title, re-confirm

3. **User supplies an ID with a command** (e.g., `run 373`, `run exec 373`) → fetch first, echo title, then confirm:
   > "Confirming RFP #373 — [Title]. Proceed? (Yes / No)"

4. **Cache the active RFP ID** in session memory as `ACTIVE_RFP_ID` along with `ACTIVE_RFP_TITLE`. Clear on `reset rfp`, on session end, or when user names a different RFP.

5. **Every write action** (`run update`, `run decision`, `run web update`, posting a note, attaching a document) requires a fresh confirmation of the ID even if cached:
   > "Writing to RFP #[ID] — [Title]. Confirm? (Yes / No)"

6. If the user issues a command with no ID and no active session ID, refuse to execute and ask for the ID.

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

#### Delete a Note
```
DELETE https://apps.altajan.com/admin/api/v1/rfp_notes.php?id={note_id}
```
Returns HTTP 200 with `{ "success": true, "id": <note_id>, "message": "Note deleted" }`. Use when reposting a corrected note (the API does not support PATCH/PUT on notes).

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

The `note` body must follow this structure. **Use single-line spacing only — NO blank lines between sections.** The Alta UI renders the note compactly; extra blank lines double/triple the visual gap and degrade readability.

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

**Formatting rules:**
- Section headers (e.g., `SCOPE:`, `KEY DATES:`) are on their own line with no blank line above or below.
- Multi-paragraph content within a section may use `\n` between paragraphs but NOT `\n\n`.
- When constructing the JSON payload, use single `\n` separators — never `\n\n`.
- Lists (numbered or bulleted) sit directly under the section header with no blank line.

---

## Command Reference

All commands require a verified RFP ID per the **RFP ID Verification Protocol** above. If the ID is missing, ask first. If an ID is cached from earlier in the session, re-confirm: *"Are we still working on RFP #[ID]?"*

`run help` or `commands` → display this list to the user.

---

### `run [ID]` — Full Review

Run the complete Executive Analysis Framework (Steps 1–7 below).

Workflow:
1. Verify ID.
2. `GET /rfps.php?id={ID}` + `GET /rfp_notes.php?rfp_id={ID}`.
3. Execute all 7 analysis steps. Produce executive brief + 35-question scorecard + labor cost + profit/revenue + max-budget comparison.
4. Double confirmation before write (per § API-Integrated Workflow Step 3).
5. `POST /rfp_notes.php` with structured note.
6. Offer follow-up field updates.

---

### `run exec [ID]` — Executive Quick Review

5–10 bullet executive summary. Skip the full 35-question scorecard.

Output:
- **Recommendation:** BID / NO-BID / CONDITIONAL + one-line rationale
- **Composite score:** X.X / 10
- **TCV / ACV / max budget:** $X / $Y / $Z
- **Key dates:** due date + pre-bid + award
- **Top 3 opportunities** (one line each)
- **Top 3 risks** (one line each)
- **Estimated labor cost vs. budget** (if available)
- **Win probability:** X%

No note posted unless user says "post it" — then run the standard double-confirmation flow.

---

### `run docs [ID]` — Document Inventory & Attach

1. `GET /rfp_documents.php?rfp_id={ID}` → list all attached documents (name, URL, size).
2. Pull `source_url` and `rfp_link` from RFP record.
3. Compare: report which linked docs are NOT yet attached.
4. If user says **"download"** or **"attach"** → for each unattached URL run `POST /rfp_documents.php` (HTTPS only, 50 MB max, confirm each).
5. If user says **"show"** → return the list with clickable URLs and short descriptions.

---

### `run labor cost [ID]` — Labor Specs + Cost Extraction

1. Extract from RFP scope:
   - **Trade classifications** (e.g., journeyman electrician, project manager, foreman, laborer)
   - **Prevailing wage / Davis-Bacon / union** requirements
   - **Per-trade certifications** (OSHA 30, state licenses, security clearances)
   - **Crew composition + headcount per phase**
   - **Estimated hours or days per role**
2. Build labor cost table:

   | Role / Trade | Hours | Burdened Rate ($/hr) | Subtotal |
   |--------------|-------|----------------------|----------|

3. Compute **Total Labor Cost** = Σ subtotals. Show math.
4. Flag any role missing rate data → ask user for input or use industry default (mark as ASSUMPTION).
5. Offer write-back:
   ```
   PUT /rfps.php?id={ID}
   { "estimated_hours": <total>, "estimated_budget": <total_labor_cost> }
   ```
6. Confirm before write.

---

### `run profit [ID]` / `run revenue [ID]` — Profit & Revenue Estimate

Includes mandatory max-budget comparison when the RFP specifies a cap.

1. Pull from RFP: `estimated_budget`, `contract_value`, `contract_value_max` (cap).
2. Pull our total cost: labor cost (from `run labor cost` if cached) + materials + overhead + contingency. Ask user for any missing pieces.
3. **If the RFP has a max / cap budget**, produce this table:

   | Metric | Value |
   |--------|-------|
   | Max budget (cap) | $X |
   | Our total cost | $Y |
   | Potential revenue ($) | $X − $Y = $Z |
   | Potential revenue (%) | (Z / X) × 100 = N% |
   | Margin % at cap | (Z / X) × 100 = N% |
   | Status | ✅ Under cap / ⚠️ Tight (< 10% margin) / ❌ Over cap |

4. **If our cost > cap** → flag immediate NO-BID or re-scope. Do not proceed with bid recommendation until resolved.
5. **If no cap disclosed** → estimate revenue from scope + comparable projects, label as ASSUMPTION.
6. Offer write-back:
   ```
   PUT /rfps.php?id={ID}
   {
     "our_bid_amount":          <our price>,
     "estimated_profit_margin": <margin %>,
     "bid_margin":              <margin %>
   }
   ```
7. Confirm before write.

---

### `run update [field] to [value]` — Single-Field Write

Example: `run update bid amount to $5000` → `our_bid_amount = 5000`.

1. Parse the natural-language field name. Map to a writable field from the **Writable RFP Fields** table.
2. Validate the value against the declared type (string / date / time / int / decimal) and against enum values where applicable.
3. Echo back exactly what will be written:
   > "Writing `our_bid_amount = 5000` to RFP #[ID]. Confirm? (Yes / No)"
4. On Yes → `PUT /rfps.php?id={ID}` with the single field.
5. On enum mismatch → list valid options and ask user to pick.
6. On unknown field → ask user to clarify; show closest matches from the schema.

---

### `run web update [ID]` — Re-Scan Source URLs for New Data

1. Pull `source_url` and `rfp_link` from the RFP record.
2. Fetch both. Re-extract: due_date, due_time, addenda count, amendments, posted Q&A responses, pre-bid changes, contract value updates.
3. Compare to stored values. Produce diff report:

   | Field | Stored | Web | Action |
   |-------|--------|-----|--------|

4. For each diff, ask user: **"Update? (Yes / No / skip)"** — one field at a time.
5. Apply approved updates via `PUT /rfps.php?id={ID}`.
6. If new addenda PDFs are detected → offer to attach them via the `run docs` flow.
7. If both URLs are missing → ask user to provide a source URL first.

---

### `run link [ID]` — URL Health Check

1. Pull every URL field on the RFP: `source_url`, `rfp_link`, `terms_conditions_url`, and URLs from `rfp_documents.php`.
2. HTTP HEAD each URL (follow redirects, max 5 hops).
3. Report:

   | Field | URL | Status | Notes |
   |-------|-----|--------|-------|

   - `200` → ✅ valid
   - `3xx` → ⚠️ redirect; note final URL
   - `4xx` / `5xx` → ❌ broken
   - timeout / DNS fail → ❌ unreachable
4. For broken URLs → offer to clear the field or replace with a new URL provided by the user.

---

### `run decision [value]` — Update `bid_decision`

Example: `run decision not a good fit` → set `bid_decision = "Not a Good Fit"`.

1. Fuzzy-match the user's value against the `bid_decision` enum:
   `no_decision`, `New`, `Reviewing`, `Not a Good Fit`, `Not Qualified`, `Bidding`, `Submitted`, `Missed Deadline`, `Won`, `Lost`, `Needs Attention`, `Needs Price`, `Needs Approval (Fabio/Angelica)`
2. On match → confirm with the user:
   > "Setting RFP #[ID] `bid_decision = 'Not a Good Fit'`. Reason? (optional — will write to `bid_decision_reason`)"
3. `PUT /rfps.php?id={ID}` with `bid_decision` (+ `bid_decision_reason` if supplied).
4. On no match → list all valid enum values and ask user to pick.
5. Echo result + edit URL.

---

### Command Dispatch Rules

- Commands are case-insensitive. `Run Exec 373` = `run exec 373`.
- If the user types just `run` with no ID and no active session ID → ask: *"Which command, and for which RFP?"*
- If the user combines commands (e.g., `run labor cost then run profit`) → execute sequentially, re-confirming the ID once for the chain.
- Any command that performs a write action must double-confirm (action + ID) before calling the API.
- Never batch writes silently — each `PUT` / `POST` requires its own confirmation.

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

### Step 2: Extract the 35 Questions

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

Populate `assets/scorecard-template.md` with the full 35-question breakdown. This is the appendix the CEO reads if they want to drill in.

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

- `references/question-framework.md` — The 35-question C-level framework organized into 9 categories
- `references/scoring-rubric.md` — 1-10 scoring methodology for Opportunity / Complexity / Revenue with composite formula
- `references/red-flags.md` — Deal-breaker checklist and risk signals
- `assets/executive-brief-template.md` — One-page executive summary format
- `assets/scorecard-template.md` — Full 35-question structured scorecard

## Style

Write findings in flowing prose where possible, not fragmented bullet soup. C-level readers want reasoning, not just data points. Use tables for the scorecard, financials, and timeline — prose for the recommendation and rationale.
