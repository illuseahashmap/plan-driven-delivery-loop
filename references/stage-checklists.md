# Stage checklists

Use these checks for Standard, High-risk, cross-cutting, blocked, or resumed work. Prefer project-specific rules when they are stricter.

## Track selection

- Choose **Fast** only when the change is well understood, contained, reversible, and has no high-risk trigger. Keep one concise acceptance/design checkpoint and targeted evidence; do not manufacture artifact files or stage reports.
- Choose **Standard** for ordinary features, fixes, and refactors. Keep requirements and design distinguishable and run targeted plus relevant broader verification.
- Choose **High-risk** for auth/permissions, secrets, payments, destructive behavior, migrations, sensitive data, public contracts, concurrency, cross-service work, broad blast radius, or material ambiguity. Require explicit rollback/migration reasoning, strongest applicable verification, and independent review when available.
- Escalate immediately if risk grows. Do not de-escalate merely because a required check is inconvenient.

## Plan intake

- Record the exact short-term and long-term plan paths and last known task state.
- Record delivery mode as `remote-authorized`, `local-only`, or `pending`; record commit and push authorization separately as `yes`, `no`, or `pending`, together with the target branch and upstream.
- Treat explicit requests to commit and push, deliver to the remote, or complete the loop through remote delivery as scoped authorization. Skill invocation or a generic request to fix/solve work is not remote-write authorization.
- If Git delivery intent is pending, tell the user and ask once near the start instead of surprising them at the delivery gate.
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
- Call verification `partial` or `blocked` when a required integration, container, or broader check cannot run; record the missing dependency, owner, and unblock action.
- Never modify assertions, snapshots, or fixtures solely to hide a product defect.

## Browser-test gate

- Identify base URL, startup process, auth state, seeded data, viewport, and acceptance oracle before interacting.
- Use semantic/user-facing locators and accessibility snapshots where possible.
- Re-observe page state after navigation or significant DOM changes.
- Check visible result plus console and failed network activity relevant to the flow.
- Capture screenshots, traces, or concise observations on failure; avoid exposing cookies, tokens, or unrelated page data.
- Re-run failed criteria after fixes. Promote stable critical journeys to deterministic E2E tests.

## Plan synchronization gate

- Synchronize short-term status after every verification attempt, not only after success.
- Use `done` only when required acceptance criteria and verification pass.
- Use `in-progress` or `partial` when work or evidence remains incomplete but a safe next action exists.
- Use `blocked` when safe progress cannot continue; record owner, cause, impact, and unblock trigger.
- Record verification failures and environment limitations as evidence rather than leaving the plan stale.
- Change long-term plans only for genuine roadmap, milestone, architecture, or constraint impact.

## Issue-review gate

- Keep review separate from test execution: tests establish behavior; review challenges the change and its evidence.
- Record `review_mode: self | independent | human`.
- For independent review, provide requirements/plan, base/head or bounded diff, changed files, and raw evidence. Do not provide the implementer's session history or intended verdict.
- Keep the reviewer read-only. The implementing Agent owns fixes.
- If no independent reviewer is available, label the result `self (independence unavailable)`; High-risk delivery needs explicit acceptance of that limitation unless an equivalent repository gate exists.
- Review requirements and long-term constraints against the final diff and evidence, not against the implementation summary alone.
- Check correctness, regressions, edge cases, security/privacy, compatibility, failure handling, maintainability, test validity, browser console/network results, documentation, plan synchronization, and scope creep.
- Label findings `P1` (blocking), `P2` (important), or `P3` (minor); include location/evidence, impact, and disposition.
- Return each P1/P2 to the responsible stage and rerun affected verification after fixes.
- Allow delivery only on `PASS`, or `CONCERNS` when residual risk is explicit and accepted by the authorized decision maker.
- Require complete mandatory verification evidence for `PASS`; an environment-blocked required check cannot be silently treated as a pass.
- One review covers one bounded diff. Re-review only unresolved findings and post-review changes; do not duplicate an already satisfied review gate.

## Delivery gate

- Before commit: re-check local commit authorization, diff scope, staged paths, secrets, generated files, tests, browser evidence, plan synchronization, and issue-review disposition.
- Before push: confirm authorization, branch, remote, and upstream. Never force-push or bypass required hooks.
- After push: record commit SHA and verify the remote ref. Re-read the short-term plan from disk and identify the next dependency-ready task.
