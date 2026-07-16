# AI-Assisted Development Governance

[English](README.md) | 简体中文

一个基于《VibeCoding真解》方法论蒸馏的 Hermes Skill，面向个人 AI 辅助开发。

它帮助 AI 助手在保留 Vibe Coding 探索速度的同时，对需要长期维护、复用或面向真实用户的代码引入适度的工程判断。

## 它能做什么

这个 Skill 会帮助 Hermes：

- 将任务判断为 `Vibe`（探索）、`Validation`（验证）或 `Engineering`（工程）；
- 把模糊想法整理成边界清晰的开发任务；
- 把 AI 生成的代码视为候选稿，而不是自动视为最终成果；
- 明确接口、数据约定、失败行为和验收标准；
- 控制复杂度、重复规则、职责混杂和架构偏移；
- 基于最小复现和证据调试，而不是反复猜测；
- 用适合非科班开发者的方式解释工程概念；
- 报告真实的验证结果、未验证项和剩余风险。

## 三种开发模式

| 模式 | 适用场景 | 最低验证要求 |
|---|---|---|
| `Vibe` 探索 | 一次性原型、临时脚本、实验、学习 | 最小示例可运行；保护生产路径和原始数据 |
| `Validation` 验证 | 验证一个重要流程、假设或设计 | 验收场景 + 一个重要失败或边界场景 |
| `Engineering` 工程 | 长期代码、共享接口、外部 API、持久化、凭证、用户数据、权限、并发、部署 | 明确边界；按风险测试；检查安全、重试、回滚；查看 diff 并报告风险 |

Skill 会选择与任务真实风险匹配的最低治理强度，不会对一次性实验强行要求完整生产流程。

## 安装到 Hermes

直接从 GitHub 安装：

```bash
hermes skills install https://raw.githubusercontent.com/22kkkhhh/ai-assisted-development-governance/main/SKILL.md
```

安装后重新开启会话，或执行：

```text
/reload-skills
```

## 多平台适配

仓库现在额外提供了给非 Hermes AI 编码工具使用的适配文档：

- `adapters/cursor/CURSOR_RULES.md` —— 用于 Cursor 的项目规则或 `.cursorrules`
- `adapters/codex/CODEX_AGENT.md` —— 用于 Codex 风格 coding agent 或类似代理系统

这些适配版保留同一套治理模型：`Vibe / Validation / Engineering`，但去掉了 Hermes 专用命令和工具约定。每个适配文档也包含了具体使用示例。

## 使用方式

在会话中显式加载：

```text
/skill ai-assisted-development-governance
```

然后提出任务，例如：

```text
请使用 ai-assisted-development-governance。

我想给项目增加文章去重功能。
先检查项目结构、现有代码和测试，
判断开发模式并给出最小实现方案，暂时不要改代码。
```

确认方案后：

```text
按刚才确认的方案实现。
完成后运行相关测试，检查 Git diff，
并告诉我哪些内容已经验证、哪些仍未验证。
```

复杂任务可以组合其他 Skill：

```text
先使用 ai-assisted-development-governance 判断风险，
再使用 plan 拆解任务。
```

```text
先使用 ai-assisted-development-governance 判断调试模式，
再使用 systematic-debugging 定位根因。先收集证据，不要猜修复方案。
```

提交或合并前：

```text
现在使用 requesting-code-review，
对本次改动做提交前检查。
```

## 首轮响应格式

面对新任务时，Skill 会保持首轮输出简洁：

```text
开发模式：探索 / 验证 / 工程
判断依据：2-4 条关键理由
立即动作：...
阻塞项：无 / ...
下一步：...
```

完成实现后，再补充：

```text
本次完成：...
已验证：...
仍需补充：...
剩余风险：...
```

## 相关 Skills

这个 Skill 负责判断治理强度和开发模式，不替代专门工作流：

- `plan`：多步骤实现规划；
- `systematic-debugging`：系统性根因排查；
- `test-driven-development`：测试驱动开发；
- `requesting-code-review`：提交前的安全、测试、diff 和独立审查。

## 范围与限制

这是 Hermes 的行为指导，不是独立的代码编辑器或测试运行器。项目检查、文件修改、命令执行和验证由 Hermes 工具完成。

本 Skill 基于《VibeCoding真解》的方法论提炼，是独立的操作性抽象，不是原文复刻。公开传播衍生内容前，请确认你拥有相应的授权或权利。

## 许可证

MIT
