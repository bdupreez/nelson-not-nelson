# Conductor

A Claude Code skill for coordinating agent work. It provides structured mission briefs, execution plans, risk levels, and a mission report to manage complex tasks — from single-session work through to parallel agent teams.

## What it does

Conductor gives Claude a six-step operational framework for tackling complex missions:

1. **Mission Brief** — Define the outcome, success metric, constraints, and stop criteria
2. **Assemble Team** — Choose an execution mode (single-session, subagents, or agent team) and size the team
3. **Execution Plan** — Split the mission into independent tasks with owners, dependencies, and file ownership
4. **Progress Checks** — Run checkpoints to track progress, identify blockers, and manage budget
5. **Risk Levels** — Classify tasks by risk level and enforce verification before marking complete
6. **Wrap Up** — Produce a mission report with decisions, artifacts, validation evidence, and follow-ups

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed and authenticated

## Installation

### Prompt-based (recommended)

Open Claude Code and say:

```
Install skills from https://github.com/bdupreez/nelson-not-nelson
```

Claude will clone the repo, copy the skill into your project's `.claude/skills/` directory, and clean up. To install it globally across all projects, ask Claude to install it to `~/.claude/skills/` instead.

### Manual

Clone the repo and copy the skill directory yourself:

```bash
# Project-level (recommended for teams)
git clone https://github.com/bdupreez/nelson-not-nelson.git /tmp/conductor
mkdir -p .claude/skills
cp -r /tmp/conductor/.claude/skills/conductor .claude/skills/conductor
rm -rf /tmp/conductor

# Or user-level (personal, all projects)
cp -r /tmp/conductor/.claude/skills/conductor ~/.claude/skills/conductor
```

Then commit `.claude/skills/conductor/` to version control so your team can use it.

### Verify installation

Open Claude Code and ask:

```
What skills are available?
```

You should see `conductor` listed. You can also invoke it directly:

```
/conductor
```

## Usage

### Let Claude invoke it automatically

Claude reads the skill description and loads it when your request matches — for example, when you ask for coordinated parallel work or structured mission execution. Just describe your task:

```
I need to refactor the authentication system. The work spans the API layer,
the frontend, and the test suite. Use conductor to coordinate this.
```

### Invoke it directly

Use the slash command with your mission brief:

```
/conductor Migrate the payment processing module from Stripe v2 to v3
```

### Provide a structured mission brief

For maximum control, provide your own mission brief:

```
/conductor

Mission brief:
- Outcome: All API endpoints return consistent error responses
- Success metric: Zero test failures, all error responses match the schema
- Deadline: This session

Constraints:
- Token/time budget: Stay under 50k tokens
- Forbidden actions: Do not modify the database schema

Scope:
- In scope: src/api/ and tests/api/
- Out of scope: Frontend error handling
```

## How it works

### Execution modes

The skill selects one of three execution modes based on your mission:

| Mode | When to use | How it works |
|------|------------|--------------|
| `single-session` | Sequential tasks, low complexity, heavy same-file editing | Claude works through tasks in order within one session |
| `subagents` | Parallel tasks where workers only report back to the coordinator | Claude spawns [subagents](https://code.claude.com/docs/en/sub-agents) that work independently and return results |
| `agent-team` | Parallel tasks where workers need to coordinate with each other | Claude creates an [agent team](https://code.claude.com/docs/en/agent-teams) with direct agent-to-agent communication |

### Team composition

Teams follow a simple hierarchy:

- **Coordinator** — Coordinates the mission, delegates tasks, resolves blockers, produces the final synthesis. There is always exactly one.
- **Agents** — Own individual tasks and their deliverables. Typically 2-7 per mission.
- **Reviewer** — Challenges assumptions, validates outputs, and checks rollback readiness. Added for medium/high risk work.

Team size scales with mission complexity. The skill caps at 10 total agents to keep coordination overhead manageable.

### Risk levels

Every task is classified into a risk level before execution. Higher levels require more controls:

| Level | Name | When | Required controls |
|-------|------|------|-------------------|
| 0 | Routine | Low impact scope, easy rollback | Basic validation, rollback step |
| 1 | Moderate | User-visible changes, moderate impact | Independent review, negative test, rollback note |
| 2 | Elevated | Security/compliance/data integrity implications | Reviewer assessment, failure-mode checklist, go/no-go checkpoint |
| 3 | Critical | Irreversible actions, regulated/safety-sensitive | Minimal scope, human confirmation, two-step verification, contingency plan |

Tasks at Level 1 and above also run a **failure-mode checklist**:

- What could fail in production?
- How would we detect it quickly?
- What is the fastest safe rollback?
- What dependency could invalidate this plan?
- What assumption is least certain?

### Templates

The skill includes structured templates for consistent output across missions:

- **Mission Brief** — Mission definition with outcome, constraints, scope, and stop criteria
- **Execution Plan** — Task breakdown with owners, dependencies, risk levels, and validation requirements
- **Progress Report** — Checkpoint status with progress, blockers, budget tracking, and risk updates
- **Review** — Adversarial review with challenge summary, checks, and recommendation
- **Mission Report** — Final report with delivered artifacts, decisions, validation evidence, and follow-ups

## Skill file structure

```
.claude/skills/conductor/
├── SKILL.md                                  # Main skill instructions (entrypoint)
├── agents/
│   └── openai.yaml                           # OpenAI agent interface definition
└── references/
    ├── risk-levels.md                        # Risk level definitions and controls
    ├── templates.md                          # Reusable templates for all phases
    └── team-composition.md                   # Mode selection and team sizing rules
```

- `SKILL.md` is the entrypoint that Claude reads when the skill is invoked. It defines the six-step workflow and references the supporting files.
- Files in `references/` contain detailed guidance that Claude loads on demand — they are not all loaded into context at once.

## Customisation

### Modify templates

Edit the templates in `references/templates.md` to match your team's reporting style. The templates use plain text format — adjust fields, add sections, or remove what you don't need.

### Adjust risk levels

Edit `references/risk-levels.md` to change what controls are required at each level. For example, you might require reviewer assessment at Level 1 instead of Level 2 for a security-sensitive project.

### Change team sizing

Edit `references/team-composition.md` to adjust the decision matrix or default team sizes.

## Compatibility notes

- **Subagents** are a stable Claude Code feature and work out of the box.
- **Agent teams** are experimental and disabled by default. To use the `agent-team` execution mode, enable agent teams by adding `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` to your environment or [settings.json](https://code.claude.com/docs/en/settings):

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

Without this setting, Claude will use `single-session` or `subagents` mode instead.

## License

MIT — see [LICENSE](LICENSE) for details.
