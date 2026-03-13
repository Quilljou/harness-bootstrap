# {{PROJECT_NAME}} Agent Guide

## Start Here

- Planning contract: `PLANS.md`
- Active exec plans: `docs/exec-plans/active/`
- Completed exec plans: `docs/exec-plans/completed/`
- Tech debt and deferred work: `docs/exec-plans/tech-debt/`

## Source of Truth

- Architecture and system boundaries: `ARCHITECTURE.md`
- Design and design system intent: `DESIGN.md`
- Frontend structure and UI verification: `FRONTEND.md`
- Security and auth boundaries: `SECURITY.md`
- Reliability and observability contracts: `RELIABILITY.md`
- Quality gates and validation commands: `QUALITY.md`

## Working Rules

Keep this file short.

Use it as a routing layer into repo-local docs, not as the place where all durable product or architecture detail lives.

### Planning-First

For complex features, significant refactors, investigations, or migrations, create or update an exec plan under `docs/exec-plans/active/` before changing code.

Follow the contract in `PLANS.md`. The active plan should be self-contained enough that another agent can resume from it with minimal reliance on chat history.

### Step-Wise Verification

For multi-step work:

1. update the active exec plan
2. make the smallest useful change
3. run the declared verification
4. record the result and evidence
5. continue only after the step is verified

### Context Maintenance

- Update the relevant knowledge docs in the same change whenever architecture, frontend structure, design system behavior, security boundaries, reliability contracts, or validation commands change.
- Run `{{VERIFY_COMMAND}}` after structural changes or before handing work off.
- If the validation command fails, fix the code or docs before treating the work as complete.

### Recovery and Escalation

If the same step fails three consecutive times:

1. record the issue in the active exec plan
2. move durable deferred work into `docs/exec-plans/tech-debt/` if needed
3. stop and ask for human confirmation before continuing

Do not use destructive rollback when unrelated user changes may be lost.

### Unknowns

- Do not invent product requirements, providers, or routes.
- Prefer writing down open questions, assumptions, and deferred work in the active plan or `docs/exec-plans/tech-debt/`.

## Provider and Stack Rules

{{STACK_RULES}}

{{PROVIDER_RULES}}

{{AUTH_RULES}}

## Named Skills

Use the following skills when applicable:

{{SKILL_RULES}}

## Bootstrap Notes

- Generated from `assets/templates/AGENTS.md`
- Default deployment target: `{{DEPLOYMENT_TARGET}}`
