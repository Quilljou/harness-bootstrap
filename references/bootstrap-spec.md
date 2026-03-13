# Greenfield Bootstrap Spec

## Purpose

This file defines the one-time bootstrap flow for a greenfield software repository.

Bootstrap should not build product features. It should create a repository that:

1. captures product and stack intent with minimal ambiguity
2. starts from the best available generator
3. adds durable agent-readable context directly into the repo
4. exposes future complex work through official-style ExecPlan surfaces
5. keeps provider integrations and runtime harnesses placeholder-first
6. can be resumed safely if bootstrap is interrupted

The generated repository should feel ready for iterative implementation by humans or agents.

## Design Goals

Bootstrap should align the generated repository with the ideas behind Harness Engineering and Codex Exec Plans:

- the repository is the source of truth for agent context
- `AGENTS.md` is short and routes into richer docs
- complex work uses self-contained exec plans
- durable context is versioned in the repo instead of hidden in chat history
- app boot, browser verification, and observability expectations are legible to agents
- scaffolded contracts are preferred to fake completeness

## Skill Runtime State

Bootstrap itself must keep its runtime state in `.codex/bootstrap.todo.md`.

This file is separate from the generated repository's planning surfaces.

At minimum, `.codex/bootstrap.todo.md` must record:

- current bootstrap phase
- explicit non-goals for the active phase
- current repo state snapshot
- locked decisions
- ordered atomic steps
- the next step to run
- verification method and success signal for the active step
- progress log entries with timestamps
- failure notes and self-corrections

Bootstrap must update `.codex/bootstrap.todo.md`:

- before the first mutation
- before each new phase begins
- after each completed step
- after each failed attempt that changes the next action
- immediately before stopping or handing off

Bootstrap runtime state must never be written into the generated repo's `PLANS.md` or `docs/exec-plans/*`.

## Generated Repository Contract

The generated repository should use official-style agent context surfaces.

### Root Files

Bootstrap should generate these root files:

- `README.md`
- `AGENTS.md`
- `PLANS.md`
- `ARCHITECTURE.md`
- `DESIGN.md`
- `FRONTEND.md`
- `SECURITY.md`
- `RELIABILITY.md`
- `QUALITY.md`
- `.env.example`
- `.env.local.example` when provider-backed local setup is expected

### ExecPlan Structure

Bootstrap should generate:

- `docs/exec-plans/active/README.md`
- `docs/exec-plans/completed/README.md`
- `docs/exec-plans/tech-debt/README.md`
- `docs/exec-plans/templates/exec-plan.md`

The generated repo should use:

- `docs/exec-plans/active/` for current complex work
- `docs/exec-plans/completed/` for archived finished plans
- `docs/exec-plans/tech-debt/` for durable open questions, deferred work, and debt items

### Planning Contract

The generated repository must treat `PLANS.md` as the contract for future exec plans.

`PLANS.md` should define:

- when a new exec plan is required
- where active, completed, and tech-debt notes live
- naming conventions for plan files
- the required sections of an exec plan
- how work is moved from active to completed
- how unresolved work is captured in tech-debt notes

### Knowledge Base Contract

The root knowledge docs should be thin but useful. They should provide enough durable context for a new agent to orient itself quickly.

Minimum expectations:

- `ARCHITECTURE.md`
  System shape, boundaries, provider abstractions, and major data flow
- `DESIGN.md`
  UX goals, visual direction, design system expectations, and the design system page contract
- `FRONTEND.md`
  route structure, UI shells, shared primitives, states, accessibility, and frontend verification
- `SECURITY.md`
  auth model, secrets and env handling, trust boundaries, and local-only bypass constraints
- `RELIABILITY.md`
  boot expectations, logs/metrics/traces contracts, smoke verification, and failure-handling posture
- `QUALITY.md`
  validation commands, acceptance criteria, and handoff checks

### Routing Contract

The generated `AGENTS.md` must:

- stay short
- point first to `PLANS.md`
- point to `docs/exec-plans/active/`
- point to the root knowledge docs
- explain that complex features, significant refactors, and long-running investigations require an exec plan before implementation
- explain which validation command to run before handoff

## Template Source Contract

Template source files in this skill repo live under `assets/templates/`.

The template tree should mirror generated repo paths as closely as possible so bootstrap can copy files with minimal path mapping.

Examples:

- `assets/templates/AGENTS.md` -> generated repo `AGENTS.md`
- `assets/templates/PLANS.md` -> generated repo `PLANS.md`
- `assets/templates/docs/exec-plans/active/README.md` -> generated repo `docs/exec-plans/active/README.md`

Bootstrap should not rely on `assets/templates/harness/*`. That old template family should not be part of the default output.

## Recommended Defaults

Use these defaults unless the user explicitly overrides them or the project type makes them inappropriate:

- prefer official or ecosystem-standard generators
- prefer `TypeScript` where the stack supports it well
- prefer non-interactive or scriptable setup paths
- prefer provider boundaries and placeholders over premature vendor wiring
- prefer strong defaults and explicit TODOs over guessing
- prefer stable validation entrypoints such as `check:harness` or another clearly documented equivalent

### Default Web Profile

For a standard web SaaS or web app with no stronger direction, bootstrap should propose this default web profile and ask the user to confirm or adjust it before scaffold generation:

- Framework: `Next.js` App Router
- Language: `TypeScript`
- Package manager: `pnpm`
- Styling: `Tailwind CSS`
- UI components: `shadcn/ui`
- Database: `Postgres`
- ORM: `Drizzle`
- Hosted Postgres target: `Supabase` or `Neon`
- Authentication: `better-auth`
- Social sign-in: `Google`
- Billing: `Stripe`
- Object storage: `Cloudflare R2`
- Internationalization: enabled
- Initial locales: `en`, `es`, `fr`, `de`
- Agentation dev overlay: enabled for local development
- Deployment: user-selected

## Phase 1: Information Intake

Collect the minimum product and technical context that changes the scaffold:

- project name
- project type and platform
- one-line positioning
- target audience
- primary job-to-be-done
- core user problem
- non-goals for the initial version
- preferred visual direction
- required surfaces
- stack overrides
- deployment target
- provider overrides

For web projects, this phase must explicitly confirm or adjust the default web profile before scaffold generation.

This phase must also infer whether the project likely needs AI provider scaffolding.

Use the user's product description and required surfaces to infer likely provider capabilities, then ask the user to confirm the resulting provider bundle before scaffold generation.

Default capability inference rules:

- chat, copilot, agent, research, and workflow automation products imply `LLM/router` capability
- image generation, video generation, creative studio, and media pipeline products imply `media generation` capability

Default provider mapping rules:

- inferred `LLM/router` capability maps to `OpenRouter`
- inferred `media generation` capability maps to `fal`

Provider defaults are intentionally narrow. Do not add other AI providers unless the user explicitly asks for them as provider overrides.

If no AI capability is implied, do not generate speculative AI provider scaffolding.

Record only the final locked provider decisions in `.codex/bootstrap.todo.md`. Do not write inference rationale or rejected provider candidates into runtime state.

Do not invent missing product requirements. Use defaults where safe and record unresolved questions in bootstrap runtime state until the generated repo exists.

## Phase 2: Environment Detection

Before writing files, detect the local environment.

### Required Checks

- `git --version`
- `node -v`
- selected package manager version

### Optional Checks

Run only the checks relevant to the chosen stack and providers, for example:

- `bun -v`
- `docker --version`
- `psql --version`
- `supabase --version`
- `stripe version`
- `wrangler --version`

If `git` or `node` is missing, stop and report the blocker.

If optional tools are missing, record them in `.codex/bootstrap.todo.md` and continue when safe.

Environment detection results do not need a dedicated generated-repo document. Only durable setup expectations should flow into `README.md`, `QUALITY.md`, and the validation contract.

If a local Postgres database is in scope, environment detection should also determine the best available local provisioning path, such as Docker or an existing native `postgres` installation.

## Phase 3: Repository Initialization and Scaffold Generation

If the directory is not already a git repository:

1. initialize git
2. create the initial branch
3. create a baseline `.gitignore`

If the directory is already a git repo:

- do not overwrite user history
- only add missing bootstrap surfaces

After stack selection, prefer the official or ecosystem-standard generator when one exists.

Examples:

- `create-next-app` for `Next.js`
- `create-expo-app` for `Expo`
- `create-vite` for `Vite`
- `create astro` for `Astro`

Record in `.codex/bootstrap.todo.md`:

- selected framework
- selected generator
- why that generator was chosen
- which layers will be added afterward

If no credible generator exists, create the minimum viable structure manually and record why.

## Phase 4: Layering on Top of the Base Scaffold

Once the base scaffold exists, bootstrap should layer project-specific structure on top of it.

Typical layers include:

- route or screen shells
- shared UI primitives
- provider boundaries
- environment access modules
- logger abstraction
- metrics adapter boundary
- tracing adapter boundary
- validation entrypoints
- code quality tooling
- git hooks
- placeholder modules for chosen providers
- documentation surfaces

If the user confirms `OpenRouter` or `fal`, bootstrap must generate provider-specific scaffolding rather than only generic provider placeholders.

### Placeholder-First Rule

Do not implement real business logic during bootstrap.

Do not wire production secrets into the repo.

Prefer:

- route shells
- TODOs
- provider interfaces
- adapter boundaries
- skeletal validation scripts

Confirmed AI providers must still follow the placeholder-first rule. Bootstrap should add their SDK dependencies, environment contracts, and adapter stubs without wiring real product behavior.

### Provider Boundary Rule

For each confirmed provider, bootstrap must create an explicit adapter boundary that another agent can extend later without re-deciding the integration surface.

Preferred shape:

- a shared AI capability surface, such as `lib/ai/*`
- provider-specific adapters, such as `lib/ai/providers/*`
- a stable seam between application code and provider SDK calls

Do not hard-code a single required file layout, but the generated repo must clearly separate shared capability interfaces from provider-specific adapter code.

For the current default provider profile:

- `OpenRouter` should use the OpenAI-compatible path with the `openai` SDK and an `OpenRouter` base URL
- `fal` should use `@fal-ai/client` for media generation adapters

When a provider is confirmed, bootstrap should generate it to `SDK + adapter + .env` depth:

- include the provider SDK in the initial scaffold
- create placeholder adapter modules for the confirmed provider
- connect provider configuration through the generated env access layer
- document the provider in the generated repo knowledge surfaces

### Runtime Harness Rule

For web projects, the generated repo must be legible to agents at runtime.

At minimum, bootstrap should create or document:

- a shared logger abstraction
- a metrics adapter boundary
- a tracing adapter boundary
- a stable place for server and client observability code
- a documented smoke-verification path

The default local implementation may be minimal, such as:

- console logger
- no-op or console-backed metrics adapter
- no-op tracing adapter

The important requirement is that agents can discover where logs, metrics, and traces should go later.

### Code Quality Rule

Bootstrap should provide baseline quality tooling before the repo is considered ready.

At minimum:

- one linter and formatter strategy
- `lint` script
- `typecheck` script when the stack supports it
- git hooks through `Husky` or an equivalent
- staged-file checks through `lint-staged` or an equivalent
- a stable validation entrypoint such as `check:harness` or a clearly documented equivalent

## Phase 5: Generate Agent-Readable Repo Context

Bootstrap should generate the repo context surfaces from `assets/templates/`.

### `README.md`

Must include:

- project summary
- stack summary
- local development steps
- validation command
- what is stubbed
- repo map
- next recommended steps

### `AGENTS.md`

Must include:

- repo purpose
- the routing entrypoint to `PLANS.md`
- where active exec plans live
- where root knowledge docs live
- a planning-first rule for complex work
- a step-wise verification rule
- a recovery rule after repeated failures
- named-skill reminders when relevant

### `PLANS.md`

Must define:

- when to create a plan
- where to place new plan files
- required plan sections
- how active plans become completed plans
- how open questions and debt are captured

### Root Knowledge Docs

Bootstrap must generate:

- `ARCHITECTURE.md`
- `DESIGN.md`
- `FRONTEND.md`
- `SECURITY.md`
- `RELIABILITY.md`
- `QUALITY.md`

These docs should contain durable context, not temporary bootstrap status.

### ExecPlan Directory Files

Bootstrap must create:

- `docs/exec-plans/active/README.md`
- `docs/exec-plans/completed/README.md`
- `docs/exec-plans/tech-debt/README.md`
- `docs/exec-plans/templates/exec-plan.md`

The starter exec plan template should be self-contained and resumable. It should include:

- purpose or big picture
- progress
- surprises and discoveries
- decision log
- outcomes and retrospective
- context and orientation
- plan of work
- concrete steps
- validation and acceptance
- handoff

### Validation Command Contract

Bootstrap must expose a stable validation command such as `check:harness` or an explicitly documented equivalent.

At minimum, that command should verify:

- required root docs exist
- `PLANS.md` exists
- `docs/exec-plans/*` directories exist
- internal doc references in `AGENTS.md`, `README.md`, and `PLANS.md` point to real files
- observability and environment contract files are in sync with documentation when those surfaces are generated
- confirmed provider env examples, adapter stubs, and knowledge-doc references stay in sync when provider scaffolding is generated

## Phase 6: Environment and Provider Contracts

Bootstrap must create `.env.example` and, when needed, `.env.local.example`.

Rules:

- placeholders only
- no real secrets in git
- only chosen providers or clearly marked future providers
- comments for unknowns rather than invented values

Bootstrap should also create or update local `.env.local` when values can be filled safely on the current machine.

Rules for `.env.local`:

- keep `.env.local` out of git
- write only local-machine values, never committed template values
- prefer auto-filled local values over making the user transcribe them manually
- never invent secrets that are not already available locally or provided by the user

When AI providers are confirmed, `.env.example` and `.env.local.example` should group variables by provider and clearly mark which values are `required-now` versus `required-later`.

Bootstrap should also create env access modules appropriate to the stack, such as:

- `lib/env/server.ts`
- `lib/env/client.ts`

For the default web profile, `.env.local.example` should at minimum guide the user to provide:

- app URL and app name
- locale configuration
- database connection
- auth secrets and OAuth credentials
- billing secrets when billing is in scope
- storage credentials when storage is in scope

When database connection is in scope, bootstrap should prefer to provision a local Postgres instance automatically and write the resulting connection string into `.env.local`.

Preferred order:

- reuse an already-running local Postgres instance when one is explicitly configured for the project
- otherwise start a local Postgres instance using the best available supported path, such as Docker
- if automatic provisioning is not possible, leave the value unfilled in `.env.local.example`, record the blocker, and tell the user exactly what remains to be supplied

When `OpenRouter` is confirmed, the env contract should include:

- `OPENROUTER_API_KEY`

When `fal` is confirmed, the env contract should include:

- `FAL_KEY`

For confirmed AI providers, bootstrap should try to auto-fill `.env.local` from values already available on the local machine, such as shell environment variables or other explicitly available local secret sources.

If an AI provider secret cannot be resolved locally, bootstrap must not invent one. Leave the value unfilled locally and clearly mark it as the remaining setup step.

Unconfirmed AI providers must not appear as active scaffold requirements. They may only appear as comments, future options, or deferred notes.

`SECURITY.md`, `RELIABILITY.md`, and generated provider boundary code should agree on required-now vs required-later environment values.

`README.md`, `ARCHITECTURE.md`, `SECURITY.md`, and `RELIABILITY.md` must use the same provider names and env variable names as the generated scaffold.

## Provider Profile Defaults

Use the following defaults unless the user explicitly overrides them:

- `LLM/router` capability -> `OpenRouter`
- `media generation` capability -> `fal`

Recommended SDK defaults:

- `OpenRouter`: use the OpenAI-compatible path with the `openai` SDK and an `OpenRouter` base URL as the default implementation
- `fal`: use `@fal-ai/client`

Recommended env defaults:

- `OpenRouter`: `OPENROUTER_API_KEY`
- `fal`: `FAL_KEY`

These defaults are for scaffold generation only. Users may still override providers during information intake.

## Phase 7: Post-Boot Verification

After bootstrap fills `.env.local` as far as safely possible, and after the user fills any remaining required values, bootstrap should run local verification.

### Default Web Verification Flow

For the default web profile, verification should:

1. start the local dev server
2. open the app in [$agent-browser](/Users/quill/.agents/skills/agent-browser/SKILL.md)
3. confirm the default locale landing page renders
4. confirm at least one secondary locale route renders
5. confirm an auth-required route redirects or gates correctly when unauthenticated
6. use the local auth harness for authenticated route verification when OAuth-only auth is in use
7. confirm the main app shell renders for the authenticated test session
8. confirm billing and storage entry routes render when in scope
9. confirm no required-now env crashes remain

### Design System Page Step

Once the app can boot locally, bootstrap must:

1. run [$i-teach-impeccable](/Users/quill/.agents/skills/i-teach-impeccable/SKILL.md)
2. capture repo-specific design context
3. create a design system page or route aligned with the generated tokens, primitives, states, and visual language
4. document the page expectation in `DESIGN.md` and `FRONTEND.md`
5. verify the page renders locally
6. record the result in `.codex/bootstrap.todo.md`

## Done Criteria

Bootstrap is complete only when all of the following are true:

1. product identity and template scope are recorded
2. environment detection has been performed
3. the user has confirmed or adjusted the proposed default stack
4. the repo is initialized
5. a base scaffold was generated or a documented fallback was used
6. layered skeleton files and baseline provider boundaries exist
7. `README.md`, `AGENTS.md`, and `PLANS.md` exist
8. `ARCHITECTURE.md`, `DESIGN.md`, `FRONTEND.md`, `SECURITY.md`, `RELIABILITY.md`, and `QUALITY.md` exist
9. `docs/exec-plans/active/`, `docs/exec-plans/completed/`, `docs/exec-plans/tech-debt/`, and `docs/exec-plans/templates/exec-plan.md` exist
10. `.env.example` exists and `.env.local.example` exists when local provider-backed setup is expected
11. `.env.local` has been created or updated with all safely auto-resolvable local values, and the user has been prompted only for the remaining required values
12. `.codex/bootstrap.todo.md` exists and can resume the bootstrap run
13. code quality tooling is configured
14. a stable validation command exists and is documented
15. app boot has been attempted after local env setup
16. browser smoke verification has been attempted for web projects
17. a shared logger abstraction and metrics/tracing adapter boundaries exist
18. i18n scaffolding exists for the default web profile
19. baseline SEO placeholders exist for the default web profile
20. the design system page exists and has been locally verified for web projects
21. generated docs route future agents into `PLANS.md` and the knowledge base rather than relying on chat context
22. when `OpenRouter` or `fal` is confirmed, the corresponding SDK dependency, adapter stub, `.env` contract, and knowledge-doc coverage exist and agree with each other
23. when no AI provider is confirmed, the scaffold does not include speculative `OpenRouter`, `fal`, or other AI-provider wiring

## Recovery Protocol

If the same planned step fails three consecutive times, bootstrap must stop blind retries.

The agent must:

1. record the failure in `.codex/bootstrap.todo.md`
2. summarize attempted commands or fixes
3. attempt a safe rollback only when unrelated user work will not be lost
4. ask for human confirmation before continuing

Before high-risk operations, create or note a safe checkpoint when feasible.

High-risk operations include:

- project generators
- large lockfile rewrites
- auth setup across multiple routes
- billing setup with webhooks
- storage setup that touches runtime and env boundaries

## Suggested Bootstrap Prompt

If an agent uses this file as its operating contract, it should behave like this:

> Run the greenfield bootstrap for this repo. First collect the minimum required project and stack context, including what this project actually is. Then detect the local environment, initialize the repository if needed, choose the best available project generator for the selected stack, generate the base scaffold, and layer the project-specific structure, root knowledge docs, `PLANS.md`, and `docs/exec-plans/*` on top. Do not implement real business features or production integrations. Use placeholders and stubs where needed. Keep bootstrap runtime state in `.codex/bootstrap.todo.md`, not in the generated repo's planning docs. Once the app can boot locally, use [$i-teach-impeccable](/Users/quill/.agents/skills/i-teach-impeccable/SKILL.md) and add a design system page aligned with the generated design system. Use a stable validation command such as `check:harness` or a clearly documented equivalent before handoff.
