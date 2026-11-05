---
name: india-data-centre-screen
description: Run an early-stage Data Centre site screen for a place in India with the India Energy Atlas connector. Use when the user asks whether an Indian city, district or locality suits a data centre, wants to compare Indian Data Centre markets, or asks about grid, connectivity, water or industrial tariff context for a data centre site in India.
---

# India Data Centre site screen

Use the India Energy Atlas connector to produce an evidence-labelled, early-stage screen. A screen ranks places for further study. It does not establish grid headroom, interconnection approval, a tariff bill, water allocation, land title, permits or engineering feasibility. Say this in every answer.

## Workflow

1. **Find the place.** Call `india_grid_data_centre_search` with the city, district or locality name the user gave (for example "Navi Mumbai", "Hosur", "Noida Sector 62"). Results are India-only, at most five.
   - If more than one plausible match comes back, pick the one that matches the user's state or context and say which you picked. Ask only if the matches are in different states and nothing in the conversation decides it.
   - Do not ask for a street address, and do not screen a home or another person's private location. The tool is for commercial siting.
   - If the user already gave coordinates for a commercial site, skip the search.
2. **Screen it.** Call `india_grid_data_centre_site_screen` with the chosen `latitude` and `longitude`, and a short `label`.
3. **Compare, if asked.** For "how does it compare" or "which markets are strongest", call `india_grid_data_centre_market_bands` with the relevant `metric` (`potential`, `demand`, `connectivity`, `water`, `industrial-tariff`) and an optional `area` filter.
4. **Grid context, if asked.** For the state's demand, supply position or carbon intensity, use the `india-grid-data` skill's tools for that state.

## How to report the screen

Lead with: the place matched, the score out of 100, the band and its label, and the confidence.

Then a table of components (grid access, load ecosystem, digital connectivity, water, industrial tariff, industrial ecosystem):

- Show each component's score as returned.
- Keep "Not reported" as "Not reported". It is not zero and not a weakness; it is missing evidence. List missing components together under "What the screen could not assess".
- Give the industrial tariff as the indicative energy-charge range and the number of DISCOMs it spans, with its effective date. Say it is an energy charge only, not a full bill (it excludes demand charges, duties, surcharges and open-access charges).

Close with the limits, in one short paragraph: what the screen does not prove (headroom, interconnection, tariff bill, water allocation, title, permits, feasibility) and the next diligence step for the weakest or missing component, for example a STU or DISCOM feasibility request for grid access.

## Data handling to mention if asked

The place text is sent from India Energy Atlas's server to Mapbox, its location-search provider, to find coordinates. No user identifier is sent. See https://www.energymap.in/privacy.
