# 04 — Inventory-Aware E-Commerce Recommendations

## Method
Two-tower retrieval model, re-ranked with an explicit inventory/availability
constraint layer.

## Dataset
H&M Personalized Fashion Recommendations (Kaggle) — customer purchase
history plus article metadata; article-level purchase frequency is used as
an inventory/availability proxy (since true live stock levels aren't in the
public dataset).

## Business Setting
Two-sided in a looser sense than the other case studies: many independent
sellers/articles (supply) with limited, decaying availability, and shoppers
(demand) whose relevance signal is behavioral. The failure mode this case
study targets is specific: a purely relevance-ranked recommender will keep
recommending items that are effectively unavailable (out of stock, discontinued,
oversold), which wastes the recommendation slot and frustrates the shopper.

## Approach
1. Train a standard two-tower retrieval model (customer tower + article
   tower) on purchase history — this is the relevance signal, same
   architecture idea as case study 2 but applied to product retrieval.
2. Build an availability signal per article over time (e.g. rolling recent
   purchase rate as a stock-depletion proxy, or explicit "discontinued after
   date X" if inferable from the data).
3. **Naive baseline** — rank purely by relevance score, ignoring
   availability; measure how often top-k recommendations include
   effectively-unavailable items.
4. **Inventory-aware re-ranking** — apply a penalty/filter for low-
   availability items at serving time and compare recommendation quality
   (recall@k on realized purchases) and "wasted slot rate" against the naive
   baseline.
5. Discussion: this is a real, well-documented failure mode in marketplace
   recommenders (Etsy and Amazon-marketplace both write about it) — the
   point isn't a fancier model, it's adding the right constraint at serving
   time.

## Assumptions
1. Purchase-rate decay is used as a stock-availability proxy in the absence
   of real inventory data — flagged clearly as a simplification.
2. Relevance model and availability layer are trained/applied separately
   (not jointly optimized) — this mirrors how many real systems actually do
   it (retrieval, then business-rule re-ranking).

## Planned Outputs
- [ ] Two-tower retrieval model trained on purchase history
- [ ] Availability proxy construction per article over time
- [ ] Naive (relevance-only) top-k recommendations + wasted-slot rate
- [ ] Inventory-aware re-ranked top-k recommendations + wasted-slot rate
- [ ] Comparison: recall@k and wasted-slot rate, naive vs. inventory-aware

## Suggested Python Packages
- `torch` — two-tower model
- `pandas` / `numpy` — data handling
- `scikit-learn` — evaluation utilities
