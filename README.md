<p align="center">
  <img src="./assets/cover.svg" alt="Superpowers Quality Extension cover" width="100%">
</p>

# Superpowers Quality Extension

`superpowers-quality-extension` is a quality-gate Skill for Coding Agents. It extends Superpowers by turning durable software design ideas into checks an agent can apply during design, planning, verification, and review.

`superpowers-quality-extension` 是一个面向 Coding Agent 的质量门禁 Skill。它扩展 Superpowers：把经得起时间检验的软件设计思想，转成 Agent 在设计、计划、验证和代码审查中可以执行的检查。

## Origins / 理论来源

This extension is grounded in three influential books on software design and architecture. It does not copy them mechanically; it adapts their principles into practical Agent workflows.

这个 Extension 的主要来源，是三本对软件设计和架构影响深远的书。它不机械照搬书中的规则，而是把原则改造成 Coding Agent 可以执行、检查和复用的工作流。

| Book / 书 | Main Basis / 主要依据 | Agent Adaptation / Agent 化改造 |
| --- | --- | --- |
| **A Philosophy of Software Design** | Complexity is the central problem; deep modules hide complexity behind simple interfaces; comments should capture design intent, invariants, and non-obvious contracts. | Design quality gate: complexity budget, deep-module review, information hiding, interface intent, strategic programming. |
| **Clean Architecture** | The Dependency Rule keeps source dependencies pointing toward policy; business rules should be independent of frameworks, databases, UI, and external agencies; boundaries translate data shapes. | Architecture boundary gate: protect use cases and domain rules from HTTP, ORM, SQL, SDK models, UI details, and external DTOs. |
| **Clean Code** | Code is read more than written; names should reveal intent; functions should stay at a coherent abstraction level; error handling and tests should remain readable. | Local quality and verification gate: intent-revealing names, abstraction-level review, clear error semantics, maintainable tests, and effective assertions. |

| 书籍 | 书中的关键依据 | 映射到 Agent 的质量门禁 |
| --- | --- | --- |
| **《A Philosophy of Software Design》** | 软件设计的核心目标是降低复杂性；深模块用简单接口隐藏复杂实现；注释应保存设计意图、不变量和非显然契约。 | 设计质量门禁：复杂性预算、深模块检查、信息隐藏、接口意图、战略编程。 |
| **《Clean Architecture》** | Dependency Rule 要求依赖指向业务策略；业务规则应独立于框架、数据库、UI 和外部系统；边界层负责数据形状转换。 | 架构边界门禁：保护 use case 和领域规则，避免 HTTP、ORM、SQL、SDK model、UI detail、外部 DTO 污染核心逻辑。 |
| **《Clean Code》** | 代码首先是给人读的；命名要表达意图；函数应保持一致抽象层级；错误处理和测试也要可读、可维护。 | 局部质量与验证门禁：意图明确命名、抽象层级检查、清晰错误语义、可维护测试和有效断言。 |

## Why This Extension Exists / 为什么需要这个 Extension

Coding Agents often produce code that works locally but weakens the long-term system model: shallow helpers, scattered concepts, framework leakage, broad cleanup, and weak tests. This Skill adds a pause point before those patterns become part of the codebase.

Coding Agent 很容易生成“眼前能跑、长期变差”的代码：浅 helper、概念分散、框架细节泄漏、顺手扩大范围、弱测试。这个 Skill 的目的，就是在这些问题进入代码库之前增加一个质量门禁。

It is intentionally conservative: pass tests are not enough if the change raises cognitive load, crosses architecture boundaries, or cannot be verified with meaningful evidence.

它刻意保持保守：即使测试通过，只要改动增加认知负担、穿透架构边界，或无法用有意义的证据验证，就不能直接视为完成。

## How the Books Map to Agent Gates / 书中原则如何映射为 Agent 门禁

| Gate / 门禁 | Source / 来源 | What the Agent Checks / Agent 检查什么 |
| --- | --- | --- |
| Design quality / 设计质量 | A Philosophy of Software Design | Whether the change lowers complexity, keeps concepts cohesive, and prefers deep modules over shallow wrappers. |
| Architecture boundary / 架构边界 | Clean Architecture | Whether business rules stay independent of HTTP, ORM, SQL, SDKs, UI, database details, and external DTOs. |
| Local readability / 局部可读性 | Clean Code | Whether names, abstraction levels, side effects, error handling, and comments help future readers. |
| Verification quality / 验证质量 | Clean Code + Superpowers verification | Whether tests assert real behavior, would fail for meaningful mistakes, and are backed by fresh evidence. |
| Change scope / 变更范围 | Superpowers workflow discipline | Whether the diff stays inside the approved plan and avoids unrelated refactors or new abstractions. |

## Important Adaptations / 重要改造

- Do not turn Clean Code into "short functions at all costs"; short code that forces more jumping is worse.
- Do not turn Clean Architecture into ceremonial layers; introduce boundaries when they protect rules, isolate volatility, or improve testability.
- Do not turn design-first into large speculative documents; require small, explicit design notes tied to the current change.
- Do not treat TDD as the only quality gate; architecture and complexity can fail even when tests pass.

- 不把 Clean Code 机械理解成“小函数至上”；如果短函数导致更多跳转和更高认知负担，反而更差。
- 不把 Clean Architecture 机械套成仪式化分层；只有当边界能保护规则、隔离变化或提升可测试性时才引入。
- 不把设计先行变成大设计先行；只要求和当前改动相关的小型、明确设计说明。
- 不把 TDD 当成唯一质量门禁；测试通过时，架构和复杂性仍可能失败。

## Usage / 使用方式

```text
Use $superpowers-quality-extension to review this coding task for design, architecture, and verification quality gates.
```

Read `SKILL.md` first for triggering and workflow rules. Load `references/quality-gates.md` when the task has meaningful design, architecture, verification, or scope risk.

先读 `SKILL.md` 获取触发和编排规则；当任务存在明显设计、架构、验证或范围风险时，再加载 `references/quality-gates.md`。

## Superpowers Integration / 与 Superpowers 组合

- `superpowers:brainstorming`: add complexity budget, deep-module shape, boundary ownership, invariants, and verification matrix.
- `superpowers:writing-plans`: plan by use case, module, or cohesive bundle; avoid mechanical helper splitting.
- `superpowers:test-driven-development`: use TDD for concrete behavior changes, but do not skip design and architecture gates.
- `superpowers:verification-before-completion`: require evidence that is fresh and relevant to the changed risk.
- `superpowers:requesting-code-review`: review design quality, dependency direction, local readability, test effectiveness, and diff scope.

- `superpowers:brainstorming`：补充复杂性预算、深模块形态、边界归属、不变量和验证矩阵。
- `superpowers:writing-plans`：按 use case、模块或内聚改动包计划，避免机械拆 helper。
- `superpowers:test-driven-development`：用于明确行为变更，但不跳过设计和架构门禁。
- `superpowers:verification-before-completion`：要求证据新鲜，并且和本次变更风险匹配。
- `superpowers:requesting-code-review`：审查设计质量、依赖方向、局部可读性、测试有效性和 diff 范围。

## Repository Layout / 仓库结构

```text
superpowers-quality-extension/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
├── assets/
│   └── cover.svg
└── references/
    └── quality-gates.md
```
