# F1 coding protocol (2026-07-17)

Population: 98 closed-unmerged PRs by kimjune01 created after 2026-05-09T00:34:00Z (f1/closures.jsonl). Unit of analysis: one closure. All coding from the PR thread itself (gh), never from sweep's own labels.

## Fields per closure

- `repo`, `number`, `url`
- `closed_by`: maintainer | bot | self | unknown. Self-closes (pipeline aborts, superseded-by-us) are excluded from the maintainer-decision denominator.
- `closure_reason_quote`: verbatim quote of the stated closure reason from the closer or reviewing maintainer; "SILENT" if closed without comment.
- `technically_faulted`: yes | no | unknown — did any maintainer/reviewer assert a defect in the code itself? If yes, quote it.
- `axes` (multi-label, may be empty): which FrontierCode rubric axes the stated cause maps to — behavioral_correctness | regression_safety | mechanical_cleanliness | test_correctness | scope | code_quality.
- `non_diff_causes` (multi-label, may be empty): description_reasoning | interaction_cadence | policy_compliance (CONTRIBUTING/AGENTS/CLA/template) | ai_identity | standing | superseded_or_maintainer_fix | duplicate | stale | wrong_premise (issue misread; note: wrong_premise is diff-external but also maps to behavioral_correctness if the patch solves the wrong problem) | other.
- `diff_observable`: yes | no | partial — could a grader seeing only the patch have detected the stated cause?
- `sufficient`: for each coded cause, was it independently sufficient for the closure? Mark the primary sufficient cause.
- `confidence`: high | medium | low. Low or ambiguous → the closure lands in the unknown bucket and out of the floor.

## Decision rule (replaces the pre-multi-label falsifier)

Primary outcome, computed on maintainer/bot-closed PRs with confidence ≥ medium: **non-diff-decided floor** = count of closures where at least one non-diff cause was independently sufficient AND no rubric axis cause was independently sufficient. Reported as "at least N of M."

Falsifier: if a majority of confidently-coded maintainer closures are diff-axis-decided (a rubric axis cause sufficient), F1 fails and F0's magnitude claim narrows to "the diff is insufficient at the margin."

## Coder discipline

- Quote or it didn't happen: every code cites thread text or an observable fact (e.g. bot auto-close message, template checklist).
- Silent closures code as unknown cause (not as non-diff) unless an observable mechanism (org block, batch-close timestamp cluster) is documented.
- Second coder: cross-family (codex) blind pass on every closure the primary coder marks non-diff-decided, plus a 10-closure random sample of the rest. Disagreements reported, not resolved.
