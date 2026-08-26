---
name: plan-driven-delivery-loop
description: Run plan-driven software delivery from short- and long-term plan review through requirements, design, implementation, unit and browser verification, plan updates, authorized Git delivery, and final plan resumption.
metadata:
  short-description: Plan-driven development and delivery loop
---

# Plan-Driven Delivery Loop

Use this skill when the user wants a feature, bug fix, refactor, or other software change managed as a complete delivery loop. The canonical loop is fixed:

**查看短期计划 → 查看长期计划约束 → 需求分析 → 方案设计 → 编码实现 → 单元测试 → 实际浏览器测试 → 更新短期计划/长期计划 → 问题审查 → 提交推送到远端 → 查看短期计划**

Keep the loop visible and leave the repository and planning records in a reviewable state. For backend-only work, the browser-test stage may be marked `N/A` only with a concrete reason and an equivalent integration or API verification.

At the start, establish a resumable workflow checkpoint containing: selected task ID, current stage, short-term plan path, long-term plan path(s), branch, and base commit. Also record Git delivery mode as `remote-authorized`, `local-only`, or `pending`, plus separate `yes` / `no` / `pending` authorization for commit and push. A direct request to commit and push, deliver to the remote, or execute the complete loop through remote delivery authorizes only the focused changes for the identified branch and upstream. Merely invoking this skill or asking to fix, solve, or complete a task does not authorize remote writes. If delivery mode is `pending`, surface that fact and ask once near the start; do not wait until the delivery stage to reveal that the loop cannot finish. Continue safe local stages when useful while keeping the pending gate visible. Update the checkpoint as stages complete. Use stable IDs for requirements, tasks, evidence, blockers, and follow-ups.

## Operating loop

Run every stage in this order. Do not skip or reorder a stage without recording why.

1. **查看短期计划** — locate the project's active short-term plan using existing conventions (`TODO`, sprint/task/status files, `plans/`, or project-specific instructions). Identify the selected task's stable ID, state, dependencies, blockers, priority, and definition of done. Select only an actionable task whose dependencies are satisfied; do not silently switch tasks. Re-read from disk rather than relying on conversation memory.
2. **查看长期计划约束** — locate the roadmap, product plan, architecture principles/constitution, milestones, ADRs, or other long-term records. Extract relevant constraints, priorities, non-goals, compatibility expectations, and target milestones into a constraint checklist. Link each applicable constraint to the requirement or design decision it governs. If plans conflict, stop for a decision before coding.
3. **需求分析** — define WHAT and WHY before HOW: goal, user impact, scope, non-goals, user stories/scenarios, observable acceptance criteria, risks, and unresolved questions. Give each acceptance criterion a stable ID. Check ambiguity, contradictions, missing edge cases, and consistency with both plans. If a missing decision materially changes the solution, pause before design.
4. **方案设计** — record the selected approach, alternatives rejected, affected files/modules, interfaces/contracts, data model, failure behavior, security/privacy implications, migration/rollback, and ordered tasks with dependencies. Map every task to acceptance criteria and include its verification method and exact likely file paths. Run a cross-artifact check: every requirement must be covered, every task justified, and every long-term constraint respected. For consequential changes, obtain approval at this gate before coding.
5. **编码实现** — verify a clean or understood baseline before edits. Implement in small, reviewable increments, preserving unrelated user changes and existing conventions. For behavior changes, prefer a nested RED → GREEN → REFACTOR loop inside this stage when practical; never weaken tests merely to make implementation pass. After each increment, update task state and inspect the focused diff.
6. **单元测试** — run targeted tests first, then the relevant broader suite plus lint/type/build checks when applicable. Record exact commands, exit status, passed/failed/skipped counts, and report paths. Prove new tests can detect the changed behavior when practical; a test never observed failing may be a false-positive test. Classify failures as implementation defect, test defect, environment blocker, or unrelated baseline failure, with evidence. If a required integration, container, or broader test cannot run, describe verification as partial or blocked—not complete—and name the missing environment, owner, and unblock action.
7. **实际浏览器测试** — inspect the app's browser-test configuration, base URL, startup command, authentication, and seeded data before testing. Use a real browser and user-facing locators/accessibility snapshots. For each browser acceptance criterion, define an explicit oracle and verify visible outcome, navigation, interaction states, console errors, failed network requests, and relevant responsive/authenticated states. Re-snapshot after state changes; capture bounded screenshots/traces on failure. Stable critical flows should graduate to scripted Playwright/E2E coverage. For non-UI work, record `N/A` plus equivalent integration/API verification.
8. **更新短期计划/长期计划** — only after verification passes, update the short-term task using explicit states such as `planned`, `in-progress`, `blocked`, `later`, `done`, or the project's equivalents. Record evidence, blocker owner/trigger, residual risk, and next action. Update generated dashboards/summaries in the same change when the project requires them. Update long-term plans only when scope, milestone, architecture direction, or constraints changed; preserve history and explain why.
9. **问题审查** — review the complete change against short- and long-term plans, requirements, design, code diff, unit-test evidence, browser evidence, documentation, and plan updates. Check correctness, regressions, edge cases, security/privacy, compatibility, failure handling, maintainability, test validity, browser-console/network findings, traceability, and unrelated scope. Record findings as `P1` blocking, `P2` important, or `P3` minor, with file/evidence location and disposition. Return P1/P2 findings to the responsible earlier stage; do not proceed while blocking findings remain. The decision must be `PASS`, `CONCERNS`, or `FAIL`. `PASS` requires all required verification evidence. Missing required evidence due to environment limitations must be classified as a blocker or `CONCERNS`; delivery may continue only when repository policy allows it and the authorized decision maker explicitly accepts the stated residual risk.
10. **提交推送到远端** — proceed only after issue review returns `PASS`, or `CONCERNS` with explicitly accepted residual risk. Re-check the checkpoint's separate commit and push authorization states before mutating Git history or the remote. Check `git status`, staged paths, secrets/generated artifacts, branch, remote, and upstream. Create a focused commit only when local commit delivery is authorized, using repository conventions and including plan/test/review changes needed for traceability. Push only when remote writes are authorized. Never force-push, rewrite shared history, bypass required hooks, or include unrelated changes. After push, verify remote branch/upstream and record commit SHA; if either action is unauthorized or failed, report the exact local/remote state without claiming the loop is complete.
11. **查看短期计划** — re-read the active short-term plan from disk after commit/push. Confirm the completed task state and evidence, choose the next actionable item only from tasks with satisfied dependencies, and report blockers, deferred items, and the loop's next entry point. Do not begin the next implementation cycle without a new or continuing request.

## Stage gates

- Do not begin development while the short-term/long-term plan relationship, goal, or acceptance criteria is materially ambiguous.
- Do not treat a plan file's existence as proof that it was reviewed; report the files and constraints actually read.
- Do not let requirement, task, plan, test, and browser evidence IDs drift; traceability must survive handoff and resumption.
- Do not mark unit or browser testing complete when a relevant check failed, was skipped, or could not run; state the blocker and its impact.
- Do not call verification complete when a required integration or container-backed check could not run; use `partial` or `blocked` and name the unblock action.
- Do not mark requirements complete if any acceptance criterion has no test or verification evidence.
- Do not update plans to claim completion before unit and browser testing have passed or been explicitly marked N/A with evidence.
- Do not mark a task `done` while its required plan/dashboard synchronization is stale.
- Do not commit or push while issue review is `FAIL`, while any P1 remains, or while a P2 lacks a fix or explicit risk decision.
- A failed unit test returns to coding; a failed browser test returns to coding; a missing or contradictory requirement returns to requirements; a design flaw returns to design.
- Do not push unless remote-write authorization is present. Never force-push or include unrelated changes.
- Resolve and report Git delivery intent near the start. Skill invocation alone is not commit or push authorization, and missing authorization must not be disclosed only at stage 10.
- Keep authorization boundaries unchanged: the loop does not authorize deployment, external messages, destructive cleanup, or unrelated refactors.

## Artifacts

Prefer existing project conventions. If none exist, keep workflow artifacts under a clearly named, reviewable directory such as `docs/workflows/<change-id>/` and use the templates in [references/artifact-templates.md](references/artifact-templates.md). Plan files are source-of-truth records, not disposable scratch notes. For detailed stage checks, read [references/stage-checklists.md](references/stage-checklists.md) when a task is cross-cutting, risky, blocked, or being resumed after context loss.

## Progress reporting

At each stage, briefly state: current stage, evidence produced, decision or issue, and next transition. At handoff, include:

- requirement and acceptance-criteria summary;
- short-term and long-term plan files reviewed and updated;
- workflow stage status and artifact locations;
- requirement-to-test/evidence traceability;
- files or components changed;
- unit-test commands and results;
- browser-test route, steps, evidence, and result (or N/A justification);
- documentation and plan updates;
- issue-review decision, findings, and dispositions;
- commit authorization, push authorization, target branch, and upstream;
- commit hash, remote, push result, or explicit reason push was not performed;
- explicit follow-up requirements or “none identified.”

For a small change, stages may be lightweight but must still be acknowledged. For a risky change, expand the requirements, test, and review gates rather than skipping them.
