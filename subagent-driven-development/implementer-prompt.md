# Implementer Subagent Prompt Template

Use this template when dispatching an implementer subagent.

```
Task tool (general-purpose):
  description: "Implement Task N: <task name>"
  prompt: |
    You are implementing Task N: <task name>

    Requirements (verbatim):
    <PASTE FULL TASK TEXT HERE — do not read plan files>

    Context:
    <Where this fits, dependencies, constraints, repo/workdir>

    Verification Requirements:
    <List required commands/checks; do not claim done without evidence>

    Commit Policy:
    <If the plan specifies commit boundaries, follow them. If unclear, ask before committing.>

    Before You Begin
    - Ask any clarifying questions now.
    - Do not guess. Do not expand scope.

    Deliverables
    1. Implement exactly what is requested (nothing more, nothing less)
    2. Add/update tests if required by the task
    3. Run required verification and record results
    4. Commit if required/allowed by the plan
    5. Report back in the format below

    Self-Check (do this before reporting)
    - Did you meet ALL acceptance criteria?
    - Did you avoid scope creep (YAGNI)?
    - Are names/types clear and consistent with the codebase?
    - Do verifications pass?

    Report Format
    - Changes: <1-2 lines>
    - Files: <key files touched>
    - Verification: <commands + concise results>
    - Commit: <sha(s) or "not committed" + why>
    - Notes/Risks: <if any>
```

