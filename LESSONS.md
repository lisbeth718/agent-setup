# Lessons

Each rule in this repo, and what went wrong before it existed.

## Sub-agents need a pinned model
Agents launched without a model inherited the main session's model, the most expensive one, for routine edits. Now every sub-agent is a pinned agent type per role (`fixer`, `hunter`, `researcher`). Nothing inherits.

## The reviewer is never the builder
A builder reviewing its own work misses what it already rationalized. A separate, clean reviewer does not share those assumptions. In a pre-publish review of a sibling repo of server scripts, the reviewer found a monitoring script that had measured 0 containers for months and an SSH watcher blind on newer OpenSSH. Neither was in the brief. The builders had read both scripts many times.

## Verified vs reasoned
Agents reported things as tested that they had only reasoned about. Fixers now report two lists: VERIFIED (ran it, saw the output) and REASONED (read the code, believe it works). The main session re-runs the checks anyway.

## Never-ending processes go in the background
Agents started dev servers and watch mode in the foreground and the session hung. Long builds get a timeout; anything that does not exit runs in the background.

## No symlinked node_modules in worktrees
Symlinking `node_modules` into a fresh git worktree broke Turbopack. Run `npm ci` instead.

## Check generated code before calling a build broken
A fresh worktree was missing generated ORM types, and the build failed in ways that looked like real breakage. Before declaring a build broken, check that generated code (ORM client, route types) exists and regenerate it.

## Diagnostics must be read-only, and alerts may be your own doing
A badly quoted diagnostic SQL query fired a statement-error alert on production. Diagnostics are read-only and carefully quoted, and the first check on any alert is whether the agent caused it.

## Pinned versions need deliberate upgrades
Pinning means a model does not change under you, but it also means it does not improve unless someone moves it. An older pinned reviewer was retired and replaced on purpose, with the human's say-so, not silently.

## Auto mode: narrow allows, soft denies, human runs writes
In auto mode, pre-approve narrow read-only production lookups. Soft-deny seed and RLS-style scripts so they need a conscious yes. Remote writes stay blocked; the human runs them with `! <command>` in the session. See `settings/`.

## No em dashes, and brief replies
Em dashes and long replies read as AI output. Em dashes are banned in code, copy and docs, and the fixer greps its own diff for them. Replies follow brief mode.

## Long status replies waste attention
Multi-paragraph status updates cost the human more than the work they described. Brief mode applies to every reply: answer first, only what changes the next action.
