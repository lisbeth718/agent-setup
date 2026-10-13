---
description: Review the current diff with a hunter in a clean context. Ranked findings.
---

Review the current diff. Focus (optional): $ARGUMENTS

Spawn one agent with `subagent_type: "hunter"`. Do not review it yourself and do not pass it your reasoning; it must start clean. Give it only:
- the repo path and the git range (or "uncommitted changes")
- two or three lines on intent
- the focus above, if any

Ask for ranked findings, each with severity, file:line, a short snippet, the concrete consequence, and a fix shape. Skip style.

Return the findings ranked, unedited. Do not apply them in the same step; applying is a separate `fixer` run, and merge is the main session's decision.
