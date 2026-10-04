# save-project-knowledge

A Claude Code skill that runs at the end of a work session and persists only the durable knowledge (verified solutions, decisions, architecture changes, project rules, gotchas, reusable workflows) into your project's knowledge files, so you can safely clear the conversation.

## Install

```bash
npx skills add gabcoin/save-project-knowledge
```

## Use

At the end of a session, run `/save-project-knowledge`.

It inspects existing docs first (`CLAUDE.md`, `.claude/rules/`, `docs/`), updates instead of duplicating, removes obsolete entries, and ends with a report that says whether it is safe to clear the conversation.

## Shorter command

Want `/spk`? Copy the skill folder to a second name in your skills directory:

```bash
cp -r ~/.claude/skills/save-project-knowledge ~/.claude/skills/spk
```

then change `name:` in the copy's `SKILL.md` to `spk`.
