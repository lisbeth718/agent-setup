---
description: Apply one change across many repos with batched fixers, validate each, push one by one.
---

Change to apply across repos: $ARGUMENTS

1. List affected repos by inspecting lockfiles or source, not by guessing. Show the list with a count.
2. Write one fixer spec used for every repo: the change, overrides/resolutions for transitive dependencies if relevant, and acceptance checks (tsc, lint, build, tests).
3. Launch `fixer` agents in batches, one per repo, each told not to commit or push.
4. Validate each repo's build yourself. Do not accept a fixer's own report as proof.
5. Commit and push one repo at a time on its default branch. A failure in one repo does not block the others. Never use GitHub Actions.
6. Spawn a `hunter` to spot-review the risky repos (major-version framework bumps, changed lockfile resolutions).
7. Reply in brief mode: pushed N, blocked M with one-line reasons.
