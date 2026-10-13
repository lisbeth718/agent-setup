---
description: Triage a pasted alert. Find the root cause before touching code.
---

Alert to triage:

$ARGUMENTS

1. Do not edit anything yet. Work out what the alert measures and how, by reading the code that raises it.
2. Gather evidence with read-only commands only. Quote SQL and shell carefully: a malformed diagnostic can itself fire a statement-error alert. Check whether this alert could be caused by something an agent ran recently.
3. State the root cause in one sentence, with the evidence. Distinguish a real fault from a measurement problem (for example page cache counted as used memory).
4. Fix at the right layer (the watcher, not the service, if the measurement is wrong). Delegate to a `fixer` with an explicit file list and acceptance checks.
5. Run lint, typecheck, build. For anything beyond a one-line fix, run `/review` (a clean hunter) before pushing. Then commit and push the default branch; the build server builds it.
6. Reply in brief mode: cause, fix, what you verified vs only reasoned.
