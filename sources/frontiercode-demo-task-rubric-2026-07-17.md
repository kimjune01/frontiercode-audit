# Demo task rubric, retrieved 2026-07-17

Source: https://cognition.com/frontiercode interactive "Explore a task" demo, Opus 4.8 tab, after clicking "Run eval" (browser session; the grading animation replays pre-recorded results, this is a display artifact, not a live run). Task: LOG_WARNING encapsulation on the jsonschema C++ repo.

## Verdict shown

"Fail · 24%" — "8/10 criteria met. Converted only the first line of each multi-line warning and left the continuation std::cerr lines untouched — baking in the assumption that LOG_WARNING() and std::cerr are the same stream. Fails both multi-line blockers."

## The 10 criteria, verbatim

1. Update src/resolver.h to use LOG_WARNING to print the multi-line warning in the CustomResolver constructor — **BLOCKER, FAILED**
2. Update src/command_bundle.cc to use LOG_WARNING to print the multi-line warning in the options.contains("without-id") conditional — **BLOCKER, FAILED**
3. Update src/logger.h to implement a LOG_WARNING function that takes no arguments and returns a non-const std::ostream & to standard error with a warning: prefix pre-printed — passed
4. Update src/command_lint.cc to use LOG_WARNING for the single-line warning in the disable_lint_rules helper — passed
5. Update src/command_validate.cc to use LOG_WARNING for the single-line warning in the get_schema_template helper — passed
6. Update src/command_validate.cc to use LOG_WARNING for the single-line The JSONL file is empty warning — passed
7. make -C ~/repos/jsonschema — the project builds cleanly — passed
8. clang_format_test — the code is clang-format clean — passed
9. Check out the reference test/CMakeLists.txt and test/*.sh, then make — the implementation passes the reference test suite — passed
10. Restore everything except the reference tests to the base commit — make then fails, proving the tests exercise the change — passed

## Task brief operative sentences (agent-facing)

"Encapsulate all warning logs in a new auto LOG_WARNING() -> std::ostream & method in src/logger.h such that: Warnings are always printed to standard error; Warnings are always printed, independently of --verbose; The helper automatically prints the warning: prefix. Use this new function in every instance of warning: <message> messages throughout the codebase."

## Live leaderboard state (same retrieval)

FrontierCode 1.1 Main, best reasoning mode: 1. Fable 5 (xhigh) 53.5% / $13.09; 2. GPT-5.6 Sol (max) 47.5%; 3. Opus 4.8 (max) 46.5%; 4. GPT-5.5 (xhigh) 43.0%; 5. Sonnet 5 (xhigh) 42.7%; 6. Grok 4.5 (high) 42.4%; 7. SWE-1.7 42.3% / $1.97; ... 17. Qwen 3.7 Plus 10.2%. Chart legend: "Each line connects a model's runs across reasoning efforts; marker shapes distinguish harnesses." Score note: "Solutions that don't pass blocking criteria receive 0."
