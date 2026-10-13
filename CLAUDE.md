# Output rule (always on)

Super short replies, relevant info only. Follow ~/.claude/skills/brief/SKILL.md on every reply, every session:
- Answer/verdict first, one sentence.
- Then only what changes the user's next action: links, values, blockers, their steps.
- No background, no journey narration, no restating, no offers, no headers unless long.
- Big deliverables: link + 3 bullets max.

# Subagent models (always on, every project)

Every Agent call names its role explicitly; never launch `general-purpose` without a model (it inherits the main session's model).
- Hunters (deep review, judgment-heavy): `subagent_type: "hunter"` (pinned to Opus 5.5, `claude-opus-5-5`). Don't pass `model: "opus"`, use the hunter type so the version stays pinned.
- Fixers (scoped edits, feature builds with a file list and spec): `subagent_type: "fixer"` (pinned to Sonnet 5.5, `claude-sonnet-5-5`), validated with tsc + build. Sonnet 5.5 is the default for all sub-agent build work.
- Reviews: Fable or Opus 5.5 only, in a separate clean agent/session from the builder.
- The main session (Fable): coordination, validation, merge decisions only.
- Tool-agnostic version of these rules (Cursor etc.): `~/Development/AGENTS.md`.
- Slash commands (install into `~/.claude/commands/`) (`/triage-alert`, `/ship`, `/review`, `/prepublish`, `/sweep`) wrap the common loops; they use the pinned agent types above.
