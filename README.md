# Visa Processing Interactive Dashboard — GitHub Pages Deployment

Static, self-contained, **offline Excel-backed** dashboard. No Jekyll build,
no backend, no database, no login required. Data is fed to it through a
standardized Excel (`.xlsx`) file.

## Files

```
docs/
  index.html   ← the full dashboard (open in any browser)
  .nojekyll    ← disables Jekyll (fixes the SCSS build error)
.github/
  workflows/
    deploy.yml ← deploys docs/ to GitHub Pages (static, no build step)
```

There is **no `data/` folder** anymore. Excel is the database — the dashboard
only reads the file you load and (optionally) keeps a copy in your browser's
localStorage for convenience.

## How the data flows

```
Excel file (the source of truth)
        │  Load Excel
        ▼
Dashboard  ──►  filter / analyze / export
        │  Export Excel
        ▼
Excel file (hand to a colleague or open on another machine)
```

Nothing is uploaded to any server. Everything runs in the browser.

## Using the dashboard

1. Open the deployed page (or `docs/index.html` directly).
2. Click **📂 Load Excel**, pick a `.xlsx` file with the standardized columns
   (or click **📄 Template** to download a blank 3-row sample first).
3. Use the sidebar to switch between **Overview / Trends / Correlations /
   Insights / Records**, and apply the multi-dimension filters.
4. Click **⬇ Export Excel** any time to save the current dataset — email it,
   put it on a shared drive, or open it on another device.

### Standardized Excel columns

| # | Column           | Example                              |
|---|------------------|--------------------------------------|
| 1 | ID               | 1                                    |
| 2 | Applicant        | John Doe                             |
| 3 | Visa Type        | Business Visa / Visitor Visa / ABTC  |
| 4 | Nationality      | Singapore                            |
| 5 | Destination      | United States (or `ABTC`)            |
| 6 | Status           | Request Received … Request Completed and Closed |
| 7 | Request Date     | 2024-09-01                           |
| 8 | Completion Date  | 2024-10-15 (blank if not done)       |
| 9 | Processing Days  | 44 (blank if unknown)                |
|10 | Priority         | Normal / High / Urgent               |

The ABTC section on the Overview appears automatically for any row where
**Visa Type = ABTC** OR **Destination = ABTC** (booknoting flag).

## Deploy Steps

1. Copy `docs/` and `.github/` into your repository root.
2. Repository **Settings → Pages → Build source = "GitHub Actions"**.
3. Push to `main`. The workflow deploys automatically.
4. Visit `https://<user>.github.io/<repo>/`.

## Notes

- The page is fully shareable: anyone with the link can open and analyze the
  data you loaded. To let others see the *same* dataset, give them the Excel
  file (by exporting it) — there is no cross-device live sync by design, which
  keeps the tool simple and dependency-free.
- `.nojekyll` is required so GitHub Pages serves the HTML unmodified.
