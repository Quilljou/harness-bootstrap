# {{PROJECT_NAME}} Planning Contract

This repository uses self-contained exec plans for complex work.

## When To Create A Plan

Create or update a plan before:

- complex features
- significant refactors
- migrations
- long-running investigations
- multi-step bug fixes with meaningful uncertainty

Skip a formal plan only for truly small, obvious, single-step edits.

## Where Plans Live

- Active work: `docs/exec-plans/active/`
- Completed work: `docs/exec-plans/completed/`
- Tech debt and deferred work: `docs/exec-plans/tech-debt/`

## Naming

Prefer `YYYY-MM-DD-short-title.md` for active and completed plans.

Tech debt notes may use either the same date format or a short durable slug if that reads better.

## Required ExecPlan Sections

Every active exec plan should include:

- Purpose / Big Picture
- Progress
- Surprises and Discoveries
- Decision Log
- Outcomes and Retrospective
- Context and Orientation
- Plan of Work
- Concrete Steps
- Validation and Acceptance
- Handoff

## Lifecycle

1. Create or update a file under `docs/exec-plans/active/`
2. Keep it current while the work is in flight
3. Move it to `docs/exec-plans/completed/` when the work is done
4. Move durable deferred work or unresolved follow-ups into `docs/exec-plans/tech-debt/`

## Validation

Run `{{VERIFY_COMMAND}}` before handing work off.

If the work changes architecture, frontend structure, design system behavior, security boundaries, reliability contracts, or quality gates, update the corresponding root knowledge docs in the same change.
