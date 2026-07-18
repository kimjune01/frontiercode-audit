# Right of reply: an external audit of FrontierCode

*Published 2026-07-18 as https://github.com/kimjune01/frontiercode-audit/issues/1. Direct send to the FrontierCode team pending; the issue is the public record.*

---

Hello. I audit coding benchmarks (SWE-bench Verified, DeepSWE, SWE-bench Pro, Terminal-Bench, MirrorCode; method at https://june.kim/how-to-audit-a-benchmark). I've completed an audit of FrontierCode from your published pages and one measurement of my own, and I'm sending you the findings before I publish anything. Corrections are welcome, and I'll link your response from the audit.

The audit, with receipts, protocols, and per-finding falsifiers, is at https://github.com/kimjune01/frontiercode-audit.

Some of the design holds up well under audit. Your diagnosis of SWE-bench's false-positive problem matches what my own audits keep finding. Reverse-classical testing is a good deterministic invention. The hack-report step, four-solution rubric calibration, and researchers solving tasks themselves are QC most benchmarks lack. The private task set is an anti-contamination credit.

The main finding (AUDIT.md, F0/F1) is a measurement. The headline asks "would the maintainer actually merge this PR?" but the grader sees none of the PR-level information maintainers use. I measured the cost of that omission on 98 closed-unmerged PRs whose outcomes are known. In at least 52 of 59 confidently-coded maintainer closures, two coders from different model families found the deciding cause outside your six rubric axes. They found a diff-quality cause decisive in exactly 3. The population, its bounds, the coding protocol, and the coded threads are all in the repo. Two conclusions survive the bounds. Off-diff causes can dominate a real contribution population. And I found no published criterion-validity number connecting FrontierCode verdicts to actual maintainer decisions. The claim "trust it to evaluate the production readiness of their strongest models" presumes that number.

Six further findings come from your published artifacts alone. Each is detailed with quotes and falsifiers in AUDIT.md (F2, F5, F7, Governance):

1. One underdetermined requirement accounts for both failed blockers in the only public task-level grading receipt, and your commentary notes the two solutions are "behaviorally the same" (F7, task-wise).
2. The grading oracle is identified only as "an LLM"; no model name, version, or pinning policy is published. Readers cannot assess judge drift or the judge's independence from the systems it grades (F7, oracle).
3. The published materials do not establish that adjacent leaderboard gaps exceed grader noise; ranks 4 through 7 sat within 0.7 points at capture (F7, precision).
4. Each leaderboard entry is a jointly varying model-harness-effort configuration, and the numbered column reads as a model ranking (F2).
5. Deprecated Diamond scores still circulate as the benchmark's public numbers, with no disclosed reason or comparability guidance (F5).
6. No ownership or conflict-of-interest statement appears on the pages reviewed, while SWE-1.7 is scored on the board and its launch post says the model and the benchmark share design principles. Shared principles can legitimately produce a high score, but the public evidence cannot distinguish that from tuning to benchmark-specific characteristics while trial-level validation stays impossible from the released artifacts (Governance).

I recommend that you publish enough to make the verdicts publicly checkable; the itemized minimum is in AUDIT.md under "Ask for concrete disclosures". I also recommend that you publish the number the headline implies, the criterion validity of FrontierCode verdicts against adjudicated real merge decisions on post-cutoff PRs. Every finding above has a falsifier in the audit. I'd rather link your ablation than my inference.

June Kim
https://june.kim · audit: https://github.com/kimjune01/frontiercode-audit (CC BY-SA-NS)
