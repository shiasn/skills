---
name: subagent-driven-development
description: Use when executing implementation plans with independent tasks in the current session
---

# Subagent-Driven Development

## Overview

Execute an existing plan in the current session by dispatching a fresh implementer subagent per task, then enforcing two hard gates after each task:

1. Spec compliance review (build what was requested, nothing more/nothing less)
2. Code quality review (clean, tested, maintainable)

**Core principle:** Fresh context per task + spec gate + quality gate. No gate, no progress claim.

**Announce at start:** "I'm using the subagent-driven-development skill to execute this plan in this session."

## When to Use / When NOT

**Use:**
- You have a written plan and tasks are mostly independent.
- You want to stay in this session while keeping context clean.

**NOT:**
- No plan yet → use `writing-plans` (or `brainstorming` if requirements are unclear).
- Tasks are tightly coupled and need iterative design decisions → use `development-workflow`.
- You want execution in a separate session with batch checkpoints → use `executing-plans`.

## Inputs (Required)

- Plan reference (path or identifier)
- Full task text for ALL tasks (controller must extract and paste; subagents must not read plan files)
- Work directory
- Verification bar (per task, plus final if required)
- Commit policy (follow the plan; if plan is silent, ask before choosing)

**Missing input handling:**
- If you cannot paste full task text or the verification bar is unclear → STOP and ask.

## Workflow

### Step 1: Prepare Once

1. Read the plan once.
2. Extract all tasks verbatim (full text) and any shared constraints.
3. Create TodoWrite with all tasks.

### Step 2: Per Task Loop

For each Task N:

1. Dispatch implementer subagent using `./implementer-prompt.md` with:
   - Full task text (verbatim)
   - Minimal scene-setting context (where this fits, dependencies)
   - Required verifications
   - Commit policy (if specified)
2. If implementer asks questions: answer clearly, then re-dispatch.
3. Implementer implements and reports back (must include verification evidence).
4. Dispatch spec compliance reviewer using `./spec-reviewer-prompt.md`.
5. If spec review fails: send issues to the SAME implementer subagent to fix, then re-run spec review.
6. After spec passes, dispatch code quality reviewer:
   - Use the template at `requesting-code-review/code-reviewer.md`
   - Provide base/head SHAs and the plan/task reference
7. If quality review fails: same implementer fixes, then re-run quality review.
8. Mark Task N complete in TodoWrite.

### Step 3: Finish

After all tasks are completed and verified:
- Optionally dispatch a final code reviewer for the full range.
- Then complete development using `finishing-a-development-branch`.

## Output Requirements

**Implementer report must include:**
- What changed (1-2 lines) + key files
- Verification evidence (commands + concise results)
- Commit SHA(s)
- Any risks/notes

**Spec reviewer report must include:**
- ✅ or ❌
- If ❌: precise missing/extra items with `file:line` references

**Code quality reviewer output:**
- Must follow `requesting-code-review/code-reviewer.md`

## Stop Conditions (Must Stop and Ask)

STOP when:
- Task text is ambiguous in a way that changes behavior
- Verification bar is missing/unclear
- Reviews repeatedly fail due to unclear requirements (plan likely needs revision)
- Fixing a failure would require scope expansion not in the plan

## Verification (Minimum Bar)

- Per task: run the verifications required by the plan/task before claiming completion.
- Final: run the plan’s final verification set (if specified) before claiming “done”.

## Context Hygiene (Context Discipline)

- Subagents must not read plan files; controller pastes full task text.
- Provide only the minimal code pointers needed for the current task.
- Prefer targeted search over broad scanning.

## Integration

- `writing-plans` produces the plan this workflow executes.
- `requesting-code-review` provides the reviewer template.
- `finishing-a-development-branch` is required after completion.
- Cross-skill references are global; missing copies under `tmp/` are not errors.

