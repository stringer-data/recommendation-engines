# Recommendation Engines — Case Studies

A portfolio of recommendation and matching methods, each chosen for a genuinely
different problem shape rather than repeating the same technique on new data.
The focus is **marketplace matching** — problems where you're not just ranking
items for one user, but matching two sides of supply and demand under real
constraints (mutual preference, scarcity, pricing).

## Case Studies

| # | Case Study | Method | Problem Shape | Dataset |
|---|---|---|---|---|
| 1 | [Job Marketplace Matching](case_studies/01_job_marketplace_matching/) | Collaborative filtering vs. two-tower embeddings | Two-sided preferences, both sides have sparse signal | Job posting / resume matching dataset (Kaggle) |
| 2 | [Stable Matching (Dating)](case_studies/02_stable_matching_dating/) | Gale-Shapley deferred acceptance | Mutual preference — relevance alone doesn't guarantee a stable outcome | Speed Dating Experiment (Columbia Business School, Kaggle) |
| 3 | [Inventory-Aware E-Commerce](case_studies/03_inventory_aware_ecommerce/) | Two-tower retrieval + inventory-aware re-ranking | Recommending in-stock, sellable items — not just relevant ones | H&M Personalized Fashion Recommendations (Kaggle) |
| 4 | [Ad Auction Matching](case_studies/04_ad_auction_matching/) | CTR prediction + auction mechanism (second-price) | Matching driven by pricing/bidding, not just relevance | Avazu / Criteo Click-Through Rate dataset (Kaggle) |

## Why four different methods

A standard recommender asks "what would this user like?" A marketplace has to
ask that question from **both sides at once**, plus respect constraints a
single-sided recommender never faces:

- **Mutual preference** — both sides are choosing. Relevance to one side
  doesn't guarantee the other side agrees, and greedy matching can produce
  unstable outcomes where two participants would rather have matched with
  each other than their assigned partners (case study 2).
- **Two-sided sparsity** — cold start on both sides simultaneously, worse
  than single-sided cold start. Embedding-based retrieval tends to generalize
  better than pure collaborative filtering here (case study 1).
- **Scarcity/availability** — recommending an item nobody can actually buy
  (out of stock, sold out) is worse than not recommending it at all (case
  study 3).
- **Price-mediated matching** — sometimes the "match" is decided by who's
  willing to pay, not just who's most relevant (case study 4).

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
    01_job_marketplace_matching/
      README.md          # method, dataset, approach, assumptions
      data/raw/           # gitignored — see SOURCE_NOTE.md
      notebooks/
      src/                # reusable matching/model code pulled out of notebooks
    02_stable_matching_dating/
      ...
    03_inventory_aware_ecommerce/
      ...
    04_ad_auction_matching/
      ...
```
