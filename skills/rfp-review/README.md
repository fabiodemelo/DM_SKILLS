# RFP Review

Analyze RFPs (Request for Proposal), RFQs, RFIs, SOWs, and tender documents from a C-level executive perspective. Produces a structured executive brief with a 1-10 opportunity rating, bid/no-bid recommendation, a full 35-question scorecard, labor cost extraction, profit/revenue estimate against max-budget caps, and direct integration with the Alta Apps API.

## What It Does

Transforms dense procurement documents into a crisp executive briefing that answers one question: **should we bid, and if so, how?**

For every RFP or tender you drop in, the skill produces:

- **Composite rating (1-10)** combining Opportunity, Complexity, and Revenue scores
- **Bid / No-Bid / Conditional-Bid recommendation** with one-sentence rationale
- **Executive brief** — one-page summary covering top 3 upsides, top 3 risks, key deadlines, financials, and competition
- **Full 35-question scorecard** spanning Strategic Fit, Financial, Resources, Risk & Compliance, Commercial Terms, Competition, Timeline, Labor/Schedule/Cost-Cap, and Go/No-Go
- **Labor cost extraction** — trades, prevailing wage flags, certifications, hours, burdened-rate × hours table
- **Profit & revenue estimate vs. max-budget cap** — potential revenue in $ + %, margin %, ✅/⚠️/❌ status, auto-NO-BID if cost exceeds cap
- **Execution schedule** — project phases inside the contract term, separate from procurement milestones
- **Red flag assessment** — deal-breakers flagged with Tier 1/2/3 severity
- **Q&A question list** — strategic questions to submit to the issuing authority
- **Next actions checklist** — internal meetings, legal reviews, partner engagement, decision deadlines

## When to Use

Trigger whenever you:

- Share an RFP, RFQ, RFI, SOW, or tender document (PDF, Word, text)
- Paste procurement text and ask for analysis
- Ask "should we bid?", "is this worth it?", "analyze this opportunity"
- Request a bid/no-bid decision, proposal review, or tender evaluation
- Work on government contracts, enterprise procurement, or sales pursuits

Example prompts:

```
Review this RFP and tell me if we should bid on it.
[attach RFP PDF]
```

```
Analyze this tender and produce an executive brief.
```

```
Should we go after this proposal? Here's the SOW.
```

## How It Works

The skill follows a 7-step workflow:

1. **Ingest & inventory** — identifies every document, flags missing pieces
2. **Extract 35 questions** — answers each with RFP citations
3. **Score the opportunity** — Opportunity × 0.4 + (11 − Complexity) × 0.3 + Revenue × 0.3
4. **Flag red flags** — checks against a curated deal-breaker list (incl. cost-over-cap auto NO-BID)
5. **Executive brief** — one-page summary for the CEO incl. labor cost + cost-vs-cap
6. **Scorecard** — full 35-question appendix
7. **Next actions** — concrete checklist with deadlines

## Commands

All commands verify the active RFP ID before any action. If no ID is set, the skill asks: *"Give me the RFP ID."* If an ID is cached, it confirms: *"Are we still working on RFP #X?"* Every write requires double confirmation.

| Command | What it does |
|---------|--------------|
| `run [ID]` | Full 7-step review + 35-question scorecard + post note to Alta |
| `run exec [ID]` | 5-10 bullet executive summary, no note posted unless asked |
| `run docs [ID]` | List attached docs, compare vs source URLs, attach missing |
| `run labor cost [ID]` | Extract trades, prevailing wage, hours, burdened rates → cost table |
| `run profit [ID]` / `run revenue [ID]` | Cost vs max-budget cap → potential revenue $ + %, margin %, status flag |
| `run update [field] to [value]` | Single-field write to Alta (e.g., `run update bid amount to $5000`) |
| `run web update [ID]` | Re-scan `source_url` + `rfp_link`, diff vs stored, per-field confirm |
| `run link [ID]` | HTTP HEAD every URL field, report broken / redirects |
| `run decision [value]` | Update `bid_decision` (e.g., `run decision not a good fit`) |
| `run help` | Show this command list |

## The 35-Question Framework

Organized into 9 categories:

| Category | Questions |
|----------|-----------|
| Strategic Fit | Scope, alignment, marquee value, follow-on revenue |
| Financial | TCV/ACV, payment terms, margin, bid cost, cash flow |
| Resources & Capabilities | Staffing, investment, past performance, capability gaps |
| Risk & Compliance | Certifications, liability, insurance, penalties, data security |
| Commercial Terms | Pricing model, IP, subcontracting, termination |
| Competitive | Incumbent, competitors, Pwin |
| Timeline & Process | Key dates, meetings, evaluation process |
| Labor, Schedule & Cost Cap | Labor specs, execution schedule, total hours, labor cost, cost vs max budget |
| Go/No-Go | Recommendation, Q&A strategy |

## Installation

### Claude Code

```bash
cp -r skills/rfp-review ~/.claude/skills/rfp-review
```

### Cowork

```bash
cp -r skills/rfp-review ~/.cowork/skills/rfp-review
```

### Claude.ai

Upload the skill folder via the skills UI, or reference the SKILL.md content directly.

### First Run

On first use, the skill will automatically ask you for your **Alta Apps personal API key**. You do not need to configure anything beforehand — just install the skill and invoke it. Claude will guide you through getting the key:

1. Log in to Alta Apps → `https://apps.altajan.com/admin/`
2. Click your name → **Settings & API Key**
3. Click **Generate Personal API Key**
4. Copy and paste it when prompted

The key is validated immediately with a live API call. It is stored for the session only and never written to disk.

## Structure

```
rfp-review/
├── SKILL.md                              # Core skill definition + workflow
├── README.md                             # This file
├── references/
│   ├── question-framework.md             # The full 35 questions, 9 categories
│   ├── scoring-rubric.md                 # 1-10 scoring methodology + composite formula
│   └── red-flags.md                      # Tier 1/2/3 deal-breaker checklist
└── assets/
    ├── executive-brief-template.md       # One-page executive summary format
    └── scorecard-template.md             # Full 35-question structured scorecard
```

## Example Output

After analyzing an RFP, you get:

**The Rating**

| Dimension | Score | Notes |
|-----------|-------|-------|
| Opportunity | 7/10 | Strong strategic fit, solid expansion potential |
| Complexity | 5/10 | Moderate — new compliance requirement |
| Revenue | 6/10 | Healthy margin, Net-60 terms |
| **Composite** | **6.4/10** | **Conditional Bid** |

**The Recommendation: CONDITIONAL BID**
Pursue only if we can secure SOC 2 Type II certification before the award date and negotiate the liability cap from unlimited to 1x fees.

...followed by the full executive brief and 35-question scorecard.

## Author

Fabio DeMelo
[demelos.com](https://www.demelos.com) — AI experts

---

Part of the [DM_SKILLS](https://github.com/fabiodemelo/DM_SKILLS) library.
