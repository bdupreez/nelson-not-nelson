# Conductor

Conductor is a Claude Code skill for coordinating agent work. It provides a six-step operational framework: Mission Brief, Assemble Team, Execution Plan, Progress Checks, Risk Levels, and Wrap Up.

## Project structure

```
.claude/skills/conductor/
  SKILL.md              — Main entrypoint (what Claude reads)
  references/           — Supporting docs loaded on demand
    risk-levels.md      — Risk level definitions (Level 0–3)
    templates.md        — Reusable templates (briefs, plans, reports)
    team-composition.md — Mode selection & team sizing rules
  agents/               — Agent interface definitions
demos/                  — Example applications built with Conductor
```

## No build system

This is a documentation-driven skill with zero runtime dependencies. There is no package manager, no build step, and no test suite.

## Testing changes

Install the skill locally and run a mission to verify. Either tell Claude Code "Install skills from https://github.com/harrymunro/nelson" or copy the skill directory manually:

```bash
mkdir -p <target-project>/.claude/skills
cp -r .claude/skills/conductor <target-project>/.claude/skills/conductor
```

Then invoke `/conductor` in Claude Code.

## Code style

- Keep instructions simple and clear
- Markdown for all documentation; YAML for agent interfaces
- The battleships demo (`demos/battleships/index.html`) uses vanilla HTML/CSS/JS with no dependencies

## Git workflow

- Branch from `main`
- Commit messages: imperative mood, concise summary line
- Open a PR for review

## Environment

`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` must be set to enable the `agent-team` execution mode (configured in `.claude/settings.local.json`).
