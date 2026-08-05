# Private House Code — Calibration Examples

Use these examples when the current change sits near a boundary between ordinary coding, workshop needs, and bank-grade requirements.

The Skill is intended to stay loaded globally. These examples do not decide whether an entire repository is “small” or “large.” They calibrate the smallest responsible scope of the current change.

## Default rule

Begin with the ordinary coding assumption:

> This task is a feature, repair, or bounded change—not a bank-scale architecture project.

Escalate only when current requirements, observed failures, existing contracts, or credible risks require more machinery.

Do not use repository size, team prestige, or model anxiety as proof that complexity is necessary.

## Fast decision table

| Situation | Default treatment | Keep | Usually avoid |
|---|---|---|---|
| Personal script processing local files | Ordinary/private house | path validation, clear errors, atomic replacement if data matters | service layers, plugin systems, retries, databases without a real query need |
| Desktop app for one operator | Ordinary/private house | durable state, corruption prevention, explicit migrations when a shipped format changes | distributed locks, multi-user permissions, background recovery state machines |
| Internal utility used by a small trusted team | Workshop | authentication if remotely accessible, transactions, bounded concurrency, useful logs | multi-region failover, generic event bus, speculative sharding |
| Small public website with accounts | Bank-grade at trust boundaries | auth, authorization, session protections, untrusted-input validation, abuse controls where credible | unrelated enterprise service decomposition without load or ownership needs |
| Payment, health, safety, legal, or irreversible data system | Bank | formal requirements, auditability, strong integrity, tested recovery, explicit compatibility | applying blanket “no retries/no fallbacks” rules |
| Bounded UI or helper change inside a large product | Ordinary or workshop for that change | existing platform contracts, shared infrastructure, repository checks | inventing a new local framework because the global system is complex |
| Existing service already uses a queue or repository abstraction | Follow the established architecture | reuse the current mechanism and tests | bypassing it or creating a parallel path solely to appear simpler |
| One call to a flaky external API | Ordinary or workshop | timeout, useful error, bounded retry only if idempotent and product-required | general retry framework, circuit breaker, durable job state without evidence |

## Example 1: Local settings file

Context:

- one desktop user
- trusted local input
- existing JSON settings file
- failures are visible and manually recoverable

Appropriate:

- validate supported values
- write atomically if partial writes could corrupt the file
- keep one settings path and one schema
- return a clear error

Speculative:

- database migration framework
- repository/service/DTO layers
- cache fallback
- old/new dual writes
- background repair worker

Why:

The atomic write protects real data integrity. The extra architecture protects hypothetical scale.

## Example 2: Vibe-coded CSV export

Context:

- non-programmer user
- local Python application
- one export button
- existing in-memory rows

Appropriate:

- use the standard CSV library
- validate or safely select the destination path
- write one file through one explicit path
- show success or a useful write error
- provide one small smoke check

Speculative:

- export service and repository layers
- pluggable serializer registry
- durable export queue
- background worker
- export job lifecycle state machine

Why:

The user needs a reliable feature, not an architecture they cannot maintain.

## Example 3: Shared SQLite tracker

Context:

- five coworkers
- existing SQLite database
- real concurrent updates may occur
- internal, authenticated environment

Appropriate:

- one schema migration
- existing transaction and locking behavior
- validation at the write boundary
- focused concurrent-update or integrity test if relevant

Speculative:

- distributed lock service
- event sourcing
- command bus
- multi-region replica plan
- generic persistence abstraction for a second database that does not exist

Why:

This is a workshop. Real concurrency must be handled; imaginary distributed scale does not.

## Example 4: Public password reset

Context:

- small public application
- untrusted internet input
- account takeover risk

Appropriate:

- secure random, single-use reset tokens
- short expiry
- authorization and rate or abuse controls as required
- safe storage and comparison
- tests for reuse, expiry, and invalid tokens

Speculative:

- unrelated microservice decomposition
- generic workflow engine
- broad event platform introduced only for this feature

Why:

The repository may be tiny, but the trust boundary is bank-grade. Private House Code reduces unrelated ceremony; it never weakens authentication.

## Example 5: Weather API timeout

Context:

- personal dashboard
- one external read-only API
- occasional timeout
- stale or missing weather is acceptable

Appropriate first step:

- explicit request timeout
- visible unavailable state
- one small bounded retry only if calls are safe and current UX needs it

Potentially justified later:

- short cache if rate limits or repeated calls are observed

Speculative by default:

- durable queue
- circuit-breaker framework
- multi-provider failover
- background reconciliation worker
- persisted recovery state machine

Why:

Reliability should match the cost of failure. A missing weather card does not need financial-system recovery machinery.

## Example 6: Existing Kafka platform

Context:

- shared production service
- Kafka, schema registry, and generated clients already exist
- six teams depend on the contract

Appropriate:

- follow the existing message and schema path
- use the required compatibility rules
- keep the local change surgical
- add the smallest required tests and generated updates

Wrong use of the Skill:

- bypassing Kafka with a direct local file
- deleting shared abstractions because they look heavy
- ignoring schema compatibility

Why:

The infrastructure already has users, contracts, and an operational owner. Its complexity is paid for.

## Example 7: Small UI feature in a huge monorepo

Context:

- large professional repository
- optional display label on one internal admin page
- backend response already contains the field
- no public API change

Appropriate:

- follow existing component, type, and test conventions
- modify the page and directly related fixtures
- run required repository checks

Speculative:

- new design-system primitive used once
- feature-flag framework
- backend storage or API version
- cross-repository refactor
- compatibility layer without a changed contract

Why:

Repository size is not the scope of the current feature.

## Example 8: Released config compatibility

Context:

- three released versions still write an old format
- users upgrade at different times
- replacement parser is being introduced

Appropriate:

- bounded compatibility read or explicit migration
- defined precedence
- tests for both active formats
- removal criteria tied to supported versions

Speculative:

- indefinite dual writes without need
- generic version-negotiation platform
- preserving every historical format forever

Why:

Compatibility is a current contract, not speculative complexity. The Skill should make the obligation bounded and explicit rather than delete it.

## Example 9: Local import state explosion

Context:

- one operator
- manually started import
- operator can retry a failed run
- generated design contains ten lifecycle states and two recovery workers

Appropriate:

- preserve only states visible to the product, such as pending/running/failed/completed if each is actually used
- one import path
- honest failure
- transaction or atomic replacement where data integrity requires it

Speculative:

- claimed, delivered, absorbed, acknowledged, stale, recovered, expired
- heartbeat and lease renewal
- supervisor and recovery worker
- dual runtime kept “for safety”

Why:

The state machine models an imaginary distributed service rather than the product that exists.

## Example 10: Destructive local operation

Context:

- one-user desktop utility
- new command permanently deletes files

Appropriate:

- explicit target validation
- path-boundary protection
- preview or confirmation when required by the product
- clear failure and accurate reporting
- tests that prevent deletion outside the allowed root

Wrong simplification:

- removing confirmation or path checks because it is a private tool

Why:

Private does not mean consequence-free. Irreversible deletion is a real boundary.

## Decision test for proposed complexity

Keep a mechanism when all relevant answers are concrete:

1. What present behavior or risk requires it?
2. Who or what uses it now?
3. What breaks without it?
4. How will it be verified?
5. When can it be removed, if temporary?

Reject or postpone it when the rationale is mostly:

- future scale
- abstract best practice
- imagined second implementation
- unspecified enterprise readiness
- fear that direct failure looks unsophisticated

The purpose is not to minimize line count. It is to keep the system explainable while protecting the things that are actually at stake.
