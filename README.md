# campaign-dashboard-v1

**PearlView Dental — Paid Acquisition Dashboard**

Private, auto-updated dashboard for PearlView Dental's Google Ads (and future Meta) paid acquisition. Maintained by the Triage Orchestrator agent.

## Files

| File | Purpose |
|---|---|
| `index.html` | The master dashboard — open locally in any browser |
| `GOOGLE_ADS_SETUP.md` | API setup guide for connecting the Google Ads data pipeline |

## How It Updates

The Triage Orchestrator agent updates `index.html` on every morning/evening run (07:00 / 17:00 CDT) and pushes the changes here automatically. The dashboard is also served inline in the Hermes desktop app.

## Dashboard Features

- **Verdict banner** — one-line account state
- **R5 config drift traffic lights** — Presence-only, Auto-apply, Manual CPC, Budget
- **Performance tiles** — 7-day rolling with DoD variance labels
- **Geo tier table** — by market tier (North Aurora / Aurora / Montgomery / paused)
- **Quality Score tracker** — keyword-level QS components (the real scoreboard)
- **Open items** — action queue with blocked/open/done status
- **Data integrity panel** — clean window tracking
- **Interactive command bar** — buttons that trigger analysis runs in Hermes

## Viewing

Open `index.html` in any browser, or view inline in Hermes via `::preview`.
