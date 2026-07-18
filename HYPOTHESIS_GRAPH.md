# FrontierCode Audit Hypothesis Graph (2026-07-17)

The audit is an experiment on a benchmark. Each hypothesis is a claim about what FrontierCode measures versus what it says it measures. Numbering is F-prefixed to avoid colliding with sweep's H-series.

## F0: The diff is insufficient to predict a merge decision

**Prediction:** Among real-maintainer PR closures where the diff clears an adversarial quality gate, a majority cite non-diff causes (description reasoning, interaction cadence, policy/template compliance, contributor standing). A benchmark that grades only the diff therefore explains a minority of the variance in the outcome it claims to measure.

**Status: SUPPORTED by existing data, not yet framed against FrontierCode directly.** Sweep closure taxonomy (as of retro 13, 2026-05-13): pipeline errors 17, credence tests 7, external 10, AI detection via description 1. QA found zero bugs on 4/5 "AI slop"-labeled PRs; 80% of slop rejections were policy-based. Lab side: iteration lifts one-shot diff approval 43% → 91%; field adjusted approval ~80–84%; the residue is the social layer.

**Falsifier:** A dataset of real closures where diff-quality causes dominate after honest reclassification. Or: demonstration that at Diamond-task difficulty, diff correctness failures preempt the social layer entirely (the maintainer never reaches the description because the patch is wrong).

## F1: FrontierCode's rubric axes and real rejection causes are near-disjoint

**Prediction:** Mapping each sweep closure to FrontierCode's six rubric axes leaves the majority of closures unmapped. The unmapped set clusters on axes FrontierCode does not score: description, cadence, compliance, standing.

**Status: NOT YET RUN.** The mapping exercise is the audit's first concrete deliverable. Input: sweep closure records. Output: a two-column table, closure → rubric axis or NONE.

**Falsifier:** ≥50% of closures map cleanly onto the six axes. Then the critique narrows from "the diff is insufficient" to "the diff is insufficient at the margin," a weaker claim.

## F2: Score depends on harness as much as model

**Prediction:** FrontierCode leaderboard deltas between models are confounded by agent-harness differences. Our 43% → 91% result moved approval 48pp with the model held constant and only the review loop varied, a larger delta than the entire Diamond leaderboard spread (13.4% top to <5%). If a loop moves outcomes more than a model swap, a leaderboard that reports model names is mislabeled.

**Status: SUPPORTED in our regime, untested in theirs.** Community concerns point the same way (variance and reproducibility, per Latent Space roundup).

**Falsifier:** Cognition publishes per-model harness configs showing a fixed harness across entries, plus variance bounds tight enough that a review-loop intervention could not account for inter-model gaps.

## F3: The private dataset makes the headline claims unauditable

**Prediction:** The 81% SWE-bench false-positive figure and the per-task rubrics cannot be verified or refuted by anyone outside Cognition's evaluation-access program. Every public citation of FrontierCode scores inherits this unverifiability.

**Status: CONFIRMED as a structural fact** (Cognition states the dataset stays private; access on request for model developers). The open question is whether the evals shops that report it (Epoch AI tracks FrontierCode) independently re-run or merely relay. Per the bench-audit targeting rule, the evals-shop representation is where an audit finding lands, since the harness itself is out of reach.

**Falsifier:** Cognition releases a public validation subset with rubrics, or a third party with access publishes an independent re-grade.

## F4: A correct diff with a varied description changes the merge outcome

**Prediction:** Holding the patch fixed and varying only the PR description (reasoning present vs absent, motivation accurate vs overclaimed) changes real-maintainer outcomes. Sweep already contains natural experiments (ruff#25066 closed on summary reasoning; H16 overclaimed-motivation tax; H17 description-treatment arm). A designed version is the buildable perturbation this audit should lead with.

**Status: NATURAL EXPERIMENTS SUPPORT IT; designed test not run.** Note H17's 12-PR treatment arm outcomes need to be pulled from sweep before designing anything new.

**Falsifier:** Fixed-diff description variation produces no measurable outcome difference across n≥20 paired PRs. Then the description axis is decorative and FrontierCode's omission of it is harmless.

## Causal chain

```
F3 (private dataset) → audit surface = disclosures + leaderboard
F1 (axis mapping)   → quantifies the gap F0 claims
F2 (harness confound) → attacks the leaderboard's model attribution
F4 (fixed-diff perturbation) → the receipt: diff constant, outcome moves
```

F4 is the headliner. F1 is the cheapest next step (all inputs on local disk).
