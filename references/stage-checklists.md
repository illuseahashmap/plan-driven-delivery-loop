# Stage checklists

Use these checks for cross-cutting, risky, blocked, or resumed work. Prefer project-specific rules when they are stricter.

## Plan intake

- Record the exact short-term and long-term plan paths and last known task state.
- Keep task IDs immutable once requirements, commits, or tests refer to them.
- Treat `blocked` as requiring an owner and unblock trigger; treat `later` as deferred work with a resume condition, not as completed work.
- If a summary/dashboard is generated from plans, update it through the project's generator rather than editing it manually.

## Requirements and design gate

- Requirements describe observable behavior and value; design describes implementation choices.
- Each acceptance criterion has at least one planned verification method.
- Each design task names dependencies, likely files, acceptance criteria, and verification.
- Check cross-artifact consistency before implementation: no orphan requirement, unjustified task, unresolved contradiction, or violated long-term constraint.
- For a significant change, stop after presenting the design until the required approval is available.

## Implementation and unit-test gate

- Capture baseline test failures before attributing them to the change.
- Prefer small increments. After each increment, inspect the diff and update the task checklist.
- For testable behavior, prove the test fails for the missing/broken behavior before relying on its passing result when practical.
- Run targeted tests before the broader suite. Record skipped tests and environmental limitations separately from passes.
- Never modify assertions, snapshots, or fixtures solely to hide a product defect.

## Browser-test gate

- Identify base URL, startup process, auth state, seeded data, viewport, and acceptance oracle before interacting.
- Use semantic/user-facing locators and accessibility snapshots where possible.
- Re-observe page state after navigation or significant DOM changes.
- Check visible result plus console and failed network activity relevant to the flow.
- Capture screenshots, traces, or concise observations on failure; avoid exposing cookies, tokens, or unrelated page data.
- Re-run failed criteria after fixes. Promote stable critical journeys to deterministic E2E tests.

## Plan synchronization gate

- Update short-term status, evidence, residual risk, blockers, and next action in one coherent change.
- Change long-term plans only for genuine roadmap, milestone, architecture, or constraint impact.

## Issue-review gate

- Review requirements and long-term constraints against the final diff and evidence, not against the implementation summary alone.
- Check correctness, regressions, edge cases, security/privacy, compatibility, failure handling, maintainability, test validity, browser console/network results, documentation, plan synchronization, and scope creep.
- Label findings `P1` (blocking), `P2` (important), or `P3` (minor); include location/evidence, impact, and disposition.
- Return each P1/P2 to the responsible stage and rerun affected verification after fixes.
- Allow delivery only on `PASS`, or `CONCERNS` when residual risk is explicit and accepted by the authorized decision maker.

## Delivery gate

- Before commit: check diff scope, staged paths, secrets, generated files, tests, browser evidence, plan synchronization, and issue-review disposition.
- Before push: confirm authorization, branch, remote, and upstream. Never force-push or bypass required hooks.
- After push: record commit SHA and verify the remote ref. Re-read the short-term plan from disk and identify the next dependency-ready task.
