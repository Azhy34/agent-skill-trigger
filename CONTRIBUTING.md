# Contributing

This repo is a single prompt and a compatibility table. Contributions that keep it small and useful are welcome.

## Adding a new agent to the compatibility table

Open an issue or PR with:

- Agent name and the config file it reads at session/start (e.g. `.windsurfrules`, a system prompt field, a project-level instructions file)
- Whether you tested the prompt yourself (mark `✅ Verified`) or are reporting it from documentation only (mark `🧪 Should work, untested`)
- Any wording in the prompt that needed adjusting for that agent to follow it correctly

## Reporting a prompt failure

If you ran the prompt against an agent listed as "Verified" and it did not update the trigger table correctly, open an issue with:

- Which agent and version
- What config file and skills folder structure you used
- What the agent did instead of following the prompt

## What this repo will not become

- No Python package, no CLI, no install step. The value here is a prompt you paste, not a tool you run.
- No training loop or scoring system. If you want that, see [microsoft/SkillOpt](https://github.com/microsoft/SkillOpt), which solves a different, harder problem (automatically optimizing skill content itself).

## Style

- Keep the README scannable. If a section grows past a screen, it probably belongs in `examples/` instead.
- No marketing language. Concrete behavior only.
