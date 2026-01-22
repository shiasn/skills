---
name: git-commit-guidelines
description: Use when user explicitly requests a git commit - checks for commit configuration files and follows project conventions, falls back to Conventional Commits if no config found. Commit messages should prioritize Chinese.
---

# Git Commit Guidelines

## Overview

**Use this skill ONLY when user explicitly requests a git commit.**

This skill ensures commit messages follow project conventions by checking for configuration files first, then falling back to Conventional Commits standard.

**Core principle:** Follow project conventions first, then standard conventions.

**Announce at start:** "I'm using the git-commit-guidelines skill to create a properly formatted commit message."

## When to Use

**ONLY when:**

- User explicitly asks to commit changes
- User says "commit", "提交", "git commit", etc.
- User requests creating a commit message

**DO NOT use when:**

- User hasn't explicitly requested a commit
- Just making code changes
- Only staging files without committing

## The Process

### Step 1: Check for Commit Configuration Files

Check for common commit configuration files in priority order:

```bash
# Check for Commitizen config
ls .cz-config.{cjs,js,json} .commitizenrc.{cjs,js,json} commitizen.json 2>/dev/null

# Check for git message template
ls .gitmessage .gitmessage.txt 2>/dev/null

# Check for commitlint config
ls commitlint.config.{cjs,js,mjs,ts} .commitlintrc.{cjs,js,json,yaml,yml} 2>/dev/null

# Check package.json for commit config
grep -q "config.*commitizen\|commitlint" package.json 2>/dev/null && echo "package.json"
```

**If multiple found:** Use the first one found in priority order.

**If none found:** Proceed to Step 3 (Conventional Commits).

### Step 2: Read and Apply Configuration

**If configuration file found:**

1. Read the configuration file: `cat <config-file>`
2. Extract format: For Commitizen look for `types`, `scopes`; for `.gitmessage` read template; for commitlint read validation rules
3. Apply the format: Follow exact format, use defined types/scopes, match style

### Step 3: Use Conventional Commits (Fallback)

**If no configuration file found:**

Use Conventional Commits specification with Chinese descriptions.

**Format:**

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types (类型):**

| Type       | 中文说明 | 使用场景                |
| ---------- | -------- | ----------------------- |
| `feat`     | 新功能   | 添加新功能              |
| `fix`      | 修复     | 修复 bug                |
| `docs`     | 文档     | 仅文档变更              |
| `style`    | 格式     | 代码格式（不影响功能）  |
| `refactor` | 重构     | 代码重构（非功能/修复） |
| `perf`     | 性能     | 性能优化                |
| `test`     | 测试     | 添加或修改测试          |
| `chore`    | 构建     | 构建工具或辅助工具变更  |
| `ci`       | CI       | CI 配置变更             |
| `build`    | 构建     | 构建系统变更            |
| `revert`   | 回退     | 回退之前的提交          |

**Scope (范围):** 可选，说明影响范围，如模块、功能等。

**Description (描述):** 简短描述，使用中文，不超过 50 个字符。

**Body (正文):** 可选，详细描述变更内容、动机等，使用中文。

**Footer (页脚):** 可选，用于关闭 issue、不兼容变更等。

### Step 4: Analyze Changes and Determine Batch Commits

Before writing commit message, analyze what changed:

```bash
# Check git status
git status

# Check staged changes
git diff --cached

# Check unstaged changes (if committing all)
git diff
```

**Identify:**

- What files changed?
- What type of change? (feature, fix, refactor, etc.)
- What scope/area affected?
- Any breaking changes?
- Any issues closed?

**When multiple files changed, check if they should be committed separately:**

**Group files by logical change:**

- **Same feature/fix** → Single commit (e.g., `UserLogin.tsx`, `UserLogin.test.tsx`, `loginApi.ts`)
- **Different features/fixes** → Multiple commits (e.g., `UserLogin.tsx` + `OrderList.tsx`)
- **Different types** → Multiple commits (e.g., `UserService.ts` + `README.md`)

**Decision matrix:**

| Scenario                     | Action                         |
| ---------------------------- | ------------------------------ |
| All files for one feature    | Single commit                  |
| Files for different features | Batch commits                  |
| Code + unrelated docs        | Batch commits                  |
| Code + tests (same feature)  | Single commit                  |
| Code + style/formatting      | Separate commits (style first) |
| Multiple unrelated fixes     | Batch commits                  |

**If batch commits needed:**

1. **Group files logically** and commit each group separately
2. **Order matters:** Style/formatting first, then dependencies, then features/fixes, tests with related code

**Example batch commit workflow:**

```bash
# Group 1: Formatting (do first)
git add .prettierrc
git commit -m "style: 更新 Prettier 配置"

# Group 2: User login feature
git add src/components/UserLogin.tsx src/api/loginApi.ts
git commit -m "feat(用户): 添加用户登录功能"

# Group 3: Order list feature
git add src/components/OrderList.tsx src/api/orderApi.ts
git commit -m "feat(订单): 添加订单列表功能"

# Group 4: Documentation
git add README.md
git commit -m "docs: 更新功能说明文档"
```

### Step 5: Write Commit Message

**Following the format from Step 2 or Step 3:**

1. **Choose appropriate type** based on changes
2. **Add scope** if relevant (e.g., `feat(用户管理):`)
3. **Write description** in Chinese, concise and clear
4. **Add body** if needed for complex changes
5. **Add footer** for issue references

**Examples:**

```bash
# Simple feature
git commit -m "feat(用户管理): 添加用户登录功能"

# Feature with body
git commit -m "feat(订单系统): 添加订单取消功能

用户现在可以取消未支付的订单，取消后库存自动恢复。

Closes #123"

# Bug fix
git commit -m "fix(支付模块): 修复支付超时问题

修复了支付接口超时未正确处理的问题，现在会正确显示错误信息。

Fixes #456"

# Refactor
git commit -m "refactor(工具函数): 重构日期格式化函数

将日期格式化逻辑提取为独立函数，提高代码复用性。"

# Documentation
git commit -m "docs: 更新 API 使用文档

添加了新的 API 端点使用示例和参数说明。"
```

### Step 6: Verify Commit Message

**Before executing commit, verify:**

- [ ] Type is appropriate for changes
- [ ] Description is clear and in Chinese
- [ ] Follows project config (if found) or Conventional Commits
- [ ] No typos or unclear language
- [ ] Issue references are correct (if any)

## Best Practices

| Practice          | GOOD                                     | AVOID                            |
| ----------------- | ---------------------------------------- | -------------------------------- |
| **Batch commits** | Separate commits for different features  | `git add .` with all changes     |
| **Chinese**       | `feat(用户管理): 添加用户登录功能`       | `feat(user): add login feature`  |
| **Specific**      | `fix(支付): 修复微信支付回调处理错误`    | `fix: 修复bug`                   |
| **Present tense** | `feat(订单): 添加订单取消功能`           | `feat(订单): 添加了订单取消功能` |
| **Concise**       | `refactor(工具函数): 提取日期格式化逻辑` | Long, verbose descriptions       |

## Common Mistakes

| Wrong                 | Correct                                   |
| ---------------------- | ------------------------------------------ |
| `feat: 重构用户服务`   | `refactor(用户服务): 重构用户服务代码结构` |
| `feat: add user login` | `feat(用户): 添加用户登录功能`             |
| `fix: 修复问题`        | `fix(支付): 修复支付超时未正确处理的问题`  |

## Quick Reference

| 类型       | 说明   | 示例                           |
| ---------- | ------ | ------------------------------ |
| `feat`     | 新功能 | `feat(用户): 添加登录功能`     |
| `fix`      | 修复   | `fix(支付): 修复超时问题`      |
| `docs`     | 文档   | `docs: 更新 API 文档`          |
| `style`    | 格式   | `style: 格式化代码`            |
| `refactor` | 重构   | `refactor(工具): 重构日期函数` |
| `perf`     | 性能   | `perf(列表): 优化渲染性能`     |
| `test`     | 测试   | `test(用户): 添加登录测试`     |
| `chore`    | 构建   | `chore: 更新依赖版本`          |

## Integration with Other Skills

**This skill is used AFTER:** development-workflow, finishing-a-development-branch, verification-before-completion

**This skill works with:** verification-before-completion, using-git-worktrees, development-workflow

## Red Flags

**STOP if:**

- User hasn't explicitly requested a commit
- Trying to commit without understanding changes
- Commit message doesn't follow project conventions
- Using English when Chinese is preferred

**ALWAYS:**

- Check for configuration files first
- Use Chinese for descriptions
- Follow project conventions if found
- Use Conventional Commits as fallback
- Verify message before committing

## The Bottom Line

**Commit messages should be clear, consistent, and in Chinese.**

1. Check for project configuration files first
2. Follow project conventions if found
3. Use Conventional Commits as fallback
4. Prioritize Chinese descriptions
5. Be specific and clear
6. Batch commits for multiple logical changes

**Only use this skill when user explicitly requests a commit.**
