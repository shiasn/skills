---
name: react-19-changes
description: Use when writing React code - documents React 19 changes from React 18 including ref as prop, forwardRef deprecation, Context simplification, Actions, and other breaking changes
---

# React 19 Changes from React 18

## Overview

**REQUIRED:** Read this skill BEFORE writing any React code.

React 19 introduces significant changes from React 18. AI training data may be outdated. This skill documents the latest React 19 changes to ensure code follows current best practices.

**Core principle:** Write React 19 code, not React 18 patterns.

**Announce at start:** "I'm using the react-19-changes skill to ensure code follows React 19 patterns."

## When to Use

**MANDATORY before:**

- Writing React components
- Using React hooks
- Passing refs between components
- Using Context API
- Writing form handling code
- Any React-related code changes

**This skill must be read FIRST** - before writing React code, before refactoring React components, before adding new React features.

## Major Breaking Changes

### 1. `ref` as Regular Prop (No More `forwardRef`)

**React 18 (OLD):**

```tsx
import { forwardRef } from "react";
const MyInput = forwardRef(({ placeholder }, ref) => {
  return <input placeholder={placeholder} ref={ref} />;
});
```

**React 19 (NEW):**

```tsx
// ref is now a regular prop - no forwardRef needed
function MyInput({ placeholder, ref }) {
  return <input placeholder={placeholder} ref={ref} />;
}
```

**Key Points:**

- `ref` is now a regular prop in function components
- No need to wrap with `forwardRef`
- `forwardRef` is deprecated and will be removed in future versions
- TypeScript: `ref` should be typed as `React.Ref<T>` in props

### 2. `useRef` Requires Initial Value

**React 18 (OLD):**

```tsx
const ref = useRef(); // Could be undefined
```

**React 19 (NEW):**

```tsx
const ref = useRef(null); // Must provide initial value
```

**Key Points:**

- `useRef` now requires an initial value parameter
- Improves type safety
- Common initial values: `null`, `0`, `''`, `{}`, `[]`

### 3. Context as Provider (Simplified)

**React 18 (OLD):**

```tsx
<ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
```

**React 19 (NEW):**

```tsx
<ThemeContext value="dark">{children}</ThemeContext>
```

**Key Points:**

- Context can be used directly as provider
- No need for `.Provider` suffix
- `.Provider` still works for backward compatibility

### 4. String `ref` Removal

**React 18 (OLD):**

```tsx
<input ref="input" /> // String ref - REMOVED
```

**React 19 (NEW):**

```tsx
<input ref={(input) => (this.input = input)} /> // Callback ref
```

**Key Points:**

- String refs are completely removed
- Must use callback refs or `useRef` hook

### 5. `ref` Callback Cleanup Function

**React 19 (NEW):**

```tsx
<input
  ref={(ref) => {
    if (ref) ref.focus();
    return () => console.log("Input unmounted"); // Cleanup on unmount
  }}
/>
```

**Key Points:**

- Ref callbacks can return cleanup functions
- Cleanup runs when component unmounts or ref changes

### 6. Actions (Form Handling)

**React 19 introduces Actions for async form handling:**

```tsx
import { useFormStatus, useFormState } from "react-dom";

// useFormStatus - Get form submission status
function SubmitButton() {
  const { pending } = useFormStatus();
  return (
    <button type="submit" disabled={pending}>
      {pending ? "Submitting..." : "Submit"}
    </button>
  );
}

// useFormState - Manage form state with async actions
function MyForm() {
  const [state, formAction] = useFormState(
    async (prevState, formData) => {
      const result = await submitForm(formData);
      return { success: true, data: result };
    },
    { success: false }
  );

  return (
    <form action={formAction}>
      <input name="email" />
      <SubmitButton />
    </form>
  );
}
```

**Key Points:**

- `useFormStatus` - Access form submission status from child components
- `useFormState` - Manage form state with async actions
- Simplifies optimistic updates and error handling

### 7. Server Components Support

**React 19 adds native support for Server Components:**

```tsx
// Server Component (runs on server)
async function ServerComponent() {
  const data = await fetchData();
  return <div>{data}</div>;
}

// Client Component (runs on client)
("use client");
function ClientComponent() {
  const [state, setState] = useState(0);
  return <button onClick={() => setState((s) => s + 1)}>{state}</button>;
}
```

**Key Points:**

- Server Components run on server only
- Use `'use client'` directive for Client Components
- Server Components can be async

## Common Mistakes

| ❌ Wrong (React 18)               | ✅ Correct (React 19)         |
| --------------------------------- | ----------------------------- |
| `forwardRef((props, ref) => ...)` | `function({ ref, ...props })` |
| `useRef()`                        | `useRef(null)`                |
| `<input ref="input" />`           | `<input ref={(el) => ...} />` |

## TypeScript Examples

```tsx
// Ref as prop type
interface MyInputProps {
  placeholder: string;
  ref?: React.Ref<HTMLInputElement>;
}

function MyInput({ placeholder, ref }: MyInputProps) {
  return <input placeholder={placeholder} ref={ref} />;
}

// useRef with type
const inputRef = useRef<HTMLInputElement>(null);
const countRef = useRef<number>(0);
const dataRef = useRef<Data | null>(null);
```

## Migration Checklist

When updating React 18 code to React 19:

- [ ] Remove `forwardRef` - use `ref` as regular prop
- [ ] Replace string refs with callback refs
- [ ] Add initial values to all `useRef` calls
- [ ] Update Context usage (optional - `.Provider` still works)
- [ ] Add cleanup functions to ref callbacks if needed
- [ ] Consider using Actions for form handling
- [ ] Update TypeScript types for `ref` props

## Quick Reference

| Feature           | React 18               | React 19                        |
| ----------------- | ---------------------- | ------------------------------- |
| Ref passing       | `forwardRef` required  | Direct `ref` prop               |
| `forwardRef`      | Required               | Deprecated                      |
| String refs       | Supported              | Removed                         |
| `useRef`          | Optional initial value | Required initial value          |
| Context           | `.Provider` required   | Direct usage supported          |
| Ref cleanup       | Not supported          | Supported                       |
| Form Actions      | Not available          | `useFormStatus`, `useFormState` |
| Server Components | Limited                | Native support                  |
| Web Components    | Basic support          | Improved support                |

## Integration with Other Skills

**This skill runs BEFORE:** frontend-design, development-workflow, checking-auto-imports

**This skill is REQUIRED for:** Any React component development, hooks, refs, forms, or Context usage

## The Bottom Line

**React 19 changes are significant.**

- `ref` is now a regular prop - no `forwardRef` needed
- `forwardRef` is deprecated
- `useRef` requires initial value
- Context can be used directly
- New Actions for forms
- Better server component support

**Always check this skill before writing React code.**

This prevents using outdated React 18 patterns in React 19 projects.
