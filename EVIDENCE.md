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
- Source snapshots: `sources/*.html`, retrieved 2026-07-17 via curl.
- Their SWE-bench critique: ~81% false-positive rate claim (passing solutions that would not merge). Unverifiable externally; see F3.
- Leaderboard (Diamond, as of 2026-07): Claude Opus 4.8 ~13.4%, GPT-5.5 6.3%, Gemini 3.1 Pro 4.7%.

## Third-party tracking and commentary

- Epoch AI tracks it: https://epoch.ai/benchmarks/frontiercode
- Latent Space roundup (variance/reproducibility concerns; harness-dependence noted by practitioners): https://www.latent.space/p/ainews-frontiercode-benchmarking
- Aggregator leaderboards relaying scores without re-running: benchlm.ai, llm-stats.com, benchmarklist.com

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
- Whether Epoch independently re-runs FrontierCode or relays Cognition's numbers.
- FrontierCode's harness spec per leaderboard entry, if disclosed anywhere.
- Exact sweep closure records export for the F1 mapping table (`outcomes.py` records in the sweep repo).
