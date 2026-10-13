# agent-setup

The global rules, sub-agents and skills I use, plus slash commands and a settings template that package the same loops, to run Claude Code and other coding agents across about 90 repos as a solo builder. Deploys are push-to-deploy on my own servers. For anyone who works alone or in a small team and wants a lean, opinionated setup to copy from.

## Core ideas

- Role-pinned models: planning on the main session, build work on Sonnet fixers, review on Opus hunters. Every sub-agent is a pinned type, nothing inherits.
- The reviewer is never the builder. Reviews run in a clean context.
- Verified vs reasoned: agents must say what they ran and what they only read.
- Brief output: answer first, only what changes the next action.
- Push-to-deploy, no CI: push the default branch, the build server builds. Keep lint and types clean instead.

## Repo map

- `AGENTS.md`: tool-agnostic rules (model routing, folder layout, deploy).
- `CLAUDE.md`: global Claude Code rules (output style, sub-agent models).
- `agents/`: `fixer` (scoped edits, Sonnet), `hunter` (read-only review, Opus), `researcher` (web research, Opus).
- `skills/brief/`: output-length mode.
- `commands/`: slash commands `triage-alert`, `ship`, `review`, `prepublish`, `sweep`.
- `settings/`: example `settings.json` (auto mode, model effort) and notes.
- `WORKFLOW.md`: the four loops and prompt templates.
- `LESSONS.md`: each rule and what went wrong before it existed.

## Install

```sh
mkdir -p ~/.claude/agents ~/.claude/skills ~/.claude/commands ~/Development
cp -n agents/*.md ~/.claude/agents/
cp -rn skills/* ~/.claude/skills/
cp -n commands/*.md ~/.claude/commands/
cp -n CLAUDE.md ~/.claude/CLAUDE.md      # -n never overwrites; if you already have one, append by hand
cp -n AGENTS.md ~/Development/AGENTS.md  # or your projects root
```

`settings/settings.example.json` is not copied: merge it into `~/.claude/settings.json` by hand (see `settings/README.md`). Edit model IDs to whatever you have access to and adjust the folder layout to your own.

Read next: [WORKFLOW.md](WORKFLOW.md), [LESSONS.md](LESSONS.md).

## License

MIT
