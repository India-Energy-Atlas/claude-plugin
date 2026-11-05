---
name: india-grid-data
description: Answer questions about India's electricity grid with the India Energy Atlas connector. Use when the user asks about Indian state or national electricity demand, peak demand, Power Supply Position, grid frequency, fuel mix, carbon intensity, demand forecasts, forecast accuracy, open-access charges, or which Atlas datasets exist.
---

# India grid data

Use the India Energy Atlas connector tools to answer with sourced, timestamped data. The connector is read-only and anonymous. It cannot trade, operate grid assets or change anything.

## Pick the tool

| User asks about | Tool | Notes |
|---|---|---|
| Which states or UTs are covered, valid state slugs | `india_grid_states` | Call first when a state slug is unknown. |
| One state's profile: capacity mix, peak demand, sources | `india_grid_state_detail` | Pass `state_slug`, for example `delhi`, `andhra-pradesh`. |
| Latest demand across states | `india_grid_live_snapshot` | No arguments for "latest". Pass `ist` (ISO 8601, IST) for a past slot. |
| Hourly demand forecast | `india_grid_demand_forecast` | `horizon_hours` 1 to 168, optional `state`. |
| How good the forecasts are | `india_grid_demand_accuracy` | Read the returned method field to say what was scored. |
| Modelled versus reported demand | `india_grid_demand_reconciliation` | `window` is `24h`, `7d` or `30d`. Report the computed-at date. |
| Daily energy met, shortage, peak by state or region | `india_grid_posoco_psp` | Grid-India (formerly POSOCO) Power Supply Position. |
| Grid frequency | `india_grid_frequency` | `hours` lookback. |
| Carbon intensity (gCO2/kWh) | `india_grid_carbon_intensity` | Optional `state` or `zone` (`northern`, `western`, `southern`, `eastern`, `north-eastern`). |
| Fuel mix behind the carbon estimate | `india_grid_carbon_mix` | Optional `state`. |
| Open-access charges | `india_grid_open_access` | Optional `state`. |
| What data exists, freshness, provenance | `india_grid_catalog` | Use before choosing a dataset for a new question. |

For IEX prices use the `india-power-market-brief` skill. For Data Centre siting use the `india-data-centre-screen` skill.

## Rules for every answer

1. **Keep provenance.** State the source, the timestamp or IST slot, the unit and any coverage figure the tool returns. Times in Indian results are IST (UTC+05:30) unless the field says UTC.
2. **Label modelled data.** Forecasts, reconciliation estimates and carbon intensity are modelled. Say so in plain words. Never present a modelled or forecast value as a measured official figure.
3. **Report gaps as gaps.** If a state has no data in a slot, name it as missing. Do not treat missing as zero, and do not fill it from memory.
4. **Report coverage.** For the live snapshot, give `states_with_data` out of `states_total` and the coverage share. If coverage is low, say the all-India figure is partial or unavailable.
5. **Report any aggregation.** If a result says it was bucketed, sampled or summarised, say so and give the bucket size.
6. **Use the right units.** Demand in MW (or GW for national totals), energy in MU (million units = GWh), frequency in Hz, carbon intensity in gCO2/kWh, charges in the unit the tool returns (often ₹/kWh or ₹/MWh). Do not convert silently; if you convert, show both.
7. **Stay in scope.** The connector has public aggregate data. It has no premises meter data, no verified spare transformer or substation capacity, and no share prices. Do not ask for a home address. Offer area-level data instead.
8. **No trading or investment advice.** Describe data. Do not recommend trades, bids or investments.

## Useful context for interpretation

- India runs five regional grids (Northern, Western, Southern, Eastern, North-Eastern) synchronised as one national grid. Grid-India operates it through NLDC, five RLDCs and state SLDCs.
- Scheduling uses 15-minute time blocks, 96 per day.
- The Indian Electricity Grid Code operating band for frequency is 49.90 to 50.05 Hz. Values far outside 49 to 51 Hz in a long series are more likely data errors than grid events; flag them rather than interpret them.
- Open-access charges change with state regulatory orders. Always give the effective date and source, and tell the user to confirm with the order before any commercial use.

## Answer shape

Lead with the direct answer (the number, comparison or trend). Then a short table if more than three values. Then one line of provenance: source, time, coverage, modelled or observed. Keep caveats to what changes the reading of the numbers.
