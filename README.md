# frontiercode-audit

An audit of FrontierCode (Cognition, launched 2026-06-08). Main critique: the benchmark grades the diff, and the diff is insufficient to answer the question the benchmark claims to answer.

## The claim under audit

Verbatim, from the announcement (snapshots in `sources/`): "We're the first benchmark to measure code mergeability," asked as "Would the maintainer actually merge this PR?", offered so that "developers, enterprises, and researchers can trust it to evaluate the production readiness of their strongest models," in a world where "AI-generated code becomes the dominant path to production." That is a construct claim in the automation-goalpost register, not a model-discrimination claim; their ranking claim is explicitly derived from it ("81% less misclassification errors... This means that FrontierCode scores are the most accurate ranking currently available").

The grading inputs are the patch alone: held-out tests plus a maintainer-authored rubric whose blockers are "criteria that a maintainer would consider hard stops during code review," scored mean@5 with any failed blocker zeroing the run. No PR description, no commit messages, no review interaction. The audit's mode is construct-validity calibration: how well does this instrument measure what that claim names?

## The critique

Merge decisions in the wild are made about contributions, and the diff is a minority input to them. The audit measured this (F1, 2026-07-18): all 98 closed-unmerged PRs from the sweep pipeline's population were re-coded from their GitHub threads by two independent coders from different model families. **At least 52 of 59 confidently-coded maintainer closures (88%) were decided by causes outside FrontierCode's six rubric axes**; both coders found a diff-quality cause decisive in exactly 3. The non-diff majority splits into a social layer (AI identity 15, policy/template compliance 8, cadence, standing, description reasoning — scrapy's closer: "Closing without a review"; ruff#25066: "the summary doesn't explain why certain decisions were made") and an ecosystem layer (superseded by maintainer fix 16, duplicate 4, stale 2). Four "closed" PRs turned out to be accepted work — cherry-picked or ported with credit — so the closed/merged binary itself mislabels outcomes in both directions. Receipts: `f1/` in this repo.

Supporting lab result ([does-iteration-mitigate-slop-slope](https://june.kim/does-iteration-mitigate-slop-slope)): an adversarial review loop takes one-shot diff quality from 43% to 91% reviewer approval on the same code. Diff quality is the solvable part; what remains between a clean diff and a merge is the layer FrontierCode doesn't grade.

Stated bound: the F1 population is one AI-assisted contributor's PRs during a public campaign, which enriches AI-identity closures; and at FrontierCode's task difficulty, diff correctness may bind before the social layer is reached. The floor is a property of this population. What it establishes is that a population exists — and a growing class of contributors resembles it — where a diff-only grader predicts the wrong outcome dimension in 9 of 10 closures.

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
2. **F0 + F1, the construct gap, now measured.** The claimed construct is the merge decision; the measured construct is diff quality; and on the audited population the gap is a dual-coder floor: 52 of 59 maintainer closures decided outside the rubric axes, 3 decided on them. The model-discrimination retreat is closed by their own text.
3. **F5 + score hygiene.** Diamond, the subset the launch press quoted, was deprecated by v1.1; Epoch still charts it, at each model's best-performing reasoning effort (max-over-configs). Two headline statistics circulate for the same runs (rubric score and a binary pass rate Epoch declines to show).
4. **F2, harness confound, confirmed.** Models ran under different harnesses (Claude Code, Codex, Gemini CLI, mini-SWE-agent, Devin per Epoch's export), so the board ranks (model, harness, effort) triples presented as models.
5. **F4, existence confirmed; causal upgrade returned null.** Maintainers demonstrably decide on non-diff information (ruff#25066, cucumber/gherkin, scrapy). The H17 description-treatment arm, pulled and read, does not show description variation moving merge outcomes: 1 of 12 merged, 7 of 12 drew zero maintainer attention — in that arm the binding variable was attention, not description content. The existence claim stands; the causal claim stays unproven and is reported as such.

## Audit surface

The dataset is private. Cognition grants evaluation access to model developers on request and does not publish tasks. The auditable surface is their published methodology, grading-criteria disclosures (v1.1: 1,000+ criteria audited, 75 relaxed, Diamond deprecated), leaderboard claims, and the relay chain through evals shops. The 81% SWE-bench false-positive figure is theirs and unverifiable from outside.

## Files

- [AUDIT.md](./AUDIT.md) — audit hypotheses with falsifiers and the recommendation
- [EVIDENCE.md](./EVIDENCE.md) — receipts: our data, their disclosures, third-party commentary
- [worklog/WORK_LOG.md](./worklog/WORK_LOG.md) — dated session log
