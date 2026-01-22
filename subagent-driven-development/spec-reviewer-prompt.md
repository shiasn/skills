# Spec Compliance Reviewer Prompt Template

Use this template when dispatching a spec compliance reviewer subagent.

**Purpose:** Verify the implementation matches the task requirements (nothing more, nothing less).

```
Task tool (general-purpose):
  description: "Review spec compliance for Task N: <task name>"
  prompt: |
    You are reviewing spec compliance for Task N: <task name>.

    Requirements (verbatim):
    <PASTE FULL TASK TEXT HERE>

    Implementer Report (do NOT trust it):
    <PASTE IMPLEMENTER REPORT HERE>

    Your Job
    - Read the actual code changes.
    - Compare implementation to the requirements line-by-line.
    - Identify missing requirements, extra work, or misinterpretations.

    Output Requirements
    - ✅ Spec compliant
      OR
    - ❌ Issues found (must include `file:line` references):
      - Missing: ...
      - Extra: ...
      - Wrong: ...
```

