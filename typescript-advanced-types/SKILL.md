---
name: typescript-advanced-types
description: Master TypeScript's advanced type system including generics, conditional types, mapped types, template literals, and utility types for building type-safe applications. Use when implementing complex type logic, creating reusable type utilities, or ensuring compile-time type safety in TypeScript projects.
---

# TypeScript Advanced Types

## Overview

Use advanced TypeScript types to make illegal states unrepresentable while keeping types readable and compile-time cost reasonable.

**Core principle:** Start simple, add type-level complexity only when it buys real safety.

## When to Use / When NOT

**Use:**
- You need non-trivial generics, conditional types, mapped types, template literal types, or `infer`.
- You are building reusable utilities, SDKs, or strongly-typed configuration APIs.
- You need compile-time safety guarantees (not just runtime validation).

**NOT:**
- The problem is primarily runtime validation → use runtime checks (and a schema library if applicable).
- The type logic is not necessary for the API (avoid “types for types’ sake”).

## Inputs (Required)

- Intended public API surface (types or usage examples)
- Constraints: TypeScript version, strictness settings, lint rules, performance constraints
- Where the types will be consumed (internal-only vs exported public API)

**Missing input handling:**
- If the usage examples are missing → STOP and ask for 2-3 concrete examples (inputs/outputs).

## Workflow

1. **Define the contract first**
   - Write down the intended usage in 2-3 small examples (can be pseudo-code).
2. **Start with the simplest type**
   - Prefer explicit unions, simple generics with constraints, and existing built-in utilities.
3. **Add precision incrementally**
   - Only introduce conditional types / `infer` / template literals if the simpler version is unsafe or too permissive.
4. **Build a “type test” loop**
   - Add a small file that exercises the type contract and is compiled by `tsc` (or the repo’s typecheck command).
   - Prefer `satisfies` for “should compile” checks and intentional “should fail” cases where your repo allows them.
5. **Watch readability and compile cost**
   - Keep exported types easy to understand.
   - Avoid deep recursion and overly distributive conditionals unless necessary.
6. **Verify**
   - Run the project’s typecheck and ensure the types don’t leak `any` or force unsafe assertions.

## Pitfalls to Watch

- **Accidental distributive conditional types:** `T extends X ? ... : ...` distributes over unions. If you want non-distributive behavior, wrap with a tuple: `[T] extends [X] ? ... : ...`.
- **Overusing `infer`:** if `infer` makes the type unreadable, reconsider the API.
- **Reimplementing built-ins:** prefer TypeScript’s built-in utility types and standard patterns.
- **Type-level overfitting:** types that perfectly model one caller but hurt other callers’ ergonomics.

## Stop Conditions (Must Stop and Clarify)

- You cannot state acceptance criteria for the type contract (what should compile vs. not).
- The type solution becomes so complex that it’s effectively unmaintainable.
- Compile-time performance regresses materially (slow typecheck, recursion depth errors).

## Context Hygiene

- For large type problems, ask for:
  - The existing type definitions involved
  - 2-3 representative usage examples
  - The exact type errors (copy/paste)
- Avoid dumping a full codebase into context; narrow to the minimal types and call sites.

## References (Load Only When Needed)

- Cookbook (large, example-heavy): `tmp/typescript-advanced-types/references/cookbook.md`
  - Use when you need concrete patterns or a reminder of specific advanced constructs.

