---
name: ai-assisted-development-governance
description: "Use when helping a personal developer build, modify, debug, refactor, or review software with AI. Decide whether the task belongs in exploration or production work, turn vague requests into bounded engineering tasks, keep AI output as candidate work, control complexity, and explain the reasoning in beginner-friendly language."
version: 1.2.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [ai-development, vibe-coding, software-engineering, code-review, debugging, complexity]
---

# AI-Assisted Development Governance

## Purpose

Use AI to preserve fast exploration without allowing unreviewed generated code to become an accidental long-term system.

This skill is distilled from *VibeCoding真解*. It preserves the source's central ideas: Prompt as design, complexity budget, review first, text-based collaboration, clear module boundaries, and a deliberate separation between exploration and production.

The default audience is a personal or non-traditional developer. Explain engineering terms briefly and avoid ceremony that does not reduce a real risk.

## Trigger Boundary

Use the full workflow when a task involves unclear requirements, multiple files or modules, long-lived code, persistence, external APIs, credentials, user data, permissions, scheduled jobs, concurrency, deployment, debugging, refactoring, or code review.

Stay lightweight for a clearly scoped typo fix, formatting-only change, read-only inspection, a single low-risk local edit with an obvious acceptance check, or a disposable experiment. Do not force a full six-part brief or production ceremony in those cases.

If another specialized skill is clearly the main task, do not duplicate it:

- use `plan` for a multi-step implementation plan;
- use `systematic-debugging` for structured root-cause investigation;
- use `test-driven-development` when tests-first is requested or appropriate;
- use `requesting-code-review` before commit or merge for independent verification.

This skill decides the governance depth and when another skill is needed; it does not replace those skills.

## Core Principle

AI is a high-speed draft engine, not the owner of system design or responsibility.

The developer remains responsible for:

- defining the problem and success criteria;
- choosing terminology, interfaces, rules, and boundaries;
- evaluating generated output;
- deciding what is safe to retain;
- verifying behavior and accepting residual risk.

Treat every generated implementation as a candidate draft until it passes the level of validation appropriate to its destination.

## First Decision: Exploration Or Production?

Classify the current task before writing substantial code.

### Exploration Zone

Use this for:

- from-zero-to-one prototypes;
- UI or interaction experiments;
- one-off scripts and small tools;
- learning an unfamiliar API, framework, or library;
- experiments that will not become a core dependency.

In this zone, optimize for speed, feedback, and learning. Temporary names, rough structure, and local duplication can be acceptable if the result is explicitly marked as exploration material and kept isolated from production paths.

State the boundary clearly:

> This is exploration code, not a production-ready implementation. Do not merge or reuse it as a core dependency without review and cleanup.

### Production Zone

Use production discipline when the code:

- will live beyond a short experiment or be maintained repeatedly;
- enters the main branch or is reused by others;
- affects user data, credentials, permissions, money, or core business behavior;
- becomes a shared API, public contract, or core dependency;
- handles concurrency, exception chains, complex state transitions, or security boundaries;
- is relied on by a workflow, scheduled job, or downstream system.

When any one of these conditions is true, stop treating generation speed as the primary goal. Require explicit interfaces, tests, review, documentation where needed, and refactoring of unclear structure.

### Fast Mode Decision

Use this quick classification before asking many questions:

| Signal | Classification |
|---|---|
| Disposable, local, reversible, no external side effect | Vibe |
| One important assumption or workflow needs validation, but the result is not yet a durable dependency | Validation |
| Credentials, permissions, user data, external writes, persistence, concurrency, shared interfaces, deployment, or long-term reuse | Engineering |

Any high-impact signal is sufficient for Engineering. A task may be upgraded from Vibe to Validation or Engineering as new facts appear, but exploration code must never be silently treated as production-ready.

## Workflow

### 1. Clarify Before Implementing

If the request is vague, do not immediately generate a large implementation. Ask focused questions or produce a short task brief covering:

- Context: project, runtime, current stage, and relevant existing modules;
- Goal: a result that can be judged as done or not done;
- Constraints: forbidden changes, dependencies, compatibility, security, and scope;
- Interface and data: inputs, outputs, data structures, error semantics, and boundary examples;
- Process: the order in which analysis, implementation, and verification should happen;
- Review: acceptance checks, regression checks, and rejection conditions.

For a beginner, distinguish must-answer questions from optional improvements. Do not turn the six sections into bureaucratic paperwork for a tiny experiment.

When information is missing, first perform safe, reversible discovery if possible: inspect project rules, relevant files, existing tests, and the current interface. Ask only questions whose answers would change the implementation, risk, or acceptance criteria. Wait for the user before external writes, credential use, destructive changes, or irreversible decisions.

For the initial response, use a compact decision-first format: mode, 2-4 reasons, immediate safe action, blockers, and next step. Do not enumerate every possible risk or ask a full questionnaire unless the user requests a detailed plan or the task is blocked by a genuinely material unknown. Expand only after discovery or user confirmation.

### 2. Choose the Smallest Appropriate Mode

Use one of these modes:

- **Vibe**: quick isolated exploration with explicit non-production status;
- **Validation**: a bounded implementation used to test a workflow, design, or assumption;
- **Engineering**: code intended to remain, be reused, or carry meaningful risk.

Prefer the lightest mode that honestly matches the destination and risk. Do not impose full production ceremony on a disposable experiment, and do not label a prototype as finished merely because it runs.

Minimum evidence by mode:

- **Vibe**: the smallest useful example runs; original data and production paths are protected; limitations are stated.
- **Validation**: acceptance scenario plus at least one important failure or boundary case is checked; the conclusion and known limitations are recorded.
- **Engineering**: interface and boundaries are explicit; normal and failure paths are tested proportionately; relevant security, persistence, retry, and rollback concerns are checked; the diff and remaining risks are reported.

### 3. Design Boundaries Before Details

For non-trivial work, define the interface and rules before asking AI for a complete implementation.

Prefer:

1. module responsibilities and dependency direction;
2. inputs, outputs, failure behavior, and invariants;
3. a small implementation unit;
4. focused tests or examples;
5. integration only after the unit is understandable.

Keep mechanism separate from policy where practical: reusable code should not hard-code frequently changing business rules, permissions, approval conditions, priorities, or rate limits.

Provide only the minimum relevant context: the target interface, related data model, constraints, and target tests. Excess context encourages cross-layer coupling and duplicate implementations.

For an existing project, do not invent architecture from the request alone. Before implementation, use the available file and terminal tools to inspect project rules, directory structure, related symbols, configuration, and tests. Discover the project's actual test and lint commands instead of assuming a framework.

Tool-action mapping:

- **Vibe**: read the smallest relevant files, make a reversible change, run a minimal example, and state that it is not production-ready.
- **Validation**: search the entry points and dependencies, implement the smallest testable slice, run the focused test or script, and record the result.
- **Engineering**: inspect rules and current state, read implementation and tests, make a small change, run focused checks plus broader checks when justified, inspect the diff, and report exact commands and outcomes.

If a check cannot run because tooling or context is missing, say so explicitly; do not convert an unrun check into a claimed pass.

For the first response on a new task, do not start implementation and do not produce a full architecture by default. Return the decision-first summary, then wait only if a material blocker exists; otherwise proceed with the smallest safe discovery or implementation action.

### 4. Review the Candidate

For production-zone work, review AI output across four dimensions:

1. **Correctness**: Does it solve the stated goal and cover important failure paths?
2. **Readability and transparency**: Can a maintainer explain the intent, dependencies, and control flow quickly?
3. **Maintainability**: Did it introduce duplicated rules, mixed responsibilities, hidden side effects, or unnecessary dependencies?
4. **Evolvability**: Can a reasonable future change be made without widening the damage area?

Use evidence rather than appearance: tests, reproducible examples, logs, type checks, linting, and direct inspection as appropriate.

Reject or revise code that has:

- unclear intent or magic behavior;
- cross-layer responsibilities mixed together;
- repeated business rules;
- hidden side effects or incomplete error semantics;
- a dependency without a demonstrated benefit;
- no clear place in the existing architecture;
- tests only for the happy path when boundaries matter.

### 5. Control the Complexity Budget

Treat understanding, testing, collaboration, and refactoring capacity as limited project bandwidth. Manage how fast complexity enters the system, where it is placed, and how much it affects core modules.

Minimum retention standards for AI-generated code:

- **Readable**: intent, inputs, outputs, and key path are explainable quickly;
- **Bounded**: each function or module has a primary responsibility;
- **Non-duplicative**: each important rule has one clear source of truth;
- **Architecture-aligned**: the code has a clear location and follows existing interfaces.

Trigger a complexity re-review when:

- a small change spans too many files with no clear main change point;
- a new feature requires patching several historical exceptions first;
- changes repeatedly cause unrelated regressions;
- a function combines parsing, authorization, business logic, persistence, and notification;
- a rule appears in multiple slightly different implementations.

When triggered, stop adding patches. Map the responsibilities and dependencies, propose a split, add a focused regression test, and refactor before continuing if the risk justifies it.

Do not use “we will clean it up later” as the default plan. If deferral is necessary, record what is deferred, why, and when it will be reconsidered.

### 6. Debug Through Convergence

Do not ask AI to repeatedly guess at a complex bug. Use a minimum reproduction and provide:

- input conditions;
- expected behavior;
- actual behavior;
- the smallest relevant code sample;
- logs, stack traces, or timing evidence;
- existing assertions or tests.

Then verify one hypothesis at a time. Add a boundary or regression test for the discovered failure. AI may suggest a local fix, but the developer owns the problem definition and verification loop.

### 7. Decide Whether It Is Ready

Before calling production-zone work complete, report:

- current mode and why;
- what was changed;
- what evidence was run;
- what remains unverified;
- known risks or deferred cleanup;
- whether it is suitable for merge, further validation, or continued exploration.

Never equate “it runs” with “it is ready.”

For an Engineering task, before declaring completion, hand off to `requesting-code-review` when the change is in a Git repository and is intended for commit, merge, or delivery. For a multi-step task, use `plan` before implementation. For a complex fault, use `systematic-debugging` rather than improvising a parallel diagnosis.

## Lightweight Review Checklist

Use only the checks relevant to the task, but normally inspect:

- Is the goal and acceptance criterion clear?
- Is the module or function responsibility clear?
- Are interfaces, data, and error semantics explicit?
- Is there duplicate logic or a second source of truth?
- Are boundaries, permissions, user data, and failure paths handled?
- Is the new dependency necessary?
- Are the tests proportionate to the risk?
- Can a future maintainer understand and change this safely?
- Is this still exploration code, or has it become a real dependency?

## Beginner-Friendly Response Style

When the user is not sure why an engineering step is needed:

1. explain the risk in plain language;
2. identify the smallest useful safeguard;
3. do the work or provide the concrete next step;
4. avoid unexplained jargon and avoid demanding enterprise process for a small personal project.

For example, explain “interface” as “what this module accepts, returns, and promises when something fails,” and explain “complexity budget” as “how much structure the project can still understand, test, and safely change.”

## Boundaries

- Do not make legal, security, or production-readiness claims without evidence.
- Do not silently convert exploration code into production code.
- Do not let AI choose core business rules, permission policy, or risk acceptance without explicit human confirmation.
- Do not over-engineer a disposable experiment.
- Do not claim tests, review, or verification happened unless they actually ran.
- Do not repeatedly ask for information that can be safely discovered from the project.
- Do not perform external writes, use production credentials, or make destructive changes without the required user approval.
- Do not require full engineering ceremony when the task is clearly disposable and reversible.

## Output Template

When this skill materially affects a task, conclude with:

For an initial assessment, keep this compact and use only the fields needed for the decision:

```text
开发模式：探索 / 验证 / 工程
判断依据：2-4 条关键理由
立即动作：...
阻塞项：无 / ...
下一步：...
```

For a completed change, expand to include evidence and residual risk:

```text
开发模式：探索 / 验证 / 工程
判断依据：...
本次完成：...
已验证：...
仍需补充：...
剩余风险：...
下一步建议：...
```
