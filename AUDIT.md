# FrontierCode Audit (2026-07-17)

This is a desk audit: it reads FrontierCode's own disclosures against receipts that already exist (sweep's field record, the slop-slope lab result). No new experiments are run. Each hypothesis is a claim about what FrontierCode measures versus what it says it measures. Numbering is F-prefixed to avoid colliding with sweep's H-series. Sin taxonomy and check numbers reference [How to Audit a Benchmark](https://june.kim/how-to-audit-a-benchmark).

## The construct ladder

Five constructs the benchmark's language moves among, ordered from defensible to sold:

1. patch acceptability under review (normative patch quality)
2. PR mergeability in a named repository
3. probability of historical merge
4. production readiness
5. model capability ranking

Their methods measure (1). Their headline sells (2) ("measure code mergeability: would the maintainer actually merge this PR?"), their conclusion sells (4) ("trust it to evaluate the production readiness of their strongest models"), and their ranking claim sells (5) as derived from (2). The defensible retreat — "we mean a normative patch-quality judgment, not a prediction of merge outcomes" — is closed by their own conclusion paragraph: production readiness is a claim about outcomes. Every finding below states which rung it attacks.

## Checklist coverage

Of the 11 checks, the private dataset blocks or degrades 6. That asymmetry is itself a finding (F6).

| # | check | runnable? | maps to |
|---|-------|-----------|---------|
| 1 | claim vs construct | yes (their blog) | F0 |
| 2 | paper vs repo | no repo to read | blocked |
| 3 | task selection | partially (construction section) | F5 |
| 4 | recompute headline | partially (leaderboard only) | F2 |
| 5 | retrieve one receipt | fails publicly | F6 |
| 6 | run the answer key | blocked (no golds shipped) | F6 |
| 7 | read what assertions pin | partially (disclosed grading methods + 1 public task) | F7 |
| 8 | probe spec determinacy | partially (1 public task) | F7 |
| 9 | mutate gold and regrade | blocked | F6 |
| 10 | what survives publication | yes (privacy helps here; credit to record) | — |
| 11 | audit the run | only for runs we execute ourselves | future |

## F0: The diff under-measures the merge decision (claim clause, rung 2 vs rung 1)

**Sin class: claim (check 1).** The audit's mode is construct-validity calibration. The equivocation is quotable and verbatim-pinned (EVIDENCE.md, sources/): the claimed construct is mergeability in service of an automation goalpost ("as AI-generated code becomes the dominant path to production") and a certification role. The grading inputs are the patch alone; blockers are "criteria that a maintainer would consider hard stops during code review," all diff-properties. The grader can be sound on what it checks while what it checks is not the claim.

**Decision rule applied (2026-07-17):** if their stated goal were only model discrimination (rung 5 alone), the critique would be dropped. Verified against raw HTML: it is not. The ranking claim is derived from the construct claim ("81% less misclassification errors... This means that FrontierCode scores are the most accurate ranking"), so the model-discrimination defense abandons their own conclusion paragraph.

**What the existing evidence establishes:** maintainers reject technically clean PRs for information outside the diff. Sweep's record contains closures where QA found zero bugs (4 of 5 "AI slop"-labeled PRs) and where the stated closure reason was the description, the cadence, or policy compliance. The lab result (43% → 91% under iteration) shows diff quality is the improvable part; the field residue after diff quality saturates is the social layer.

**Magnitude (F1, run 2026-07-18):** on the audited population, at least 52 of 59 confidently-coded maintainer/bot closures were decided outside the rubric axes (two independent coders, 93% dichotomy agreement; full result in F1). **Status: SUPPORTED on the audited population** — existence (F4) plus prevalence floor (F1); the bound is the population, one AI-assisted contributor's 98 closures, not open source generally. The remaining open flank is their regime: at Diamond-level difficulty, diff-correctness failures may preempt the social layer.

**Falsifier:** a comparable coding exercise on a different contributor population where the majority of closures are diff-axis-decided.

## F1: Axis mapping of real closures (RUN 2026-07-18; prediction CONFIRMED)

**Prediction:** Mapping sweep closures to FrontierCode's rubric axes leaves the majority unmapped, clustering on description, cadence, compliance, standing.

**Protocol:** For each closure, from the PR thread itself (never sweep's own labels), record: closer identity; verbatim stated reason; any asserted code fault; every rubric axis the cause maps to (multi-label); diff-observability; the primary independently-sufficient cause; confidence. Second coder from a different model family (codex/GPT-5.5) codes blind from the verbatim quotes; disagreements reported, not resolved. Outputs are floors. Records: `f1/coded/all.jsonl`, `f1/second_coder_raw.txt`, `f1/PROTOCOL.md`.

**Population and coverage:** all 98 closed-unmerged PRs by kimjune01 since the pipeline epoch (2026-05-09), alongside 88 merged and 103 open at pull time. Closer: 57 maintainer, 8 bot, 33 self. Self-closes excluded from the maintainer-decision denominator. Of the 65 maintainer/bot closures, 6 were silent with no observable mechanism (unknown bucket, out of the floor), leaving 59 confidently coded.

**Result:** **At least 52 of 59 confidently-coded maintainer/bot closures (88%) were decided by causes outside FrontierCode's six rubric axes**, where "at least" means both coders independently agreed on the non-diff side. Both coders agreed the closure was diff-axis-decided in exactly 3 cases (crashappsec/chalk#667, mgree/ffs#146, dhonus/jellyfin-tui#192); 4 cases split across the dichotomy (tracy#1359, lightning#4741, shelley#208, dapr#9924). Inter-coder agreement: 55/59 (93%) on the diff-vs-non-diff dichotomy, 37/59 on exact labels (disagreements are mostly adjacent non-diff categories: ai_identity vs standing vs policy_compliance).

**Decomposition of the 53 primary-coder non-diff closures:** superseded-or-maintainer-fix 16, ai_identity 15, policy_compliance 8, duplicate 4, other 4 (including a 21-second cross-repo batch-close cluster at pallets, incompatible with per-diff evaluation), interaction_cadence 2, stale 2, wrong_premise 1, standing 1. Two distinct non-diff families: the *social layer* (ai_identity, policy, cadence, standing, description ≈ 26) and the *ecosystem layer* (superseded, duplicate, stale ≈ 22) — a diff-only grader predicts neither, but only the first is about the contribution's communication; the second is about the world state the benchmark's frozen snapshot can't see.

**The ecological-accuracy kicker found in the data:** at least 4 "closed" PRs are actually accepted work — fish-shell#12754 cherry-picked to master preserving June Kim authorship, opensquilla#21 ported to dev "preserving your authorship," polars#27561's fix incorporated with credit, evebox#369 committed with co-author credit. The closed/merged binary itself mislabels outcomes in both directions, which any "would it merge" validation against raw GitHub outcomes must handle.

**Bounds, stated:** this is one contributor's PR population, AI-generated, mostly small fixes, in a period when the contributor's public campaign made ai_identity closures more likely than a typical contributor would face. The 88% floor is a property of this population, not of open source generally. What it settles is the existence and prevalence *here*: a grader scoring these 59 diffs on the six axes would have predicted the wrong outcome dimension in at least 52 cases.

**Falsifier (pre-registered ≥50% diff-axis-decided): did not trigger** — diff-axis-decided is 3/59 both-coder, 6/59 primary-coder.

## F2: Leaderboard attribution is confounded — model, harness, and effort vary jointly (score clause, rung 5)

**Sin class: score (check 4).**

**Status: confound CONFIRMED; materiality UNRESOLVED.** Epoch's methodology: "Models are run through agent harnesses such as Claude Code, Codex, Gemini CLI, mini-SWE-agent, and Devin; we keep each model's harness and best-performing reasoning effort in the data export." Cognition's own chart legend: "marker shapes distinguish harnesses." The board ranks (model, harness, effort) triples, selects each model's best-performing effort (max-over-configs), and presents the result as a model ranking. What is confirmed is the confound's existence; how much of any inter-model gap it explains is unresolved, and the falsifier below is about materiality. To verify from Epoch's data export: whether the headline comparisons pair unmatched harnesses (the export lists harness per entry).

**Self-audit note:** the earlier supporting argument (our 43% → 91% loop result exceeds the old Diamond spread) is cross-regime and is motivation only.

**Falsifier (materiality):** per-harness ablations showing inter-model gaps stable across harnesses, with variance bounds from the mean@5 runs.

## F3: RETIRED, merged into F6 (2026-07-17)

The narrow claim (the 81% figure is externally unverifiable) is a special case of F6, and its open question (does Epoch re-run or relay?) is resolved: relay, verbatim "We source results from Cognition's public FrontierCode data." Kept as a numbered stub so cross-references stay valid.

## F4: Maintainers decide on information absent from the diff (existence stage)

**Claim:** Real merge decisions use inputs a diff-only grader never sees. This is an existence claim, and F1's re-verified record establishes it beyond the original examples: ruff#25066 closed on the summary's missing reasoning; cucumber/gherkin rejected on interaction cadence with a fix path offered for the code; scrapy#7512 "Closing without a review" — the diff explicitly unevaluated.

**Causal upgrade: RETURNED NULL (H17 pulled 2026-07-18, f1/h17-outcomes.jsonl).** The 12-PR description-treatment arm does not show description variation moving outcomes: 1/12 merged, 4/12 closed, 7/12 open with zero maintainer touch. The treatment couldn't act because most PRs never got attention — the arm measures the attention bottleneck, not the description effect. One closure (chalk#667) engaged the experiment framing and judged the fix wrong, a partial disclosure-detection hit consistent with the graph exposing rather than coaxing. F4 therefore stands as existence only; no fixed-diff causal claim is made anywhere in this audit.

**Falsifier (for the existence claim):** the F1 record, reread, reclassifies the non-diff closures to diff causes. The dual-coder pass makes this concrete: it would require overturning 52 both-coder codes.

## F5: IF Diamond was selected on model failure, its floor is partly selection-induced (selection clause; UNRESOLVED)

**Sin class: selection (check 3).** Diamond was "the hardest 50" tasks. If hardest means "fewest models pass," the subset selects on failure, and failure enriches for defective tasks, because an underdetermined spec, an unsolvable subtask, and a mis-keyed criterion all present identically as "no model passes." The conditional is unresolved: how "hardest" was operationalized is not disclosed, and nothing public demonstrates each task solvable by someone other than its author (a completed human baseline, not attestation — the QC section's "researchers also solve the tasks themselves" covers a random subset, which is a partial answer).

**The stale-score point stands unconditionally:** v1.1 deprecated Diamond ("audits blocker criteria and deprecates the Diamond subset"), and the launch-cycle numbers (13.4%/6.3%/4.7%) still circulate — Epoch's chart reports the Diamond score today. Why Diamond was deprecated is not disclosed; the audit does not infer defectiveness from deprecation. Rubric-defect evidence lives in F7, where it has a receipt.

**Falsifier:** disclosure that Diamond ranking was difficulty-rated independent of model pass rates, plus completed human baselines.

## F6: The benchmark is not publicly auditable at the verdict level (gold/oracle/spec/frame clauses)

**Checks 5–9 cannot be run from released artifacts.** No scored trial's artifact is publicly retrievable: every public verdict is unsupported by a publicly retrievable receipt, and the public leaderboard cannot be re-derived from anything Cognition has released. Check 6 — run the answer key, which found shipped gold defects in all three benchmarks it was previously pointed at (DeepSWE 4/113, SWE-bench Pro 3/731, Terminal-Bench 2.1 6/89; a track record, not a base rate) — is closed to anyone outside the access program. Whether admitted model developers receive per-trial artifacts is undisclosed; the bounded claim is public unauditability, which is the claim that matters, because the benchmark's public life (the leaderboard, the press, the relay chain) is exactly the part no one can check. That the defect class is populated needs no inference: v1.1 relaxed 75 overly strict criteria — their internal audit found them after launch.

**Relay confirmed (2026-07-17):** Epoch sources results from "Cognition's public FrontierCode data," re-runs nothing, and charts the deprecated subset. The public score's chain of custody is producer say-so end to end.

**Falsifier:** a public validation subset with retrievable per-trial receipts, or a third party with access publishing an independent re-grade with receipts.

## Governance: undisclosed conflict of interest (standalone finding; raises the burden of transparency, not proof of biased scores)

The factual core, each item receipted: Cognition owns the benchmark. Cognition scores and markets its own model on it (SWE-1.7 at 42.3% on 1.1 Main, in a results table beside third-party benchmarks, no ownership note). The benchmark and the model's training share stated principles ("formulating and refining principles for good agentic software engineering both in evaluation, with FrontierCode 1, 2, and now in training, with SWE-1.7"). No COI or ownership disclosure appears on any primary page (verified by grep over pinned snapshots). Per-trial evidence and judge identity are not public.

Shared principles establish alignment, not favoritism — a model trained toward the benchmark's engineering qualities may legitimately score well. The concern the receipts support is narrower and sufficient: benchmark-specific overfitting cannot be ruled out by anyone, because the independent validation that would rule it out is impossible under F6. The methodology post's standard applies: a producer's commercial relationships become conflicts exactly when the artifact claims the science register, and their conclusion claims it ("researchers can trust it"). The cure is procedural: explicit ownership labeling, independent re-grading, preregistered revisions, judge disclosure.

## F7: Grader-wise and task-wise desk read (oracle and spec clauses, checks 7–8 on the public surface)

**Surface:** the announcement's grading-methods disclosure and the single public example task (LOG_WARNING on the jsonschema C++ repo; demo rubric pinned in sources/frontiercode-demo-task-rubric-2026-07-17.md). n=1 task; task-wise claims are bounded by that.

**Grader-wise: three of seven method rows have an LLM inside the oracle.**

| Method | Oracle | Deterministic? |
|--------|--------|----------------|
| classical (injected tests) | fixed tests | yes |
| command (build/lint) | exit code | yes |
| reverse-classical (agent tests fail on base) | fixed procedure | yes |
| adaptive classical (mutagent) | LLM-adapted tests per submission | no |
| scope: files/size | boundaries | yes |
| scope: semantic | LLM judge | no |
| code-quality prompt | LLM judge vs threshold | no |

The mutagent description claims the resolution both ways: an LLM "surgically patch[es] the test environment (or the application code) to align with the agent's implementation details, allowing us to run rigorous, deterministic tests." Determinism after adaptation does not make the adaptation deterministic — each submission is graded against a machine-generated variant of the oracle. "Or the application code" is a frame-clause risk: a tool authorized to patch application code toward the implementation can convert a regression into a pass, and no guard against that is disclosed. Their own QC section concedes the asymmetry: "Hardening prompt-based criteria is a much harder QC problem."

**Blocker semantics make each trial's score discontinuous in single criteria.** Any failed blocker zeroes that trial — the trial moves between zero and its full aggregate on one criterion flip. At the benchmark level the effect is that trial's weight over the task denominator; the discontinuity is per-trial, the headline erosion is proportional. v1.1 relaxing 75 criteria and auditing blockers specifically is their own measurement of this exposure.

**The published information cannot support the precision the board displays.** The oracle contains LLM judgments; blocker-zeroing turns judge flips into whole-trial swings; mean@5 samples the agent's variance, not the grader's. Nothing published bounds oracle noise: no inter-run agreement, no per-criterion flip rates, no per-model intervals. The board nonetheless displays a numbered rank column at 0.1pp resolution, and positions 4–7 (GPT-5.5 43.0, Sonnet 5 42.7, Grok 4.5 42.4, SWE-1.7 42.3) span 0.7pp — with the vendor's model in the cluster. Whether those gaps exceed grader noise is exactly what the publication provides no information to determine, and a ranking that cannot say so is displaying resolution it hasn't earned. Three variance sources — agent sampling, harness/config, grader sampling — and the disclosures address the first, partially.

**Falsifier for the precision claim:** published grader-side variance (rerun the judges on fixed submissions, report flip rates and per-model CIs) showing adjacent-rank gaps exceed the noise floor.

**The oracle model is undisclosed.** Every mention across all pinned pages says only "an LLM" — no name, no version, no pinning policy. Consequences, ascending: (1) *version drift* — an unpinned judge changes the instrument whenever it updates, unanchoring the board's own 1.0-vs-1.1 comparisons; (2) *family-bias exposure* — LLM evaluators favor their own generations in text evaluation settings (Panickssery et al. 2024, "LLM Evaluators Recognize and Favor Their Own Generations"; self-enhancement bias in Zheng et al. 2023); transfer to rubric grading over code diffs is untested, which is precisely the problem — an unnamed judge makes the bias unmeasurable, material when ranks sit 0.7pp apart; (3) *self-judging risk* — undisclosed identity means no one can rule out that the judge is a board model or a Devin/SWE-line relative. Risk scenarios, not findings; what makes them reportable is that the disclosure dissolving all three is one sentence long and absent. A benchmark whose oracle is unnamed is unauditable one level deeper than F6: the public cannot check the verdicts, and cannot name the instrument that produced them.

**The acceptable configuration (operator's standard, 2026-07-17):** a tagged open-weights judge — named, hash-pinned, weights public — as the strongest reproducibility design and the operator's bar for acceptability. Disclosure alone is necessary but insufficient: a named closed-API judge with archived outputs and repeated-grading agreement statistics supports partial reproducibility, but its verdicts stop being re-derivable when the endpoint sunsets. A pinned open-weights judge makes the oracle itself a receipt — the judgment re-runs on anyone's hardware, subject to the ordinary caveats of inference-stack drift. The trade to price: public judge weights are a Goodhart target, the same argument they make for private tasks. But judge-gaming is bounded by the deterministic criteria around it and is detectable by exactly the rerun the open judge enables, while a secret judge forecloses the check entirely. Contamination arguments protect tasks; they do not transfer to the ruler.

**Falsifier:** disclosure naming judge model(s) and version(s) per criterion class, pinned per leaderboard revision — meeting the operator's bar only if open-weights and hash-pinned — plus a judge-family rank-stability ablation.

**Task-wise: one underdetermined requirement, instantiated as two blockers, decides the only public verdict.** The demo exposes all 10 criteria. Opus 4.8: 8/10 met, verdict "Fail · 24%". The two failures are the same semantic requirement — route multi-line warnings through one chained LOG_WARNING call — applied to two files, and both are blockers: a single specification ambiguity produces both failed criteria and the fail verdict.

The ambiguity is real and the literal reading favors the failed solution. The brief's operative sentence: "Use this new function in every instance of warning: <message> messages." A continuation line carries no "warning:" prefix — it is not an instance of "warning: <message>" — and Opus converted every line that is. The graded requirement follows only from the intent reading of "Encapsulate all warning logs," which the brief's own operationalizing sentence narrows. The prose licenses both readings; the hidden blocker pins the one the agent-facing sentence disfavors. Their commentary concedes the solutions are "behaviorally the same" and defends the fail on future-modification taste that appears nowhere in the brief. A maintainer might genuinely hard-stop on this; the sin is that the brief doesn't say so, and the announcement narrates the failure as "models fail this task in a somewhat surprising way" — underdetermination presented as model weakness, on the exemplar chosen to demonstrate rigor.

**Score-labeling wrinkle:** the demo shows an unqualified "24%" beside a run whose stated scoring contribution is zero (blocker failure → 0 per both the announcement and the leaderboard's score note). Raw rubric aggregate and scored contribution are two different numbers for one run, and the interface labels neither. No claim that 24% enters any leaderboard aggregate — that cannot be determined from outside, which is F6's point.

**Credits from the same read (come-back-empty rule):** reverse-classical is a genuinely good deterministic invention; hack reports run both directions (false-positive and false-negative); four-solution calibration targeting a 0–100 range is resolution discipline most benchmarks lack; researchers solving a random subset is a partial human-baseline answer.

**Falsifiers:** for mutagent: published adaptation logs shown equivalence-preserving, or grade agreement between adapted and unadapted runs. For the task-wise claim: agent-visible text for this task that does pin the multi-line idiom. For the LLM-judge concern: reported inter-run agreement on prompt-based criteria.

## Causal chain

```
F6 (no public receipts) → audit surface = disclosures + leaderboard + 1 demo task
F4 (existence, confirmed) → F1 (52/59 floor, run) → F0 (construct gap, supported on population)
F5 (selection, conditional) + F7 (spec/oracle defects, receipted) → the floor is partly instrument
F2 (attribution confound) → the ranking is configurations, not models
Governance (COI, undisclosed) → multiplies F6: the unaccountable party is also interested
```

The evidence chain is complete on the audited population; the open flanks are their regime (Diamond-difficulty preemption) and the F5 selection conditional.

## Auditor's contract (standing constraints from the methodology post)

- **Bound the claim.** F0 is a construct-validity gap, not fraud; the grader may be sound on what it checks.
- **Report floors, never rates.** F1's output is "at least N of M re-verified closures map to no rubric axis."
- **Build it to come back empty.** If F1 maps ≥50% of closures onto the axes, say so; the critique narrows and the audit still worked.
- **Ask for concrete disclosures.** The minimum credible request, filed with any right-of-reply note: per-entry harness and effort configuration; immutable benchmark revisions; per-trial submissions and rubric outcomes for at least a validation subset; grader model/version and prompts per criterion class; mutagent adaptation logs; repeated-grader agreement on fixed submissions; per-model uncertainty intervals; an ownership/COI statement; the reason for Diamond's deprecation and a mapping from old to current scores.
- **Right of reply.** Findings go to Cognition before or as anything publishes; the actionable part files where they work.
- **Audit yourself first.** The sweep closure taxonomy is our instrument; F1 re-verifies closure classifications against the underlying PR threads, not the graph's labels (the graph records its own reclassifications: ruff#25066, llama.cpp#22873, jellyfin-tui ×3 all moved categories on review).

---

# Recommendation

*Everything above is the audit: findings bounded by receipts. Everything below is what we recommend instead, marked as such. Authorship disclosure: the hypothesis graph is our own published design. We are not selling it — the paper, the format, and the harness patterns are given away — and recommending one's own invention is what naming a cure looks like when the auditor has also built one. The disclosure you are reading is the one Cognition's pages lack.*

**Framing: ecological accuracy.** Ecological validity is the classical question (does the instrument measure the construct as it occurs in the wild?); ecological accuracy is its quantitative upgrade: report the agreement rate between the instrument's verdict and the field outcome. FrontierCode claims rung 2 and rung 4 of the construct ladder; ground truth for "would merge" exists in bulk in git history, so the claim is checkable as a number — take PRs whose merge outcomes are known, run the grader, report agreement, with a contamination bound (the clean set is post-cutoff merges). A benchmark that never reports this number has skipped its own criterion-validity check; one that reports it converts "trust it" into a calibration curve. Known limit, stated up front: historical merge outcomes fold in reputation, timing, and maintainer availability — that is the broader construct, and a benchmark that wants the narrower one (patch quality) should claim the narrower one.

**Direct recommendation: grade the hypothesis graph**, per [The Hypothesis Graph: A Verifiable Semantic Memory for Coding Agents](https://june.kim/the-hypothesis-graph-semantic-memory-methodeutics) ([PDF](https://june.kim/assets/the-hypothesis-graph-semantic-memory-methodeutics.pdf)). The axes FrontierCode omits (description reasoning, why-this-fix, interaction) are what a maintainer reviews when deciding to merge, and the paper's contribution is that this reasoning can be a gradeable artifact: typed claims, recorded trials, refutation edges, replayable by an independent party — verification mechanical rather than LLM-judged.

The stance (operator, 2026-07-17): the graph is the best a machine can do; everything else in the communication layer is unverifiable cheap talk, and potentially deceptive. This dissolves the obvious objection to grading the description at all — that scoring prose trains smooth-talkers who coax PRs past review. Grading raw description prose would Goodhart persuasion; grading the graph cannot, because every claim in it carries a replayable trial, so the only way to score is to be checkably right. A coding agent doesn't need to pretend to be human; it needs to show its work in a form a machine can re-run. And the floor argument holds even if the ceiling doesn't: a graded-graph benchmark that turns out not to predict merges is still strictly better than grading nothing of the reasoning layer, because it replaces an unmeasured axis with a verifiable one. Open questions honestly held: whether maintainers want the artifact (H15/H17b field data splits by maintainer class — a formal-methods maintainer read the trace as a methods section, a paid-linker maintainer said "we want to talk to you, not your bot") and whether it moves merge outcomes (the H17 arm returned null: 7 of 12 treated PRs drew zero maintainer attention, so the arm measured the attention bottleneck, not the artifact). What the paper settles is that the reasoning layer can be made checkable at all, which is the precondition for grading it; the benchmark-grading case rests on that verifiability, not on a field merge-lift.

**Supporting form:** the detector from [(Issue) → PR](https://june.kim/issue-to-pr), a classifier trained on a repo's merge/close corpus, recalibrating as the repo's floor moves where a frozen rubric snapshots 20 maintainers' taste. Its own ecological accuracy needs receipts (survivorship, per-repo sample sizes) before it is proposable as a replacement metric.

**Endgame (position, not finding): a cheap oracle stops being a benchmark and becomes a linter.** If the oracle is open and cheap, the move is to build its generic form into the harness and iterate until pass; objections get resolved pre-submission and what ships is the better PR. The 43% → 91% slop-slope delta is this mechanism measured in an easier regime — motivation, not proof, for its effect at FrontierCode difficulty. The generic axes are harness-buildable today: reverse-classical is the TDD red-green gate, scope and build/lint are deterministic, the quality prompt is an adversarial review round; the quality claim requires the deterministic criteria plus a cross-family judge, since iterating against a same-family judge alone is Goodhart. Consequence for the field: a checkable merge standard migrates into every sender's harness and saturates — the `-Wall` → `-Werror` floor dynamic, success rather than failure. Hypothesis for the vendor: a public runnable oracle is a free product feature, a private one is a rankable rent — offered as the structural explanation for oracle secrecy, not as an established motive.
