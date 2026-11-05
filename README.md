# India Energy Atlas plugin for Claude

![India Energy Atlas](assets/icon.png)

Read-only public data on India's electricity grid, in Claude. Ask about state electricity demand, Grid-India Power Supply Position, grid frequency, fuel mix, carbon intensity, demand forecasts, Indian Energy Exchange (IEX) day-ahead prices, open-access charges, or early-stage Data Centre site screening in India. Answers keep the source, timestamp, unit and coverage from the data, and modelled values are labelled as modelled.

No account, key or sign-in is needed.

Published by India Energy Atlas Private Limited, operator of [energymap.in](https://www.energymap.in).

## What is in the plugin

| Component | What it does |
|---|---|
| Connector `india-energy-atlas` | Remote MCP server at `https://api.energymap.in/mcp/public`. 18 read-only tools. No authentication. |
| Skill `india-grid-data` | Picks the right tool for demand, supply position, frequency, carbon, forecasts and open-access questions, and keeps provenance, coverage and modelled-data labels in the answer. |
| Skill `india-power-market-brief` | Describes IEX day-ahead prices: hourly patterns, price-duration, bidding-area divergence and the price ceiling. Descriptive only, never trading advice. |
| Skill `india-data-centre-screen` | Runs place search, site screen and market comparison for an Indian location, and states what a screen does not prove. |

The full tool list, with arguments, is at [energymap.in/developer/mcp/public](https://www.energymap.in/developer/mcp/public).

## Use it

1. Add the plugin from the Claude directory, then connect the India Energy Atlas connector from the plugin's Connectors tab. Leave authentication empty.
2. Ask a question. For example:
   - "Compare the latest electricity demand across Indian states. Cite the IST slot, coverage and source timestamps, and name the states with no data."
   - "Give me the next 24 hours of electricity-demand forecasts for Karnataka and say clearly that they are modelled."
   - "Show the southern region's grid carbon intensity over the last 24 hours and explain the units and modelling basis."
   - "How did IEX day-ahead prices behave over the last 30 days? When were the peak hours?"
   - "Find Navi Mumbai and screen its early-stage Data Centre development potential."

In Claude Code, install the plugin from this repository:

```
/plugin install india-energy-atlas --marketplace India-Energy-Atlas/claude-plugin
```

Or add only the connector:

```bash
claude mcp add --transport http india-energy-atlas https://api.energymap.in/mcp/public
```

## Data and privacy

- **What the plugin sends.** When Claude calls a tool, the tool name and its arguments (for example a state slug, region, date, lookback period, forecast horizon, a place name, or a coordinate) go to India Energy Atlas at `api.energymap.in`. Your conversation is not sent, and the connector does not ask for your name, email, phone number, password or payment details.
- **Third party.** The place search tool sends the place text, from India Energy Atlas's server, to Mapbox, Inc. for location search. Mapbox receives the place text and fixed search settings (India only, English, up to five results), not your IP address or any identifier.
- **Retention.** Security logs (IP address, user agent, tool, minimum arguments, response code, latency, time) are kept for up to 90 days for delivery, rate limiting and abuse prevention. Daily counts by tool contain no identifiers. Requests are not used for advertising or model training.
- **The plugin itself** contains only Markdown, JSON and an icon. It runs no local code, hooks or scripts.

Privacy policy: [energymap.in/privacy](https://www.energymap.in/privacy). Terms: [energymap.in/terms](https://www.energymap.in/terms).

## Limits

- Read-only. No tool can trade, bid, operate grid assets or change any system.
- Public, aggregate data. No premises meter data and no verified substation or transformer headroom.
- Data Centre screens are early-stage comparisons. They do not establish grid headroom, interconnection approval, a tariff bill, water allocation, land title, permits or engineering feasibility.
- Public data refreshes every 15 minutes to daily, depending on the source.
- Rate limits: 20 requests per second per IP; Data Centre tools share 300 calls per 10 minutes.

## Sources

Grid-India (NLDC, RLDC) and state SLDC publications, IEX public market data, Central Electricity Authority emission factors, and state regulatory commission orders, compiled by India Energy Atlas. Each tool result names its source and data timestamp.

## Support

Email [hello@energymap.in](mailto:hello@energymap.in), or open an issue in this repository.

## License

Apache License 2.0. See [LICENSE](LICENSE). The data returned by the connector is governed by the India Energy Atlas [terms](https://www.energymap.in/terms), not by this license.
