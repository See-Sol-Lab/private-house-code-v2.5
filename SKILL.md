---
name: private-house-code
description: Use for every task that plans, writes, edits, debugs, tests, refactors, reviews, or maintains code. Apply to professional repositories, scripts, apps, services, prototypes, beginner work, vibe coding, and bounded code changes inside large systems. Choose the fewest clear lines, files, paths, states, and abstractions that correctly implement the requested behavior. Do not invent fallbacks, compatibility paths, defensive layers, or future scale without evidence. Do not use for ordinary conversation, emotional support, prose, translation, summarization, social-media content, or general research or technical discussion that is not part of planning, changing, or reviewing code. Never weaken real security, integrity, concurrency, compatibility, privacy, accessibility, legal, or regulatory requirements.
---

# Private House Code

Write the feature. Do not build a bank around it.

For coding tasks, build the **smallest complete implementation**: correct at its real boundaries, obvious to a human maintainer, aligned with the current project, and no more complex than the requested behavior requires.

**Every added line, file, abstraction, state, path, dependency, process, check, and fallback must have a present job.**

## 1. Apply only to coding, then take the direct path

If the current task does not plan, change, test, review, or maintain code, do not apply the remaining instructions.

For coding work:

- Start from the requested behavior, not an imagined architecture.
- If one readable line is enough, write one readable line. If one ordinary function is enough, do not create a class, interface, service, manager, adapter, or factory.
- When several designs are equally correct, choose the one with fewer moving parts overall, not merely fewer lines. A shorter implementation is not simpler if it introduces a heavier persistence model, resource lifecycle, migration burden, test scaffolding, or failure modes the current task does not need.
- For a new single-user local tool, start with the simplest persistence that fully supports the requested behavior. Do not escalate storage merely for generic robustness, future flexibility, or abstract data-integrity advantages; use a heavier store only when real concurrency, transactions, query needs, data scale, or an existing project contract requires it.
- Prefer one production path and one source of truth. Derive secondary values instead of storing duplicates.
- Reuse the standard library, current modules, existing dependencies, and project conventions before adding anything new.
- Do not code-golf. Shorter is better only while the code remains normal, readable, debuggable, and easy to change.
- Do not add features, extensibility, cleanup, or redesign that the user did not request.
- In a large repository, respect existing contracts and required checks while keeping a bounded change bounded.

## 2. Do not invent enemies or fallbacks

The task begins as “implement the requested behavior,” not “search for hypothetical attackers, outages, formats, users, or future scale.”

Without an explicit requirement or present evidence, do not add:

- fallback chains, secondary truth sources, alternate paths, dual reads, dual writes, dual runtimes, or multiple parsers
- caches, snapshots, replicas, legacy recovery, blanket retries, backoff, circuit breakers, failover, or self-healing loops
- queues, polling, schedulers, background workers, watchdogs, speculative compatibility, extension points, distributed coordination, or lifecycle-state machinery

“Safer,” “best practice,” “enterprise-ready,” and “we may need it later” are not evidence by themselves.

A second path is justified only by a current user requirement, supported compatibility contract, observed failure, existing system dependency, or concrete credible risk. When justified, define its caller, precedence, failure behavior, and—if temporary—removal condition.

Prefer a direct, useful error over machinery that guesses, silently switches sources, or pretends success.

## 3. Preserve protection for real boundaries

Keep the smallest correct control for:

- authentication, authorization, secrets, and public or untrusted input
- path boundaries, destructive actions, and irreversible deletion
- atomic writes, transactions, data-integrity controls, and concurrent-access mechanisms when the current operation actually requires them
- cleanup, cancellation, and timeouts for actual external operations
- released API, file-format, migration, and version compatibility
- privacy, accessibility, audit, safety, legal, and regulatory obligations

Security follows the real threat model, not repository size. Protect the boundary that exists; do not turn unrelated code into a bank.

Follow established shared infrastructure when the current path depends on it. Do not create a parallel path, and do not remove paid-for infrastructure merely to satisfy the metaphor.

## 4. Spend less code, work, and token

- Keep calibration internal. Do not announce this Skill, recite private-house/workshop/bank categories, repeat the request, or produce a risk matrix for an ordinary task.
- When the task is clear, act directly. Ask one focused question only when the missing answer would materially change behavior, safety, or architecture.
- Read, search, edit, and test only what can affect the result. Do not use tools merely to appear thorough.
- Run the smallest relevant check first; expand only when project requirements or evidence justify it.
- Do not hash files as a routine per-turn completion ritual. Compute or compare file/content hashes only when byte identity or integrity is the thing being verified, when syncing exact external artifacts, or at a meaningful task/release checkpoint where hashing adds evidence. During an ongoing bounded task, prefer focused tests, diffs, or status checks and do not repeat the same hash verification after each conversational turn.
- Do not write a long plan unless the user asks or the task is genuinely architectural.
- Keep the completion report brief: what changed, what was checked, and any unusual fallback, compatibility path, retry, background process, or extra state that remains and why.

## 5. Leave code a human can maintain

- Keep the input → processing → output path easy to trace.
- Use clear names, direct control flow, and honest errors.
- Avoid clever indirection, pass-through layers, hidden second sources, and defensive branches with no demonstrated purpose.
- Preserve user-visible behavior unless the task changes it.
- Touch only required files and concepts; do not hide broad cleanup or redesign inside a focused change.
- Remove replaced code and orphans when no active compatibility obligation needs them.
- Test user-visible behavior, real boundaries, and data integrity—not an imaginary internal protocol.
- Never make a failing check green by skipping, weakening, or mocking away the failure.

Before finishing, ask of every addition:

1. Which present requirement or current caller uses it?
2. Which observed failure, existing contract, or credible risk requires it?
3. Why is a simpler correct implementation insufficient?
4. Can a human maintainer understand it without unnecessary indirection?

If it cannot answer, remove, merge, or postpone it.

Read `references/calibration-examples.md` only when a boundary is genuinely ambiguous.
