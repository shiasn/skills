---
name: frontend-design
description: Create distinctive, production-grade frontend interfaces with high design quality. Use this skill when the user asks to build web components, pages, artifacts, posters, or applications (examples include websites, landing pages, dashboards, React components, HTML/CSS layouts, or when styling/beautifying any web UI). Generates creative, polished code and UI design that avoids generic AI aesthetics.
license: Complete terms in LICENSE.txt
---

# Frontend Design

## Overview

Create production-grade UI with high design quality while respecting the project’s constraints (design system, tokens, accessibility, performance, and maintainability).

**Core principle:** Constraints first (design system + tokens + accessibility), then aesthetics.

**REQUIRED BEFORE CODING:** 
- For JavaScript/TypeScript projects, use **checking-auto-imports** skill first to identify auto-imported modules
- For React projects, use **react-19-changes** skill to ensure code follows React 19 patterns (not outdated React 18 patterns)
- For form/data validation with Zod, use **zod-4-changes** skill to ensure Zod 4.x patterns (not outdated Zod 3.x patterns)

## When to Use / When NOT

**Use:**
- The user asks to build or improve UI: components, pages, layouts, styling, or visual polish.

**NOT:**
- Requirements are unclear or multiple UX directions are plausible → use `brainstorming` first.
- Purely functional changes with no UI/UX impact → do not trigger this skill.

## Inputs (Required)

- What to build: component/page/app artifact
- Target environment: framework + build system + routing (if relevant)
- Design constraints: design system, tokens, typography rules, spacing/grid rules
- Behavior constraints: accessibility expectations, responsive targets, performance constraints
- Acceptance criteria: what “done” looks like

**Missing input handling:**
- If any required input is missing and affects the design direction → STOP and ask one question (or offer 2-3 options).

## Design Thinking

Before coding, lock the constraints and then pick a clear design direction that fits them:
- **Purpose:** what problem does the UI solve, and who uses it?
- **Constraints:** design system rules, tokens, accessibility, performance, responsiveness
- **Direction:** one coherent visual/interaction direction (avoid mixing multiple competing styles)
- **Differentiation (optional):** one memorable “signature” detail that does not break constraints

**Critical:** be intentional. “Minimal” still requires precision; “rich” still requires restraint.

## Workflow

1. **Inventory existing UI primitives**
   - Identify what the project already uses (component library, tokens, patterns).
2. **Draft the structure**
   - Layout composition, component breakdown, state/props boundaries.
3. **Implement with the design system first**
   - Prefer existing components and their APIs; only add custom styling when necessary.
4. **Add polish within constraints**
   - Spacing, typography hierarchy, states, micro-interactions (if allowed).
5. **Verify**
   - Typecheck/lint/build as appropriate
   - Basic accessibility checks: keyboard navigation, focus visibility, label associations
6. **Report**
   - What changed, key files, and how to verify locally

## Frontend Aesthetics Guidelines

Focus on:
- **Typography:** use the project’s typography system; if none exists, keep choices conservative unless user explicitly wants custom fonts.
- **Color & theme:** do not hardcode colors; use the project’s token/theming system.
- **Motion:** prefer subtle, purposeful motion; avoid motion that harms usability.
- **Spatial composition:** make hierarchy obvious; spacing consistency is more important than novelty.
- **Details:** add texture/visual depth only when it serves the direction and doesn’t break constraints.

Avoid generic, context-free styling. Always tie visual decisions back to the product purpose and constraints.

## Stop Conditions

- Requirements are unclear and you cannot define acceptance criteria
- The user expects a “brand new design language” but the project uses a strict design system
- The only way to satisfy aesthetics would violate constraints (tokens, accessibility, performance)

## Context Hygiene

- Reuse existing components/patterns before creating new ones.
- Don’t introduce new dependencies without explicit approval.

## Bottom Line

Deliver a cohesive UI that looks intentional and ships safely: constraints → structure → implementation → verification → report.
