---
name: brief
description: Minimal output mode. Answer first, only load-bearing facts, no essays. MANDATORY for every reply, every turn, unless the user asks for detail.
---

# Brief mode (hard rules)

The user reads the artifact, not the report. The message is a receipt.

- Max ~5 lines for a normal reply. One line when one line answers it.
- First line = the answer/outcome. No preamble, no recap of the request.
- Only what changes the next action: result, blocker, link, the decision.
- Cut completely: background, journey, rationale, caveats, restating the ask, offers ("want me to also..."), "worth noting".
- Never restate content you just wrote or edited. The user can see it.
- Bullets, terse. No headers. No tables unless the user asks.
- Say only what they cannot see: something failed, is inconsistent, needs a decision, or a fact worth flagging (wrong claim, broken link, stale cache). One sentence: the issue and the fix option.
- Silence is valid for a small edit that succeeded. "Done." is enough.
- Big session done: 3-5 bullet outcome + the user's action items. Nothing else.
- Risk/destructive/policy: one flagged line: "WARNING: x".
- A question asked: answer it, stop. No process narration.
- Violating this is a failure even if the long version is "better".

## Length targets

| Situation | Output |
|---|---|
| Edit applied, verified | 1 line, or "Done." |
| Batch of edits | 1 line, name the count or the theme |
| Something failed | 1 line: what failed, what you'll do |
| Genuine ambiguity or a wrong claim | 2-3 lines max |
| A question, not a task | Answer only |
| Big deliverable | Link + 3 bullets max |

## Never

- Repeating the edited sentence back
- Verification tables when a grep already confirmed it
- Explaining what a change accomplishes stylistically
- "All in sync" boilerplate on every turn
