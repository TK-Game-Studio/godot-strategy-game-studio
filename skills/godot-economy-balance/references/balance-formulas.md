# Balance Formulas And Checks

## Core quantities

- Net rate of a building: `outputs_per_day − inputs_per_day − upkeep_per_day` (valued in a common unit, e.g. gold-equivalent or "food-days").
- Payback time: `cost_value / net_value_per_day`.
- Chain ratio: producers needed = `consumer_input_rate / producer_output_rate`.
- Population supported: `food_output_per_day / food_need_per_person_per_day`.
- Worker utilization: `assigned_workers / required_workers` — keep the economy limited by an intentional bottleneck (land, workers, or a key input), not by accident.

## Cost scaling

- Linear: `base + k * n` — gentle, good for cozy games.
- Exponential: `base * r^n` with r ≈ 1.07–1.5 — strong pressure, good for incremental/4X expansion costs.
- Soft caps: upkeep or unrest that grows with count (`k * n^1.2`) to discourage sprawl.

## Phase targets (example template)

| Phase | Time | Population | Key unlock | Pressure |
|---|---|---|---|---|
| Settle | 0–10 min | 10–20 | Tier 1 housing, food | none |
| Grow | 10–30 min | 30–80 | Tier 2 goods | first season/threat |
| Specialize | 30–60 min | 80–200 | Tier 3, trade | recurring threats |
| Late | 60+ min | 200+ | Megaproject | crisis/scenario |

## Automated checks for balance runs

- No resource hits zero for more than N days on default difficulty with the scripted build order.
- Population curve monotonic in the first 30 minutes on default.
- Every building is built at least once in at least one scripted strategy (otherwise it's useless or hidden).
- Hard mode fails the naive build order; default mode does not.

## Sources

- Game Developer, "How to tune a simulation game" (1849: resource chains mapped as a graph; master spreadsheet feeding per-city calculations)
- Game Developer, "Decision Modeling and Optimization in Game Design, Part 7: Production Queues"
- Game Developer, "My Approach To Economy Balancing Using Spreadsheets"
