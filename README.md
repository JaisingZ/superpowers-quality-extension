<p align="center">
  <img src="./assets/cover.svg" alt="Superpowers Quality Extension cover" width="100%">
</p>

# Superpowers Quality Extension

`superpowers-quality-extension` 是一个面向 Coding Agent 的质量门禁 Skill。它不替代 Superpowers，而是在设计、计划、验证和代码审查之间补上三类检查：设计质量、架构边界、验证质量。

## 适用场景

- 复杂功能、重构、架构边界调整或跨模块变更。
- Agent 可能为了快速完成而拆出浅 helper、扩大变更范围或引入新抽象层。
- 业务规则可能直接依赖 HTTP、ORM、SQL、SDK、外部 DTO 或框架类型。
- 测试只覆盖 status 200、`not null`、mock 调用次数，缺少业务断言或新鲜验证证据。

## 核心门禁

| Gate | 关注点 | 失败信号 |
| --- | --- | --- |
| Design quality | 复杂性预算、深模块、信息隐藏 | 调用者需要记住更多隐含规则，概念被拆散，helper 变浅 |
| Architecture boundary | 依赖方向、业务规则隔离 | 核心逻辑引用 HTTP/ORM/SQL/SDK/UI/外部 DTO |
| Verification quality | 测试有效性、新鲜证据 | 弱断言、过度 mock、陈旧输出、无法证明业务行为 |
| Change scope | 改动边界 | 未计划的 API/schema/error-code/dependency/style 变更 |

## 与 Superpowers 组合

- `superpowers:brainstorming`：补充 complexity budget、deep-module shape、boundary ownership、invariants 和 verification matrix。
- `superpowers:writing-plans`：按 use case、module 或 cohesive bundle 计划，不机械拆成一堆小 helper。
- `superpowers:test-driven-development`：用于明确行为变更，但不跳过设计和架构门禁。
- `superpowers:verification-before-completion`：要求证据新鲜且与风险匹配。
- `superpowers:requesting-code-review`：审查设计质量、依赖方向、局部可读性、测试有效性和 diff 范围。

## 文件结构

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

## 快速使用

```text
Use $superpowers-quality-extension to review this coding task for design, architecture, and verification quality gates.
```

先读 `SKILL.md` 获取触发和编排规则；当任务有明显设计、架构或验证风险时，再加载 `references/quality-gates.md`。
