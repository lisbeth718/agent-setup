# Agent rules (all projects under ~/Development)

Applies to every AI coding tool: Claude Code, Cursor, or anything else that reads AGENTS.md. Project-level AGENTS.md / CLAUDE.md files add project rules on top of this.

## Model routing

| Role | Model | ID |
|---|---|---|
| Sub-agents, background agents, delegated build/fix work | Sonnet 5.5 | `claude-sonnet-5-5` |
| Reviews (code review, bug hunt, adversarial check before merge/publish) | Fable 5.1 or Opus 5.5 | `claude-fable-5-1` / `claude-opus-5-5` |
| Main session: planning, coordination, merge decisions | Fable 5.1 or Opus 5.5 | `claude-fable-5-1` / `claude-opus-5-5` |

- Always name the model when starting a sub-agent. Never let it inherit the main session's model.
- The reviewer must not be the agent that built the change: review in a separate, clean session or agent.
- Validate sub-agent work with lint, typecheck and build before merging.
- Sub-agents report VERIFIED (ran it) separately from REASONED (only read it).
- Diagnostics against production are read-only; check whether an alert is your own doing.

## Folder layout

- `apps/` products, one clone per repo, folder named after the production domain. Never clone a repo that already exists here.
- One folder per organization or client for work owned by them.
- `infra/` handbook, server docs, shared skills and scripts.
- `archive/` dead repos. `_inbox/` loose files.

## Deploy

Push to the default branch; the build server builds it. Do not add or rely on GitHub Actions for deploys. Keep builds lint-clean and type-clean.
