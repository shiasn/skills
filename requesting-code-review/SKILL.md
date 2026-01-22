---
name: requesting-code-review
description: Use when completing tasks, implementing major features, or before merging to verify work meets requirements
---

# Requesting Code Review

Dispatch code-reviewer subagent to catch issues before they cascade.

**Core principle:** Review early, review with evidence, fix issues before they compound.

## When to Request Review

**Mandatory:**

- After each task in subagent-driven development
- After completing major feature
- Before merge to main

**Optional but valuable:**

- When stuck (fresh perspective)
- Before refactoring (baseline check)
- After fixing complex bug

## How to Request

## Inputs (Required)

- A clear requirements reference (plan link, spec, issue, acceptance criteria)
- A git range to review (base SHA → head SHA)
- A brief statement of what was implemented (for reviewer focus)

**Missing input handling:**
- If you cannot confidently choose a base SHA, STOP and ask (or provide 2-3 base options with trade-offs).

## Workflow

### Step 1: Choose the Review Range (base/head)

```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

Guidance:
- Prefer a base that matches the start of the unit of work (previous task commit, branch base, or `origin/main`).
- If the diff is too large to review meaningfully, split work or review in multiple ranges.

### Step 2: Dispatch code-reviewer subagent

Use the template at `code-reviewer.md`.

Provide:
- `{WHAT_WAS_IMPLEMENTED}` / `{DESCRIPTION}`: what changed (short)
- `{PLAN_REFERENCE}` / `{PLAN_OR_REQUIREMENTS}`: what it should do
- `{BASE_SHA}` / `{HEAD_SHA}`: the exact diff range

**Placeholders:**

- `{WHAT_WAS_IMPLEMENTED}` - What you just built
- `{PLAN_OR_REQUIREMENTS}` - What it should do
- `{BASE_SHA}` - Starting commit
- `{HEAD_SHA}` - Ending commit
- `{DESCRIPTION}` - Brief summary

### Step 3: Act on feedback

- Fix Critical issues immediately
- Fix Important issues before proceeding
- Note Minor issues for later
- Push back if reviewer is wrong (with reasoning)

### Step 4: Re-review if needed

- If you made non-trivial changes in response to review, request review again over the new range (or same range if it’s still accurate).

## Integration with Workflows

**Subagent-Driven Development:**

- Review after EACH task
- Catch issues before they compound
- Fix before moving to next task

**Executing Plans:**

- Review after each batch (3 tasks)
- Get feedback, apply, continue

**Development Workflow:**

- Review in REVIEW phase (Phase 5)
- Use for significant features
- Create REVIEW.md with detailed analysis

**Ad-Hoc Development:**

- Review before merge
- Review when stuck

## Red Flags

**Never:**

- Skip review because "it's simple"
- Ignore Critical issues
- Proceed with unfixed Important issues
- Argue with valid technical feedback

**If reviewer wrong:**

- Push back with technical reasoning
- Show code/tests that prove it works
- Request clarification

See template at: requesting-code-review/code-reviewer.md
