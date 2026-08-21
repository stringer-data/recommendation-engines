# Recommendation Engines — Case Studies

A portfolio of recommendation and matching methods, each chosen for a genuinely
different problem shape rather than repeating the same technique on new data.
The focus is **marketplace matching** — problems where you're not just ranking
items for one user, but matching two sides of supply and demand under real
constraints (capacity, mutual preference, scarcity, pricing).

## Case Studies

| # | Case Study | Method | Problem Shape | Dataset |
|---|---|---|---|---|
| 1 | [Ride-Hailing Assignment](case_studies/01_ride_hailing_assignment/) | Linear assignment (Hungarian algorithm) vs. greedy matching | One-sided ranking isn't enough — need a capacity-respecting assignment | NYC TLC / Chicago taxi trip data |
| 2 | [Job Marketplace Matching](case_studies/02_job_marketplace_matching/) | Collaborative filtering vs. two-tower embeddings | Two-sided preferences, both sides have sparse signal | Job posting / resume matching dataset (Kaggle) |
| 3 | [Stable Matching (Dating)](case_studies/03_stable_matching_dating/) | Gale-Shapley deferred acceptance | Mutual preference — relevance alone doesn't guarantee a stable outcome | Speed Dating Experiment (Columbia Business School, Kaggle) |
| 4 | [Inventory-Aware E-Commerce](case_studies/04_inventory_aware_ecommerce/) | Two-tower retrieval + inventory-aware re-ranking | Recommending in-stock, sellable items — not just relevant ones | H&M Personalized Fashion Recommendations (Kaggle) |
| 5 | [Ad Auction Matching](case_studies/05_ad_auction_matching/) | CTR prediction + auction mechanism (second-price) | Matching driven by pricing/bidding, not just relevance | Avazu / Criteo Click-Through Rate dataset (Kaggle) |

## Why five different methods

A standard recommender asks "what would this user like?" A marketplace has to
ask that question from **both sides at once**, plus respect constraints a
single-sided recommender never faces:

- **Capacity** — a driver can only take one rider, a job has one opening.
  Ranking isn't enough; you need an *assignment* (case study 1).
- **Mutual preference** — both sides are choosing. Relevance to one side
  doesn't guarantee the other side agrees, and greedy matching can produce
  unstable outcomes where two participants would rather have matched with
  each other than their assigned partners (case study 3).
- **Two-sided sparsity** — cold start on both sides simultaneously, worse
  than single-sided cold start. Embedding-based retrieval tends to generalize
  better than pure collaborative filtering here (case study 2).
- **Scarcity/availability** — recommending an item nobody can actually buy
  (out of stock, sold out) is worse than not recommending it at all (case
  study 4).
- **Price-mediated matching** — sometimes the "match" is decided by who's
  willing to pay, not just who's most relevant (case study 5).

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Each case study's `data/raw/SOURCE_NOTE.md` will document exactly how to
(re)download its dataset — nothing is bundled in this repo.

## Repository Structure

```
recommendation-engines/
  README.md
  requirements.txt
  case_studies/
    01_ride_hailing_assignment/
      README.md          # method, dataset, approach, assumptions
      data/raw/           # gitignored — see SOURCE_NOTE.md
      notebooks/
      src/                # reusable matching/model code pulled out of notebooks
    02_job_marketplace_matching/
      ...
    03_stable_matching_dating/
      ...
    04_inventory_aware_ecommerce/
      ...
    05_ad_auction_matching/
      ...
```
