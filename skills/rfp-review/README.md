# RFP Review

Analyze RFPs (Request for Proposal), RFQs, RFIs, SOWs, and tender documents from a C-level executive perspective. Produces a structured executive brief with a 1-10 opportunity rating, bid/no-bid recommendation, and a full 30-question scorecard.

## What It Does

Transforms dense procurement documents into a crisp executive briefing that answers one question: **should we bid, and if so, how?**

For every RFP or tender you drop in, the skill produces:

- **Composite rating (1-10)** combining Opportunity, Complexity, and Revenue scores
- **Bid / No-Bid / Conditional-Bid recommendation** with one-sentence rationale
- **Executive brief** — one-page summary covering top 3 upsides, top 3 risks, key deadlines, financials, and competition
- **Full 30-question scorecard** spanning Strategic Fit, Financial, Resources, Risk & Compliance, Commercial Terms, Competition, Timeline, and Go/No-Go
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
2. **Extract 30 questions** — answers each with RFP citations
3. **Score the opportunity** — Opportunity × 0.4 + (11 − Complexity) × 0.3 + Revenue × 0.3
4. **Flag red flags** — checks against a curated deal-breaker list
5. **Executive brief** — one-page summary for the CEO
6. **Scorecard** — full 30-question appendix
7. **Next actions** — concrete checklist with deadlines

## The 30-Question Framework

Organized into 8 categories:

| Category | Questions |
|----------|-----------|
| Strategic Fit | Scope, alignment, marquee value, follow-on revenue |
| Financial | TCV/ACV, payment terms, margin, bid cost, cash flow |
| Resources & Capabilities | Staffing, investment, past performance, capability gaps |
| Risk & Compliance | Certifications, liability, insurance, penalties, data security |
| Commercial Terms | Pricing model, IP, subcontracting, termination |
| Competitive | Incumbent, competitors, Pwin |
| Timeline & Process | Key dates, meetings, evaluation process |
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

## Structure

```
rfp-review/
├── SKILL.md                              # Core skill definition + workflow
├── README.md                             # This file
├── references/
│   ├── question-framework.md             # The full 30 questions, 8 categories
│   ├── scoring-rubric.md                 # 1-10 scoring methodology + composite formula
│   └── red-flags.md                      # Tier 1/2/3 deal-breaker checklist
└── assets/
    ├── executive-brief-template.md       # One-page executive summary format
    └── scorecard-template.md             # Full 30-question structured scorecard
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

...followed by the full executive brief and 30-question scorecard.

## Author

Fabio DeMelo
[demelos.com](https://www.demelos.com) — AI experts

---

Part of the [DM_SKILLS](https://github.com/fabiodemelo/DM_SKILLS) library.
