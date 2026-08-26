# 计划驱动的开发交付闭环

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/agent-skill-111827)](SKILL.md)
[![GitHub stars](https://img.shields.io/github/stars/illuseahashmap/plan-driven-delivery-loop?style=social)](https://github.com/illuseahashmap/plan-driven-delivery-loop)

一个面向 Codex 等编码 Agent 的自适应工作流 skill：从短期计划和长期约束出发，按风险选择流程深度，完成实现、适当验证、计划同步、问题审查和授权 Git 交付，再回到下一项计划。

**[English](README.md)**

## 解决什么问题

编码 Agent 很擅长生成代码，但持续开发时容易出现这些问题：

- 只看眼前需求，偏离长期路线图；
- 需求、设计、任务和测试之间缺少追踪关系；
- 单元测试通过，却没有在真实浏览器中验证；
- 测试未完成就把计划标为完成；
- 未经过问题审查就提交推送；
- 完成一项任务后没有重新读取计划，导致下一轮失去上下文。

本 skill 将这些步骤固定成一个可恢复、可验证的闭环：

```text
选择深度：Fast | Standard | High-risk
→ 查看短期计划和长期约束
→ 需求/设计（深度随风险变化）
→ 编码实现和适当验证
→ 同步状态：done | partial | blocked
→ 问题审查：self | independent | human
→ 授权后提交推送到远端
→ 重新查看短期计划
```

## 核心能力

- 使用稳定 ID 关联需求、任务、测试证据、问题和后续工作；
- 检查路线图、架构原则、里程碑和 ADR 约束；
- 通过 Fast、Standard、High-risk 三种轨道按风险调整流程深度；
- 对需求、设计和任务进行跨工件一致性检查；
- 记录单元测试命令、退出码、数量及基线差异；
- 在真实浏览器里验证用户可见结果、控制台和网络错误；
- 每次验证尝试后同步计划：通过标记 `done`，未完成标记 `partial/in-progress`，无法安全继续标记 `blocked`；
- 使用 `P1/P2/P3` 和 `PASS/CONCERNS/FAIL` 完成问题审查；
- 高风险改动在能力可用时使用独立、只读、限定 diff 的审查；无法独立审查时明确标记限制；
- 在流程开始阶段确认提交和推送授权，避免到交付阶段才发现无法闭环；
- 必需的集成测试或容器测试无法执行时，只能标记为“部分验证”或“阻塞”，不能声称验证完成；
- 只有在获得授权后才允许推送远端，禁止强推和无关提交；
- 推送后重新读取短期计划；若计划文件不存在，则重新核对用户请求、仓库规则和本轮工作摘要，再确认下一项依赖已满足的任务。

## 安装

使用 Skills CLI：

```bash
npx skills add illuseahashmap/plan-driven-delivery-loop@plan-driven-delivery-loop
```

Codex 也可以手动安装：

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/illuseahashmap/plan-driven-delivery-loop.git \
  "$HOME/.agents/skills/plan-driven-delivery-loop"
```

`$HOME/.agents/skills` 是 OpenAI 当前推荐的用户级目录。旧版 Codex 可能使用 `$HOME/.codex/skills`；只有当前版本无法发现 `.agents/skills` 时才使用旧目录，并避免同时安装两份同名 skill。若安装后未出现，请重启 Codex。

## 使用

```text
使用 $plan-driven-delivery-loop，执行当前仓库短期计划中的下一项可执行任务。
```

该 skill 仅支持显式调用，不会因普通的功能开发、缺陷修复或重构请求自动触发。

如需完成远端交付，请明确授权：

```text
使用 $plan-driven-delivery-loop 完成当前任务，包括问题审查、创建聚焦提交并推送到当前上游；本请求授权该范围内的 Git 交付。
```

## 工作流模板

仓库没有计划文件时，Fast 模式直接使用用户请求和仓库规则，不强制创建计划体系；Standard 模式可使用轻量工作摘要；High-risk 模式只有在缺失决策会实质影响安全或设计时才暂停。

- [工件模板](references/artifact-templates.md)
- [阶段检查清单](references/stage-checklists.md)

## 安全边界

该 skill 不会自动获得部署、发送外部消息、删除无关文件、改写 Git 历史或推送远端的权限。仅调用 skill 不代表授权远端写入。skill 会在流程开始阶段确认提交和推送意图；远端写入必须由用户或仓库工作流明确授权。

## 设计参考

- [BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD)：直接入口与完整规划入口并存，按上下文选择深度；
- [OpenSpec](https://github.com/Fission-AI/OpenSpec)：以动作代替刚性阶段，计划和任务状态可在实施中持续更新；
- [GitHub Spec Kit](https://github.com/github/spec-kit)：仅在存在实质歧义时增加澄清、检查清单和一致性分析门禁；
- [Superpowers](https://github.com/obra/superpowers)：使用独立、只读、限定 Git diff 的代码审查上下文。

## 贡献

欢迎提交 Issue 和 Pull Request。提出流程变更前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可证

[MIT](LICENSE)
