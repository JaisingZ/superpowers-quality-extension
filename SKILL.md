---
name: superpowers-quality-extension
description: Use when a coding task may increase complexity, cross architecture boundaries, weaken verification quality, create shallow helpers, leak framework or persistence details into business logic, expand change scope, or require design/code-review quality gates alongside Superpowers workflows.
---

# Superpowers Quality Extension

## Overview

Use this as a quality gate around Superpowers workflows. It adds design quality, architecture boundary, and verification quality checks without replacing `superpowers:brainstorming`, `superpowers:writing-plans`, `superpowers:verification-before-completion`, or `superpowers:requesting-code-review`.

Core principle: improve the system model, not just the diff. A change that passes tests but makes future understanding harder is not done.

## Workflow

1. Before planning implementation, identify the use case, current model, proposed model, module boundaries, invariants, and expected verification evidence.
2. Run the quick gates below. If any gate fails, pause and redesign before writing or accepting code.
3. For non-trivial design, architecture, or test-quality decisions, read `references/quality-gates.md` and apply the relevant checklist.
4. During review, check the final diff against the same gates. Do not approve unrelated refactors, framework leakage, weak tests, or completion claims without fresh evidence.

## Quick Gates

| Gate | Fail Signal | Required Response |
| --- | --- | --- |
| Design quality | More caller knowledge, scattered concepts, hidden state, shallow helpers, or extra timing/config constraints | Rework toward deeper modules and lower complexity |
| Architecture boundary | Business rules mention HTTP, ORM, SQL, SDK models, UI, framework types, or external DTOs | Move translation and external detail to adapters/boundaries |
| Verification quality | Tests assert only `not null`, status 200, call counts, or mocks; no fresh run evidence | Add meaningful behavior assertions and run current verification |
| Change scope | Unplanned API/schema/error-code/dependency/style changes or broad cleanup | Stop and get a revised plan before expanding scope |

## Superpowers Integration

- With `superpowers:brainstorming`: add complexity budget, deep-module shape, boundary ownership, invariants, and verification matrix to the design.
- With `superpowers:writing-plans`: plan by use case, module, or cohesive bundle. Do not mechanically split every helper or produce shallow layers.
- With `superpowers:test-driven-development`: use TDD for concrete behavior changes, but do not skip design gates for new modules, domain models, or architecture boundaries.
- With `superpowers:verification-before-completion`: require evidence that is both fresh and relevant to the risk, not just any green command.
- With `superpowers:requesting-code-review`: ask reviewers to check design quality, dependency direction, local readability, test effectiveness, and diff scope.

## Pressure Scenarios

Use these to self-test the skill:

- A simple change is split into many one-line helpers. Expected: reject shallow decomposition unless it lowers reader burden.
- Business logic directly consumes HTTP, ORM, SQL, SDK, or external DTO types. Expected: require boundary translation.
- Completion is claimed after weak tests or stale output. Expected: require meaningful assertions and fresh verification evidence.

## Common Mistakes

- Do not turn Clean Code into "short function at all costs"; short code that forces more jumping is worse.
- Do not turn Clean Architecture into ceremonial ports and adapters; introduce boundaries when they protect rules, isolate volatility, or improve testability.
- Do not turn design-first into large speculative documents; require small, explicit design notes tied to the current change.
- Do not let Superpowers TDD become the only quality gate; architecture and complexity can fail even when tests pass.
