# 01 — Ride-Hailing Rider-Driver Assignment

## Method
Linear (bipartite) assignment — Hungarian algorithm — compared against greedy
nearest-driver matching.

## Dataset
NYC TLC trip record data, or Chicago taxi trip data (both public, both include
pickup/dropoff coordinates and timestamps that can stand in for rider requests
and available drivers within a time window).

## Business Setting
Two-sided, **capacity-constrained**. A driver can serve exactly one rider at a
time; a rider needs exactly one driver. This is the cleanest case for seeing
why "recommend the best driver to each rider" independently doesn't work —
if every rider's top choice is the same nearby driver, most of those
recommendations are infeasible.

## Approach
1. Bucket trip requests and available drivers into short time windows (e.g.
   5-minute slices) to build realistic small bipartite matching problems.
2. Build a cost matrix per window: cost = distance/ETA from each available
   driver to each waiting rider.
3. **Greedy baseline** — each rider claims its nearest available driver in
   sequence; measure total assignment cost and how often later riders get
   stuck with a bad match because good drivers were already claimed.
4. **Optimal assignment** — solve the same window with the Hungarian
   algorithm (`scipy.optimize.linear_sum_assignment`) and compare total cost
   and fairness (variance in wait time across riders) against greedy.
5. Stretch: add a simple decay for "surge" by penalizing assignments that
   leave a whole zone under-served, connecting this to why real systems blend
   assignment with pricing.

## Assumptions
1. Drivers and riders within a time window are treated as simultaneously
   available (in reality this is streaming/online, not batch — worth
   discussing but not required to model in v1).
2. Cost is proxied by distance/ETA only; real systems also weight driver
   rating, cancellation risk, and rider fare sensitivity.
3. One rider maps to one driver (no pooled/shared rides).

## Planned Outputs
- [ ] Time-windowed rider/driver extraction from raw trip data
- [ ] Cost matrix construction
- [ ] Greedy assignment baseline with total cost + wait-time variance
- [ ] Hungarian algorithm assignment, same metrics
- [ ] Comparison plot: greedy vs. optimal cost and fairness across windows
- [ ] Discussion: where greedy is "good enough" vs. where it visibly breaks down

## Suggested Python Packages
- `scipy` — `optimize.linear_sum_assignment` for the Hungarian algorithm
- `pandas` / `numpy` — data handling
- `matplotlib` / `seaborn` — plots
- `geopy` or manual haversine — distance calculation if working from lat/lon
