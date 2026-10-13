---
description: Run lint, typecheck, build and tests, then commit and push the default branch.
---

Ship the current changes. Notes from the user: $ARGUMENTS

1. Run `npm run lint`, `npx tsc --noEmit`, `npm run build`, and `npm test` if a test script exists. Use timeouts; never start dev servers or watch mode in the foreground.
2. If anything fails, fix what the change broke and re-run. If a failure predates the change, say so and stop.
3. Check the diff for em dashes and stray files (`git diff | grep -n $'\xe2\x80\x94'`, `git status`).
4. Commit with a short message describing why. Push the default branch. Deploys are push-to-deploy on the build server: never add or use GitHub Actions.
5. Reply in brief mode: pushed or blocked, one line.
