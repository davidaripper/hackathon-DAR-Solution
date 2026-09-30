# Food Access Finder — Hack for Humanity 2026

**Challenge 2: The Last Mile of Food Access**  
Team: Crossby Dessalines · Adriel Allen · Rohan Vibhuti · Dave Ripper  
Event: Hack for Humanity, UConn Hartford, September 30, 2026

---

## The Problem

Food insecurity in Connecticut is well-documented, but the practical barrier for many families isn't availability — it's **proximity and transit**. A pantry 2 miles away is useless if you have no car, the bus doesn't run there, and it closes in 30 minutes.

Existing tools (Google Maps, 211 CT, Foodshare finder) return **distance-ranked lists**. They don't answer: *"What can I actually reach before closing, given how I travel?"*

---

## Our Solution

A **hybrid web app with a conversational interface** scoped to Greater Hartford:

- **Chat in:** User types natural language — *"I don't have a car, I'm near Park Street, I need food today"*
- **Rich web out:** Ranked option cards with travel times by bus/walk, today's hours, phone, and an **AI "likely open" confidence score**
- **Reachability-ranked, not distance-ranked** — the core differentiator
- **Hand-curated dataset** (~25 verified sites) — no live scraping, zero runtime dependencies

### Key Features

| Feature | Why It Matters |
|---------|----------------|
| Transit/walking-first ranking | Solves the actual barrier: reachability, not proximity |
| AI "likely open" score (0–100) | Handles inconsistent hours honestly; judges love the AI angle |
| Plain-language directions | Caseworker-usable; works on phone browser |
| No backend, no auth, no database | Nothing to fail at 4:55 PM |

---

## Tech Stack

- **Frontend:** Single-page vanilla HTML/CSS/JS (ES modules)
- **Data:** `data/sites.json` — 25 hand-curated Greater Hartford sites
- **Deployment:** Static hosting (GitHub Pages, Netlify, Replit, Vercel)
- **No build step** — open `index.html` in browser or serve with `npx serve`

---

## Project Structure

```
hackathon-DAR-Solution/
├── index.html          # Main app (self-contained)
├── data/
│   └── sites.json      # 25 curated food access sites
├── docs/
│   ├── team-strategy.md      # Full team strategy
│   ├── detailed-definition.md # Detailed spec (Dave's briefing)
│   └── guide-script.txt      # Event guide full text
└── README.md
```

---

## Quick Start

```bash
# Option 1: Open directly
open index.html

# Option 2: Local server (recommended for ES modules)
npx serve .
# or
python -m http.server 8000
```

Then visit `http://localhost:3000` (or 8000).

---

## Data Schema

Each site in `sites.json`:

```json
{
  "id": "park-st-pantry-01",
  "name": "Park Street Pantry",
  "address": "123 Park St",
  "town": "Hartford",
  "type": "pantry",              // pantry | mobile_market | snap_grocer
  "phone": "860-555-0100",
  "hours": {
    "mon": ["09:00", "17:00"],
    "tue": ["09:00", "17:00"],
    "wed": null,
    "thu": ["09:00", "17:00"],
    "fri": ["09:00", "14:00"],
    "sat": null,
    "sun": null
  },
  "snap_accepted": false,
  "transit_notes": "CTtransit 41/47 stop at Park & Main, 2 min walk",
  "lat": 41.7578,
  "lng": -72.6734,
  "verified": "2026-09-30",
  "notes": "ID required; first-come first-served"
}
```

**Curation rules:** Every row has name, address, town, type, and at least phone OR hours. `null` hours = unverified (scored lower honestly).

---

## The "Likely Open" Score (AI Differentiator)

| Score | Label | Meaning |
|-------|-------|---------|
| 90–100 | **Likely open** | Open per hours, verified recently |
| 60–89 | **Probably open — call to confirm** | Open per hours but unverified/stale |
| 30–59 | **Hours unverified — call first** | No hours data for this day |
| 0 | **Closed now** | Known closed today |

The AI does two jobs: (1) reconciles messy hours text into structured schema during curation, (2) generates the one-line rationale per result. This is the **meaningful, intentional AI use** the rubric scores.

---

## Demo Script (2 Minutes)

1. **0:00–0:20** — Problem framing: "Maria has no car. Google Maps shows pantries 8 miles away. 211 gives a phone number. Neither tells her what she can *actually reach before closing*."
2. **0:20–0:30** — Type scenario live: *"I don't have a car, I'm near Park Street, I need food today"*
3. **0:30–1:30** — Results appear: 3 reachable options ranked by bus/walk time, open-now scores. Tap top card → directions + hours + phone. Narrate one decision: *"It skipped the closer pantry — closed Wednesdays."*
4. **1:30–1:50** — "Distance-ranked vs. reachability-ranked — that's the difference between a list and an answer."
5. **1:50–2:00** — Next step: expand towns, partner with 211/Foodshare for live hours.

---

## Team Roles

| Person | Role | Workstream |
|--------|------|------------|
| **Adriel** | Data Science, Vibe Coding | Data curation, schema, open-now scoring |
| **Rohan** | SWE, UX Design, Data Science | App scaffold, chat UI, deployment |
| **Crossby** | Market Research, Domain, Vibe Coding | Problem evidence, persona, slide outline |
| **Dave** | 20 yrs radiology IT systems (integration, workflow, SQL, troubleshooting) | User workflow, integration testing, dataset validation (SQL), red-team demo, presentation |

---

## Checkpoints (Event Day)

| Time | Checkpoint |
|------|------------|
| 1:00 PM | "Does it run?" — Scaffold live on mock data, first 15 real rows reviewed |
| 2:30 PM | "Would a user trust it?" — Full dry run with real data, Dave's 5 messy inputs. **Feature freeze.** |
| 4:00 PM | Submit. Rehearse 5-min presentation (timed). |

---

## Deployment

**GitHub Pages:**
1. Push to `main` branch
2. Settings → Pages → Deploy from branch `main` / `/ (root)`
3. Live at `https://davidaripper.github.io/hackathon-DAR-Solution/`

**Replit:** Import from GitHub → runs instantly with multiplayer editing.

**Netlify/Vercel:** Drag `index.html` + `data/` folder → instant deploy.

---

## License

Team owns 100% of project IP (per event rules). License TBD by team.

---

## Links

- Event guide: https://docs.google.com/document/d/1ywTrtjYXKwiVL79X_s7yxLkdWMA4mgZhFxHWce1KegI/edit
- Event page: https://luma.com/aic-ha-sept-30
- Organizer contact: hello@nisreencain.com