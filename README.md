# Greenfield Bootstrap Skill

This repository contains a skill for bootstrapping new software projects into agent-readable repositories.

It is not a product codebase. It is a template-and-spec repo that helps an agent turn a vague greenfield idea into a starter repository with:

- a generator-first scaffold
- official-style planning surfaces such as `PLANS.md` and `docs/exec-plans/*`
- root knowledge docs for architecture, design, frontend, security, reliability, and quality
- placeholder-first provider boundaries
- a resumable bootstrap runtime state in `.codex/bootstrap.todo.md`

## What This Skill Generates

The target repository should end up with:

- `README.md`
- `AGENTS.md`
- `PLANS.md`
- `ARCHITECTURE.md`
- `DESIGN.md`
- `FRONTEND.md`
- `SECURITY.md`
- `RELIABILITY.md`
- `QUALITY.md`
- `docs/exec-plans/active/`
- `docs/exec-plans/completed/`
- `docs/exec-plans/tech-debt/`
- `docs/exec-plans/templates/exec-plan.md`

For web projects, the generated repo should also expose:

- an app that can boot locally
- browser smoke-verification paths
- logger, metrics, and tracing adapter boundaries
- a design system page added after the app can run

## Guiding Ideas

This repo is explicitly influenced by:

- [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- [Using PLANS.md for multi-hour problem solving](https://developers.openai.com/cookbook/articles/codex_exec_plans)

In practice, that means:

- the repository is the source of truth for agent context
- `AGENTS.md` is a routing layer, not an encyclopedia
- durable context lives in versioned docs
- complex work is captured in self-contained exec plans
- bootstrap runtime state is separate from future product work

## Repository Structure

- [SKILL.md](/Users/quill/Developer/color44/SKILL.md)
  The compact skill entrypoint
- [references/bootstrap-spec.md](/Users/quill/Developer/color44/references/bootstrap-spec.md)
  The full operating contract for bootstrap
- [assets/templates](/Users/quill/Developer/color44/assets/templates)
  Template source files laid out to mirror generated repo paths
- [agents/openai.yaml](/Users/quill/Developer/color44/agents/openai.yaml)
  UI-facing skill metadata

## Template Layout

`assets/templates/` is organized to mirror generated repo paths so bootstrap can copy files with minimal translation.

Examples:

- `assets/templates/AGENTS.md`
- `assets/templates/PLANS.md`
- `assets/templates/ARCHITECTURE.md`
- `assets/templates/docs/exec-plans/active/README.md`
- `assets/templates/docs/exec-plans/templates/exec-plan.md`

The old `assets/templates/harness/` structure is intentionally removed. New repositories should use the official-style planning and knowledge-doc layout instead.

## Scope And Boundaries

This skill is responsible for:

- selecting a good stack and scaffolder
- generating a credible starter repository
- adding durable planning and knowledge surfaces
- keeping runtime integration boundaries placeholder-first

This skill is not responsible for:

- implementing the real business logic
- wiring production secrets
- hiding durable context in chat history
- using generated planning docs as bootstrap runtime state
