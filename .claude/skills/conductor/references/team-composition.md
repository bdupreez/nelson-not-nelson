# Team Composition Reference

Use this file to choose execution mode and team size.

## Mode Selection

Choose the first condition that matches.

1. If work is sequential, tightly coupled, or mostly in the same files, use `single-session`.
2. If work is parallel but each worker only needs to report to coordinator, use `subagents`.
3. If workers must coordinate directly across task boundaries, use `agent-team`.

## Decision Matrix

| Condition | Preferred Mode | Why |
| --- | --- | --- |
| Single critical path, low ambiguity | `single-session` | Lowest coordination overhead |
| Parallel discovery, synthesis by coordinator | `subagents` | Fast throughput without peer chatter |
| Parallel implementation with dependencies | `agent-team` | Supports agent-to-agent coordination |
| High threat or high impact scope | `agent-team` + reviewer | Adds explicit control points |

## Team Sizing (up to 10 total)

- Small mission: `1 coordinator + 2-3 agents`.
- Medium mission: `1 coordinator + 4-5 agents`.
- Large mission: `1 coordinator + 6-7 agents`.
- Add `1 reviewer` at medium/high threat.
- Keep one coordinator only.

## Role Guide

- `coordinator`: Defines mission brief, delegates, tracks dependencies, resolves blockers, final synthesis.
- `agent`: Owns assigned task and artifacts.
- `reviewer`: Challenges assumptions, validates outputs, checks rollback readiness.

## Anti-Patterns

- Creating teams for work that is mostly linear.
- Splitting one file across multiple agents.
- Letting reviewer become a ticket queue.
- Adding agents without reducing critical path length.
