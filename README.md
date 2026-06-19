# Agent Skill Trigger

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![No install required](https://img.shields.io/badge/install-none%20required-8dbb3c)](#quickstart)
[![Works with](https://img.shields.io/badge/works%20with-Claude%20Code%20%7C%20Cursor%20%7C%20Windsurf%20%7C%20GPT%20%7C%20Gemini-blue)](#tested-with)

A copy-paste prompt that keeps your AI coding agent's skill files in sync with a trigger table, so the agent reads the right context automatically instead of guessing or starting from scratch every session.

## Overview

Agent skill files (focused docs with best practices, paths, commands for a recurring task) only help if the agent actually opens them. In practice they sit unread, because nothing in the one file the agent always loads tells it when to reach for them. This repo is a single prompt that scans your skills folder, writes a trigger table mapping real user phrases to skill files, and keeps that table in sync as skills are added or renamed. No package, no dependency, no training loop. One prompt, pasted once per maintenance pass.

## Quickstart

1. Find your agent's config file in the [Tested With](#tested-with) table below (`CLAUDE.md`, `.cursorrules`, etc.) — this is the file your agent already reads every session.
2. Copy the prompt from [The Prompt](#the-prompt) section, replace `<CONFIG_FILE>` and `<SKILLS_DIR>` with your actual file and folder.
3. Paste it into a chat with your agent. Review the diff it produces in your config file before accepting.

See [`examples/`](examples/) for what the resulting table looks like in a real `CLAUDE.md` or `.cursorrules`.

## The problem

Most agent setups have skill files: focused docs with best practices, file paths, commands, examples for a specific recurring task (writing a report, running a deploy, doing keyword research, whatever you repeat often).

These files sit in a directory. The agent's config file (the one that actually loads into context at session start) does not know they exist unless you tell it. So the agent either ignores them or you have to say "read this file" every single time.

## The fix

Keep a trigger table inside your agent's config file. Not task descriptions, the actual phrases you say when you want something done:

```
"write a report"     -> read skills/report-writer.md
"check the traffic"  -> read skills/analytics.md
"deploy to prod"      -> read skills/deploy.md
```

Since the config file is already loaded into context, the agent follows the table reliably. No guessing, no wasted tokens re-deriving what already exists on disk.

## Tested With

| Agent | Config file read at session start | Typical skills location | Status |
|---|---|---|---|
| Claude Code | `CLAUDE.md` | `.agent/skills/` or `.claude/skills/` | ✅ Verified |
| Cursor | `.cursorrules` (or `.cursor/rules/`) | same folder or `.agent/skills/` | ✅ Verified |
| Windsurf | `.windsurfrules` | same folder or `.agent/skills/` | 🧪 Should work, untested |
| ChatGPT / custom GPT | system prompt / project instructions | wherever you reference it from | 🧪 Should work, untested |
| Gemini (Code Assist, etc.) | system prompt / `GEMINI.md` if supported | same pattern | 🧪 Should work, untested |

The mechanism is identical across all of them: one file loads automatically, everything else needs to be pointed to. See [CONTRIBUTING.md](CONTRIBUTING.md) to report a verified or failing run for an untested agent.

## The Prompt

Paste this into a chat with your agent. Replace `<CONFIG_FILE>` and `<SKILLS_DIR>` with whatever applies from the table above.

```
Go into <SKILLS_DIR> (or the equivalent skills/rules folder in this project).

For each skill file found:
1. Read the first ~15 lines (description and any existing trigger notes).
2. Understand: what task is this skill for, and what phrases would a user
   realistically type when they want that task done.

Then open <CONFIG_FILE> and find the skills table (a section literally
named "Skills" or similar).

Update that table:
- Add or keep a column called "Triggers (user phrases)".
- For each skill, write 4-7 concrete phrases a real user would actually type
  to ask for that task. Not abstract descriptions ("report writing") —
  living phrases ("write me a report", "need a report on...", "summarize this
  for the team").
- At the top of the section, add this rule: "On first mention of any trigger
  phrase — read the skill file immediately, before asking questions or
  writing code. Path: <SKILLS_DIR>/{skill-name}/SKILL.md (or equivalent)."

If a skill file exists in the folder but is missing from the table, add it.
If <CONFIG_FILE> has no "Skills" section yet, create one at the end of the file.

Do not invent skills that don't exist in the folder. Do not change the
content of the skill files themselves, only the trigger table in the config file.
```

See [`examples/claude-code-skills-table.md`](examples/claude-code-skills-table.md) and [`examples/cursorrules-skills-table.md`](examples/cursorrules-skills-table.md) for what this produces.

## What this actually does (and does not do)

It does not make the agent magically "discover" files. The agent follows an explicit instruction that lives in a file it already reads. That is the whole trick: write the instruction once, in the one place guaranteed to load every session.

It does not replace good skill files. A trigger table pointing at a bad or outdated skill file just makes the agent confidently read bad context faster. Keep the skill files themselves accurate — that part is still on you.

It does not optimize skill content. If you want a system that actually rewrites and improves skill text based on scored feedback, see [microsoft/SkillOpt](https://github.com/microsoft/SkillOpt) — a research project that trains skill documents like model weights. This repo solves a narrower problem: getting the agent to open the file at all.

## Maintenance

Run the prompt again after any session where you added, renamed, or removed a skill file. Takes a couple of minutes, keeps the table from drifting out of sync with what is actually on disk.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE).
