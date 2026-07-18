# frontiercode-audit

An audit of FrontierCode (Cognition, launched 2026-06-08). Main critique: the benchmark grades the diff, and the diff is insufficient to answer the question the benchmark claims to answer.

## The claim under audit

FrontierCode's pitch is "would a maintainer actually merge this PR?" Its grading inputs are the patch alone: held-out tests plus a maintainer-authored rubric over six axes (behavioral correctness, regression safety, mechanical cleanliness, test correctness, scope, code quality). No PR description, no commit messages, no review interaction. Source: https://cognition.com/blog/frontier-code

## The critique

Merge decisions in the wild are made about contributions, and the diff is a minority input to them. Two independent bodies of evidence, both ours, both public:

1. **Field data** (sweep pipeline, ~180 resolved PRs to real maintainers, closure taxonomy in [kimjune01/sweep/HYPOTHESIS_GRAPH.md](https://github.com/kimjune01/sweep/blob/master/HYPOTHESIS_GRAPH.md)): roughly 70% of closures trace to non-diff causes. Description reasoning (ruff#25066: "summary doesn't explain why decisions were made"), interaction cadence (cucumber/gherkin: "I don't get the impression there is a human in the loop" on a correct patch), template and policy compliance (the #1 pipeline error category), contributor standing. QA found zero bugs on 4 of 5 PRs maintainers labeled "AI slop."

2. **Lab data** ([does-iteration-mitigate-slop-slope](https://june.kim/does-iteration-mitigate-slop-slope)): an adversarial review loop takes one-shot diff quality from 43% to 91% reviewer approval on the same code. Diff quality is the solvable part. The residual gap between the 91% lab rate and the ~80–84% adjusted field rate is "fits our project," which no diff-only rubric scores.

So FrontierCode measures mergeable diffs while claiming mergeable PRs, and the axes it omits are the ones our data shows dominate observed rejections.

## What FrontierCode gets right

The audit records credits with the same discipline as defects. On current reading:

- **The diagnosis of SWE-bench is correct and ours agrees.** Autotest-only grading passes patches a maintainer would reject. Their false-positive critique and our slop-slope finding point at the same defect from opposite sides.
- **Decay resistance (check 10).** The private dataset is a real anti-contamination credit, the same move MirrorCode earned credit for leaving out ungradeable targets. The scored artifact cannot be memorized from the public web. The critique is the unpriced trade: privacy buys decay resistance at the cost of falsifiability, and the announcement prices only one side.
- **Maintainer-authored rubrics are a construct upgrade.** Moving the oracle from incidental autotests toward the people who actually decide merges is the right direction; the audit's claim is that it stopped partway (at the diff).
- **Adaptive grading.** Accepting valid alternative implementations addresses the overly-specific-test failure mode directly (the plural class from the determinacy audit).
- **A live internal audit loop.** v1.1 audited 1,000+ criteria and relaxed 75. That the loop exists is a credit; that only insiders can run it is F6.
- **Terse prompts.** Expecting the agent to infer intent from a humanlike problem statement matches how contribution actually works (goal transmission over ticket compliance).

## Scope boundary (the honest counter)

FrontierCode's Diamond subset is much harder than sweep's sweet spot (median 40-line boring fixes). In their regime, diff correctness plausibly is the binding constraint; in ours, the social and communication layer binds. The audit's claim is about the benchmark's stated question, which is about merging, a decision that regime difference does not hand back to the diff.

## Audit surface

The dataset is private. Cognition grants evaluation access to model developers on request and does not publish tasks. The auditable surface is therefore their published methodology, grading-criteria disclosures (v1.1: 1,000+ criteria audited, 75 relaxed), and leaderboard claims. The 81% SWE-bench false-positive figure is theirs and unverifiable from outside.

## Files

- [HYPOTHESIS_GRAPH.md](./HYPOTHESIS_GRAPH.md) — pre-registered audit hypotheses with falsifiers
- [EVIDENCE.md](./EVIDENCE.md) — receipts: our data, their disclosures, third-party commentary
- [worklog/WORK_LOG.md](./worklog/WORK_LOG.md) — dated session log
