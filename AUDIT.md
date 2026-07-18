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
| 7 | read what assertions pin | partially (disclosed grading methods + 1 public task) | F6, F7 |
| 8 | probe spec determinacy | partially (1 public task) | F6, F7 |
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

## F7: Grader-wise and task-wise desk read (checks 7–8 on the public surface)

**Surface:** the announcement's grading-methods disclosure and the single public example task (LOG_WARNING on the jsonschema C++ repo, embedded in the announcement and leaderboard pages; pinned in sources/). n=1 task; every claim below is bounded by that.

**Grader-wise: three of six methods have an LLM inside the oracle.**

| Method | Oracle | Deterministic? |
|--------|--------|----------------|
| classical (injected tests) | fixed tests | yes |
| command (build/lint) | exit code | yes |
| reverse-classical (agent tests fail on base) | fixed procedure | yes |
| adaptive classical (mutagent) | LLM-adapted tests per submission | no — adaptation is an LLM judgment |
| scope: files/size | boundaries | yes |
| scope: semantic + code-quality prompt | LLM judge vs threshold | no |

The mutagent description claims the resolution both ways: an LLM "surgically patch[es] the test environment (or the application code) to align with the agent's implementation details, allowing us to run rigorous, deterministic tests." Determinism after adaptation does not make the adaptation deterministic — each submission is graded against a machine-generated variant of the oracle that no one else sees, stacked on mean@5 sampling variance. And "or the application code" is a frame-clause risk: a tool authorized to patch application code to make tests align with the implementation is structurally adjacent to masking a regression; whether it is guarded is undisclosed. Their own QC section concedes the asymmetry: "Hardening prompt-based criteria is a much harder QC problem."

**Blocker semantics amplify single-criterion defects.** Any failed blocker zeroes the run. So one plural or misdetermined blocker doesn't perturb a score, it deletes it — the headline number's sensitivity to a single bad criterion is total. v1.1 relaxing 75 "overly strict" criteria and auditing blocker criteria specifically is their own measurement of this exposure.

**A stochastic oracle caps the leaderboard's meaningful precision, and the board is ranked inside that cap.** An LLM-judged criterion can flip verdicts across identical reruns; through blocker-zeroing, one flip moves a task's score between zero and its full aggregate, so grader noise is amplified, not averaged. mean@5 samples the agent's variance, not the grader's — nothing published bounds oracle noise, no inter-run agreement, no per-model confidence interval. Yet the board reports to 0.1pp and ranks on it: positions 4 through 7 (GPT-5.5 43.0, Sonnet 5 42.7, Grok 4.5 42.4, SWE-1.7 42.3) span 0.7pp across four models — with their own SWE-1.7 in the cluster, its rank and cost-per-point story riding on gaps plausibly inside grader noise. The instrument's disclosed mechanics cannot support the resolution its leaderboard displays. This also compounds F6: a deterministic grader's verdict could at least be reproduced by anyone granted access; a stochastic oracle's verdicts are unreproducible even in principle without published seeds, judge versions, and adaptation logs, none of which exist publicly.

**Falsifier for the precision claim:** published grader-side variance (rerun the judge ensemble on fixed submissions, report per-criterion flip rates and per-model score CIs) showing adjacent-rank gaps exceed the noise floor.

**The oracle model is undisclosed.** Every mention across the announcement, v1.1 post, and leaderboard page says only "an LLM" — mutagent "uses an LLM," "An LLM reviews agent's diff," "LLM-based checks," "we prefer LLM grading." No model name, no version, no pinning, on any page (verified by grep over the pinned snapshots). Three consequences, ascending: (1) *version drift* — an unpinned judge silently changes the instrument whenever it updates, so entries graded at different times are scored by different oracles and the board's cross-time comparisons are unanchored; (2) *family bias* — LLM judges measurably favor outputs stylistically close to their own family, so an unnamed judge is an unnamed prior over the leaderboard, material when ranks 4–7 sit 0.7pp apart; (3) *self-judging* — if the grading LLM is a model that appears on the board, or a Devin/SWE-line relative, the vendor's undisclosed judge is scoring the vendor's disclosed competitor rankings; they disclose using Devin for rubric hack-testing, which makes the silence on the grading model conspicuous rather than incidental. A benchmark whose oracle is a secret model is unauditable one level deeper than F6: outsiders cannot retrieve the verdicts, and could not name the instrument that produced them even if they could.

**The acceptable configuration (operator's standard, 2026-07-17):** a tagged open-weights model as oracle — named, version-pinned by checkpoint hash, weights public. Disclosure alone is necessary but insufficient: a named closed-API judge still drifts with provider updates and dies with endpoint deprecation, so its verdicts stop being re-derivable the day the endpoint sunsets. A pinned open-weights judge makes the oracle itself a receipt: anyone can rerun the judgment forever, which is the only configuration under which "make every claim a rerun" holds for LLM-graded criteria. The honest trade to price: public judge weights are an optimizable target (agents can Goodhart against the judge directly), the same argument they make for private tasks. But judge-gaming is bounded by the deterministic criteria surrounding it (classical, reverse-classical, build, scope-files/size) and is detectable by exactly the rerun the open judge enables — whereas a secret judge fails the science register unconditionally. Contamination arguments protect tasks; nothing analogous justifies a secret ruler.

**Falsifier:** a disclosure naming the judge model(s) and version(s) per criterion class with a pinned-version policy per leaderboard revision — fully cured only if the judge is open-weights and hash-pinned — plus an ablation showing rank stability across judge families.

**Task-wise: full rubric retrieved (2026-07-17, sources/frontiercode-demo-task-rubric-2026-07-17.md), and the determinacy read sharpened.** The interactive demo exposes all 10 criteria for the one public task. Opus 4.8's run: 8/10 met, verdict "Fail · 24%". The only two failures are the two multi-line-warning criteria, and both are BLOCKERS — the entire fail verdict rests on the idiom question.

The determinacy classification is now stronger than "plural"; the literal reading favors the failed solution. The brief's operative sentence is "Use this new function in every instance of warning: <message> messages." A continuation line of a multi-line warning carries no "warning:" prefix — it is not an instance of "warning: <message>" — and Opus converted every line that is. The graded requirement (route whole multi-line messages through one chained call) is derivable only from the intent reading of "Encapsulate all warning logs," which the brief's own operationalization sentence then narrows. The prose licenses both readings; the blocker silently pins the one the agent-facing sentence disfavors. Their commentary concedes the solutions are "behaviorally the same" and defends the fail on future-modification taste that appears nowhere in the brief. On the only task outsiders can inspect, 2 of 10 criteria are in the plural class, both at blocker severity, and they alone flip pass to fail. Bounded claim: n=1 task, and a maintainer might genuinely hard-stop on this; the sin is that the brief doesn't say so and the announcement narrates the resulting failure as "models fail this task in a somewhat surprising way" — underdetermination read as model weakness, the exact ambiguity F5 predicts.

**Score-presentation wrinkle from the same retrieval:** the demo displays "Fail · 24%" for a blocker-failing run while the announcement and the leaderboard's own score note say blocker failure yields zero ("Solutions that don't pass blocking criteria receive 0"). Display-only aggregate vs scored zero — two numbers for one run, unlabeled.

**F2 receipt from their own page:** the chart legend reads "marker shapes distinguish harnesses" — the board itself admits mixed harnesses per entry; this no longer rests on Epoch's description alone.

**Credits from the same read (recorded per the come-back-empty rule):** reverse-classical is a genuinely good deterministic invention; the hack-report step (author plays adversary, both false-positive and false-negative directions) is real QC; four-solution calibration targeting a 0–100 score range is resolution discipline most benchmarks lack; researchers solving a random subset themselves is a partial answer to F5's human-baseline demand (partial: a random subset, solvability-checking, not a per-task completed baseline).

**Falsifiers:** For the mutagent claim: published adaptation logs showing per-submission adaptations are semantically equivalence-preserving, or an ablation showing grade agreement between adapted and unadapted runs. For the task-wise claim: a disclosed rubric line for this task that does pin the multi-line idiom in text the agent sees (the interactive demo's rubric may contain it — retrieval pending). For the LLM-judge concern: reported inter-run agreement on prompt-based criteria across the mean@5 samples.

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

  **Endgame: a cheap oracle stops being a benchmark and becomes a linter.** If the oracle is open and cheap to run, the obvious move is to build its generic form into the agent harness and iterate until pass — every objection the rubric would raise gets resolved pre-submission, and what ships is the higher-quality PR. This is the measured result, not speculation: the 43% → 91% slop-slope delta IS iterating against an oracle until it passes, and the generic axes are already harness-buildable (reverse-classical is the TDD red-green gate, scope checks and build/lint are deterministic, the quality prompt is an adversarial review round). "Supposedly" higher quality is carried by the deterministic criteria plus a cross-family judge; iterating against a same-family LLM judge alone is Goodhart. Two consequences. For the field: the benchmark's endpoint is the `-Wall` → `-Werror` floor dynamic — a checkable merge standard belongs in every sender's harness, saturates, and stops discriminating, which is success, not failure ("the bench is the stepping stone"). For the audit: this names the structural reason the oracle stays secret. A public, runnable oracle is a free product feature; a private one is a rankable rent. Task privacy has a contamination defense; oracle privacy has a business model.
- **Right of reply.** Findings go to Cognition before or as anything publishes; the actionable part files where they work.
- **Audit yourself first.** The sweep closure taxonomy is our instrument; F1 must re-verify closure classifications against the underlying PR threads, not trust the graph's labels (the graph itself records reclassifications: ruff#25066, llama.cpp#22873, jellyfin-tui ×3 all moved categories on review).
