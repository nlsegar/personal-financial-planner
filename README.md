# Financial Dashboard Builder — Claude Skill

A Claude skill that builds a comprehensive, personalized single-file HTML financial dashboard from your real account data. No server, no API keys, no data leaves your machine.

## Screenshots

![Overview](screenshots/01-overview.png)
![Portfolio](screenshots/02-portfolio.png)
![Retirement](screenshots/03-retirement.png)
![Monte Carlo](screenshots/04-montecarlo.png)
![Action Items](screenshots/05-actions.png)

## What It Does

Give Claude your financial information (or upload screenshots from Copilot Money, Mint, Empower, etc.) and it builds a full interactive dashboard with:

- **Net worth overview** with investable-only toggle
- **Portfolio breakdown** by account, tax treatment, and platform
- **Budget tracking** with editable categories
- **Retirement projections** with 3 scenarios and Roth conversion planning
- **529 & college planning** per child with cost coverage analysis
- **Monte Carlo simulations** for both retirement and college (log-normal, normal, and fat-tail models)
- **Live stock quote refresh** for your fund holdings
- **Prioritized action items** and annual tax calendar

The output is a single `.html` file you can open in any browser, serve from localhost, or push to a private GitHub repo.

## Installation

### Claude.ai Projects
1. Download this repo (or clone it)
2. In Claude.ai, create or open a Project
3. Go to Project Knowledge → Add Content
4. Upload the `SKILL.md` file and the `references/` folder contents

### Claude Code / Desktop
Drop the `financial-dashboard-builder/` folder into your skills directory.

## Usage

Just ask Claude naturally:

- *"Build me a financial dashboard"*
- *"Here are my accounts, create a portfolio tracker"*  
- *"I want to run Monte Carlo simulations on my retirement plan"*
- *Upload Copilot/Mint screenshots* → *"Build a dashboard from these"*

Claude will:
1. Ask you about your accounts, income, goals, and family
2. Build a personalized dashboard with real data
3. Run it through 3 expert reviewers (UX, CFP, Quant)
4. Iterate until quality score exceeds 90/100
5. Help you host it (local file, localhost server, or private GitHub)

## What's Included

```
financial-dashboard-builder/
├── SKILL.md                              # Main skill instructions
├── references/
│   ├── data-collection-guide.md          # Financial intake questionnaire
│   ├── dashboard-architecture.md         # Technical spec for the HTML dashboard
│   ├── expert-review-criteria.md         # Scoring rubrics for 3 expert reviewers
│   ├── github-setup-guide.md             # Step-by-step GitHub/hosting instructions
│   └── account-limits.md                 # 2025/2026 retirement & education contribution limits
└── README.md                             # This file
```

## Key Design Principles

- **Privacy first** — single HTML file, no external data transmission, no analytics
- **Real data only** — never invents balances; labels estimates as estimates
- **Conservative projections** — never includes temporary income in base forecasts
- **Correct math** — separates payroll deductions from take-home; doesn't conflate 529 and retirement contributions; shows time-weighted retirement pacing, not misleading raw percentages
- **Expert-reviewed** — every dashboard goes through UX, financial planning, and quantitative review

## Hosting Options

| Method | Difficulty | Live Quotes | Always-On |
|--------|-----------|-------------|-----------|
| Double-click HTML file | Zero | No (CORS) | No |
| `python3 -m http.server 8080` | Easy | Yes | While running |
| Private GitHub repo | Medium | No | Yes (via GitHub Pages with Pro plan) |
| Home server / Raspberry Pi | Medium | Yes | Yes |

See `references/github-setup-guide.md` for detailed instructions for each option.

## Screenshots

*Dashboard is generated per-user with their real data. No screenshots included to protect privacy.*

## FAQ

**Q: Is my financial data sent anywhere?**  
A: No. The dashboard is a static HTML file. All data lives in a JavaScript config object in the file itself. No API calls are made except the optional Yahoo Finance quote refresh (which only sends ticker symbols, not your balances).

**Q: Can I update my balances later?**  
A: Yes. Open the HTML file in any text editor, find the `ACCOUNTS`, `KIDS`, and `NW` config objects near the top of the `<script>` section, and update the numbers. Or ask Claude to rebuild it with new data.

**Q: Does the Monte Carlo simulation account for taxes?**  
A: The MC uses real (inflation-adjusted) returns. It doesn't model tax drag on taxable accounts specifically — returns are pre-tax. For most users with a mix of tax-advantaged and taxable accounts, this is a reasonable simplification.

**Q: What if I don't have all the information Claude asks for?**  
A: Claude will build with what you have and clearly label anything estimated. You can always update later.

## License

MIT — use it however you want. Just don't publish anyone else's financial data.

## Credits

Built with Claude (Anthropic) using the Claude Skills framework. Chart.js for visualizations.
