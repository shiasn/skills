---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write a plan that is executable, auditable, and verifiable: break a multi-step task into Tasks, define acceptance criteria, and define the minimum verification required to claim progress.

Assume the implementer is a skilled engineer but has limited context about the codebase/tooling/constraints. The plan must be self-contained enough to execute without guesswork.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** This should be run in a dedicated worktree (created by brainstorming skill).

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

## When to Use / When NOT

**Use:**
- You have a spec/requirements and the work is multi-step (needs decomposition, acceptance criteria, verification) before touching code.

**NOT:**
- The requirements are unclear or there are multiple plausible approaches with meaningful trade-offs → use `brainstorming` first.
- A plan already exists and you just need to execute it → use `executing-plans` or `subagent-driven-development`.

## Inputs (Required)

- **Source of truth**: spec/requirements/issue/doc path
- **Scope constraints**: in-scope/out-of-scope, time/risk constraints, compatibility requirements
- **Verification bar**: minimum verification types expected (e.g., typecheck/lint/test/build)
- **Plan location**: where plans should live (default: `docs/plans/`)

**Missing input handling:**
- If any required input is missing and it affects decomposition/acceptance/verification → STOP and ask for it (one question at a time, or offer 2-3 options).

## Plan Document Header

**Every plan MUST start with this header (you may add more sections as needed):**

```markdown
# [功能名称] 实施计划

> REQUIRED SUB-SKILL：使用 executing-plans 按任务逐步实施此计划。

**目标：** [用一句话描述要构建什么]

**范围：**
- 包含：…
- 不包含：…

**架构：** [用 2-3 句话说明实现思路/方法]

**技术栈：** [关键技术/库]

**验收标准：** [可观察结果/边界条件]

**验证方式：** [将使用的命令/检查点类型]

**风险与回滚：** [最大风险点 + 回滚策略]

**未决问题：** [如有；一次只列一个问题或给出选项]

---
```

## Task Structure

```markdown
### Task N: [Component Name]

**Files:**

- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Acceptance Criteria:**
- [Observable outcomes / edge cases this task must satisfy]

**Step 1:** [action + output/checkpoint]

**Step 2:** [action + output/checkpoint]

**Step 3:** [action + output/checkpoint]

**Verification (Minimum for this task):**
- [command/check] → [expected: pass/no errors/meets criteria]

```

## Workflow (How to Write the Plan)

1. Extract the goal and scope from the source of truth
2. Write constraints and explicit non-goals
3. Identify the likely impact surface (files/modules/components)
4. If multiple approaches exist, propose 1 recommended approach + 2-3 alternatives with trade-offs
5. Write the Task list (ordered by risk and dependency)
6. For each Task, write: Files, Acceptance Criteria, Steps, Verification
7. Add risks and rollback strategy
8. Add open questions (if any; ask one question at a time, or offer 2-3 options)
9. Self-check the plan’s executability (see Verification below)
10. Save the plan and present execution options

## Output Requirements (Hard Requirements)

- **Exact file paths**: every Files entry is an exact path; for Modify, include key line references when practical.
- **Independently verifiable steps**: each step has a clear “done” definition (output/checkpoint).
- **No large implementation code blocks**: only include “clarifying code” for format/interface/boundaries; otherwise prefer text + acceptance/verification.
- **Verification cannot be omitted**: define at least the minimum verification set (typecheck/lint/test/build).
- **DRY/YAGNI**: do not expand scope; write non-goals explicitly.

## Stop Conditions (Must Stop and Clarify)

- Requirements/scope are unclear and you cannot write acceptance criteria
- Multiple mutually exclusive approaches exist with meaningful architectural/compatibility impact
- You cannot determine the key impact files or the verification bar

## Verification (Plan Self-Check Before Handoff)

- Does every Task include: Files, Acceptance Criteria, Steps, Verification?
- Does the header include: goal/scope/architecture/acceptance/verification/risks+rollback/open questions?
- Did you avoid large implementation code blocks (only minimal clarifying code)?
- Did you define the execution handoff (below)?

## Execution Handoff

After saving the plan, offer execution choice:

**"Plan complete and saved to `docs/plans/<filename>.md`. Three execution options:**

**1. Subagent-Driven (this session)** - I dispatch fresh subagent per task, review between tasks, fast iteration

**2. Parallel Session (separate)** - Open new session with executing-plans, batch execution with checkpoints

**3. Development Workflow** - Use development-workflow skill for iterative development cycle

**Which approach?"**

**If Subagent-Driven chosen:**
- **REQUIRED SUB-SKILL:** Use subagent-driven-development
- Stay in this session
- Fresh subagent per task + code review

**If Parallel Session chosen:**
- Guide them to open new session in worktree
- **REQUIRED SUB-SKILL:** New session uses executing-plans

**If Development Workflow chosen:**
- **REQUIRED SUB-SKILL:** Use development-workflow
- Follow iterative cycle: design → implement → refine → document → review
