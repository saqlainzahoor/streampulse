# One Health mapping

One Health treats human, animal, and ecosystem health as connected. StreamPulse's Insights view is built around one rule: **an ecological reading only matters as much as the exposure pathway attached to it.** The same turbidity or E. coli number carries very different weight depending on who touches that water and how.

## How each prototype site is framed

| Site | Ecological role | Exposure pathway | Why it matters for One Health |
|---|---|---|---|
| Meadow Brook | Headwater, forested buffer | No direct recreational use | Baseline / early indicator of upstream land-use change; low direct human exposure risk |
| Schoolyard Reach | Mid-catchment | Used for youth field studies | Human exposure is episodic and supervised, but involves children — lower background risk tolerance |
| Millrace Outfall | Below a 40-block stormwater outfall | No direct contact, but feeds Cedar Dog Park ~½ day downstream | Leading indicator: its trend predicts risk elsewhere before that risk is directly observed |
| Cedar Dog Park | Downstream of Millrace Outfall | Popular off-leash water access for people and dogs | Highest-weighted site: direct, frequent water contact + upstream contamination source = the clearest recreational-illness risk pathway in the watershed |
| Confluence Park | Where the creek meets the river | 3 km upstream of a drinking-water intake | Even moderate readings deserve tighter monitoring, because the downstream consequence (drinking water) is higher-stakes than the readings alone suggest |

## The general pattern

```
ecological_signal (turbidity, DO, E. coli, macroinvertebrates)
        +
exposure_context (recreational contact? downstream intake? vulnerable population nearby?)
        =
One Health read (who is at risk, how soon, and what to do about it)
```

This is what separates a stream-health dashboard from a One Health dashboard: the same water-quality number is not a fixed risk — it's multiplied by who's downstream and how they use the water. A production version would formalize `exposure_context` as structured metadata per site (recreational-use flag, distance to intake, vulnerable-population flag, animal-contact flag) rather than hand-written text, so the "One Health read" can be generated and updated automatically as new sites are added.
