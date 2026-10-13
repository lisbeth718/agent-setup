---
description: Leak review against a private denylist, then a bug review, before anything goes public.
---

Target repo: $ARGUMENTS

1. Find the denylist path: env var `DENYLIST_PATH`, else `git config --get agent.denylist`. If neither is set, stop and say so. Never copy denylist contents into the repo or the reply.
2. Spawn a `hunter` in a clean context. Task: case-insensitive match of every denylist line against all tracked files and git metadata (commit messages, author names and emails, trailers, tags, branches). Also look for IP addresses, hostnames, tokens, emails, internal tool names, client names. Report matches with the matched text cut to its first 3 characters.
3. In the same or a second hunter: bug review of every script and config. Does it work on current tool versions? Does it fail silently on empty results? Do ID formats, paths and log formats match production?
4. Apply findings with a `fixer`. Amend history if metadata leaked.
5. Re-grep the denylist and IPv4 patterns over files (excluding .git) and git log. Must be 0.
6. Do not push. Report in brief mode and wait for the human to say "push".
