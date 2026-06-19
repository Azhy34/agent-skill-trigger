# Example: CLAUDE.md skills section (Claude Code)

This is a real-shape example of what the prompt produces, with the project's own details replaced by generic placeholders. Drop a section like this at the end of your `CLAUDE.md`.

```markdown
## Skills — when to read what

Rule: on first mention of any trigger phrase below, read the matching skill file immediately, before asking questions or writing code. Path: `.agent/skills/{name}/SKILL.md`

| Skill | Triggers (user phrases) | Path |
|---|---|---|
| report-writer | "write a report", "need a report on...", "summarize this for the team", "draft the weekly update" | `.agent/skills/report-writer/SKILL.md` |
| deploy | "deploy to prod", "push the release", "ship this", "is staging up to date" | `.agent/skills/deploy/SKILL.md` |
| db-migration | "add a migration", "change the schema", "new column on...", "migrate the table" | `.agent/skills/db-migration/SKILL.md` |
| customer-support-tone | "draft a reply to this ticket", "how should I respond to...", "customer is upset about" | `.agent/skills/customer-support-tone/SKILL.md` |
```

## Why this works in Claude Code specifically

`CLAUDE.md` is read into context automatically at the start of every session (and again after compaction). The table above does not need a separate activation mechanism — it rides on a file Claude Code already loads. The only job left for you is keeping it accurate as skills get added or renamed, which is what the prompt in the main [README](../README.md) automates.
