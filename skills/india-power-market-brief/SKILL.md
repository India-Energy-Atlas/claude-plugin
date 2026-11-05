---
name: india-power-market-brief
description: Describe Indian Energy Exchange (IEX) day-ahead prices with the India Energy Atlas connector. Use when the user asks about IEX prices, market clearing prices, area or bidding-zone prices, peak and off-peak hours, price spikes, price-duration curves, or price patterns relevant to storage or demand flexibility in India.
---

# India power market brief

Use the India Energy Atlas connector's IEX tools to describe recent day-ahead market prices. These tools describe the past. They do not place, recommend or forecast trades, and they are not investment advice.

## Pick the tool

| Question | Tool | Arguments |
|---|---|---|
| Hour-by-day pattern, when prices peak | `india_grid_iex_heatmap` | `days` lookback |
| Share of hours above each price level | `india_grid_iex_price_duration` | `days` lookback |
| Prices by bidding area, regional divergence, congestion | `india_grid_iex_area_prices` | `date` (YYYY-MM-DD) for one day, or `days` |
| Demand context for the same period | `india_grid_live_snapshot` or `india_grid_posoco_psp` | see the `india-grid-data` skill |

For a broad "how has the market looked" question, call the heatmap and the price-duration curve for the same `days`, then add area prices only if the user asks about regions.

## How to read the results

- Prices are in ₹/MWh unless the result says otherwise. ₹1,000/MWh equals ₹1/kWh. Give ₹/kWh in brackets when the user is not a market specialist.
- The day-ahead and real-time markets on IEX have a price ceiling of ₹10,000/MWh (₹10/kWh). Hours at the ceiling mean the market cleared at the cap, usually with unmet buy bids. Say this rather than calling it an ordinary high price.
- Trading uses 15-minute blocks, 96 per day. Hours in results are IST.
- India has 13 bidding areas. When area prices diverge, that indicates transmission congestion between areas for those blocks. Uniform prices mean the market cleared without area splitting.
- Evening peaks after sunset and low midday prices during high solar output are the common pattern. Describe what the data shows rather than assuming it.
- If the result says it was summarised (percentile points, hourly grid, or a subset of divergent blocks), state that and how.

## Answer shape

1. One-sentence headline: the average or median price and the main pattern.
2. A compact table: peak window, off-peak window, share of hours at or above the ceiling, any area divergence.
3. Provenance: the period covered, "IEX public market data via India Energy Atlas", and the data timestamp.
4. If the user asks what to do with this (for example battery sizing), give descriptive implications only, such as the spread between low and high hours, and say it is not a trading or investment recommendation.

## Do not

- Do not execute or suggest specific bids, volumes or trades.
- Do not predict future prices from these tools. For a demand forecast use `india_grid_demand_forecast` and label it modelled.
