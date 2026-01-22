---
name: changelog-generator
description: Automatically creates user-facing changelogs from git commits by analyzing commit history, categorizing changes, and transforming technical commits into clear, customer-friendly release notes. Turns hours of manual changelog writing into minutes of automated generation.
---

# Changelog Generator

Transform technical git commits into user-facing release notes that are accurate, readable, and scoped to what actually shipped.

**Core principle:** Don’t invent. Derive every line from real commits and verified behavior.

## When to Use This Skill

- Preparing release notes for a new version
- Creating weekly or monthly product update summaries
- Documenting changes for customers
- Writing changelog entries for app store submissions
- Generating update notifications
- Creating internal release documentation
- Maintaining a public changelog/product updates page

## When NOT to Use

- You don’t know the git range/version window yet → clarify first.
- You need deeply technical documentation for engineers → write a technical change log separately.

## Inputs (Required)

- Git range: `base..head` OR a date window OR a version tag range
- Audience: customers, internal stakeholders, or mixed
- Tone/house style: plain, formal, friendly, etc. (use an existing guideline file if provided)
- Noise policy: what to exclude (tests, refactors, build tooling) and what to include (user impact, breaking changes)

**Missing input handling:**
- If the git range is unclear → STOP and ask for it (offer 2-3 base options if helpful).

## Workflow

1. Select the git range (base/head or tags) and confirm it with the user if ambiguous.
2. Extract commit subjects for the range, then scan diffs/stat where needed to understand impact.
3. Group changes into stable categories (e.g., Features, Improvements, Fixes, Breaking Changes, Security).
4. Rewrite each change in user language:
   - Lead with the user-facing outcome
   - Avoid implementation details unless needed for clarity
   - Avoid making promises not supported by the code/commits
5. Remove noise:
   - Drop pure refactors/tests unless they materially impact users (e.g., performance, reliability)
6. Add release metadata if requested (version, date, links).
7. Final pass: consistency, completeness, and scope verification.

## Output Requirements

- Default to no emojis unless the user explicitly wants them.
- Each bullet must be traceable to one or more commits in the chosen range.
- Call out breaking changes explicitly and provide migration notes if available.
- Keep language concise; avoid marketing exaggeration.

## Verification (Minimum Bar)

- Confirm the git range: `git log --oneline <base>..<head>` (or equivalent for date windows).
- Spot-check a few key bullets against `git show`/`git diff --stat` to ensure accuracy.

## How to Use

### Basic Usage

From your project repository:

```
Create a changelog from commits since last release
```

```
Generate changelog for all commits from the past week
```

```
Create release notes for version 2.5.0
```

### With Specific Date Range

```
Create a changelog for all commits between March 1 and March 15
```

### With Custom Guidelines

```
Create a changelog for commits since v2.4.0, using my changelog 
guidelines from CHANGELOG_STYLE.md
```

## Tips

- Run from your git repository root
- Specify date ranges for focused changelogs
- Use your CHANGELOG_STYLE.md for consistent formatting
- Review and adjust the generated changelog before publishing
- Save output directly to CHANGELOG.md

## Related Use Cases

- Creating GitHub release notes
- Writing app store update descriptions
- Generating email updates for users
- Creating social media announcement posts
