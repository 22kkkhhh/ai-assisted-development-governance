# AI-Assisted Development Governance

A Hermes Skill distilled from *VibeCoding真解* for personal AI-assisted software development.

It helps an AI assistant decide whether a task belongs in exploration, validation, or engineering mode; clarify boundaries before implementation; control complexity; debug from evidence; and report verification results and residual risk.

## Install in Hermes

```bash
hermes skills install https://raw.githubusercontent.com/22kkkhhh/ai-assisted-development-governance/main/SKILL.md
```

After installation, start a new session or run `/reload-skills`.

## Use

Explicitly load it in a session:

```text
/skill ai-assisted-development-governance
```

Then ask, for example:

```text
请使用 ai-assisted-development-governance。
先检查项目结构，判断这个任务属于探索、验证还是工程模式，
再给出最小实现方案和验证方式，暂时不要改代码。
```

## Scope

This Skill is a development governance guide, not a test runner or code editor. Hermes tools perform file inspection, code changes, and verification.

For specialized work, combine it with:

- `plan` for multi-step implementation plans;
- `systematic-debugging` for root-cause debugging;
- `test-driven-development` for tests-first development;
- `requesting-code-review` before commit or merge.

## License

MIT
