# StreamPulse

**Urban freshwater stream monitoring, translated into One Health intelligence.**

> From streams to streams: turning citizen science into actionable One Health intelligence.

StreamPulse is a working prototype for the OneAquaHealth hackathon. It takes the same messy, jargon-heavy data a volunteer collects streamside and turns it into a site-level health score, a trend view, and a plain-language "what this means for people and ecosystems nearby" summary.

**Live prototype:** see [Prototype / Demo](#prototype--demo) below.

---

## Track alignment

**Primary track: Track 2 — Data-to-Insight**
*Challenge: turn citizen-collected data into actionable stream health and One Health insights.*

Stream data is only useful if someone who isn't a limnologist can act on it. StreamPulse's Dashboard and Insights views take raw readings (turbidity, dissolved oxygen, temperature, E. coli indicator, macroinvertebrate counts) and produce:
- a single blended health score per site, shown as a simple gauge rather than a spreadsheet;
- a 14-day trend chart per metric;
- a **One Health read** that combines the ecological number with *who is exposed* (a stormwater outfall's readings are meaningless on their own — what matters is that they predict conditions half a day later at the dog park downstream);
- a short predictive note ahead of a forecast weather event, so the insight is anticipatory, not just descriptive.

**Full coverage of all seven tracks**, because reliable insight depends on good input, sustained participation, and the data actually going somewhere:

| Track | Where it shows up in StreamPulse |
|---|---|
| Track 1 — Citizen Science UX | The "Report a Stream" flow asks plain-language questions ("How clear is the water?") instead of requiring NTU/mg/L knowledge up front, and converts answers to technical readings behind the scenes. |
| Track 2 — Data-to-Insight | **Primary track.** Dashboard with a watershed map, per-site health gauges, 14-day trend charts, and One Health insight summaries. |
| Track 3 — AI-Supported Assessment | A real **z-score anomaly check** flags submitted readings that are statistically unusual for that site, showing the exact math — never silently accepted or corrected. A rule-based **Field Assistant** answers common questions from a fixed, reviewed set of answers (explainable "AI prompts," not a live language model call). |
| Track 4 — Awareness & Storytelling | A **Learn** tab explains the One Health concept in plain language, plus a flip-card indicator glossary (NTU, DO, cfu, macroinvertebrates). |
| Track 5 — Community & Gamification | Reporting streaks, badges, a leaderboard, a **weekly challenge** ("report at 2 sites to earn Explorer"), and a **share** button for social spread. |
| Track 6 — Resilience Informatics | A **least-squares linear regression** model, fit live on each site's recent readings, projects conditions 2 days ahead and drives the early-warning banner — plus a Recommended Resilience Actions panel. |
| Track 7 — Digital Health Standards | A **Data & Standards** tab structures any site's latest reading as a FHIR-style `Observation` resource (JSON, copyable). An **autonomous triage agent** runs a fixed, rule-based sequence automatically after every report submission (validate → recalculate score → check forecast threshold → decide) and stops before any real-world action, requiring explicit human approval — a transparent illustration of an "AI agent" pattern without a live external API call. |

---

## Problem

Municipal environmental agencies and citizen-science programs collect large volumes of stream data, but it typically sits in spreadsheets only a trained specialist can read. Volunteers rarely see the downstream consequence of what they measured, so engagement drops, and agencies miss early signals of contamination or ecosystem stress that could inform public-health decisions (beach/creek closures, drinking-water intake alerts, disease-vector risk).

## Solution

A four-part web app:

1. **Dashboard** — every monitored site as a health gauge, a trend chart, and a One Health risk read.
2. **Report a Stream** — a guided, jargon-free data-entry wizard with an explainable, human-in-the-loop validation check.
3. **Insights** — One Health synthesis across the watershed plus a short predictive early-warning note.
4. **Community** — streaks, badges, and a leaderboard for sustained participation.

## Target users

- **Volunteer citizen scientists** (students, neighborhood groups) who need a low-friction way to contribute reliable data.
- **Municipal environmental / public-health staff** who need a fast read on ecosystem and exposure risk across many sites without manually triaging spreadsheets.
- **One Health coordinators** who need the ecological-to-human-health translation layer raw sensor data doesn't provide on its own.

## Expected impact

- Higher-quality, more consistent citizen data (guided entry + validation).
- Faster identification of contamination or ecological stress near human-use areas (parks, drinking-water intakes, recreational sites).
- Earlier warnings ahead of weather-driven risk spikes, instead of reactive responses after readings come in.
- Higher volunteer retention through visible, meaningful feedback on individual contributions.

---

## Prototype / Demo

The prototype is a single self-contained web app: [`src/index.html`](src/index.html).

**Run it locally** — no build step, no dependencies to install:
```bash
git clone <this-repo-url>
cd streampulse
open src/index.html      # macOS
# or: xdg-open src/index.html   (Linux)
# or: just double-click it / serve it: python3 -m http.server, then visit localhost:8000/src/
```

It loads Chart.js and Google Fonts from CDN at runtime; everything else (data, logic, styling) is inline in the one file, so it also works as a static page on GitHub Pages, Netlify, or any static host.

**What's simulated vs. real:** all readings are generated sample data for five illustrative sites in a fictional "Millrace Creek" watershed (see `src/index.html`, `sites` array). The scoring heuristic, validation-flag logic, and forecast note are simple, documented rules meant to demonstrate the *interaction pattern* — see [`docs/scoring-and-validation.md`](docs/scoring-and-validation.md) for exactly how each is computed, and [`docs/roadmap.md`](docs/roadmap.md) for what a production version would need (real sensor/lab feeds, a trained anomaly model, weather API integration, auth, a real database).

## Repository structure

```
streampulse/
├── README.md
├── LICENSE
├── src/
│   └── index.html          # the full prototype (HTML + CSS + JS, CDN deps only)
└── docs/
    ├── scoring-and-validation.md   # exactly how the health score & AI-assist flag work
    ├── one-health-mapping.md       # how each site's ecological data maps to a human/animal risk
    └── roadmap.md                  # what changes to go from prototype to production
```

## Tech notes

- Zero build step: plain HTML/CSS/JS.
- Chart.js (UMD, via CDN) for the trend chart.
- Fonts: Fraunces (display) + IBM Plex Sans (UI) + IBM Plex Mono (data readouts), via Google Fonts.
- Browser `localStorage` persists a viewer's streak/badges locally (no backend in this prototype).
- Light/dark theme aware (`prefers-color-scheme`).

## License

MIT — see [LICENSE](LICENSE).
