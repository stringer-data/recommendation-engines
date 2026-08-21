# 04 — Ad Auction Matching

## Method
CTR (click-through rate) prediction feeding a second-price (Vickrey) auction
mechanism, compared against pure CTR-ranked matching.

## Dataset
Avazu or Criteo Click-Through Rate prediction dataset (Kaggle) — impression-
level ad data with click labels and (for this case study) simulated bid
values, since neither public dataset includes real bids.

## Business Setting
Price-mediated matching. Unlike the other four case studies, "who gets
matched to this slot" isn't decided by relevance alone — it's decided by a
combination of predicted relevance (CTR) and willingness to pay (bid), and
the mechanism used to combine them has to be strategy-proof (advertisers
shouldn't be able to profit by misreporting their true value).

## Approach
1. Train a CTR prediction model (logistic regression baseline, then a
   gradient-boosted tree) on the impression data — this produces predicted
   relevance per (user, ad) pair.
2. Simulate bid values per advertiser (since real bids aren't in the
   dataset) to create a full auction scenario per impression.
3. **Naive baseline** — allocate the slot to the highest predicted CTR,
   ignoring bid entirely (pure relevance matching, no price signal).
4. **Second-price auction on expected value** — rank by `bid x predicted
   CTR` (expected revenue), winner pays the second-highest bid x their own
   CTR (or the standard "GSP" generalization for multiple slots) — this is
   close to how real ad auctions (Google, Facebook) actually rank and price.
5. Discuss: why ranking by `bid x CTR` rather than bid alone matters (it's
   what keeps the mechanism honest and keeps low-relevance-but-high-bid ads
   from crowding out relevant ones), and why second pricing (not first
   price) is what makes truthful bidding the advertiser's best strategy.

## Assumptions
1. Bids are simulated (drawn from a distribution, possibly correlated with
   advertiser "quality") since real bid data isn't public — flagged clearly.
2. Single-slot auction per impression to start; multi-slot generalized
   second-price (GSP) is a stretch extension, not required for v1.
3. CTR model is trained offline on historical data, not updated online.

## Planned Outputs
- [ ] CTR prediction model (logistic regression baseline + GBT comparison)
- [ ] Simulated bid generation per advertiser
- [ ] Naive CTR-only allocation
- [ ] Second-price auction allocation (rank by bid x CTR, pay second price)
- [ ] Comparison: revenue, relevance (CTR of winning ad), and a worked
      example showing truthful bidding is optimal under second pricing

## Suggested Python Packages
- `scikit-learn` — logistic regression CTR baseline
- `xgboost` or `lightgbm` — gradient-boosted CTR model
- `pandas` / `numpy` — data handling
