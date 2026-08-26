---
name: plan-driven-delivery-loop
description: Run an adaptive, plan-driven software delivery loop that scales requirements, design, verification, review, and authorized Git delivery to the change's risk.
metadata:
  short-description: Adaptive plan-driven delivery loop
---

# Plan-Driven Delivery Loop

Use this skill when the user wants a feature, bug fix, refactor, or other software change managed as a complete delivery loop. Preserve this order of concerns:

**查看短期计划 → 查看长期计划约束 → 需求/设计 → 编码实现 → 适当验证 → 同步计划状态 → 问题审查 → 授权 Git 交付 → 查看短期计划**

The quality gates are invariant; the ceremony is not. Merge adjacent concerns for small work when no decision, evidence, or authorization boundary is hidden. Do not emit eleven headings or create workflow artifacts merely to prove compliance. Expand the process when ambiguity, blast radius, irreversibility, or repository policy makes additional evidence valuable.

At the start, choose an execution track and establish a resumable checkpoint containing: selected task ID, current concern, short- and long-term plan paths, branch, base commit, and Git delivery intent. Record delivery mode as `remote-authorized`, `local-only`, or `pending`, plus separate `yes` / `no` / `pending` authorization for commit and push. A direct request to commit and push, deliver to the remote, or execute the complete loop through remote delivery authorizes only the focused changes for the identified branch and upstream. Merely invoking this skill or asking to fix, solve, or complete a task does not authorize remote writes. Surface pending intent near the start rather than surprising the user at delivery.

## Execution tracks

- **Fast** — for contained, reversible, well-understood changes such as documentation, styling, configuration, or a small defect with narrow impact. Combine plan review, acceptance criteria, and design into one concise checkpoint; run only relevant targeted verification; use a focused self-review unless repository policy requires more. Do not create standalone workflow documents unless the repository already requires them.
- **Standard** — the default for ordinary features, fixes, and refactors. Keep requirements and design distinguishable, run targeted plus relevant broader verification, synchronize plans, and perform a substantive review. Use an independent reviewer when the change is cross-cutting or a fresh context would materially improve confidence.
- **High-risk** — for authentication/authorization, secrets, payments, destructive or irreversible behavior, migrations, sensitive data, public contracts, concurrency, cross-service changes, broad blast radius, or materially ambiguous requirements. Make each concern and its evidence explicit, include rollback/migration analysis, run the strongest applicable verification, and require independent review when that capability is available or repository policy requires it.

When uncertain, choose `Standard`. Escalate tracks when implementation reveals greater risk; never use a lower track to bypass a failing gate. Track choice changes depth and reporting, not the obligation to understand plans, verify relevant behavior, synchronize status, review the final change, and respect delivery authorization.

## Operating loop

Move through these concerns in order. A Fast-track update may combine several concerns in one action and one report.

1. **计划与授权** — read the active short-term plan from disk, identify the dependency-ready task and definition of done, then read the applicable roadmap, architecture, milestone, and ADR constraints. Resolve conflicts before coding. Record track choice, branch/base, and delivery authorization.
2. **需求与设计** — define observable acceptance criteria and the smallest coherent approach. Fast work may use a few inline bullets. Standard/High-risk work should make scope, non-goals, affected contracts, failure behavior, verification, and consequential alternatives traceable. Obtain a design decision when a missing choice materially changes the solution.
3. **编码实现** — establish a clean or understood baseline, preserve unrelated user changes, and implement in reviewable increments. Prefer RED → GREEN → REFACTOR for behavior changes when practical; never weaken tests to hide a defect.
4. **适当验证** — select evidence from the acceptance criteria and risk, not from a fixed ritual. Run targeted unit tests and relevant lint/type/build checks; add broader, integration, API, container, or real-browser verification when the changed behavior requires it. User-facing behavior needs a real browser and explicit visible oracle. Backend-only work may mark browser testing `N/A` with equivalent integration/API evidence. Record exact commands, outcomes, skipped checks, and baseline failures. A required check that cannot run makes verification `partial` or `blocked`, never complete.
5. **同步计划状态** — update the short-term plan immediately after the verification attempt, regardless of outcome. Use `done` only when required acceptance criteria and verification pass; use `in-progress` or `partial` when useful work exists but evidence or implementation remains incomplete; use `blocked` when safe progress cannot continue, with owner, reason, and unblock trigger. Update long-term plans only when roadmap, milestone, architecture direction, or constraints actually changed.
6. **问题审查** — keep verification and review distinct: verification demonstrates behavior; review challenges scope, design, correctness, regressions, security/privacy, compatibility, failure handling, maintainability, test validity, evidence gaps, and unrelated changes. Record `review_mode: self | independent | human`, findings as `P1/P2/P3`, and `PASS/CONCERNS/FAIL`. One correctly scoped review satisfies the gate for that diff; re-review only findings and changes made after it.
7. **授权 Git 交付** — proceed only on `PASS`, or `CONCERNS` with explicit acceptance of residual risk. Re-check separate commit and push authorization, staged scope, secrets/generated artifacts, branch, remote, and upstream. Create a focused commit and push only within authorization. Never force-push, rewrite shared history, bypass required hooks, or include unrelated changes. Verify the remote ref and record the commit SHA.
8. **回到短期计划** — re-read the active short-term plan from disk after delivery or a recorded local/blocked handoff. Confirm the task state and evidence, identify the next dependency-ready item, and stop unless the user requested another cycle.

## Review independence

For independent review, dispatch a separate reviewer Agent/context when available. Give it the requirements or plan, base and head SHAs (or bounded diff), changed files, and raw verification evidence—not the implementer's private reasoning or a conclusion to confirm. The review is read-only; the implementing Agent applies fixes. Independent review is required for High-risk work when reviewer capability exists, and recommended for consequential Standard work.

If independent review is unavailable, perform a fresh diff-based self-review and label it `self (independence unavailable)`. Never claim independence. For High-risk delivery, disclose the limitation and obtain explicit acceptance unless repository policy provides an equivalent human or CI review gate.

## Stage gates

- Do not begin development while the short-term/long-term plan relationship, goal, or acceptance criteria is materially ambiguous for the selected track.
- Do not treat a plan file's existence as proof that it was reviewed; report the files and constraints actually read.
- Do not let requirement, task, plan, test, and browser evidence IDs drift; traceability must survive handoff and resumption.
- Do not mark unit or browser testing complete when a relevant check failed, was skipped, or could not run; state the blocker and its impact.
- Do not call verification complete when a required integration or container-backed check could not run; use `partial` or `blocked` and name the unblock action.
- Do not mark requirements complete if any acceptance criterion has no test or verification evidence.
- Always synchronize the short-term plan after a verification attempt. Do not claim completion before required verification passes or is validly marked N/A with evidence.
- Do not mark a task `done` while its required plan/dashboard synchronization is stale.
- Do not commit or push while issue review is `FAIL`, while any P1 remains, or while a P2 lacks a fix or explicit risk decision.
- A failed unit test returns to coding; a failed browser test returns to coding; a missing or contradictory requirement returns to requirements; a design flaw returns to design.
- Do not push unless remote-write authorization is present. Never force-push or include unrelated changes.
- Resolve and report Git delivery intent near the start. Skill invocation alone is not commit or push authorization, and missing authorization must not be disclosed only at the delivery concern.
- Keep authorization boundaries unchanged: the loop does not authorize deployment, external messages, destructive cleanup, or unrelated refactors.

## Artifacts

Prefer existing project conventions. If none exist and the selected track benefits from durable artifacts, keep them under a clearly named, reviewable directory such as `docs/workflows/<change-id>/` and use [references/artifact-templates.md](references/artifact-templates.md). Plan files are source-of-truth records, not disposable scratch notes. Fast work normally reuses existing plans and the final handoff instead of creating a new artifact directory.

## Progress reporting

Report meaningful transitions, decisions, blockers, and evidence—not every merged concern. Fast work normally needs a concise outcome summary rather than stage-by-stage narration. At handoff, include only applicable items:

- requirement and acceptance-criteria summary;
- short-term and long-term plan files reviewed and updated;
- workflow stage status and artifact locations;
- execution track and why it fit;
- requirement-to-test/evidence traceability;
- files or components changed;
- unit-test commands and results;
- browser-test route, steps, evidence, and result (or N/A justification);
- documentation and plan updates;
- issue-review decision, findings, and dispositions;
- review mode and any independence limitation;
- commit authorization, push authorization, target branch, and upstream;
- commit hash, remote, push result, or explicit reason push was not performed;
- explicit follow-up requirements or “none identified.”

For detailed track selection and gate checks, read [references/stage-checklists.md](references/stage-checklists.md) when the work is Standard, High-risk, blocked, cross-cutting, or resumed after context loss.
