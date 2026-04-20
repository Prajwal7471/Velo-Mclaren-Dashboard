# Velo × McLaren F1 — Modern Oral Sales, Supply Chain & 2026 Outlook Dashboard

A single-file interactive dashboard analysing BAT's 2025 Modern Oral (Velo) performance and modelling 2026 scenarios. Built to be deployable on GitHub Pages with zero build step.

**Live demo:** _add your GitHub Pages URL here_

---

## The question

BAT's Modern Oral category grew revenue **+47.4% to £1,165m in FY2025**, with Velo's US volume share rising from **6.1% in Nov 2024 to 24.0% in Dec 2025**. The category is now BAT's fastest-growing New Category franchise, and Velo Plus reached **#2 in US volume and value** within its first twelve months.

Rapid growth creates two problems for 2026 planning:

1. **Is our supply chain sized for the trajectory?** Volume +47% against a fully-distributed retail footprint suggests the binding constraint is capacity, not demand.
2. **How do we defend against the leader?** Zyn still holds a ~55–70% estimated share. The Velo moat is the **70% Velo Plus repurchase rate** — which has to survive a price or regulatory counter-punch.

This dashboard turns those two questions into numbers.

---

## What's in the dashboard

Six sections, all on one page, all interactive:

1. **Executive Overview** — four headline KPIs plus a three-bullet executive summary (the three moves for 2026)
2. **Sales Performance** — revenue/volume bar, US share trajectory with disclosed endpoints + a labelled logistic fit, 14-month rollout milestone ribbon
3. **Competitive Landscape** — share donut, Velo-vs-Zyn trajectory with a **55–70% uncertainty band** rather than a false point estimate, and a price/innovation positioning matrix
4. **Supply Chain & Operations** — a bullwhip demand-amplification framework, an inventory days-on-hand view with safety-stock and target bands, and a ranked risk register with revenue-at-risk and named owners
5. **2026 Outlook** — a three-scenario (Bear +15% / Base +30% / Bull +45%) fan chart, a live scenario summary, **a parametric LTV unit-economics calculator**, and a **capacity-expansion payback + 3-year NPV model**
6. **Strategic Recommendations** — five prioritised decisions. Each has a named owner, cost/effort, quantified expected outcome, and a specific measurement KPI.

---

## Design principles

### Credibility over polish
Every chart carries a **vintage badge**: green for BAT-disclosed actuals, amber for modelled/illustrative visualisations, blue for operational frameworks with no BAT source. The US share chart plots only the two disclosed endpoints as solid markers; the curve between them is a dashed logistic fit and is labelled as such. Zyn's share is shown as a band (55–70%), not a false point estimate.

### Decisions, not descriptions
The Strategic Recommendations section is deliberately structured as **what to do, who owns it, what it costs, what it returns, and how it's measured**. This replaces the typical dashboard "insights" section that restates the headline numbers.

### Parametric, not prescriptive
The LTV and capex payback calculators are sliders, not fixed numbers. This lets a reviewer stress-test assumptions directly — the most common senior-management response to any model is "but what if the retention drops to 60%?"

### Single-file, zero build
Deliberately built as one `index.html` with CDN dependencies (Tailwind, Chart.js) so it deploys to GitHub Pages with no CI, no build, no lock files. Target size under 300 KB; actual size is ~80 KB.

---

## Data provenance

| Metric | Value | Source | Vintage |
|---|---|---|---|
| Modern Oral revenue 2024 | £791m | BAT FY2025 Preliminary Results (comparative) | Feb 2026 |
| Modern Oral revenue 2025 | £1,165m | BAT FY2025 Preliminary Results | Feb 2026 |
| Revenue / volume growth | +47.4% / +47.1% | BAT FY2025 | Feb 2026 |
| US Velo volume share (Nov 2024) | 6.1% | BAT H1 2025 commentary | Jul 2025 |
| US Velo volume share (Dec 2025) | 24.0% | BAT FY2025 + Circana/Nielsen | Feb 2026 |
| Velo Plus US repurchase | 70% | BAT H2 2025 commentary | Feb 2026 |
| US rollout | ~135k stores, 90% weighted distribution | BAT FY2025 | Feb 2026 |
| Zyn share | 55–70% range | Public industry estimates | Q4 2025 |
| on! share | ~8.5% | Industry estimates | Q4 2025 |

Everything in the 2026 Outlook section is explicitly a parametric model. No internal BAT data is used; there is none in this repo.

---

## Methodology notes

- **US share trajectory fit.** A logistic curve between the two disclosed endpoints. Chosen over linear because category adoption of a new product rarely grows linearly. The curve is **labelled illustrative** everywhere it appears.
- **Forecast fan.** Simple ±15pp around the Base case. Base (+30%) is roughly 1.5σ below trailing growth — deliberately conservative to reflect that 47% growth rarely repeats. No Monte Carlo; the parametric simplicity is a feature, not a bug, for decision use.
- **LTV model.** `LTV = monthly_contribution / (1 − monthly_retention)`. Standard geometric-series lifetime. Monthly retention is a simplification — real life has cohort-level decay — but the sensitivity behaviour is directionally right.
- **Capacity payback.** `payback_months = capex / (annual_cans × price × margin / 12)`. 3-year NPV at 10% discount rate, no terminal value. Assumes the new capacity sells out — this is flagged as a bullish assumption in the Bear-case rationale.
- **Bullwhip and inventory DOH** are framework illustrations, labelled as such — they demonstrate the operational dynamics that 47% growth implies, not BAT plant data.

---

## With internal data access, I would add

- SKU-level inventory days-on-hand by DC
- Actual plant utilisation vs nameplate capacity
- Velo vs Velo Plus cannibalisation decomposition
- Cohort-level repurchase curves (not a flat monthly retention rate)
- Real PMTA timeline milestones per SKU
- State-level share heatmap (currently aggregated to national)

The framework charts in this dashboard are deliberate placeholders for those feeds.

---

## Tech

- HTML5 + Tailwind CSS (CDN) + Chart.js 4.4 (CDN)
- Zero build step, zero package manager, no browser storage
- html2canvas + jsPDF lazy-loaded only on PDF export
- Accessibility: WCAG AA contrast, keyboard navigation, `prefers-reduced-motion` respected, skip-to-content link, `aria-pressed` on toggles
- Print stylesheet for clean paper output
- URL state for region + scenario filters (shareable deep links)
- Keyboard shortcut: `?` opens the methodology panel

---

## Run locally

```bash
# No install step required — it's one file
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy to GitHub Pages

1. Push this repo to GitHub
2. Settings → Pages → Source: `main` branch, `/ (root)`
3. Wait 30 seconds → live at `https://<user>.github.io/<repo>/`

---

## Design decisions log

A few choices a reviewer might question, with the reasoning:

- **Removed the H1/H2 period filter.** BAT's interim split of Modern Oral revenue isn't directly disclosed at this granularity, and the earlier version multiplied FY by 0.45/0.55. Fake segmentation is worse than no segmentation.
- **Replaced single-point capacity scatter with a positioning matrix.** A scatter with one observation isn't a scatter.
- **Dropped the lead-time funnel.** The four-stage split was inferred, not disclosed. Replaced with a bullwhip signal and inventory DOH — same operational story, labelled as framework rather than as data.
- **Kept the McLaren livery restrained.** Carbon-weave pattern, clip-path corners, papaya accents. No chequered flags, no pit-stop metaphors. The aesthetic is the callback; the analysis is the point.

---

## Disclosure

Portfolio project. **Not affiliated with BAT, Velo, or the McLaren F1 Team.** Built entirely from publicly disclosed information. The Velo × McLaren livery treatment is inspired by the public partnership announced for the 2025 season.

---

## Licence

MIT for the code. Public BAT figures belong to their respective sources; see Data Provenance table above.
