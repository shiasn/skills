---
name: receiving-code-review
description: Use when receiving code review feedback, before implementing suggestions, especially if feedback seems unclear or technically questionable - requires technical rigor and verification, not performative agreement or blind implementation
---

# Receiving Code Review

## Overview

Treat review feedback as hypotheses to validate, not orders to follow. Your goal is technical correctness for THIS codebase.

**Core principle:** Verify before implementing. Ask before assuming.

## When to Use / When NOT

**Use:**
- Any time you receive review feedback that implies code changes (human review, automated review, lint/tooling suggestions).

**NOT:**
- When you are asking for a review → use `requesting-code-review`.

## Inputs (Required)

- The full feedback text (copy/paste or link + quoted text)
- The code context being reviewed (PR/diff range, branch, or local files)
- Any explicit acceptance criteria mentioned by the reviewer/spec

**Missing input handling:**
- If you don’t have the exact comment/context → STOP and ask for it.

## Workflow

1. **Read:** Consume ALL feedback before reacting.
2. **Restate:** Convert each comment into a concrete requirement in your own words.
3. **Clarify (if needed):** If any item is ambiguous, STOP and ask one clarifying question (or offer 2-3 options).
4. **Verify:**
   - Locate the relevant code and usages (search callers, tests, runtime paths).
   - Reproduce the issue if it’s a behavior bug; otherwise identify the deterministic mismatch (types, API, invariants).
   - Run the smallest relevant verification step(s) (unit test, typecheck, lint, build) when possible.
   - Collect evidence (file:line references, failing/passing output, or code proof).
5. **Evaluate:** Is the suggestion correct for THIS codebase (contracts, constraints, compatibility, prior architectural decisions)?
6. **Respond (factually):**
   - If correct: implement with the minimal change, then report `Fixed: <what> (<file:line>)`.
   - If incorrect: push back with evidence, and propose the smallest alternative if needed.
   - If unverifiable: state the limitation and ask how to proceed.
7. **Implement:** One item at a time. Verify after each item. Avoid mixing unrelated refactors.

## Output Requirements (Your Reply)

- Start with the technical status: `Fixed`, `Need clarification`, or `Not changing`.
- Include `file:line` references for changes or for the evidence you relied on.
- If you ran verification: include the command(s) and concise results.

## Stop Conditions (Must Stop and Ask)

STOP when:
- A requirement is unclear or conflicts with another comment
- The suggestion implies a scope change beyond the spec/plan
- You cannot verify correctness safely (missing reproduction, missing environment assumptions)
- Applying the change would break compatibility or violate existing contracts

## Pushback Rules

Push back when:
- The suggestion breaks behavior or contracts
- The reviewer lacks key context for this codebase
- The suggestion violates constraints (e.g., performance, compatibility, architectural decisions)

How to push back:
- Be specific: evidence + `file:line` + minimal alternative (if applicable)
- Ask a pointed question if the reviewer intent is ambiguous

## Communication Rules

- No performative agreement.
- No gratitude filler.
- Be factual and concise.

## GitHub Thread Replies

Reply in the inline thread when possible; avoid top-level comments that lose context.

## Bottom Line

Verify. Decide. Then implement (one item at a time) or ask.

