# 01 — Job Marketplace Matching

## Method
Collaborative filtering (matrix factorization) compared against a two-tower
embedding model — one tower for candidates, one for jobs.

## Dataset
A public job-posting / application dataset (e.g. a Kaggle job recommendation
challenge dataset with candidate profiles, job postings, and
application/click events).

## Business Setting
Two-sided, **both sides sparse**. New candidates and new job postings show up
constantly, so neither side accumulates much interaction history before it
needs to be matched. This is the classic "two-sided cold start" problem —
worse than the single-sided cold start in typical product recommenders.

## Approach
1. Frame the label: did a candidate apply to / get matched to a job
   (implicit feedback, not an explicit rating).
2. **Collaborative filtering baseline** — matrix factorization (ALS or SVD)
   on the candidate x job interaction matrix. Note where it fails on
   candidates/jobs with few or no interactions.
3. **Two-tower model** — a candidate-side tower and a job-side tower, each
   encoding profile/posting features (skills, title, location, salary) into
   a shared embedding space, trained so that matched pairs land close
   together (in-batch negative sampling). This is the architecture real
   marketplaces (LinkedIn, Indeed) use precisely because it generalizes to
   unseen candidates/jobs from features alone, unlike pure CF.
4. Compare retrieval quality (recall@k) for warm vs. cold candidates/jobs
   between the two approaches — the gap is the point of this case study.

## Assumptions
1. Implicit feedback (applications/clicks) is treated as positive signal;
   no explicit negative signal exists, so negatives are sampled.
2. Candidate and job features are static snapshots, not full behavioral
   sequences.
3. "Match quality" is approximated by observed application behavior, which
   conflates candidate interest with job desirability — worth flagging as a
   limitation, not fixing in v1.

## Planned Outputs
- [ ] Interaction matrix construction from raw application/click data
- [ ] Matrix factorization baseline, recall@k split by warm vs. cold entities
- [ ] Two-tower model (simple feedforward encoders is enough — no need for
      anything exotic) trained with in-batch negatives
- [ ] Recall@k comparison, warm vs. cold, CF vs. two-tower
- [ ] Discussion: why marketplaces default to embedding retrieval over CF

## Suggested Python Packages
- `implicit` or `surprise` — matrix factorization baseline
- `torch` — two-tower model
- `pandas` / `numpy` — data handling
- `scikit-learn` — train/test splitting, evaluation utilities
