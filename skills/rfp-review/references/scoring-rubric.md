# RFP Scoring Rubric

Three scores on a 1-10 scale, combined into a single composite rating. Always show the math.

---

## Score 1: Opportunity (higher = better)

Measures strategic value and revenue upside beyond the immediate deal.

| Score | Criteria |
|-------|----------|
| 9-10  | Perfect strategic fit. Marquee logo or category-defining win. Clear path to multi-year, multi-product expansion. We should want this. |
| 7-8   | Strong fit with existing strategy. Solid revenue with credible expansion potential. |
| 5-6   | Adjacent to our core. Reasonable revenue but limited strategic upside. |
| 3-4   | Off-strategy. One-off revenue with little follow-on. |
| 1-2   | Actively harmful — distracts from priorities, damages brand, or conflicts with existing accounts. |

---

## Score 2: Complexity (LOWER = better)

Measures the difficulty of winning and delivering. In the composite formula, complexity is inverted as `(11 - Complexity)`.

| Score | Criteria |
|-------|----------|
| 1-2   | Trivial. We've done this 50 times. Standard T&Cs, no unusual compliance, clear scope. |
| 3-4   | Routine. Some customization but within our normal delivery envelope. |
| 5-6   | Moderate. New compliance requirement, unfamiliar geography, or harder delivery profile. |
| 7-8   | Hard. Multiple stretch areas — new certification, new technology, aggressive timeline, hostile T&Cs. |
| 9-10  | Exceptional. Would require significant reorganization, new hires, or acquisition to deliver. |

---

## Score 3: Revenue (higher = better)

Measures the financial attractiveness of the deal itself.

| Score | Criteria |
|-------|----------|
| 9-10  | Large TCV, high margin (>40%), favorable payment terms, advance or milestone billing, strong cash flow. |
| 7-8   | Meaningful TCV, healthy margin (25-40%), Net-30/60 terms, predictable cash flow. |
| 5-6   | Average. Market-rate margin (15-25%), Net-60/90 terms, some cash-flow friction. |
| 3-4   | Thin margin (<15%), Net-90+ terms, significant retainage, or heavy upfront investment. |
| 1-2   | Loss-leader or worse. Margin negative at list pricing, or payment terms threaten solvency. |

---

## Composite Formula

```
Composite = (Opportunity × 0.4) + ((11 - Complexity) × 0.3) + (Revenue × 0.3)
```

The weights (40/30/30) reflect that strategic fit slightly outweighs financial and delivery concerns, because a poor strategic fit can't be fixed by good margins.

### Decision Bands

| Composite | Recommendation |
|-----------|---------------|
| 8.0-10.0  | **Strong Bid** — prioritize and resource fully. |
| 6.5-7.9   | **Bid** — proceed with standard pursuit motion. |
| 5.0-6.4   | **Conditional Bid** — pursue only if specific conditions are met (partner, price, terms). |
| 3.5-4.9   | **Lean No-Bid** — decline unless there's a non-financial reason to pursue. |
| 1.0-3.4   | **No-Bid** — politely decline. |

---

## Worked Example

An RFP for a mid-size consulting engagement scores:
- Opportunity: 7 (strong strategic fit, modest expansion potential)
- Complexity: 5 (moderate — new compliance requirement)
- Revenue: 6 (healthy margin, Net-60 terms)

Composite = (7 × 0.4) + ((11 - 5) × 0.3) + (6 × 0.3)
         = 2.8 + 1.8 + 1.8
         = **6.4 → Conditional Bid**

---

## Red-Flag Override

Any red flag from `references/red-flags.md` marked as "automatic no-bid" overrides the numerical score. Document the override in the brief.
