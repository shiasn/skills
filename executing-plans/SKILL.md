---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Execute an existing implementation plan with strict checkpoints: review first, run tasks in batches, and report evidence between batches.

**Core principle:** Don’t guess. Follow the plan. Verify before claiming progress.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

## When to Use / When NOT

**Use:**
- You already have a written plan (usually produced by `writing-plans`) and you want to execute it in a separate session with review checkpoints.

**NOT:**
- You don’t have a plan yet → use `writing-plans`.
- Tasks are tightly coupled and require interactive design iteration → use `development-workflow`.
- You want to execute in the same session with per-task subagent reviews → use `subagent-driven-development`.

## Inputs (Required)

- Plan file path
- Execution boundary (what “done” means for this run: full plan vs. specific tasks)
- Verification bar (what must pass to claim a batch is complete; usually specified by the plan)

**Missing input handling:**
- If plan path is missing or scope is ambiguous → STOP and ask for clarification.

## Workflow

### Step 1: Load and Review Plan

1. Read plan file
2. Review critically: identify gaps, ambiguity, missing verification, risky steps
3. If concerns: raise them with your human partner before starting (do not “fill in the blanks”)
4. If no concerns: create TodoWrite and proceed

### Step 2: Execute Batch

**Default batch size: 3 tasks**

You may reduce the batch size when:
- Tasks are high-risk (migration, broad refactor, security-sensitive)
- Tasks are tightly coupled (risk of cascading rework)

If you change the batch size, state why.

For each task:

1. Mark as in_progress
2. Follow each step exactly (no hidden steps, no assumptions)
3. Run verifications as specified (or STOP if the plan omitted verifications you need)
4. Mark as completed

### Step 3: Report

When batch complete:

- What was implemented (by task)
- Files changed (high level)
- Verification evidence (commands + concise results)
- Blockers / open questions (if any)
- Say: "Ready for feedback."

### Step 4: Continue

Based on feedback:

- Apply changes if needed
- Execute next batch
- Repeat until complete

### Step 5: Complete Development

After all tasks complete and verified:

- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## Output Requirements (Batch Report Format)

Each batch report must include:
- Tasks executed (Task N names)
- Key changes (1-2 lines per task)
- Verification evidence (commands + concise pass/fail summary)
- Any deviations from plan (ideally none; if present, explain and ask for approval)
- Open questions / blockers (if present)

## Stop Conditions (Must Stop and Ask)

STOP executing immediately when:
- Plan has ambiguous instructions that change behavior
- Required inputs are missing (plan path, scope, verification bar)
- Verification fails and you cannot fix it safely within the plan’s intent
- You discover an unexpected constraint or dependency not covered by the plan

## Verification (Minimum Bar)

- Run exactly the verifications specified by the plan.
- If the plan does not specify verifications, STOP and ask what the minimum bar should be (do not invent success criteria).

## Context Hygiene (Context Discipline)

- Read the plan first; then only open the files needed for the current task.
- Prefer targeted search (e.g., `rg`) over scanning the entire repo.

## When to Stop and Ask for Help

**STOP executing immediately when:**

- Hit a blocker mid-batch (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**

- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember

- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Between batches: just report and wait
- Stop when blocked, don't guess

## Alternative Workflows

**For iterative development without detailed plan:**

- Use **development-workflow** skill instead - follows design → implement → refine → document → review cycle

**For same-session execution:**

- Use **subagent-driven-development** skill instead - fresh subagent per task with reviews
