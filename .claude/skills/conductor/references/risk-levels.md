# Risk Levels

Classify each task before execution. Apply the minimum required controls.

## Level 0: Routine

Criteria:
- Low impact scope.
- Easy rollback.
- No sensitive data, security, or compliance impact.

Required controls:
- Basic validation evidence.
- Record rollback step.

## Level 1: Moderate

Criteria:
- User-visible behavior changes.
- Moderate reliability or cost impact.
- Partial coupling to other tasks.

Required controls:
- Independent review by non-author agent.
- Validation evidence plus negative test or failure case.
- Explicit rollback note in task output.

## Level 2: Elevated

Criteria:
- Security, privacy, compliance, or data integrity implications.
- High customer or financial impact scope.
- Difficult rollback or uncertain side effects.

Required controls:
- Dedicated reviewer participation.
- Adversarial review with failure-mode checklist.
- Pre-merge or pre-release go/no-go checkpoint by coordinator.
- Staged rollout or guarded launch when possible.

## Level 3: Critical

Criteria:
- Irreversible actions.
- Regulated or safety-sensitive effects.
- Mission failure likely causes severe incident.

Required controls:
- Keep scope minimal and isolate risky changes.
- Require explicit human confirmation before irreversible action.
- Two-step verification and documented contingency plan.
- If controls are unavailable, do not execute.

## Failure-Mode Checklist

Run this checklist for Level 1+ tasks.

- What could fail in production?
- How would we detect it quickly?
- What is the fastest safe rollback?
- What dependency could invalidate this plan?
- What assumption is least certain?
