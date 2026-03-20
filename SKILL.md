---
name: financial-dashboard-builder
description: >
  Builds a comprehensive, personalized single-file HTML financial dashboard from a user's real account data.
  Covers net worth, portfolio breakdown, budget tracking, retirement projections, college/529 planning,
  Monte Carlo simulations, and prioritized action items. Use this skill whenever someone asks to: build a
  financial dashboard, create a portfolio tracker, visualize their finances, plan for retirement or college
  with Monte Carlo simulations, organize their investment accounts, or track net worth. Also trigger when
  someone uploads screenshots from Copilot Money, Mint, Empower, or any financial aggregator and wants
  a dashboard built from them. Trigger for phrases like "build me a financial dashboard", "track my
  portfolio", "retirement planning tool", "529 planning", "Monte Carlo simulation", "net worth tracker",
  or "organize my finances". This skill produces a single HTML file with no server dependencies that can
  be opened in any browser, optionally served via localhost, or pushed to a private GitHub repo.
---

# Financial Dashboard Builder

## What This Skill Does

Builds a production-grade, single-file HTML financial dashboard personalized to the user's actual accounts,
balances, goals, and family situation. The output is a self-contained `.html` file that runs entirely in
the browser — no backend, no API keys, no data leaves the user's machine.

## When to Use This Skill

- User wants a financial dashboard, portfolio tracker, or net worth visualizer
- User uploads financial screenshots (Copilot Money, Mint, Empower, etc.)
- User asks about retirement planning, college funding, or Monte Carlo simulations
- User wants to organize and visualize their investment accounts
- User says "build me a financial tool" or "help me track my money"

---

## Step 1: Gather the User's Financial Profile

Before building anything, you need real data. Collect as much as possible through conversation.
If the user uploads screenshots from a financial app, extract all account data from the images.

### Required Information

Read `references/data-collection-guide.md` for the full intake questionnaire. At minimum you need:

1. **People**: Names, ages, filing status, state of residence
2. **Income**: Gross salary, net take-home, any side income (and whether it's recurring)
3. **Accounts**: Every investment, retirement, savings, and checking account with current balances
4. **Retirement goals**: Target age, target spending, withdrawal rate preference
5. **Children** (if applicable): Names, ages, 529 balances, other earmarked accounts
6. **Monthly spending**: Total core living expenses (or budget categories if available)
7. **Debt**: Mortgage, student loans, credit cards, auto loans

### What to Clarify

Ask the user directly about anything ambiguous. Common questions:
- Are any taxable accounts earmarked for kids vs. general investing?
- What % of salary goes to 401k? Is employer match included?
- Are IRA contributions backdoor Roth? Any pro-rata concerns?
- Is side income recurring or temporary? (Never include temporary income in base projections)
- What college cost assumption — in-state, private, or blend of both?

**Critical rule**: Taxes withheld from W2 paychecks are NOT a separate cash flow line item.
Only model estimated taxes as a separate outflow if the user has 1099/self-employment income.

---

## Step 2: Build the Dashboard

Read `references/dashboard-architecture.md` for the full technical spec. The dashboard has 8 tabs:

### Tab 1: Overview
- Net worth stat cards (investable, cash, real estate, debt)
- Net worth composition pie chart with investable-only toggle
- Investment accounts horizontal bar chart
- Monthly cash flow table (show payroll deductions separately from take-home)
- Key metrics progress bars (retirement pacing, emergency fund, 529 funding, tax-advantaged utilization)
- Cash & banking accounts table with credit card balances

### Tab 2: Portfolio
- Full account table with owner, type, platform, balance, status tags
- Tax treatment distribution (taxable / traditional / Roth / education)
- Annual contribution priority waterfall
- Fund holdings table with live price refresh button
- Accounts-to-consolidate summary (if applicable)

### Tab 3: Budget & Cash Flow
- Editable budget categories with live-updating doughnut chart
- "Sample data" banners if using estimates instead of real transaction data
- 12-month spending trend (simulated or from real data if provided)

### Tab 4: Retirement Pacing
- Dynamic SVG gauges showing time-weighted pacing (NOT raw % of dollar target)
- Retirement income plan table
- Projected growth with 3 scenarios (conservative/expected/optimistic)
- Scenario toggle for spouse return-to-work or income changes
- Roth conversion ladder / bridge gap strategy section (if applicable)

### Tab 5: 529 & College Planning
- Per-child cards showing 529 + other earmarked accounts
- Projected growth to college enrollment
- College cost coverage comparison (savings vs in-state vs private)
- SECURE 2.0 rollover eligibility dates

### Tab 6: Kids' Portfolios
- Individual pie charts per child
- Growth projection lines

### Tab 7: Monte Carlo Engine
- **Retirement mode**: Starting portfolio, annual contribution, return/volatility, inflation,
  years to retire, target, spending, Social Security, return model selection
- **College mode**: Per-child, 529 balance, contributions, other investments, cost assumptions,
  glide path toggle
- Return models: Log-Normal (GBM, default), Normal, Fat-Tails (Student's t, ν=5)
- Results: Target hit rate, 40-year survival rate, worst 5% first-5-year sequence,
  percentile distribution, median estate at age 90+, contribution sensitivity table,
  college cost gap amounts
- Fan chart (percentile bands) + auto-scaled histogram

### Tab 8: Action Items
- Prioritized by urgency (High/Medium/Low)
- Annual tax & contribution calendar

### Technical Requirements

- **Single HTML file** — all CSS, JS, and data inline. Only external dependency: Chart.js via CDN
- **Fonts**: Use distinctive Google Fonts (not Inter/Roboto/Arial). Pair a display font with a mono font.
- **Dark theme**: Deep navy/slate backgrounds, not pure black
- **All financial data in a centralized JS config object** at the top of the script section
  so the user can update balances easily
- **Live quote refresh**: Include fund symbols and a button that fetches prices from Yahoo Finance.
  Handle CORS gracefully — show "snapshot" fallback if blocked.
- **Responsive**: Works on desktop and tablet. Budget rows collapse on mobile.
- **Chart.js 4.x** for all charts

### Contribution Math — Get This Right

This is where most financial dashboards get the math wrong. Be precise:

1. **401k/HSA contributions** are payroll-deducted BEFORE take-home pay. They are new investment
   money, but they don't come from the user's checking account. Show them separately.
2. **IRA contributions** come from take-home cash. These ARE a cash flow outflow.
3. **529 contributions** come from take-home cash. These fund KIDS' accounts, not retirement.
4. **For retirement Monte Carlo**: Only include contributions that go to retirement accounts
   (401k + IRA + HSA + taxable surplus). Do NOT include 529 contributions.
5. **For retirement projections**: If the user says "10% of salary to 401k", calculate
   the actual dollar amount. Don't assume they max out.

### Retirement Pacing — Get This Right

Never show raw `current_portfolio / target` as a percentage. This is misleading because it
ignores decades of compounding ahead. Instead:

- Project the current portfolio forward at expected return + contributions
- Show the projected terminal value vs the target
- Or: compute the required starting balance to hit the target and show actual/required

---

## Step 3: Expert Review

After building the dashboard, run it through 3 expert reviewers:

1. **UX Designer** — Layout, responsiveness, visual hierarchy, interaction patterns
2. **CFP/CFA** — Financial accuracy, missing planning elements, actionable insights
3. **Quantitative Finance** — Monte Carlo methodology, return modeling, statistical validity

Each expert rates 0-100 with specific improvement recommendations. If the average is below 90,
implement the fixes and re-review. See `references/expert-review-criteria.md` for detailed rubrics.

---

## Step 4: Deliver & Deploy

### Option A: Download and open locally
The HTML file works by double-clicking it. For live quote refresh, run:
```bash
python3 -m http.server 8080
# Then open http://localhost:8080/dashboard.html
```

### Option B: Push to private GitHub repo
See `references/github-setup-guide.md` for step-by-step instructions covering:
- GitHub Desktop (Windows/Mac, no command line)
- Command line git (all platforms)
- Keeping the repo private (critical — contains real financial data)

### Option C: Self-host on home network
```bash
python3 -m http.server 8080 --bind 0.0.0.0
# Access from any device at http://YOUR_LOCAL_IP:8080/dashboard.html
```

---

## Key Principles

1. **Real data only** — Never invent account balances. Use placeholders if data is missing and mark them clearly.
2. **Conservative by default** — Never include temporary income in base projections. Never assume inheritance.
3. **Transparent math** — Show where every number comes from. Label estimates as estimates.
4. **Privacy first** — Single-file HTML, no external data transmission, no analytics, no tracking.
5. **Editable** — All data in a centralized config object. User should be able to update balances in 5 minutes.
