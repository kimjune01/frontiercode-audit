# FrontierCode Audit (2026-07-17)

This is a desk audit: it reads FrontierCode's own disclosures against receipts that already exist (sweep's field record, the slop-slope lab result). No new experiments are run. Each hypothesis is a claim about what FrontierCode measures versus what it says it measures. Numbering is F-prefixed to avoid colliding with sweep's H-series. Sin taxonomy and check numbers reference [How to Audit a Benchmark](https://june.kim/how-to-audit-a-benchmark).

## Checklist coverage

Of the 11 checks, the private dataset blocks 6. That asymmetry is itself a finding (F6).

| # | check | runnable? | maps to |
|---|-------|-----------|---------|
| 1 | claim vs construct | yes (their blog) | F0 |
| 2 | paper vs repo | no repo to read | blocked |
| 3 | task selection | partially (construction section) | F5 |
| 4 | recompute headline | partially (leaderboard only) | F2 |
| 5 | retrieve one receipt | fails structurally | F6 |
| 6 | run the answer key | blocked (no golds shipped) | F6 |
| 7 | read what assertions pin | blocked (no tests shipped) | F6 |
| 8 | probe spec determinacy | blocked | F6 |
| 9 | mutate gold and regrade | blocked | F6 |
| 10 | what survives publication | yes (privacy helps here; credit to record) | — |
| 11 | audit the run | only for runs we execute ourselves | future |

## F0: The diff is insufficient to predict a merge decision

**Sin class: claim (check 1).** The audit's mode is construct-validity calibration: how well does their instrument measure what they claim to measure? The equivocation is quotable and now verbatim-pinned (EVIDENCE.md, sources/): the claimed construct is mergeability ("We're the first benchmark to measure code mergeability") in service of an automation goalpost ("as AI-generated code becomes the dominant path to production") and a certification role ("trust it to evaluate the production readiness of their strongest models"). The methods grade the patch against tests and a rubric whose blockers are "criteria that a maintainer would consider hard stops during code review," all diff-properties. The grader can be sound on what it checks while what it checks is not the claim.

**Decision rule applied (2026-07-17):** if their stated goal were only model discrimination, the critique would be dropped. Verified against raw HTML: it is not. The ranking claim is derived from the construct claim ("81% less misclassification errors... This means that FrontierCode scores are the most accurate ranking"), so the model-discrimination defense is a retreat that abandons their own conclusion paragraph. The rebuttal anticipated earlier is closed off by their own text.

**Prediction:** Among real-maintainer PR closures where the diff clears an adversarial quality gate, a majority cite non-diff causes (description reasoning, interaction cadence, policy/template compliance, contributor standing). A benchmark that grades only the diff therefore explains a minority of the variance in the outcome it claims to measure.

**Status: SUPPORTED by existing data, not yet framed against FrontierCode directly.** Sweep closure taxonomy (as of retro 13, 2026-05-13): pipeline errors 17, credence tests 7, external 10, AI detection via description 1. QA found zero bugs on 4/5 "AI slop"-labeled PRs; 80% of slop rejections were policy-based. Lab side: iteration lifts one-shot diff approval 43% → 91%; field adjusted approval ~80–84%; the residue is the social layer.

**Falsifier:** A dataset of real closures where diff-quality causes dominate after honest reclassification. Or: demonstration that at Diamond-task difficulty, diff correctness failures preempt the social layer entirely (the maintainer never reaches the description because the patch is wrong).

## F1: FrontierCode's rubric axes and real rejection causes are near-disjoint

**Prediction:** Mapping each sweep closure to FrontierCode's six rubric axes leaves the majority of closures unmapped. The unmapped set clusters on axes FrontierCode does not score: description, cadence, compliance, standing.

**Status: NOT YET RUN.** The mapping exercise is the audit's first concrete deliverable. Input: sweep closure records. Output: a two-column table, closure → rubric axis or NONE.

**Falsifier:** ≥50% of closures map cleanly onto the six axes. Then the critique narrows from "the diff is insufficient" to "the diff is insufficient at the margin," a weaker claim.

## F2: Score depends on harness as much as model

**Sin class: score (check 4).** A board that ranks model-and-harness pairs while presenting itself as ranking models is comparing configurations, and the reader can't tell.

**Prediction:** FrontierCode leaderboard deltas between models are confounded by agent-harness differences.

**Status: CONFIRMED (2026-07-17), directly from Epoch's methodology note.** Verbatim: "Models are run through agent harnesses such as Claude Code, Codex, Gemini CLI, mini-SWE-agent, and Devin; we keep each model's harness and best-performing reasoning effort in the data export." Different models ran under different harnesses, so leaderboard deltas mix model and harness by construction, and "best-performing reasoning effort" adds a max-over-configs selection on top. The board ranks (model, harness, effort) triples and presents the ranking as models.

**Self-audit note:** the earlier supporting argument (our 43% → 91% loop result exceeds the Diamond spread) is cross-regime — refactoring approval on easy tasks vs hard-task completion — and should be used as motivation, never as proof. The confirmed receipt above replaces it as the load-bearing evidence.

**Falsifier (for the residual claim that the confound is material):** Cognition publishes per-harness ablations showing inter-model gaps stable across harnesses, with variance bounds from the mean@5 runs.

## F3: RETIRED, merged into F6 (2026-07-17)

The narrow claim (the 81% figure is externally unverifiable) is a special case of F6's structural unauditability, and its open question (does Epoch re-run or relay?) is resolved: relay, verbatim "We source results from Cognition's public FrontierCode data." Kept as a numbered stub so cross-references stay valid.

## F4: A correct diff with a varied description changes the merge outcome

**Claim:** Holding the patch fixed and varying only the PR description (reasoning present vs absent, motivation accurate vs overclaimed) changes real-maintainer outcomes. This is a desk audit; no new experiments will be run. The claim rests on natural experiments already in the sweep record: ruff#25066 closed on summary reasoning with the code unchanged; H16's overclaimed-motivation tax; H17's 12-PR description-treatment arm (outcomes to be pulled from sweep, which is a records lookup, not a new run).

**Falsifier:** The existing record, honestly reread, shows description variation with no outcome difference — the treatment arm indistinguishable from control, the ruff closure reclassified to a diff cause. Then the description axis is decorative and FrontierCode's omission of it is harmless.

## F5: Diamond selection-by-failure makes the 13.4% floor ambiguous

**Sin class: selection (check 3).** Diamond is defined as the "hardest 50" tasks. If hardest means "fewest models pass," the subset selects on failure, and failure enriches for defective tasks: an underdetermined spec, an unsolvable subtask, and a mis-keyed rubric criterion all present identically as "no model passes." A near-zero headline is then ambiguous between hard and defective.

**Prediction:** The construction section does not demonstrate each Diamond task solvable by someone other than its author (a completed human baseline, not maintainer attestation). Their own v1.1 relaxation of 75 overly strict criteria is direct evidence that defects shipped in the graded rubric and were caught only by internal audit after launch.

**Evidence to pull:** how "hardest" is operationalized in the announcement; whether any human-completion baseline exists; the v1.1 changelog.

**Update (2026-07-17):** v1.1 deprecates the Diamond subset outright (leaderboard page, verbatim: "audits blocker criteria and deprecates the Diamond subset"). They killed the subset the launch press cycle quoted. This strengthens F5 (the subset was defective enough to retire) and adds a score-sin wrinkle: the circulating 13.4%/6.3%/4.7% numbers cite a deprecated subset, and secondary leaderboards still relay them.

**Falsifier:** Cognition discloses that Diamond ranking is difficulty-rated by maintainers independent of model pass rates, plus a completed human baseline per task. Attestation alone does not clear it: every failing gold in the seven prior audits shipped from a benchmark whose authors believed it correct.

## F6: The benchmark is structurally unauditable at the verdict level

**Sin classes: gold, oracle, spec, frame (checks 5–9), all unrunnable externally.** Check 5 fails by design: no scored trial's artifact is publicly retrievable, so every verdict is a promise. Check 6, the highest-yield dollar in benchmark auditing, which caught shipped gold defects in 3 of 3 benchmarks it was pointed at, cannot be run by anyone outside the access program. The base rate says some Diamond golds or criteria are defective; the design says nobody outside can find them, and v1.1's 75 relaxed criteria confirm the class is populated.

**Prediction: CONFIRMED (2026-07-17).** Evals shops citing FrontierCode relay Cognition's numbers rather than re-derive them; Epoch's methodology states it verbatim: "We source results from Cognition's public FrontierCode data." Worse, Epoch's chart reports the Diamond subset, which v1.1 deprecated — the relay is not only unverified but stale. The score's public life rests entirely on producer say-so. Per check 5's companion: a producer's commercial relationships are not conflicts when the artifact is marketing, and each is a conflict when the artifact is asked to be science. Cognition sells SWE-1.7 against this leaderboard.

**COI, now receipted (2026-07-17):** No conflict-of-interest statement exists on any primary page. The SWE-1.7 launch post scores their own model on FrontierCode 1.1 Main (42.3%, third overall) in a results table alongside third-party benchmarks, with no ownership note, and states the alignment outright: "formulating and refining principles for good agentic software engineering both in evaluation, with FrontierCode 1, 2, and now in training, with SWE-1.7." The grader and the graded model are built on the same principles by the same team, the verdicts are unretrievable by outsiders, and no disclosure marks any of it.

**Falsifier:** A public validation subset with retrievable per-trial receipts, or a third party with access publishing an independent re-grade with receipts. F6 subsumes the old F3 claim about the 81% figure; F3 stays as the narrow version.

## Causal chain

```
F3/F6 (private dataset) → audit surface = disclosures + leaderboard
F1 (axis mapping)   → quantifies the gap F0 claims
F5 (selection by failure) → 13.4% floor ambiguous between hard and defective
F2 (harness confound) → attacks the leaderboard's model attribution
F4 (description moves outcomes) → the receipt from the existing record: diff constant, outcome moves
```

F1 is the cheapest next step (all inputs on local disk). F4's evidence is already in the sweep record; pulling it is a lookup, not a run.

## Auditor's contract (standing constraints from the methodology post)

- **Bound the claim.** F0 is a construct-validity gap, not fraud; the grader may be sound on what it checks.
- **Report floors, never rates.** F1's output is "at least N of M closures map to no rubric axis," never a percentage of all merge decisions.
- **Build it to come back empty.** If F1 maps ≥50% of closures onto the six axes, say so; that narrows the critique and the audit still worked.
- **Name the cure.** Framing: **ecological accuracy**. Ecological validity is the classical question (does the instrument measure the construct as it occurs in the wild?); ecological accuracy is its quantitative upgrade: report the agreement rate between the instrument's verdict and the field outcome. FrontierCode claims to measure mergeability, and ground truth for "would merge" exists in bulk in git history, so the claim is checkable as a number — take PRs whose merge outcomes are known, run the grader, report agreement. A benchmark that never reports this number has skipped its own criterion-validity check; one that reports it converts "trust it" into a calibration curve.

  Direct recommendation: **grade the hypothesis graph**, per [The Hypothesis Graph: A Verifiable Semantic Memory for Coding Agents](https://june.kim/the-hypothesis-graph-semantic-memory-methodeutics) ([PDF](https://june.kim/assets/the-hypothesis-graph-semantic-memory-methodeutics.pdf)). The axes FrontierCode omits (description reasoning, why-this-fix, interaction) are what a maintainer reviews when deciding to merge, and the paper's contribution is that this reasoning can be a gradeable artifact: typed claims, recorded trials, refutation edges, replayable by an independent party. A diff-only rubric grades the patch and discards the reasoning; a benchmark that scored the submitted hypothesis graph alongside the patch would measure the contribution a maintainer actually evaluates, with verification mechanical (rerun the recorded trials) rather than LLM-judged. Mergeability is at bottom a claim about reviewability, and the graph is the reviewable form of agent work.

  Supporting form: the detector from [(Issue) → PR](https://june.kim/issue-to-pr), a classifier trained on a repo's merge/close corpus that predicts the construct directly and recalibrates as the repo's floor moves, where a frozen rubric encodes one snapshot of 20 maintainers' taste. Hold the cure to the audit's own standard: the detector's ecological accuracy needs receipts (survivorship, per-repo sample sizes), and any agreement number demanded from Cognition needs a contamination bound (outcome-known PRs leak into training data; the clean set is post-cutoff merges).
- **Right of reply.** Findings go to Cognition before or as anything publishes; the actionable part files where they work.
- **Audit yourself first.** The sweep closure taxonomy is our instrument; F1 must re-verify closure classifications against the underlying PR threads, not trust the graph's labels (the graph itself records reclassifications: ruff#25066, llama.cpp#22873, jellyfin-tui ×3 all moved categories on review).
