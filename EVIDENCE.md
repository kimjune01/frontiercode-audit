# Evidence

Receipts only. Every claim in the audit traces to a line here.

## FrontierCode's own disclosures

### Stated goal, verbatim (verified 2026-07-17 against raw HTML, pinned in sources/)

The goal question the audit turned on: is their stated goal model discrimination (critique weakens, per operator decision rule) or a goalpost for automation (critique stands, cite it)? Answer: goalpost for automation. All quotes from https://cognition.com/blog/frontier-code unless noted.

- Automation framing, the citable sentence: "But as AI-generated code becomes the dominant path to production, correctness is now table stakes. The question that we should be asking is: can models actually write good code?"
- Construct claim: "Would the maintainer actually merge this PR? We're the first benchmark to measure code mergeability."
- Certification claim (conclusion): "FrontierCode is the benchmark for the next generation of coding agents. We are confident developers, enterprises, and researchers can trust it to evaluate the production readiness of their strongest models."
- Signal claim: "Our benchmark provides the strongest available signal of a model's ability to write high-quality, maintainable code."
- The ranking claim is derived from the construct claim, not independent of it: "FrontierCode produces 81% less misclassification errors than other leading benchmarks. This means that FrontierCode scores are the most accurate ranking currently available." The ranking is accurate BECAUSE the mergeability measurement is claimed accurate. Retreating to "we only rank models" abandons this sentence and the conclusion paragraph.
- No technical report or paper exists; the blog posts and leaderboard page are the primary artifacts.

### Structural facts

- Announcement: https://cognition.com/blog/frontier-code (launched 2026-06-08)
  - Grading inputs: the patch, held-out tests, maintainer-authored rubric.
  - Six rubric axes: behavioral correctness, regression safety, mechanical cleanliness, test correctness, scope, code quality.
  - Task prompt: terse task description (~1/3 the length of comparable benchmarks) + generic codebase guidelines. "FrontierCode expects the agent to infer the maintainer's intent."
  - No mention of grading PR descriptions, commit messages, or agent communication. (Verified against the blog 2026-07-17; recheck before publishing.)
  - 150 tasks, 36 repos, 20+ maintainers, 40+ hours per task. Subsets: Extended 150 / Main 100 / Diamond 50.
  - Dataset private; evaluation access to model developers on request.
  - v1.1: fair-internet-use rules, 1,000+ grading criteria audited, 75 relaxed. The leaderboard page (verbatim): v1.1 "audits blocker criteria and deprecates the Diamond subset," and "Runs flagged for unfair internet use are zeroed."
- Diamond deprecation means the widely-circulated 13.4%/6.3%/4.7% scores cite a subset the current revision no longer reports. Any published number must say which revision it refers to.
- Rubric mechanics (announcement, verbatim): "Blockers represent mergeability requirements, i.e., criteria that a maintainer would consider hard stops during code review." Non-blockers are quality signals. Nothing on either list scores the PR description, commit messages, or agent communication.
- Source snapshots: `sources/*.html`, retrieved 2026-07-17 via curl. Demo-task rubric (10 criteria, Opus 4.8 verdict): `sources/frontiercode-demo-task-rubric-2026-07-17.md`, retrieved via the interactive leaderboard demo.
- Live leaderboard (1.1 Main, best reasoning mode, 2026-07-17): Fable 5 leads at 53.5% ($13.09/rollout), GPT-5.6 Sol 47.5%, Opus 4.8 46.5%, GPT-5.5 43.0%, SWE-1.7 7th at 42.3% ($1.97/rollout, the cost-efficiency story). 17 models listed. Chart legend admits "marker shapes distinguish harnesses."

### Conflict of interest (verified 2026-07-17, feeds F6)

- No COI or disclosure statement on any primary page. Grep of pinned HTML for conflict/disclos/impartial/independen/neutral/vested: zero hits in a COI sense across announcement, v1.1 post, and leaderboard page.
- The SWE-1.7 post (https://cognition.com/blog/swe-1-7, pinned in sources/) scores Cognition's own model on their own benchmark: "FrontierCode 1.1 Main 42.3%" in the headline results table, third behind Opus 4.8 (46.5%) and GPT-5.5 (43.0%), presented alongside third-party benchmarks with no note that they own this one.
- The alignment admission, verbatim: "At Cognition, we have been formulating and refining principles for good agentic software engineering both in evaluation, with FrontierCode 1, 2, and now in training, with SWE-1.7." The same principles built the grader and trained the model. Their model's behavioral analysis is even reported in FrontierCode's own vocabulary ("Behavioral tendencies on FrontierCode 1.1 Main"; scope discipline "As described in FrontierCode 1").
- Per the methodology post's check-5 companion: none of a producer's commercial relationships are conflicts when the artifact is marketing; each is a conflict when the artifact is asked to be science. Their conclusion asks for science ("researchers can trust it to evaluate the production readiness of their strongest models").
- Their SWE-bench critique: ~81% false-positive rate claim (passing solutions that would not merge). Unverifiable externally; see F3.
- Leaderboard (Diamond, as of 2026-07): Claude Opus 4.8 ~13.4%, GPT-5.5 6.3%, Gemini 3.1 Pro 4.7%.

## Third-party tracking and commentary

### Epoch AI (verified verbatim 2026-07-17, snapshot in sources/)

- Relay confirmed, no independent re-run: "We source results from Cognition's public FrontierCode data."
- They chart the deprecated subset: "Our chart reports the Diamond score: each model's rubric score on the hardest 50-task Diamond subset at its best-performing reasoning effort." Cognition's v1.1 deprecated Diamond; Epoch's chart still reports it. Fileable correction for Epoch (the evals-shop surface is where audit findings land).
- Best-performing reasoning effort = max-over-configs ranking, the check-4 score sin (ranking model-and-effort pairs presented as ranking models).
- Harness heterogeneity confirmed: "Models are run through agent harnesses such as Claude Code, Codex, Gemini CLI, mini-SWE-agent, and Devin; we keep each model's harness and best-performing reasoning effort in the data export." Different models ran under different harnesses; leaderboard deltas mix model and harness. Direct F2 receipt.
- Scoring mechanics: "mean@5 aggregation against a weighted rubric, where failing any blocker criterion yields a zero." Cognition also reports a separate binary pass rate that Epoch does not show, so two headline statistics circulate for the same runs.
- On-page disclaimer (table view, retrieved live 2026-07-18): "The data shown for this benchmark does not come from Epoch AI internal runs: it is sourced from Cognition's FrontierCode." Relay stated on the page itself, not just in methodology.
- The "data export" their methodology references was not found as a public CSV/JSON endpoint (probed 2026-07-18); the per-entry harness claim rests on their methodology sentence plus Cognition's chart legend.

### H17 treatment-arm outcomes (pulled 2026-07-18, f1/h17-outcomes.jsonl)

12 hypothesis-graph-in-body treatment PRs (treated 2026-05-14): 1 merged (otree, owner thanked), 4 closed (tracy: wrong approach with maintainer WIP preempt; bat: competing PR chosen; chalk: maintainer engaged the experiment framing and judged the fix wrong; clap: self-closed), 7 still open with zero maintainer touch. Review-touch after treatment: 3 of 12. HG/experiment framing referenced in a closure: 1 (chalk). Reading against the pre-registration: treatment merge rate does not beat control; H17's merge-lever prediction fails on this arm; the dominant outcome is no maintainer attention at all (7/12), so the binding variable in this arm was attention, not description content. H17a (disclosure as detection vector): one partial hit (chalk); otree drew a third-party "block him" comment about AI PRs generally.

### Other

- Latent Space roundup (variance/reproducibility concerns; harness-dependence noted by practitioners): https://www.latent.space/p/ainews-frontiercode-benchmarking
- Aggregator leaderboards relaying scores without re-running: benchlm.ai, llm-stats.com, benchmarklist.com

## F1 result (this audit's own measurement, 2026-07-18)

Primary evidence for the construct gap; supersedes reliance on sweep's self-reported taxonomy below. Population: all 98 closed-unmerged kimjune01 PRs since 2026-05-09 (88 merged / 103 open at pull). Two independent coders (Claude agents from threads; codex blind from verbatim quotes). At least 52/59 confidently-coded maintainer/bot closures decided outside the six rubric axes; 3/59 diff-decided both-coder; dichotomy agreement 93%. Decomposition: superseded 16, ai_identity 15, policy 8, duplicate 4, other 4 (incl. 21-second pallets batch-close cluster), cadence 2, stale 2, wrong_premise 1, standing 1. Four closures are accepted-work mislabels (fish-shell, opensquilla, polars, evebox). Records: `f1/coded/all.jsonl`, `f1/second_coder_raw.txt`, `f1/PROTOCOL.md`, `f1/h17-outcomes.jsonl`.

## Our field data (sweep)

Source: ~/Documents/sweep/HYPOTHESIS_GRAPH.md and its public mirror https://github.com/kimjune01/sweep/blob/master/HYPOTHESIS_GRAPH.md. Numbers as of the retro dates named; the graph is live and moves.

- Closure taxonomy (retro 13): pipeline error 17, credence test 7, external 10, AI-detection-via-description 1. CONTRIBUTING/template non-compliance is the #1 pipeline error subcategory (7).
- Diff quality is not the failure mode: QA found 0 bugs on 4/5 maintainer-labeled "AI slop" PRs; 80% of slop rejections were policy-based (session 6, H7 evidence).
- Description as rejection cause: ruff#25066 closed with "summary doesn't explain why decisions were made except by reference to my feedback."
- Cadence as rejection cause: cucumber/gherkin, maintainer mpkorstanje: "I don't get the impression there is a human in the loop." Patch was correct.
- Overclaimed motivation as credibility tax: sweep H16 (N=1, 2026-05-14).
- Description treatment arm: sweep H17, 12 PRs with hypothesis-graph links in body, pre-registered 2026-05-14. Outcomes need pulling before F4 design.
- Scale context: 137 shipped, 69 merged, 55% review-touched merge rate (2026-05-14 score table); median PR 40 lines.

## Our lab data (slop-slope experiment)

Source: june.kim/does-iteration-mitigate-slop-slope; repo https://github.com/kimjune01/refactor-equivalence

- One-shot LLM output: 43% reviewer approval (9/21). Same code with adversarial review loop: 91% (21/23). 48pp from the loop alone, model held constant. Feeds F2.
- Field deployment of the same loop: 101 PRs, 54 merged, ~80–84% adjusted approval after backing out non-code closures (~70% of closures).
- The residual lab-to-field gap is "fits our project," the social layer. No diff-only rubric scores it. Feeds F0.

## Methodology

- Checklist and sin taxonomy: https://june.kim/how-to-audit-a-benchmark (11 checks; 6 blocked here by the private dataset, see F6)
- Base rate for gold defects: check 6 caught shipped defects in 3 of 3 benchmarks it was run against (DeepSWE 4/113, SWE-bench Pro 3/731, Terminal-Bench 2.1 6/89). Feeds F5/F6.

## Gaps / to collect

- How "hardest" is operationalized for Main/Diamond subsets (model pass rates vs independent difficulty rating). Feeds F5.
- Whether any Diamond task has a completed human baseline (measured, not estimated). Feeds F5.
- v1.1 changelog detail on the 75 relaxed criteria: which axes, which tasks. Feeds F5/F6.

- H17 treatment-arm outcomes (were due +7d from 2026-05-14; sweep graph on disk predates resolution).
- Exact sweep closure records export for the F1 mapping table (`outcomes.py` records in the sweep repo).

Resolved 2026-07-17: Epoch relays, does not re-run (Epoch section above). Harness-per-entry is disclosed in Epoch's data export and is heterogeneous across models.
