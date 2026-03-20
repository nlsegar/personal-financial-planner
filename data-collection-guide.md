# Data Collection Guide

## Financial Profile Intake Questionnaire

Use this to gather the user's complete financial picture before building the dashboard.
Ask conversationally — don't dump the whole list at once. Prioritize what's needed for the
sections the user cares about most.

---

## Tier 1: Must-Have (can't build without these)

### People & Household
- Full names and ages of all household members
- State of residence (affects tax calculations)
- Filing status (MFJ, Single, HoH)
- Number and ages of children

### Income
- Primary W2 salary (gross) for each working adult
- Estimated net take-home per month (after taxes, 401k, HSA withholding)
- Any side income / 1099 income — and whether it's recurring or temporary
- Employer match details (% match, cap)

### Accounts (for each account, get):
- Account type (401k, Roth IRA, Traditional IRA, Taxable Brokerage, 529, UTMA, HSA, etc.)
- Platform (Vanguard, Fidelity, Schwab, etc.)
- Owner (whose name is it in)
- Current balance
- Key fund holdings (ticker symbols if known)
- Status (active, on hold, needs consolidation)

### Monthly Spending
- Total core living expenses per month
- OR a category-by-category breakdown if they have it

### Debt
- Mortgage balance + monthly payment + rate + remaining term
- Student loans, auto loans, other debt
- Credit card balances (even if paid monthly)

---

## Tier 2: Important for Full Dashboard

### Retirement
- Target retirement age for each spouse
- Target annual spending in retirement (today's dollars)
- Preferred withdrawal rate (default: 3.5-4%)
- Social Security estimates (from ssa.gov or estimate)
- Any pension income expected

### 401k / Retirement Contributions
- What % of salary goes to 401k? (Don't assume max — ask)
- Pre-tax, Roth, or split?
- Employer match formula
- Is HSA being maxed?
- Backdoor Roth IRA in use?

### Education Planning
- 529 balance per child
- Annual contribution per child
- College cost assumption: in-state, private, or blend?
- Any non-529 accounts earmarked for kids?

### Real Estate
- Primary residence estimated value (Zillow/Zestimate is fine)
- Mortgage details (or confirm paid off)
- Investment properties if any

---

## Tier 3: Nice-to-Have

### Tax
- Marginal federal tax bracket
- State income tax rate
- Tax preparer / CPA name (for action items)
- Any tax-loss harvesting opportunities

### Insurance
- Health insurance situation (employer, marketplace, COBRA)
- Life insurance coverage and type
- Disability insurance (especially important if self-employed or health issues)
- Any special medical cost considerations

### Estate
- Will / trust in place?
- Beneficiary designations up to date?
- Power of attorney / healthcare proxy?

### Behavioral Preferences
- Risk tolerance (conservative, moderate, aggressive)
- Target asset allocation (US/international/bonds split)
- Any holdings they won't sell (concentrated stock, sentimental, etc.)
- Financial advisor relationship (DIY, robo, human advisor)

---

## Extracting Data from Screenshots

If the user uploads screenshots from Copilot Money, Mint, Empower, Personal Capital, or similar:

1. **Read every account name, type, and balance** — don't skip small accounts
2. **Note the platform** for each account (shown in the app UI)
3. **Watch for**: accounts on different screens (user may need to scroll), "hidden" or "closed" 
   accounts sections, verification-required flags
4. **Categorize**: depository (checking/savings), credit cards, investments, real estate, loans
5. **Sum and cross-check** against the app's reported totals
6. **Ask about anything ambiguous**: "I see an account called 'UTMA 2205' — which child is this for?"

## Common Mistakes to Avoid

- Don't add estimated tax payments as a monthly outflow for W2-only income (taxes are already withheld)
- Don't assume 401k is maxed — ask what percentage they contribute
- Don't include temporary/bonus income in base cash flow projections
- Don't count 529 contributions as retirement savings
- Don't show `current_balance / target` as "retirement pacing" — it ignores compounding
- Don't assume inheritance or gifts in projections unless explicitly told to
