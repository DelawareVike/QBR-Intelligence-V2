# QBR-Intelligence-V2

> A single-file CSV analytics platform for Customer Success and Account Management teams. Drop in your QBR data and get instant charts, KPIs, account drilldowns, filters, AI-powered recommendations, and PDF export — no backend required.

---

## Features

**Smart Data Detection**
The app automatically detects the structure of your CSV and maps columns to the right charts and KPIs — ARR, health scores, renewal outcomes, CSM assignments, churn flags, and more. Works with any structured CSV, not just QBR data.

**7 Auto-Generated Charts**
- ARR at Risk by Renewal Month — stacked bar showing dollars at risk per month, broken out by Renewal Up / Flat / Down / Churn
- Renewal Outcome Mix — donut chart with account count and ARR per outcome
- Health Score Distribution — horizontal bar of Green / Yellow / Red accounts with churn overlay
- Churn by Industry / Geography / CSM — side-by-side total vs churned
- ARR vs Health Score Scatter — every account plotted; red = churned, green = expanded, danger zone visible at a glance
- Book of Business by CSM — total ARR vs churned ARR per CSM
- Revenue by Geography or Industry — horizontal bar with churn breakdown

**Filter & Segment**
Five live dropdowns — CSM, Industry, Geography, Health Label, Renewal Outcome. Every chart, KPI, and table updates instantly. Active filter count badge shows how many filters are on.

**Account Drilldown**
Click any row in the account table to open a full account card showing:
- ARR, New ARR, net change
- Renewal outcome and health status pills
- Animated health sub-score bars (Support, NPS, Usage, Adoption, Engagement)
- Risk flags
- CSM & AE sentiment summary
- All fields in a scrollable table

**PDF Export**
- **Portfolio report** — "Export PDF" in the header prints the full dashboard with all charts and KPI cards formatted for A4/Letter
- **Single account report** — "Export This Account" inside the drilldown modal opens a clean white printable account card in a new tab

**AI Analysis**
Powered by Claude (Anthropic). Analyzes the current filtered dataset and returns 5 data-backed observations and 5 prioritized recommendations for the CS leadership team. Filter-aware — if you've filtered to a specific CSM or industry, the AI reflects that segment.

---

## Quickstart

### Option A — Run locally (no setup)

1. Download `qbr-intelligence-v2.html`
2. Open it in any modern browser
3. Drop in your CSV
4. Optionally paste your [Anthropic API key](https://console.anthropic.com) to enable AI analysis

No server, no dependencies, no install.

### Option B — Deploy to Netlify (shared team access + secure API key)

See [Deploy to Netlify](#deploy-to-netlify) below.

---

## CSV Format

The app auto-detects column roles by name. It works best when your CSV uses recognisable column names, but will fall back gracefully for any structured data.

| Column | Example Name | Description |
|---|---|---|
| Account Name | `Account_Name` | Company or account identifier |
| ARR | `ARR` | Current annual recurring revenue |
| New ARR | `New_ARR` | Post-renewal ARR |
| Renewal Outcome | `Renewal_Outcome` | `Renewal Up`, `Renewal Flat`, `Renewal Down`, `Churn` |
| Churned Flag | `Churned` | `Yes` / `No` or boolean |
| Health Label | `Health_Label` | `Green`, `Yellow`, `Red` |
| Overall Health Score | `Health_Score_Overall` | Numeric 0–100 |
| Health Sub-scores | `Health_Support`, `Health_NPS`, etc. | Numeric 0–100 each |
| CSM | `CSM` | Customer success manager name |
| Account Executive | `Account_Executive` | AE name |
| Industry | `Industry` | Vertical or sector |
| Geography | `Geography` | Region or territory |
| Contract End | `Contract_End` | Renewal date (used for timeline chart) |
| Churn Reason | `Churn_Reason` | Free text reason for churn |
| Account Summary | `Account_Summary` | CSM/AE narrative, shown in drilldown |
| Risk Flag | `Risk_Identified` | Free text risk description |
| Salesforce ID | `Salesforce_ID` | CRM record identifier |

A sample 500-account test dataset (`qbr_accounts_500.csv`) is included in this repository.

---

## Deploy to Netlify

The Netlify version (`qbr-intelligence-netlify-v2.zip`) moves the Anthropic API key to a server-side environment variable so it is never exposed in the browser.

### 1. Create a GitHub repo and push the files

```bash
unzip qbr-intelligence-netlify-v2.zip
cd netlify-deploy
git init
git add .
git commit -m "QBR Intelligence V2"
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

### 2. Import on Netlify

1. Go to [netlify.com](https://netlify.com) → **Add new site** → **Import existing project**
2. Connect your GitHub repo
3. Netlify auto-detects `netlify.toml` — no extra config needed
4. Click **Deploy site**

### 3. Add your API key

1. Netlify dashboard → **Site configuration** → **Environment variables**
2. Add: `ANTHROPIC_API_KEY` = `sk-ant-api03-...`
3. Trigger a redeploy — **Deploys → Trigger deploy**

### Local dev with Netlify CLI

```bash
npm install -g netlify-cli
```

Create a `.env` file in the project root (never commit this):

```
ANTHROPIC_API_KEY=sk-ant-api03-...
```

Then run:

```bash
netlify dev
```

App runs at `http://localhost:8888`.

---

## Project Structure

```
netlify-deploy/
├── netlify.toml                  ← Build config, redirects, security headers
├── netlify/functions/
│   └── analyze.js                ← Serverless proxy — keeps API key server-side
└── public/
    └── index.html                ← Full application (single file)
```

The standalone `qbr-intelligence-v2.html` is identical to `public/index.html` except it calls the Anthropic API directly from the browser using a key you paste in — suitable for local use only.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Parsing | [PapaParse 5.4](https://www.papaparse.com/) |
| Charts | [Chart.js 4.4](https://www.chartjs.org/) |
| AI | [Anthropic Claude API](https://www.anthropic.com/) (`claude-sonnet-4-20250514`) |
| Fonts | [Syne](https://fonts.google.com/specimen/Syne) + [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) |
| Deployment | [Netlify](https://netlify.com) with serverless functions |
| Dependencies | Zero runtime dependencies — one HTML file |

---

## Browser Support

Works in all modern browsers. Requires JavaScript enabled.

| Browser | Support |
|---|---|
| Chrome / Edge 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 14+ | ✅ Full |
| Mobile (iOS / Android) | ✅ Responsive layout |

---

## Security Notes

- **Standalone HTML** — The API key is stored in `localStorage` in your browser. Never share the HTML file while a key is saved.
- **Netlify version** — The API key is an environment variable on the server. It is never sent to the browser or committed to the repository. This is the recommended setup for shared team use.
- **Data privacy** — CSV data is parsed entirely in the browser. No data is sent to any server except the summary statistics sent to the Anthropic API when you click "AI Analysis."

---

## License

MIT — free to use, modify, and deploy.
