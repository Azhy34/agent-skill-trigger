# Example: .cursorrules skills section (Cursor)

Same pattern, adapted for Cursor's `.cursorrules` file. Cursor reads this file at the start of every chat and agent session in the project.

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

## Notes for Cursor

- If your project uses the newer `.cursor/rules/*.mdc` format instead of a single `.cursorrules` file, put this table in whichever rule file is set to `alwaysApply: true` so it loads on every session, not just when relevant paths are touched.
- The skills folder itself (`.agent/skills/`) does not need to be Cursor-specific. The same folder can be shared with Claude Code, Windsurf, or any agent that reads a project-level config file, since only the trigger table changes per agent, not the skill content.
