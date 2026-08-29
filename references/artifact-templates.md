# Workflow artifact templates

Use only the artifacts that fit the selected track. Keep IDs stable across the workflow so requirements, tasks, tests, and review findings can refer to one another.

## Plan checkpoint

```markdown
## Short-term plan
- File: <path>
- Selected task: <stable ID/title>
- Before: <status, priority, dependencies, blockers, definition of done>
- After: <status, evidence, next action>

## Long-term constraints
- File(s): <path(s)>
- Relevant milestone/principle: <text or ID>
- Constraints applied: <summary>
- Conflict/decision: <none or decision reference>

## Workflow checkpoint
- Execution track: <Fast | Standard | High-risk; reason>
- Current concern: <plan | shape | implement | verify | sync | review | deliver | resume>
- Branch/base commit: <branch>/<SHA>
- Delivery mode: <remote-authorized | local-only | pending>
- Commit authorization: <yes | no | pending; evidence>
- Push authorization: <yes | no | pending; remote/upstream>
- Requirement/task/evidence IDs: <IDs or artifact path>
- Verification status: <complete | partial | blocked; reason/next action>
- Review mode: <self | independent | human; reviewer/context or limitation>
```

Record this checkpoint at the start and repeat the short-term section after delivery or a blocked/local handoff.

## Requirements / spec

```markdown
# <change title>

## Goal
<observable outcome and user/value impact>

## Scope
- In scope:
- Out of scope:

## Constraints and risks
- Constraints:
- Risks:

## Acceptance criteria
- AC-01: <testable behavior>
  - Scenario: <given/when/then or equivalent>
  - Planned evidence: <unit/browser/integration/manual>
- AC-02: ...

## User journey contract
- Actor and goal: <who is trying to achieve what>
- Realistic start: <where/how the actor normally begins>
- Discoverable entry: <navigation, command, API, link, or integration surface>
- Preconditions: <auth, role, data, configuration, device; how the user satisfies them>
- Critical path: <entry → action → feedback → next action → closure>
- Completion signal: <what tells the user the goal succeeded>
- Downstream use: <what the user can now do with the result>
- Recovery: <cancel, retry, back, correction, or support path>
- Intentional bypasses in verification: <none or unrelated setup with justification/evidence>

## Open decisions
- DEC-01: <question, options, decision owner/status>
```

Acceptance criteria must be observable. Avoid criteria that only describe an implementation choice.

## Design / technical decisions

```markdown
# Design: <change title>

## Selected approach
<how the change works and why>

## Alternatives considered
- <alternative>: <reason rejected>

## Affected surfaces
- Files/modules:
- API/contracts/data:
- Configuration:
- Failure/security/privacy:
- Operations/migration/rollback:

## Decisions
- DEC-01: <decision and rationale>

## Cross-artifact check
- Requirements covered by tasks: PASS | FAIL
- Tasks justified by requirements: PASS | FAIL
- Long-term constraints satisfied: PASS | FAIL
```

## Task breakdown

```markdown
- [ ] T-01 <task with likely file path> — depends: none — covers: AC-01 — verify: <command/check>
- [ ] T-02 <task with likely file path> — depends: T-01 — covers: AC-01, AC-02 — verify: <command/check>
- [ ] T-03 <plan/documentation task> — depends: T-02 — covers: <change impact> — verify: <check>
```

## Traceability and review

```markdown
| Requirement | Evidence | Result |
|-------------|----------|--------|
| AC-01       | test/path::name | PASS |
| AC-02       | manual: <check> | PASS / FAIL / BLOCKED |

## Unit tests
- Command: `<command>`
- Result: PASS | FAIL | BLOCKED
- Exit/passed/failed/skipped: <values>
- Baseline comparison: <same/new/unrelated>
- Evidence: <summary or report path>

## Browser test
- Route/URL: <route or N/A>
- Preconditions: <server/auth/data/viewport>
- Realistic start and entry: <landing surface and how the user reaches the capability>
- Steps: <concise user-visible steps>
- Oracle: <explicit expected user-visible outcome>
- Closure/downstream proof: <how the result is confirmed and remains usable in the next step>
- Setup bypasses: <none or unrelated preconditions bypassed, with justification>
- Result: PASS | FAIL | BLOCKED | N/A
- Console/network: <clean or relevant errors>
- Evidence: <screenshot/trace/report/observed result>
- N/A reason and equivalent verification: <required when N/A>

## Issue review

- Review mode: self | independent | human
- Reviewer/context: <identifier or independence limitation>

### Decision
PASS | CONCERNS | FAIL

### Findings
- [P1/P2/P3] <finding> — <file/line or evidence> — <impact> — <disposition>

### Review coverage
- Requirements/design: <result>
- Correctness/regressions/edge cases: <result>
- Security/privacy/compatibility/failure handling: <result>
- Unit/browser evidence: <result>
- Documentation/plans/traceability: <result>
- Diff scope/secrets/generated artifacts: <result>

## Follow-up requirements
- REQ-NEXT-01: <concrete deferred or discovered requirement>

## Delivery
- Commit: <hash>
- Branch/upstream: <values>
- Remote/push: <remote ref and verified result, or reason not pushed>
- Final short-term plan check: <completed status, next dependency-ready item, blockers>
```

Use `CONCERNS` for accepted residual risk or incomplete evidence that does not block the requested handoff. Use `FAIL` when the change cannot be safely accepted.
