# AI-Assisted Development Governance

[English](README.md) | [简体中文](README.zh-CN.md)

A Hermes Skill distilled from *VibeCoding真解* for personal AI-assisted software development.

It helps an AI assistant preserve the speed of Vibe Coding while adding the engineering judgment needed for code that will be maintained, reused, or exposed to real users.

## What It Does

The Skill helps Hermes:

- classify work as `Vibe`, `Validation`, or `Engineering`;
- turn vague requests into bounded development tasks;
- treat AI-generated code as a candidate draft, not an automatic final result;
- define interfaces, data contracts, failure behavior, and acceptance criteria;
- control complexity, duplicated rules, mixed responsibilities, and architecture drift;
- debug from minimum reproductions and evidence instead of guessing;
- explain engineering concepts in beginner-friendly language;
- report actual verification results, unverified items, and residual risks.

## Mode Selection

| Mode | Use For | Minimum Evidence |
|---|---|---|
| `Vibe` | Disposable prototypes, one-off scripts, experiments, learning | A small example runs; production paths and original data are protected |
| `Validation` | Testing one important workflow, assumption, or design | Acceptance scenario plus an important failure or boundary case |
| `Engineering` | Long-lived code, shared interfaces, external APIs, persistence, credentials, user data, permissions, concurrency, deployment | Explicit boundaries, proportionate tests, relevant security/retry/rollback checks, diff and risk report |

The Skill prefers the lightest mode that honestly matches the task. It does not impose full production ceremony on a disposable experiment.

## Install in Hermes

Install directly from GitHub:

```bash
hermes skills install https://raw.githubusercontent.com/22kkkhhh/ai-assisted-development-governance/main/SKILL.md
```

After installation, start a new session or run:

```text
/reload-skills
```

## Multi-Platform Adapters

This repository now includes adapter documents for non-Hermes AI coding tools:

- `adapters/cursor/CURSOR_RULES.md` — for Cursor project rules or `.cursorrules`
- `adapters/codex/CODEX_AGENT.md` — for Codex-style coding agents and similar agent instruction systems

These adapters reuse the same governance model — `Vibe`, `Validation`, `Engineering` — but remove Hermes-specific commands and tool references.

## Use It

Explicitly load it in a session:

```text
/skill ai-assisted-development-governance
```

Then ask, for example:

```text
请使用 ai-assisted-development-governance。

我想给项目增加文章去重功能。
先检查项目结构、现有代码和测试，
判断开发模式并给出最小实现方案，暂时不要改代码。
```

After confirming the approach:

```text
按刚才确认的方案实现。
完成后运行相关测试，检查 Git diff，
并告诉我哪些内容已经验证、哪些仍未验证。
```

For a complex task, combine it with specialized Skills:

```text
先使用 ai-assisted-development-governance 判断风险，
再使用 plan 拆解任务。
```

```text
先使用 ai-assisted-development-governance 判断调试模式，
再使用 systematic-debugging 定位根因。先收集证据，不要猜修复方案。
```

Before commit or merge:

```text
现在使用 requesting-code-review，
对本次改动做提交前检查。
```

## Initial Response Format

For a new task, the Skill keeps the first response compact:

```text
开发模式：探索 / 验证 / 工程
判断依据：2-4 条关键理由
立即动作：...
阻塞项：无 / ...
下一步：...
```

After implementation, it expands the report with:

```text
本次完成：...
已验证：...
仍需补充：...
剩余风险：...
```

## Related Skills

This Skill defines governance depth and development mode. It does not replace specialized workflows:

- `plan` — multi-step implementation planning;
- `systematic-debugging` — structured root-cause investigation;
- `test-driven-development` — tests-first development;
- `requesting-code-review` — pre-commit security, test, diff, and independent review.

## Scope and Limitations

This is a Hermes behavior guide, not a standalone code editor or test runner. Hermes tools perform project inspection, file changes, command execution, and verification.

The Skill is based on method extraction from *VibeCoding真解*. It is an independent operational abstraction rather than a reproduction of the source text. Please confirm your rights before redistributing derivative material.

## License

MIT
