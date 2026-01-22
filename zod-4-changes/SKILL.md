---
name: zod-4-changes
description: Use when writing Zod validation code - documents Zod 4.x changes from Zod 3.x including performance improvements, API changes (z.email(), toJSONSchema()), Zod Mini, and other breaking changes
---

# Zod 4.x Changes from Zod 3.x

## Overview

**REQUIRED:** Read this skill BEFORE writing any Zod validation code.

Zod 4.x introduces significant changes from Zod 3.x. AI training data may be outdated. This skill documents the latest Zod 4.x changes to ensure code follows current best practices.

**Core principle:** Write Zod 4.x code, not Zod 3.x patterns.

**Announce at start:** "I'm using the zod-4-changes skill to ensure code follows Zod 4.x patterns."

## When to Use

**MANDATORY before:**

- Writing Zod schemas
- Using Zod validation
- Defining data validation rules
- Working with form validation
- Any Zod-related code changes

**This skill must be read FIRST** - before writing Zod schemas, before refactoring validation code, before adding new Zod features.

## Major Changes

### 1. Performance Improvements

**Zod 4.x performance gains:**

- String parsing: **14x faster**
- Array parsing: **7x faster**
- Object parsing: **6.5x faster**
- TypeScript type instantiation: Reduced, faster compilation for large codebases

**Key Points:**

- Significant performance improvements across all validation operations
- No code changes required to benefit from performance gains
- Faster compilation in large TypeScript projects

### 2. Format Helpers as Top-Level Functions

**Zod 3.x (OLD):**

```ts
import { z } from "zod";

const emailSchema = z.string().email();
const urlSchema = z.string().url();
const uuidSchema = z.string().uuid();
```

**Zod 4.x (NEW):**

```ts
import { z } from "zod";

// Format helpers are now top-level functions
const emailSchema = z.email();
const urlSchema = z.url();
const uuidSchema = z.uuid();
```

**Key Points:**

- Format helpers (`email()`, `url()`, `uuid()`, `ip()`, etc.) are now top-level functions
- No need to chain with `z.string()` first
- Backward compatible: `z.string().email()` still works

### 3. Built-in JSON Schema Conversion

**Zod 3.x (OLD):**

```ts
import { z } from "zod";
import { zodToJsonSchema } from "zod-to-json-schema"; // External library

const schema = z.object({
  name: z.string(),
  age: z.number(),
});

const jsonSchema = zodToJsonSchema(schema); // Requires external library
```

**Zod 4.x (NEW):**

```ts
import { z } from "zod";

const schema = z.object({
  name: z.string(),
  age: z.number(),
});

// Built-in JSON Schema conversion
const jsonSchema = schema.toJSONSchema();
```

**Key Points:**

- `.toJSONSchema()` method is now built-in
- No external library needed (`zod-to-json-schema` not required)
- Direct conversion from Zod schema to JSON Schema
- Simpler dependency management

### 4. Zod Mini (Lightweight Package)

**Zod 4.x introduces `@zod/mini`:**

```ts
// Instead of full zod package
import { z } from "zod"; // ~13KB minified

// Use Zod Mini for smaller bundle size
import { z } from "@zod/mini"; // ~1.9KB minified
```

**Key Points:**

- Lightweight alternative: `@zod/mini` (~1.9KB vs ~13KB)
- Tree-shakeable for modern frontend apps
- Supports most common Zod features
- Use when bundle size is critical
- Full Zod still available for advanced features

**When to use:** Mini for frontend (small bundles), Full Zod for complex validation/backend

### 5. Unified Error Handling

**Zod 3.x (OLD):**

```ts
try {
  schema.parse(data);
} catch (error) {
  if (error instanceof z.ZodError) {
    // Handle Zod errors
  }
}
```

**Zod 4.x (NEW):**

```ts
// Error handling unified to single error parameter
const result = schema.safeParse(data);
if (!result.success) {
  // result.error is always a ZodError
  console.log(result.error.issues);
}
```

**Key Points:**

- `safeParse()` returns `{ success: boolean, data?: T, error?: ZodError }`
- Unified error handling, better type safety

### 6. Metadata Support

**Zod 4.x (NEW):**

```ts
import { z } from "zod";

// Attach strongly-typed metadata to schemas
const schema = z.string().meta({
  description: "User email address",
  example: "user@example.com",
  format: "email",
});

// Access metadata
const metadata = schema._def.metadata;
```

**Key Points:**

- Attach strongly-typed metadata to schemas
- Useful for documentation generation, API specs, code generation

### 7. Module Structure Changes

**Zod 4.x module organization:**

```ts
// Main import (same as before)
import { z } from "zod";

// Core functionality (if needed)
import { z } from "zod/v4/core";

// Mini version
import { z } from "@zod/mini";
```

**Key Points:**

- Main import unchanged: `import { z } from "zod"`
- Optional sub-path imports: `zod/v4/core`
- Zod Mini as separate package: `@zod/mini`

## Common Mistakes

| Wrong (Zod 3.x)                     | Correct (Zod 4.x)                 |
| ------------------------------------ | ---------------------------------- |
| `z.string().email()`                 | `z.email()` (preferred)            |
| `zodToJsonSchema(schema)` (external) | `schema.toJSONSchema()` (built-in) |
| Large bundle for simple validation   | Use `@zod/mini` for small bundles  |
| Complex error handling               | Use `safeParse()` with unified API |

## Migration Checklist

When updating Zod 3.x code to Zod 4.x:

- [ ] Update format helpers to top-level functions (optional, backward compatible)
- [ ] Replace `zodToJsonSchema()` with `.toJSONSchema()` if used
- [ ] Consider using `@zod/mini` for frontend apps if bundle size matters
- [ ] Review error handling to use unified `safeParse()` API
- [ ] Add metadata to schemas if needed for tooling
- [ ] Test performance improvements (no code changes needed)

## Examples

**Format Helpers:**

```ts
// Zod 3.x: z.string().email()
// Zod 4.x (preferred): z.email()
const userSchema = z.object({
  email: z.email(),
  website: z.url().optional(),
  id: z.uuid(),
});
```

**JSON Schema:**

```ts
// Zod 3.x: zodToJsonSchema(schema) (external lib)
// Zod 4.x: schema.toJSONSchema() (built-in)
const jsonSchema = userSchema.toJSONSchema();
```

**Error Handling:**

```ts
// Zod 3.x: try/catch with instanceof check
// Zod 4.x (preferred): safeParse() with unified API
const result = schema.safeParse(input);
if (!result.success) {
  console.log(result.error.issues);
} else {
  const data = result.data;
}
```

**Zod Mini:**

```ts
import { z } from "@zod/mini"; // ~1.9KB vs ~13KB
const schema = z.object({ name: z.string(), age: z.number() });
```

## Quick Reference

| Feature          | Zod 3.x                   | Zod 4.x                          |
| ---------------- | ------------------------- | -------------------------------- |
| Format helpers   | `z.string().email()`      | `z.email()` (preferred)          |
| JSON Schema      | External library required | Built-in `.toJSONSchema()`       |
| Bundle size      | ~13KB minified            | ~13KB (full) or ~1.9KB (mini)    |
| Performance      | Baseline                  | 14x (string), 7x (array) faster  |
| Error handling   | Multiple error types      | Unified `safeParse()` API        |
| Metadata support | Limited                   | Strongly-typed metadata          |
| Module structure | Single package            | Main + mini + optional sub-paths |

## Integration with Other Skills

**This skill runs BEFORE:** frontend-design, development-workflow, checking-auto-imports

**This skill is REQUIRED for:** Any Zod schema definition, data validation, form validation, or API validation

## The Bottom Line

**Zod 4.x changes are significant.**

- Format helpers are top-level functions (`z.email()` vs `z.string().email()`)
- Built-in JSON Schema conversion (`.toJSONSchema()`)
- Zod Mini for smaller bundles (~1.9KB)
- Major performance improvements (14x faster string parsing)
- Unified error handling with `safeParse()`
- Metadata support for tooling

**Always check this skill before writing Zod code.**

This prevents using outdated Zod 3.x patterns in Zod 4.x projects.
