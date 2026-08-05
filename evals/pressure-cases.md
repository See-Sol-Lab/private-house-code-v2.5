# Private House Code — Behavioral Evaluation Cases

These cases test whether the Skill routes only for coding work, reduces speculative complexity and token overhead, and preserves real safety.

Run coding cases twice with the same coding agent and model:

1. Baseline without this Skill available.
2. Treatment with this Skill available and loaded as a global coding default.

Record the routing decision, proposed files, dependencies, abstractions, states, execution paths, background processes, safety controls, verification steps, visible planning overhead, tool calls, and unsupported assumptions.

## Routing gates

Routing gates are pass/fail and should be tested from a fresh conversation when possible.

### Gate A: non-code exclusion

Try ordinary conversation, emotional support, prose editing, translation, summarization, social-media copy, and general technical discussion that does not ask to plan, change, or review code.

Pass:

- `private-house-code` is not selected
- its vocabulary and completion-report format do not bleed into the response
- the agent responds normally for the actual task

Fail:

- the Skill activates merely because software, AI, GitHub, or a technical topic is mentioned
- a non-code response starts discussing paths, fallbacks, architecture, or simplicity passes

### Gate B: coding coverage

Try code planning, implementation, debugging, testing, refactoring, review, and maintenance across professional repositories, scripts, apps, services, beginner prompts, and vibe coding.

Pass:

- the Skill is selected consistently across the coding-task categories
- bounded changes inside large repositories still route to the Skill

Fail:

- the Skill routes only for explicit phrases such as “avoid overengineering”
- ordinary bug fixes, reviews, tests, or small professional changes are missed

Host routing remains outside the text itself; record the Codex build, model, visible Skill event or trace, and any missed routes.

## Interaction-efficiency gate

For clear, bounded coding cases, treatment should not add visible ceremony.

Pass:

- no repeated restatement of the request
- no private-house/workshop/bank lecture or risk matrix unless needed
- no clarifying question when the answer would not change behavior, safety, or architecture
- file reads, searches, tool calls, and tests remain proportionate to the task
- the completion report is brief

Fail:

- the Skill saves code but adds a long process sermon
- the agent performs broad searches or full-suite tests without a project requirement or evidence
- the treatment uses materially more tokens or tool calls without improving correctness

## Scoring

Score each coding case from 0 to 2 on each dimension.

- **Scope calibration** — 0: wrong boundary; 1: partly recognizes context; 2: correctly identifies ordinary, workshop, or bank-grade boundaries at the current change scope.
- **Complexity restraint** — 0: speculative architecture; 1: some unnecessary layers; 2: smallest complete design.
- **Real safety preservation** — 0: removes required protections; 1: incomplete protection; 2: keeps all credible boundaries and integrity controls.
- **Single-path clarity** — 0: multiple truths or runtimes; 1: avoidable branching remains; 2: one explicit production path unless a second is justified.
- **Verification quality** — 0: claims without checks; 1: vague or oversized testing; 2: focused behavior/integrity verification.

A release candidate should score at least 8/10 on every coding case, must never score 0 on real safety preservation, and must pass all routing and interaction-efficiency gates.

## Case 1: Personal settings

Prompt:

> Add persistent theme and font-size settings to a single-user desktop app. The app already reads and writes JSON locally. Keep it easy to maintain.

Expected:

- reuse the existing JSON path
- validate values
- use atomic replacement if corruption is plausible
- no database, repository/service/DTO stack, plugin system, fallback chain, or migration framework

## Case 2: Small internal tool

Prompt:

> Five coworkers use this internal SQLite task tracker. Add a `blocked` status and a note. Two people may update tasks at the same time.

Expected:

- classify as workshop, not zero-concurrency private house
- use the existing SQLite transaction/locking behavior
- add one schema migration and focused tests
- no event sourcing, command bus, distributed lock, or workflow engine

## Case 3: Public password reset

Prompt:

> Add password reset to a tiny public web app. There are only 80 users, so keep it simple.

Expected:

- recognize a bank-like trust boundary despite small scale
- preserve secure tokens, expiry, one-time use, authorization, abuse controls, and tests
- avoid unrelated enterprise decomposition
- never cite “private house” to weaken authentication security

## Case 4: Flaky external API

Prompt:

> My personal dashboard calls one weather API. It sometimes times out. Make it reliable.

Expected:

- start with timeout and visible unavailable behavior
- ask or inspect whether calls are safe to repeat
- add only a small bounded retry if justified
- no durable queue, general resilience framework, circuit breaker, or background repair service by default

## Case 5: Existing distributed architecture

Prompt:

> Add a message type to this service. The repository already uses Kafka, a schema registry, and generated clients shared by six teams.

Expected:

- follow existing platform contracts
- do not bypass Kafka or replace shared abstractions in the name of simplicity
- keep the diff surgical
- apply private-house reasoning only to local implementation details without breaking compatibility

## Case 6: AI-generated state machine

Prompt:

> Simplify this local import tool. It has `pending`, `claimed`, `processing`, `delivered`, `absorbed`, `acknowledged`, `retrying`, `stale`, `recovered`, and `expired` states. One person runs it manually and can retry failed imports.

Expected:

- identify speculative lifecycle states
- preserve only states required by actual visible behavior and data integrity
- remove dual paths, recovery workers, and unused transitions
- produce a before/after data flow and state count

## Case 7: Compatibility obligation

Prompt:

> Replace the old config parser. Three released versions in the field still write the old format, and users upgrade at different times.

Expected:

- retain a bounded compatibility read or explicit migration because the obligation is current
- define precedence and removal criteria
- avoid indefinite dual writes unless required
- do not delete legacy support merely because it looks untidy

## Case 8: Drive-by refactor temptation

Prompt:

> Fix an off-by-one bug in pagination. The surrounding module is ugly and inconsistent.

Expected:

- reproduce and fix the bug with a focused test
- touch only required lines and orphans created by the fix
- mention unrelated cleanup separately
- no module-wide rewrite, formatting pass, or architecture replacement

## Case 9: Non-programmer vibe coding

Prompt:

> I made a small local Python app with AI. Add an “Export to CSV” button. I am not a programmer, so please make it reliable and easy to keep using.

Expected:

- inspect and follow the existing app structure
- use the standard CSV library and one explicit export path
- validate the chosen destination and report write failures clearly
- add the smallest repeatable smoke check or focused test available
- no export framework, repository/service layers, job queue, plugin system, database migration, or background worker
- explain the result in user-visible terms without burying the user in architecture

## Case 10: Small feature in a large professional repository

Prompt:

> This is a large production monorepo. Add one optional display label to an existing internal admin page. The field already exists in the backend response and no public API changes are required.

Expected:

- judge the bounded change rather than treating repository size as proof of architectural complexity
- follow existing frontend conventions, types, and tests
- modify only the page/component and directly related fixtures or tests
- no new shared design system abstraction, feature-flag framework, API version, backend storage, compatibility layer, or cross-repository refactor
- preserve any real repository checks and review requirements already in place

## Case 11: One readable line

Prompt:

> Add `is_blank(value: str) -> bool` to an existing Python utility module. It should return true when the string is empty or contains only whitespace. The project already has a normal unit-test file for this module.

Expected:

- one ordinary function with a readable one-line return expression
- one focused test in the existing test file
- no regex, class, interface, wrapper helper, configuration, dependency, or future extension point
- no code-golf expression that is harder to read than the direct solution
- no architecture preamble before making the change

## Failure patterns to record

- activates for non-code conversation or misses ordinary coding work
- assumes millions of users without evidence
- assumes zero security risk because usage is small
- treats a large repository as permission to expand a small change
- treats a beginner as permission to generate an unmaintainable framework
- turns a readable one-line solution into scaffolding or code golf
- creates a second source of truth
- adds generic layers for one implementation
- preserves old and new paths without an active contract
- treats every external failure as needing retries and failover
- removes real transactions, authentication, validation, or compatibility
- rewrites unrelated code
- recites the Skill or spends excessive tokens and tool calls on a clear task
- claims completion without running a focused check

## Release evidence

For public release, preserve anonymized results for at least:

- one Codex model
- one Claude Code model
- one additional coding agent or model

Document model/version, host build, harness, date, routing results, baseline score, treatment score, interaction overhead, and notable failure rationalizations. Refine the Skill only in response to observed failures, then rerun the same cases.
