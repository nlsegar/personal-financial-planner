# Expert Review Criteria

After building the dashboard, run it through 3 expert reviewers. Each rates 0-100 with
specific improvement recommendations. If the average is below 90, implement fixes and re-review.

---

## Expert 1: UX Designer (Financial Dashboard Specialist)

### Evaluate:
- **Visual hierarchy**: Can you instantly see the most important numbers?
- **Typography**: Font pairing quality, readability at all sizes
- **Color usage**: Consistent accent colors, sufficient contrast, colorblind-safe
- **Navigation**: Tab switching is instant, charts init on first visit
- **Interaction patterns**: Toggle buttons, editable inputs, refresh flows
- **Mobile responsiveness**: Grid collapses properly, nothing breaks at 768px
- **Status indicators**: Tags, badges, and labels are clear and consistent
- **Sample data labeling**: Any estimated/simulated data is clearly marked

### Red Flags (auto-deduct):
- Hardcoded SVG values that don't update dynamically (-5)
- Inline onclick handlers instead of event delegation (-3)
- No "last updated" timestamp (-3)
- Charts pop in without loading states (-2)
- Estimated data not labeled as estimated (-5)

---

## Expert 2: CFP / CFA (Personal Wealth Planning)

### Evaluate:
- **Data accuracy**: Do the numbers match what the user provided?
- **Cash flow math**: Are payroll deductions separated from take-home correctly?
- **Tax treatment**: Are accounts categorized correctly (Traditional vs Roth vs Taxable)?
- **Contribution math**: 401k % of salary calculated correctly? 529s not counted as retirement?
- **Retirement pacing**: Time-weighted projection, not raw current/target percentage?
- **Bridge gap**: Is the Social Security gap period modeled if retiring before 62/67?
- **Action items**: Are they specific, prioritized, and actionable?
- **Missing elements**: Emergency fund sizing, Roth conversion opportunities, estate planning

### Red Flags (auto-deduct):
- W2 taxes shown as separate monthly outflow (-10)
- Assuming 401k is maxed without asking (-5)
- Temporary income in base projections (-10)
- 529 contributions counted in retirement MC (-5)
- Raw portfolio/target shown as "pacing %" (-8)
- No mention of the retirement-to-SS bridge gap (-5)

---

## Expert 3: Quantitative Finance (Monte Carlo & Modeling)

### Evaluate:
- **Return models**: Log-normal (GBM) as default, with normal and fat-tails options
- **Decumulation**: Uses slightly more conservative returns than accumulation
- **Sequence risk**: Worst-case first-5-year metric included
- **College glide path**: Portfolio de-risks as college approaches
- **Histogram**: Auto-scaled bucket size based on distribution, not hardcoded
- **Sensitivity**: Contribution ±$5K/±$10K impact on target hit rate
- **Legacy estimate**: Median portfolio at end of retirement simulation
- **Fan chart**: Proper percentile bands (10/25/50/75/90)

### Red Flags (auto-deduct):
- Only normal returns, no log-normal option (-8)
- Same return assumptions for accumulation and decumulation (-5)
- Hardcoded histogram bucket size (-3)
- No sequence-of-returns risk metric (-5)
- College MC with no glide path option (-3)
- No sensitivity analysis (-5)

---

## Scoring Guide

| Range | Meaning |
|-------|---------|
| 95-100 | Production-ready, could ship to a client |
| 90-94 | Excellent, minor polish items only |
| 85-89 | Good, but 2-3 meaningful improvements needed |
| 80-84 | Solid foundation, several gaps to address |
| 70-79 | Functional but missing important elements |
| <70 | Major issues — needs significant rework |

**Target: Average ≥ 90 across all 3 experts before delivering to user.**
