# 03 — Stable Matching: Speed Dating

## Method
Gale-Shapley deferred acceptance (stable matching), compared against
"greedy mutual top-choice" matching.

## Dataset
Columbia Business School Speed Dating Experiment dataset (Kaggle) — each
participant rates every partner they met, and each also states their own
preferences, giving genuine **mutual** preference data on both sides. This
is rare and exactly what stable matching needs.

## Business Setting
Two-sided, **mutual preference matters more than pure relevance**. A
recommender that just ranks "who you'd like most" per person, independent of
who *they'd* like, can produce an outcome where two people would both rather
be matched with each other than with who they got assigned — an unstable
match. Marketplaces where both sides have to agree (dating, roommate
matching, some hiring flows) need matches that are stable, not just
individually high-relevance.

## Approach
1. Build preference rankings per participant from the rating data (each
   person's ranked list of everyone they interacted with).
2. **Greedy baseline** — match each person to their top remaining choice in
   some arbitrary order; show concretely where this produces an unstable
   pair (both prefer each other over their assigned partners).
3. **Gale-Shapley** — implement deferred acceptance (proposing side proposes
   down their list, receiving side tentatively accepts/rejects). Verify the
   result is provably stable (no blocking pair) by exhaustively checking all
   pairs.
4. Compare: who does better under Gale-Shapley — the proposing side or the
   receiving side? (This is the classic result: the proposing side gets
   their *best possible* stable partner, the receiving side gets their
   *worst possible* stable partner among stable matchings — worth
   demonstrating directly, not just citing.)
5. Stretch: what happens with unequal group sizes, or when some
   participants would rather stay unmatched than accept anyone on their
   list?

## Assumptions
1. Preferences are treated as static, complete strict rankings — no ties, no
   preferences changing over the course of the event.
2. One-to-one matching only (classic stable marriage problem, not
   many-to-one like the hospital/residency variant).
3. Both sides' preference lists are derived from post-date ratings, which is
   a reasonable but imperfect proxy for "true" preference ranking.

## Planned Outputs
- [ ] Preference list construction per participant from ratings
- [ ] Greedy matching with an explicit demonstration of an unstable pair
- [ ] Gale-Shapley implementation with a stability check (no blocking pairs)
- [ ] Proposer-optimal vs. receiver-optimal outcome comparison
- [ ] Discussion: where real dating apps' recommendation ranking and stable
      matching diverge, and why apps mostly don't run literal Gale-Shapley

## Suggested Python Packages
- `pandas` / `numpy` — data handling
- plain Python for the Gale-Shapley implementation (no library needed — the
  point of this case study is to write the algorithm, not import it)
- `matplotlib` / `networkx` — visualizing matches and blocking pairs
