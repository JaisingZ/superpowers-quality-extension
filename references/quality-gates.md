# Quality Gates Reference

Use this reference when `superpowers-quality-extension` detects meaningful design, architecture, verification, or scope risk.

## 1. Design Quality Gate

Source lens: A Philosophy of Software Design. Optimize for lower complexity, deeper modules, and less information future readers must hold in mind.

Ask before implementation:

- What current model does the code use?
- What model will the change introduce?
- What invariants must remain true?
- What information must callers know after the change?
- Which complexity is hidden inside a module, and which complexity leaks to callers?

Reject or redesign when:

- A concept becomes scattered across more files without reducing local complexity.
- Callers must remember new ordering, timing, configuration, or state constraints.
- A helper or wrapper only hides one line and adds a new name to learn.
- A future change to one business rule will require edits in more places.
- Error handling paths multiply instead of being removed by a better model.

Prefer:

- Deep modules: simple interface, substantial hidden implementation.
- Explicit interfaces: contracts, invariants, error model, idempotency, transaction and concurrency assumptions.
- Strategic programming: fix the model instead of adding patches that only satisfy today.

Judgment rule for extraction: allow a helper only when it gives a reader a useful concept name, hides real complexity, removes duplication without scattering the concept, or creates an independently testable boundary. Reject extraction when the caller and helper must be read together to understand one idea.

## 2. Architecture Boundary Gate

Source lens: Clean Architecture. Dependencies point toward policy. Frameworks, databases, UI, transports, and SDKs are details.

Ask before implementation:

- What use case is changing?
- Which rules are domain/application policy?
- Which pieces are external details?
- Where does data shape translation happen?
- Can core rules be tested without the database, web server, UI, SDK, or external service?

Reject or redesign when:

- Domain or use-case code imports framework, ORM, HTTP, JSON transport, SQL, database, UI, or SDK types.
- Controller, repository implementation, or SDK model leaks into business rules.
- External DTOs become internal models by convenience.
- SQL, annotations, HTTP status, request objects, or response objects drive core policy.
- A new layer exists only because an architecture diagram says it should.

Prefer:

- Use-case-centered changes: API, database, MQ, UI, and CLI are trigger or persistence mechanisms, not the unit of design.
- Boundary translation: external data shapes are converted at adapters.
- Ports/adapters only where they protect business rules, isolate volatility, or make meaningful tests possible.

Placement rule: follow the repository's existing boundary names first. If none exist, put external DTO/request/entity/SDK translation at the nearest adapter/controller/repository/gateway edge, and pass inward a small internal command, query, value object, or domain model shaped for the use case.

## 3. Local Code Quality Gate

Source lens: Clean Code constrained by deep-module design. Local readability matters, but decomposition must reduce complexity.

Check:

- Names express business intent, not implementation trivia.
- A function does not mix business policy, SQL, HTTP, formatting, and persistence mechanics.
- Predicate/query methods do not mutate state unexpectedly.
- Error semantics are consistent: null, empty, exception, result object, and error code each mean one thing.
- Parameters are not a hidden object model; boolean flags do not hide two behaviors.
- Duplicated logic is removed when doing so reduces reader burden.
- Comments explain contracts, invariants, error models, concurrency, transactions, and non-obvious algorithms.

Reject or redesign when:

- "Cleanup" changes module structure without revisiting design quality.
- Extracted methods force readers to jump around to understand one concept.
- Comments repeat code instead of preserving design intent.
- New abstractions are added for hypothetical future needs.

## 4. Verification Quality Gate

Source lens: evidence over claims. A test is useful only if it fails when implementation is meaningfully wrong.

Choose evidence by risk:

- Unit tests: pure rules and edge cases.
- Regression tests: historical bugs.
- Integration tests: database, transaction, cache, queue, file, network, framework wiring.
- Contract tests: APIs, events, schema, error codes, adapter behavior.
- E2E tests: critical user journeys.
- Architecture tests: dependency direction and layer boundaries.
- Static checks: lint, typecheck, forbidden imports, security scans.
- Manual review: product semantics and design tradeoffs that tools cannot judge.

Reject or redesign verification when:

- Tests assert only `not null`, status 200, call counts, or mock interactions.
- Tests are written from implementation structure instead of expected behavior.
- Everything important is mocked away.
- The result would still pass if a real business rule were inverted.
- Verification output is stale, partial, or unrelated to the changed risk.

Require completion evidence:

- Fresh command output or review evidence.
- Clear mapping from evidence to changed behavior or risk.
- Explicit note for any planned verification that could not run.

Minimum fresh evidence: name the command or review performed in this session, state the behavior or risk it covers, and include the result. For API or UI work, status-only checks are not enough; include at least one behavior-level assertion or inspected response/state that would fail if the business rule were wrong.

## 5. Change Scope Gate

Use this when the agent may broaden a task while trying to be helpful.

Stop and revise the plan before:

- Public API, schema, error code, config key, dependency, or persistence shape changes.
- Framework or library upgrades.
- Broad style rewrites or formatting churn.
- Unrelated refactors.
- New abstraction layers not named in the plan.
- Edits outside the requested module or cohesive change bundle.

Allowed small cleanup:

- Directly adjacent to the changed code.
- Needed to make the requested behavior correct or testable.
- Covered by the same verification evidence.
- Clearly listed in the final change summary.

## Pressure Scenarios

Use these as RED/GREEN checks when editing this skill.

### Shallow helper pressure

Prompt shape: "This method is 20 lines. Split it into many helpers so it looks clean. No time for design discussion."

Expected behavior:

- Identify that short functions are not automatically better.
- Ask whether decomposition lowers caller/reader burden.
- Prefer one cohesive deep module over scattered one-line wrappers.

### Boundary leakage pressure

Prompt shape: "Fastest fix: pass the HTTP request / ORM entity / SDK response directly into domain logic."

Expected behavior:

- Identify framework or external-detail leakage.
- Require translation at the boundary.
- Keep business rules testable without external infrastructure.

### Weak verification pressure

Prompt shape: "Tests pass because status is 200 and mocks were called; mark complete."

Expected behavior:

- Reject weak assertions as insufficient.
- Require behavior-level assertions tied to risk.
- Require fresh verification output before completion.
