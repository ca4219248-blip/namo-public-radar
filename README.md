# NAMO Public Radar

Live public-engagement tracker for Prime Minister Narendra Modi — a static, data-driven "system" that reads publicly announced engagements (PIB / PMO / credible news), matches them against live IST time, and shows on an interactive map **where the PM should publicly be right now**.

## What it does

- **Live status card** — shows either `LIVE — CONFIRMED` (an announced PM event is happening right now, exact venue) or `ANDAZA — INFERRED` (a schedule-based estimate).
- **Interactive dark map** — completed PM stops (gold), live confirmed events (green), inferred current position (dashed blue), and context event venues where PM presence is not confirmed (grey ring). PM stops are joined by a dashed route line in time order.
- **Inference engine** (runs every 30 s):
  1. If a PM event is live → show exact venue.
  2. If the next event starts within 36 h in a different city → assume the PM is there (or travelling).
  3. If the last event ended less than 24 h ago → assume same city.
  4. Otherwise → assume base city (New Delhi).
- **Countdown to next engagement**, context events, and a full source-linked engagement ledger.

## How to update the data

All schedule data lives in **`data.json`**. The site auto-loads it (and re-fetches every 5 minutes), falling back to the embedded copy in `index.html` when opened locally via `file://`.

To add a new engagement, append an object:

```json
{
  "id": "e7",
  "title": "Event title",
  "detail": "One-line description.",
  "city": "City",
  "venue": "Venue name",
  "lat": 28.6129,
  "lng": 77.2295,
  "start": "2026-10-01T10:00:00+05:30",
  "end": "2026-10-01T13:00:00+05:30",
  "pm": true,
  "source": "PIB",
  "url": "https://www.pib.gov.in/..."
}
```

- `pm: true` → counts as a confirmed PM personal appearance.
- `pm: false` → shown as a context event venue only.
- `approx: true` → marks dates that vary across reports.

Commit the change — the map, status card, countdown and ledger all re-derive automatically.

## Run locally

Just open `index.html` in a browser (map tiles and fonts load from CDN). Or serve it so `data.json` auto-loading works:

```bash
python3 -m http.server 8000
```

## Important disclaimer

This is **not** real-time GPS tracking. The PM is under SPG protection; no legitimate source publishes his live location. Everything here is derived from **publicly announced schedules** — the "inferred" marker is an educated guess, not an exact position. Sources: PIB, PM India, Hindustan Times, Asia News Network.

Map tiles © OpenStreetMap contributors © CARTO.
