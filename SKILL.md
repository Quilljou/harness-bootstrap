---
name: greenfield-bootstrap
description: "Generator-first bootstrap for new software repositories that should ship with official-style agent context surfaces: `AGENTS.md`, `PLANS.md`, `docs/exec-plans/*`, root knowledge docs, a resumable bootstrap todo file, and a post-start design system page pass. Use when starting a new app, site, API, internal tool, or starter template from scratch; choosing a stack and scaffolder; or turning vague product intent into an agent-readable repository without implementing real business logic."
---

# Greenfield Bootstrap

Read `references/bootstrap-spec.md` before executing a real bootstrap run.

## Core Rules

- Use this skill for greenfield setup, template regeneration, or first-pass repo scaffolding. Do not use it for ordinary feature work inside an established product repository.
- Keep bootstrap runtime state in `.codex/bootstrap.todo.md`. Do not write bootstrap progress into generated repo docs.
- Generate repositories that use `AGENTS.md` as a short routing layer, `PLANS.md` plus `docs/exec-plans/*` for future complex work, and root knowledge docs such as `ARCHITECTURE.md`, `DESIGN.md`, `FRONTEND.md`, `SECURITY.md`, `RELIABILITY.md`, and `QUALITY.md`.
- Prefer official generators, placeholder-first provider boundaries, and small verified steps over handcrafted large-batch scaffolding.
- Once the app can boot locally, run [$i-teach-impeccable](/Users/quill/.agents/skills/i-teach-impeccable/SKILL.md) to capture design context, then add and verify a design system page aligned with the generated repo's actual design primitives.
- Use `assets/templates/` as the source of generated files. The template tree mirrors target repo paths so bootstrap can copy with minimal path translation.

## Resources

- `references/bootstrap-spec.md`
  The full operating contract for intake, output structure, validation, recovery, and done criteria.
- `assets/templates/`
  Source artifacts copied into the generated repository.
