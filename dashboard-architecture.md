# Dashboard Architecture Reference

## Technical Stack

- **Single HTML file** — everything inline (CSS, JS, data)
- **Chart.js 4.x** via CDN: `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js`
- **Google Fonts** — choose distinctive pairings. Good options:
  - Display: Outfit, Sora, Plus Jakarta Sans, Manrope, General Sans
  - Mono: JetBrains Mono, Fira Code, IBM Plex Mono, Space Mono
  - **Avoid**: Inter, Roboto, Arial, system fonts
- **No frameworks** — vanilla JS, CSS custom properties, no React/Vue/build step
- **Dark theme** — deep navy/slate (not pure black), high contrast text, accent colors for data

## File Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Family Name] Financial Dashboard</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
  <style>
    /* All CSS here — use CSS custom properties for theming */
  </style>
</head>
<body>
  <!-- All HTML sections here -->
  <script>
    // === CONFIG (user edits this section to update balances) ===
    const ACCOUNTS = [...];
    const KIDS = [...];
    const NW = {...};
    const FUNDS = [...];

    // === UTILS ===
    // === NAVIGATION ===
    // === SECTION INITIALIZERS ===
    // === MONTE CARLO ENGINE ===
    // === INIT ===
  </script>
</body>
</html>
```

## Centralized Config Pattern

All user-specific data goes in config objects at the top of the script. This is critical —
the user should be able to update their dashboard by editing ONE section.

```javascript
const ACCOUNTS = [
  {
    name: 'Vanguard Brokerage',
    id: 'ACCT001',
    owner: 'User',
    type: 'Taxable',        // Taxable | Trad 401k | Roth IRA | Roth 401k | Trad IRA | 529 | UTMA | HSA
    tag: 'tb',              // CSS class for color coding
    platform: 'Vanguard',
    bal: 567249,
    status: 'Active',       // Active | Maxing | On leave | Liquidate | Consolidate | Underfunded
    stag: 'tg',             // CSS class for status tag
    funds: ['VTI', 'VXUS']  // Fund symbols for live price refresh
  },
  // ... more accounts
];

const KIDS = [
  {
    name: 'Child1',
    age: 5,
    yr: 2039,           // expected college start year
    yrs: 13,            // years to college
    b529: 52651,        // 529 balance
    otherLbl: 'UTMA',   // label for non-529 account
    otherBal: 4768,     // non-529 balance
    color: '#2dd4a0',
    dim: 'rgba(45,212,160,.12)',
    letter: 'C'         // avatar initial
  },
];

const NW = {
  invest: 1429779,
  deposit: 90898,
  realestate: 873600,
  debt: 3952,
  total: 2390327
};

const FUNDS = [
  { sym: 'VTI', name: 'Vanguard Total Stock Market ETF', acct: 'ACCT001, ACCT003', estVal: 420000 },
  // ... more funds
];
```

## Navigation Pattern

Use event delegation on the nav container — not inline onclick handlers:

```javascript
document.getElementById('nav').addEventListener('click', e => {
  const btn = e.target.closest('.nav-btn');
  if (!btn) return;
  // switch sections
});
```

## Lazy Initialization

Only build charts when a tab is first visited:

```javascript
const initialized = {};
function init(sectionName) {
  if (initialized[sectionName]) return;
  initialized[sectionName] = true;
  sectionInits[sectionName]?.();
}
```

## Monte Carlo Engine

### Return Models

```javascript
// Gaussian random (Box-Muller)
function gauss() {
  let u = 0, v = 0;
  while (!u) u = Math.random();
  while (!v) v = Math.random();
  return Math.sqrt(-2 * Math.log(u)) * Math.cos(2 * Math.PI * v);
}

// Student's t-distribution (for fat tails)
function tDist(nu) {
  const z = gauss();
  const u = 1 - Math.random();
  const chi2 = -2 * Math.log(u);
  return z / Math.sqrt(chi2 / nu) * Math.sqrt(nu / (nu - 2));
}

// Generate a single-year return
function generateReturn(model, mu, sigma) {
  if (model === 'lognormal') {
    const logMu = Math.log(1 + mu) - 0.5 * sigma * sigma;
    return Math.exp(logMu + sigma * gauss()) - 1;
  }
  if (model === 'fat-tails') return mu + sigma * tDist(5);
  return mu + sigma * gauss(); // normal
}
```

### Retirement MC Must Include:
- Accumulation phase (contributions + market returns)
- Decumulation phase (40 years, with slightly more conservative returns ~85% of equity mu)
- Social Security offset (kicks in after X years of retirement)
- Percentile fan chart (10th/25th/50th/75th/90th)
- Worst 5% first-5-year sequence metric (sequence-of-returns risk)
- Median estate value at end of simulation
- Contribution sensitivity table (±$5K and ±$10K)
- Auto-scaled histogram

### College MC Must Include:
- Glide path option (reduce equity exposure as college approaches)
- Both in-state and private cost scenarios
- College cost inflation (default 4%)
- Concrete gap amounts (median savings - cost = shortfall)

## Live Quote Refresh

```javascript
async function refreshQuotes() {
  for (const sym of symbols) {
    try {
      const resp = await fetch(
        `https://query1.finance.yahoo.com/v8/finance/chart/${sym}?interval=1d&range=1d`
      );
      const data = await resp.json();
      const meta = data.chart.result[0].meta;
      // Extract: meta.regularMarketPrice, meta.chartPreviousClose
    } catch (e) {
      // Graceful fallback — show "snapshot" instead of "LIVE"
    }
  }
}
```

**Important**: Yahoo Finance API may block CORS from `file://` protocol. The refresh button
works when served from `localhost` (via `python3 -m http.server`). Always handle the failure
case gracefully — never break the dashboard if quotes can't be fetched.

## Dynamic SVG Gauges

Don't hardcode SVG stroke-dashoffset values. Compute them from data:

```javascript
function drawGauge(containerId, value, maxVal, color1, color2, label, sublabel) {
  const pct = Math.min(value / maxVal, 1);
  const r = 65, circ = 2 * Math.PI * r;
  const offset = circ * (1 - pct);
  // Render SVG with computed dashoffset
}
```

## Responsive Breakpoints

```css
@media (max-width: 1200px) {
  .grid-4 { grid-template-columns: 1fr 1fr; }
  .grid-3 { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 768px) {
  .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; }
  /* Collapse budget rows, hide chart bars on mobile */
}
```
