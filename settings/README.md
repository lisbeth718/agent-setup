# settings

`settings.example.json` is a template for `~/.claude/settings.json`. Merge it by hand into your existing file; do not overwrite. All values are placeholders.

- `permissions.defaultMode: "auto"`: Claude Code runs without asking per tool call, using the rules below to decide what still needs a human.
- `modelSettings`: per-model `effortLevel`. Main-session models (Fable, Opus) at medium, which is enough for planning and coordination. Sonnet could go lower, since fixers get a precise spec. The values here are an example; tune it to your own cost and quality tradeoff.
- `autoMode.allow`: narrow pre-approvals. The example allows only read-only SELECT lookups on one named database container over ssh. Keep allows this specific.
- `autoMode.soft_deny`: things that should need a conscious yes even though auto mode is on. The example denies seed and RLS-style scripts, which are easy to run against the wrong database.
- `autoMode.environment`: plain-language facts the classifier uses to judge risk: who you are, where secrets live, and what counts as a sensitive target (here: any name containing "prod").

Remote writes stay blocked. When one is really needed, you run it yourself in the session with `! <command>`.

The `allow` and `soft_deny` lists start with `"$defaults"`, which keeps the built-in rules and adds yours on top. Leave it out and your list replaces the defaults. `environment` is plain facts and does not need it.
