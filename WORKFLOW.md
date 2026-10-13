# Workflow

How work actually flows. Four loops, each with the same split: the main session plans and decides, fixers (Sonnet) build, hunters (Opus) review in a clean context. Placeholders are in `<angle brackets>`.

## a) Alert triage

An alert gets pasted into the main session.

1. Find the root cause before touching code. Read the alert, read the data behind it, form a hypothesis, try to break it.
2. Write a scoped fix (usually via a fixer, with a file list).
3. lint, typecheck, build.
4. Push. The build server builds.

Real example: a "RAM headroom" alert on a database container. It looked like a leak. It was Linux page cache counted as used memory. The fix was in the watcher, which now measures working set (usage minus file cache), not raw usage. No change to the database at all.

Another case: the alert was caused by the agent itself. A diagnostic SQL query with bad quoting hit the database as a malformed statement and fired a statement-error alert. So diagnostics stay read-only and carefully quoted, and the first question for any odd alert is "did I just cause this?"

Prompt: see `commands/triage-alert.md`.

## b) Feature build

1. Main session plans, then writes a fixer spec: explicit file list plus acceptance checks (template below).
2. Fixer builds.
3. Main session runs tsc, lint, build, tests. Do not trust the fixer's word.
4. Hunter reviews the diff in a clean context.
5. Fixer applies the findings.
6. Main session decides merge.

## c) Pre-publish review

How this repo and its sibling repos were prepared for publishing.

1. Parallel fixers build each repo from private sources.
2. A hunter does a leak and bug review over files and git metadata (author, trailers, commit messages) against a private denylist.
3. Fixer applies the findings.
4. Re-grep the denylist. Must be 0.
5. The human says "push". Nothing is pushed before that.

Real result, in a sibling repo of server scripts: the reviewer found a monitoring script that had silently measured 0 containers in production for months (`docker ps -q` returns 12-char IDs, cgroup paths use 64-char IDs, so nothing ever matched). It also found an SSH watcher that was blind on OpenSSH 9.8+, because logs now come from `sshd-session`. Neither was what the review was asked to look for. A reviewer reading code with fresh eyes finds bugs, not just the thing on the checklist.

## d) Fleet-wide sweep

Example: a critical CVE across about 60 repos.

1. Main session lists affected repos (grep lockfiles, do not guess).
2. One fixer per repo, all with the same spec: bump, overrides/resolutions for transitive deps, build.
3. Main session validates each build itself.
4. Push one by one, so one bad repo does not block the rest.
5. A hunter spot-reviews the risky ones, mainly major-version framework bumps.

Prompt: see `commands/sweep.md`.

## Prompt templates

### Fixer spec

```
Repo: <path>
Goal: <one sentence>

Files you may touch (nothing else):
- <path>: <what changes>
- <path>: <what changes>

Spec:
1. <precise change>
2. <precise change>

Acceptance checks (run all, fix what you broke):
- npx tsc --noEmit
- npm run lint
- npm run build
- npm test (if a test script exists)

Rules: no refactors beyond the spec. No em dashes. Do not commit or push
unless told here: <commit message file, or "do not commit">.
Long builds: use a timeout or background. Never run dev servers or watch mode in the foreground.

Report: files changed (one line each), what you could not do and why,
VERIFIED (you ran it) vs REASONED (you only read it), check results.
```

### Hunter review

```
Review the diff in <repo> (<git range or "uncommitted changes">). You are read-only.
Context you have no memory of: <2-3 lines on intent>.

Look for: correctness bugs, missed edge cases, broken callers, security issues,
things the spec asked for that are not done, things done that were not asked.
Do not report style.

For each finding: severity (high/med/low), file:line, a 2-4 line snippet,
the concrete consequence, and the fix shape a scoped fixer can act on.
Verify each by reading the code, not by grep hits alone.
Rank by severity. If you find nothing, say what you checked.
```

### Pre-publish leak review

```
Review <repo> before it goes public. Read-only.

Denylist: <path to private denylist file>. Case-insensitive match of every line
against: all tracked files, commit messages, author names and emails, trailers,
tags, branch names. Also check for: IPv4/IPv6 addresses, hostnames, tokens and
key shapes, emails, internal tool names, client or tenant names, dates that
imply private context.

Then do a bug review of every script and config: does it work on current
versions of the tools it calls? Does it fail silently (empty result treated as
success)? Do ID formats, paths and log formats match what production emits?

Output: leaks (file:line or git object, the matched text redacted to its first
3 chars) and bugs, each ranked, each with a fix shape.
```
