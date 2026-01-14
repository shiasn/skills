---
name: checking-auto-imports
description: Use before any JavaScript/TypeScript project task - checks build tool configuration (rsbuild/vite/rslib/webpack) for auto-import plugins and documents what's automatically imported. Auto-imports apply to values, NOT types.
---

# Checking Auto-Imports

## Overview

**MANDATORY:** Execute this skill BEFORE starting any JavaScript/TypeScript project task.

Auto-import plugins automatically inject imports into your code. If you don't know what's auto-imported, you'll write code that:

- Duplicates imports (unnecessary)
- Fails to use available auto-imports (missed opportunities)
- Breaks when auto-imports change (unexpected errors)

**Core principle:** Know what's auto-imported before writing code.

**Announce at start:** "I'm using the checking-auto-imports skill to identify auto-imported modules before starting work."

## When to Use

**MANDATORY before:**

- Any JavaScript/TypeScript code changes
- Adding new components or files
- Modifying existing code
- Debugging import-related issues
- Refactoring code

**This skill must run FIRST** - before reading code, before writing code, before any analysis.

## The Process

### Step 1: Identify Build Tool

Check for build tool configuration files in priority order:

```bash
# Check for common build tool configs
ls rsbuild.config.{ts,js} 2>/dev/null
ls vite.config.{ts,js} 2>/dev/null
ls rslib.config.{ts,js} 2>/dev/null
ls webpack.config.{ts,js,mjs} 2>/dev/null
```

**If multiple found:** Use the one that exists (they're mutually exclusive in practice).

**If none found:** Check `package.json` for build scripts to infer tool.

### Step 2: Read Build Configuration

Read the configuration file completely:

```bash
# Read the config file
cat <config-file>
```

**Look for:**

- `autoImport` or `auto-import` plugins
- `unplugin-auto-import` or similar
- `@rsbuild/plugin-auto-import` or similar
- Any plugin that mentions "auto import" or "automatic imports"

### Step 3: Check Auto-Import Plugin Configuration

**If auto-import plugin found:**

1. **Read plugin configuration** - Find the `imports` or `resolvers` section
2. **Identify auto-imported modules:**

   - Libraries (e.g., `vue`, `react`, `lodash`)
   - Utilities (e.g., `console`, `process`)
   - Custom paths (e.g., `@/utils`, `@/components`)

3. **Document what's auto-imported:**

   ```markdown
   ## Auto-Imported Modules

   **From plugin configuration:**

   - `vue` - Vue 3 composition API (ref, reactive, computed, etc.)
   - `vue-router` - Router utilities (useRouter, useRoute)
   - `@/utils` - All utilities from src/utils
   - `@/components` - All components from src/components
   ```

**If no auto-import plugin found:**

Document: "No auto-import plugin detected. All imports must be explicit."

### Step 4: Check Auto-Import Declaration Files

Auto-import plugins often generate TypeScript declaration files:

```bash
# Common locations
ls auto-imports.d.ts 2>/dev/null
ls types/auto-imports.d.ts 2>/dev/null
ls src/auto-imports.d.ts 2>/dev/null
ls .volar/auto-imports.d.ts 2>/dev/null
```

**If found:** Read the file to see what's declared as auto-imported.

**Important:** These files show **type declarations** only. The actual auto-imports are **values**, not types.

### Step 5: Verify with Codebase

Check actual usage in codebase to confirm:

```bash
# Example: Check if ref is used without import
grep -r "^\s*const.*= ref(" --include="*.{ts,tsx,js,jsx}" | head -5
```

**If code uses `ref()` without import statement:** Confirms auto-import is active.

### Step 6: Document Findings

Create a summary:

```markdown
## Auto-Import Status

**Build Tool:** [rsbuild/vite/rslib/webpack]
**Config File:** [path]
**Auto-Import Plugin:** [plugin name or "none"]

**Auto-Imported Items:**

- [List all auto-imported modules, functions, utilities]

**Important Notes:**

- Auto-imports apply to VALUES only, NOT types
- You still need explicit type imports: `import type { ... }`
- Auto-imported items don't need `import` statements in code
```

## Common Auto-Import Plugins

### unplugin-auto-import

```typescript
// vite.config.ts or rsbuild.config.ts
import AutoImport from "unplugin-auto-import/vite";

export default {
  plugins: [
    AutoImport({
      imports: [
        "vue", // Auto-import Vue APIs
        "vue-router", // Auto-import router APIs
        "pinia", // Auto-import Pinia APIs
      ],
      resolvers: [
        // Custom resolvers
      ],
    }),
  ],
};
```

### @rsbuild/plugin-auto-import

```typescript
// rsbuild.config.ts
import { pluginAutoImport } from "@rsbuild/plugin-auto-import";

export default {
  plugins: [
    pluginAutoImport({
      imports: ["vue", "vue-router"],
    }),
  ],
};
```

## Critical Rules

### Rule 1: Auto-Imports Are Values Only

**❌ WRONG:**

```typescript
// Don't assume types are auto-imported
function useUser(): User {
  // Error: User type not imported
  return ref({});
}
```

**✅ CORRECT:**

```typescript
// Types must be explicitly imported
import type { User } from "@/types";
function useUser(): User {
  return ref({}); // ref is auto-imported (value)
}
```

### Rule 2: Don't Duplicate Auto-Imports

**❌ WRONG:**

```typescript
import { ref, computed } from "vue"; // Unnecessary if auto-imported

export function useCounter() {
  return { count: ref(0) };
}
```

**✅ CORRECT:**

```typescript
// No import needed if auto-imported
export function useCounter() {
  return { count: ref(0) }; // ref is auto-imported
}
```

### Rule 3: Check Before Writing Code

**❌ WRONG:**

```typescript
// Writing code without checking auto-imports
export function useAuth() {
  const router = useRouter(); // Is this auto-imported? Unknown.
}
```

**✅ CORRECT:**

```typescript
// After checking: useRouter is auto-imported from vue-router
export function useAuth() {
  const router = useRouter(); // Confirmed: auto-imported, no import needed
}
```

## Integration with Other Skills

**This skill runs BEFORE:**

- **frontend-design** - Must know auto-imports before designing components
- **development-workflow** - Must check before IMPLEMENT phase
- **react-19-changes** - For React projects, check auto-imports first, then React 19 patterns
- **systematic-debugging** - Import errors might be auto-import related
- **writing-plans** - Plans should account for auto-imports

**This skill is REQUIRED for:**

- Any JavaScript/TypeScript project work
- Any frontend development task
- Any code modification in JS/TS projects

## Red Flags

**STOP if you:**

- Start writing code without checking auto-imports
- Assume imports are needed without verification
- Add explicit imports for auto-imported items
- Use types without explicit type imports (auto-imports don't cover types)

**ALWAYS:**

- Check build config FIRST
- Document auto-imported items
- Verify with codebase examples
- Remember: auto-imports = values only, not types

## Example Workflow

```
You: I'm using the checking-auto-imports skill to identify auto-imported modules before starting work.

[Step 1: Check for config]
Found: vite.config.ts

[Step 2: Read config]
Found: unplugin-auto-import with imports: ['vue', 'vue-router']

[Step 3: Check declaration file]
Found: auto-imports.d.ts in root

[Step 4: Read declaration file]
Confirmed: ref, reactive, computed, useRouter, useRoute are auto-imported

[Step 5: Verify with codebase]
Found examples using ref() without imports - confirms auto-import active

[Step 6: Document]
## Auto-Import Status

**Build Tool:** Vite
**Config File:** vite.config.ts
**Auto-Import Plugin:** unplugin-auto-import

**Auto-Imported Items:**
- `vue` - ref, reactive, computed, watch, etc.
- `vue-router` - useRouter, useRoute

**Important Notes:**
- Auto-imports apply to VALUES only, NOT types
- Type imports still required: `import type { User } from '@/types'`
- No explicit imports needed for auto-imported values

Ready to proceed with code changes.
```

## Quick Reference

| Build Tool | Config File                  | Common Auto-Import Plugin     |
| ---------- | ---------------------------- | ----------------------------- |
| Vite       | `vite.config.{ts,js}`        | `unplugin-auto-import`        |
| Rsbuild    | `rsbuild.config.{ts,js}`     | `@rsbuild/plugin-auto-import` |
| Rslib      | `rslib.config.{ts,js}`       | `unplugin-auto-import`        |
| Webpack    | `webpack.config.{ts,js,mjs}` | `unplugin-auto-import`        |

## The Bottom Line

**Auto-imports are invisible but critical.**

Check them FIRST. Document them. Use them correctly.

Values are auto-imported. Types are not.

This skill prevents import-related bugs and unnecessary code.
