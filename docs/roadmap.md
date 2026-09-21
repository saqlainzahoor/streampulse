# Roadmap: prototype → production

This prototype demonstrates the interaction patterns (guided entry, explainable validation, One Health synthesis, gamified retention, early warning) end-to-end with simulated data. To become a deployable tool for an agency or citizen-science program:

## Data
- Replace the hardcoded `sites` array with a real backend (Postgres/PostGIS for spatial queries, or a managed service) fed by volunteer submissions and, where available, continuous sensor/lab data.
- Add a real map view (site locations, catchment boundaries, land-use overlay) — the current prototype uses a site list/gauge grid instead of a map to stay within this environment's constraints; a production build would use a proper mapping library.
- Track provenance per reading: who submitted it, device/method used (test strip vs. lab sample vs. sensor), and QA status.

## AI-assisted validation (Track 3)
- Move from a single-metric threshold to a model trained on multi-metric history plus weather context, still surfaced as an explainable flag with reasons, never a silent auto-correction.
- Add a reviewer queue for agency staff to confirm/reject flagged readings, feeding back into the model.

## Early warning (Track 6)
- Integrate a real precipitation/weather forecast API.
- Model each site's historical storm-response lag (time between rainfall and a turbidity/E. coli spike) from real data rather than a fixed illustrative number.
- Add configurable alert thresholds and notification channels (email/SMS) for agency staff and, optionally, opted-in volunteers near a flagged site.

## Community & retention (Track 5)
- Move streaks/badges from `localStorage` to a real account system so progress isn't lost per-device.
- Add site "adoption" (a volunteer or school claims a site as their own to monitor regularly) and team/challenge features.

## Interoperability (Track 7, future work)
- Define a data-exchange schema so stream-health and exposure data can be shared with public-health systems (e.g. mapped toward relevant FHIR resources such as Observation/RiskAssessment) rather than living only in this app.
- Publish an API so partner agencies or research groups can pull site data programmatically.

## Accessibility & scale
- Full accessibility audit (screen reader flow through the wizard, keyboard navigation, color-contrast check beyond the current pass).
- Localization for non-English-speaking volunteer communities.
- Multi-watershed support with an org/admin layer for agencies managing many catchments.
