---
name: app-design-patterns
description: Select and apply strong application design archetypes and UI patterns with clear tradeoffs.
license: MIT
---

Use this skill when designing shells, screen variants, and overall UX direction.

## Core Archetypes

- Operations Cockpit: dense, status-first, rapid scanning
- Collaborative Workspace: shared context, comments/activity, assignment flows
- Pipeline/Workflow Board: stage-based progression with clear transitions
- Record + Detail Console: list/detail split view for data-heavy CRUD
- Feed + Inbox Triage: chronological review and fast batch actions
- Studio/Canvas: creative editing with tool panels and preview surface

## Shell Pattern Mapping

- Sidebar: best for many sections and deep IA
- Top Nav: best for shallow IA and broad pages
- Nav Rail: best for power users and compact chrome
- Hybrid Top+Side: best for complex products with global utilities
- Mobile Tabs + Desktop Expand: best for frequent mobile usage

## Option Generation Rules

When asked for design options:

- Propose 2-3 distinct options (not cosmetic variations)
- Explain tradeoffs for scan speed, learnability, and mobile behavior
- Recommend one option and explain why for this product context

## Quality Bar

- Each option must have a clear information hierarchy
- Primary actions and secondary actions must be unambiguous
- Empty/loading/error states must be accounted for
- Accessibility implications should be considered (focus order, contrast, touch targets)
