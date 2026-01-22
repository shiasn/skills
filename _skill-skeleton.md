# SKILL.md Skeleton (Unified Structure)

> Purpose: Use this skeleton to rewrite SKILL.md files under `tmp/` so that triggered behavior is stable, outputs are auditable, and context usage is controlled.

## Overview (1 sentence)

- What problem does this skill solve?
- What does it produce (plan, checklist, decision, code change, verification evidence)?

## When to Use / When NOT (boundary)

**Use:**
- When is it mandatory?
- When is it recommended?

**NOT:**
- When should it NOT be used (switch to another skill / clarify first)?

## Inputs (minimum required info)

- Required inputs: e.g., spec/plan path, scope, error stack, git range, target directory
- Optional inputs: e.g., preferred approach, time/risk constraints

**Missing input handling:**
- If any required input is missing → STOP and ask for it (one question at a time, or offer 2-3 options).

## Workflow (steps + checkpoints)

1. Step 1 (goal / action / output / checkpoint)
2. Step 2 (goal / action / output / checkpoint)
3. ...

Requirements:
- Each step is independently executable and verifiable; ideally reversible or safely repeatable
- Turn uncertainty into explicit Stop Conditions (don’t guess)

## Output Requirements (hard requirements)

- What fields must the output include (files, acceptance criteria, verification, risks, open questions)?
- If output is markdown: define the heading/section structure
- If output is a report: define severity levels and required `file:line` references

## Stop Conditions (must stop to clarify / escalate verification)

- Requirements/scope unclear
- Multiple valid approaches with significant trade-offs
- Cannot reproduce / no evidence for a claim
- Verification fails repeatedly and cannot be fixed safely

## Verification (minimum bar)

- What is the minimum verification set (commands / checks / expected result)?
- What to do when verification fails (stop, rollback, request info, adjust plan)?

## Context Hygiene (context discipline)

- Read only the minimal set of files needed for the current step
- Prefer targeted search (e.g., `rg`) over broad scanning
- Avoid loading large docs; load on demand and record sources when relevant

## Integration (hand-offs to other skills)

- REQUIRED SUB-SKILL: which steps must switch to other skills
- Keep cross-skill references as-is (global skills may not be copied into `tmp/`; that is not an error)
