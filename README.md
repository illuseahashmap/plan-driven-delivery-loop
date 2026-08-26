# Plan-Driven Delivery Loop

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/agent-skill-111827)](SKILL.md)
[![GitHub stars](https://img.shields.io/github/stars/illuseahashmap/plan-driven-delivery-loop?style=social)](https://github.com/illuseahashmap/plan-driven-delivery-loop)

An adaptive agent skill for shipping software through a traceable, plan-driven loop—from roadmap constraints to risk-sized verification, review, and authorized Git delivery.

**[中文文档](README.zh-CN.md)**

## Why this skill?

Coding agents are good at producing code, but long-running work often drifts away from the roadmap, skips real browser verification, updates plans too early, or pushes before the change is reviewed.

This skill turns delivery into an evidence-based loop:

```text
Select depth: Fast | Standard | High-risk
                      ↓
Short-term plan → Long-term constraints
                      ↓
Requirements/design (depth follows risk)
                      ↓
Implementation → Relevant verification
                      ↓
Sync status: done | partial | blocked
                      ↓
Review: self | independent | human
                      ↓
Authorized commit & push
                      ↓
Re-read short-term plan → next task / repeat
```

## What it enforces

- Short-term tasks are selected only when their dependencies are satisfied.
- Long-term roadmap, architecture, milestone, and ADR constraints are checked before design.
- Fast, Standard, and High-risk tracks right-size ceremony without removing quality gates.
- Requirements use stable IDs and observable acceptance criteria.
- Design tasks map back to requirements and include verification methods.
- Implementation stays focused and preserves unrelated changes.
- Unit-test results include commands, exit status, counts, and baseline comparison.
- User-facing changes are exercised in a real browser with an explicit success oracle.
- Plans are synchronized after every verification attempt: `done` on success, `partial`/`in-progress` for incomplete work, and `blocked` when safe progress cannot continue.
- Issue review uses `P1` / `P2` / `P3` findings and `PASS` / `CONCERNS` / `FAIL` decisions.
- High-risk work uses an independent, read-only, diff-scoped reviewer when available; self-review is labeled honestly when independence is unavailable.
- Commit and push intent is confirmed near the start, so delivery cannot become a last-minute surprise.
- Missing required integration or container tests are reported as partial/blocked verification, never as a complete pass.
- Remote pushes require explicit authorization; force-push and unrelated changes are prohibited.
- The loop ends by re-reading the short-term plan from disk and identifying the next actionable task.

## Install

With the Skills CLI:

```bash
npx skills add illuseahashmap/plan-driven-delivery-loop@plan-driven-delivery-loop
```

For Codex, you can also install manually:

```bash
git clone https://github.com/illuseahashmap/plan-driven-delivery-loop.git
cp -R plan-driven-delivery-loop ~/.codex/skills/plan-driven-delivery-loop
```

The skill becomes available on the next agent turn.

## Use

```text
Use $plan-driven-delivery-loop to implement the next actionable item in this repository.
```

Or invoke it naturally:

```text
Follow the plan-driven delivery loop for this feature, including unit tests,
real browser verification, issue review, plan updates, a focused commit, and a
push to the current upstream. This request authorizes that scoped Git delivery.
```

## Workflow artifacts

The skill prefers existing repository conventions. When a project has no workflow format, it provides reusable templates for:

- plan checkpoints and resumable stage state;
- requirements and acceptance criteria;
- design decisions and cross-artifact consistency;
- dependency-aware task breakdowns;
- unit-test and browser-test evidence;
- issue-review findings and delivery records.

See [artifact templates](references/artifact-templates.md) and [stage checklists](references/stage-checklists.md).

## Safety boundaries

This workflow does not grant permission to deploy, send external messages, rewrite Git history, remove unrelated files, or push to a remote. Invoking the skill alone is not remote-write authorization. The skill checks commit and push intent near the start; remote writes must be explicitly authorized by the user or repository workflow.

## Inspired by

The workflow adapts proven patterns from:

- [BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD) — direct and planned entry paths with depth chosen from context.
- [OpenSpec](https://github.com/Fission-AI/OpenSpec) — actions rather than rigid phases, living task state, and optional verification paths.
- [GitHub Spec Kit](https://github.com/github/spec-kit) — optional clarification, checklist, and analysis gates for meaningful ambiguity.
- [Superpowers](https://github.com/obra/superpowers) — independent, read-only review against requirements and a bounded Git diff.
- [Playwright](https://github.com/microsoft/playwright)

It remains a small, standalone skill and does not require those projects.

## Contributing

Issues and pull requests are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before proposing workflow changes.

## License

[MIT](LICENSE)
