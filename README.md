# frontiercode-audit

An audit of FrontierCode (Cognition, launched 2026-06-08). Main critique: the benchmark grades the diff, and the diff is insufficient to answer the question the benchmark claims to answer.

## The claim under audit

Verbatim, from the announcement (snapshots in `sources/`): "We're the first benchmark to measure code mergeability," asked as "Would the maintainer actually merge this PR?", offered so that "developers, enterprises, and researchers can trust it to evaluate the production readiness of their strongest models," in a world where "AI-generated code becomes the dominant path to production." That is a construct claim in the automation-goalpost register, not a model-discrimination claim; their ranking claim is explicitly derived from it ("81% less misclassification errors... This means that FrontierCode scores are the most accurate ranking currently available").

The grading inputs are the patch alone: held-out tests plus a maintainer-authored rubric whose blockers are "criteria that a maintainer would consider hard stops during code review," scored mean@5 with any failed blocker zeroing the run. No PR description, no commit messages, no review interaction. The audit's mode is construct-validity calibration: how well does this instrument measure what that claim names?

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

## Findings so far, ranked

1. **F6 + COI, the strongest and fully receipted.** The dataset is private, verdicts are unretrievable, and 6 of the methodology's 11 checks are structurally blocked — every public score rests on producer say-so. Epoch confirms it relays rather than re-runs ("We source results from Cognition's public FrontierCode data"). Meanwhile Cognition trains SWE-1.7 "on the same principles" as the benchmark, ranks it third on its own board, and discloses no conflict anywhere. The two absences compound: disclosure prices a conflict, but the reader can only act on that price when receipts exist. A disclosed conflict on a falsifiable artifact is normal science; an undisclosed conflict on an unfalsifiable one prices the number at zero in the register they chose ("researchers can trust it").
2. **F0, the construct gap.** The claimed construct is the merge decision; the measured construct is diff quality. Sweep's field data (closure taxonomy) and the slop-slope lab result locate the decisive axes — description, cadence, compliance, standing — outside the rubric. The model-discrimination retreat is closed by their own text.
3. **F5 + score hygiene.** Diamond, the subset the launch press quoted, was deprecated by v1.1; Epoch still charts it, at each model's best-performing reasoning effort (max-over-configs). Two headline statistics circulate for the same runs (rubric score and a binary pass rate Epoch declines to show).
4. **F2, harness confound, confirmed.** Models ran under different harnesses (Claude Code, Codex, Gemini CLI, mini-SWE-agent, Devin per Epoch's export), so the board ranks (model, harness, effort) triples presented as models.
5. **F4, the receipt to build.** Fixed diff, varied description, real maintainer outcomes: the one perturbation that tests the construct gap directly. Sweep's natural experiments support it; the designed version is pending.

## Audit surface

The dataset is private. Cognition grants evaluation access to model developers on request and does not publish tasks. The auditable surface is their published methodology, grading-criteria disclosures (v1.1: 1,000+ criteria audited, 75 relaxed, Diamond deprecated), leaderboard claims, and the relay chain through evals shops. The 81% SWE-bench false-positive figure is theirs and unverifiable from outside.

## Files

- [PREREG.md](./PREREG.md) — pre-registered audit hypotheses with falsifiers
- [EVIDENCE.md](./EVIDENCE.md) — receipts: our data, their disclosures, third-party commentary
- [worklog/WORK_LOG.md](./worklog/WORK_LOG.md) — dated session log
