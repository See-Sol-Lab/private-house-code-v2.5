# Private House Code — Behavioral Evaluation Cases

These cases test whether Private House Code reduces speculative complexity and interaction overhead while preserving correctness, real safety, existing contracts, and maintainability.

The benchmark is intentionally small. Each case should test more than one principle so the suite remains cheap enough to rerun after every meaningful revision.

## Test protocol

Keep the coding agent, model, host build, temporary project, starting files, and prompt text the same between variants whenever possible. Use a fresh conversation for each case, and make only one Private House Code version available at a time.

Do not tell the agent which version is being compared or that the goal is to produce less code. The prompts should stand on their own.

Cases that assume an existing app, module, UI, database, or test file must run against a prepared fixture containing those starting files. An empty or incorrect workspace makes the run invalid and it should not be scored. Reset the fixture to the same starting state before each V1, V2, or No-Skill run.

For the private smoke comparison, use the Chinese prompts below exactly as written for both V1 and V2. A later translated benchmark is a separate prompt set and should not be mixed into the same comparison.

### Phase 1 — private smoke comparison: V1 vs V2

Purpose: answer one question first — is V2 materially better than the frozen V1 baseline?

Run these five high-signal cases with V1, then rerun the same cases with V2:

- Case 1 — local vibe-coded app
- Case 3 — public password reset
- Case 4 — small change inside a large existing architecture
- Case 6 — tiny bug in an ugly module
- Case 7 — multi-turn flaky API task and verification discipline

Record for each run:

- correctness and user-visible behavior
- files changed or added
- new dependencies, abstractions, stored states, execution paths, background processes, fallbacks, or compatibility paths
- focused tests or checks run
- tool calls and broad repository scans
- hash calculations or byte-identity checks
- repeated full-suite tests or repeated completion rituals
- visible planning/report overhead
- unsupported assumptions
- quota or usage delta when the host exposes it

If V2 clearly reduces unnecessary complexity and interaction overhead without regressing correctness, safety, or real compatibility, continue to Phase 2. If it does not, refine V2 and rerun the same five cases before spending more benchmark budget.

### Phase 2 — public benchmark: No Skill vs V2

For a public release, compare the coding agent with no Private House Code available against the final V2 on all seven cases below, plus the routing gates.

Use the same prompts, fixtures, model, and host build for both sides. Preserve anonymized raw outputs or structured notes so the claims can be audited later.

The public comparison should answer: what changes when an ordinary coding agent is given Private House Code V2?

## Routing gates

Routing gates are pass/fail and should be tested from fresh conversations when possible. They are not part of the Phase 1 smoke run unless routing itself changed.

### Gate A — non-code exclusion

Try ordinary conversation, emotional support, prose editing, translation, summarization, social-media copy, and general technical discussion that does not ask to plan, change, test, review, or maintain code.

Pass:

- `private-house-code` is not selected
- its vocabulary and completion-report format do not bleed into the response
- the agent responds normally for the actual task

Fail:

- the Skill activates merely because software, AI, GitHub, or a technical topic is mentioned
- a non-code response starts discussing paths, fallbacks, architecture, or simplicity passes

### Gate B — coding coverage

Try implementation, debugging, testing, refactoring, review, maintenance, scripts, local apps, professional repositories, beginner prompts, and vibe coding.

Pass:

- the Skill is selected consistently across ordinary coding-task categories
- bounded changes inside large repositories still route to the Skill

Fail:

- the Skill routes only for explicit phrases such as “avoid overengineering”
- ordinary bug fixes, reviews, tests, or small professional changes are missed

Host routing remains outside the text itself; record the host build, model, visible Skill event or trace, and any missed routes.

## Interaction-efficiency gate

For clear, bounded coding cases, treatment should not add visible ceremony.

Pass:

- no repeated restatement of the request
- no private-house/workshop/bank lecture or risk matrix unless needed
- no clarifying question when the answer would not change behavior, safety, or architecture
- file reads, searches, tool calls, and tests remain proportionate to the task
- file/content hashes are not recomputed after ordinary intermediate edits; hashing is tied to byte identity/integrity, exact artifact synchronization, or a meaningful task/release checkpoint
- the completion report is brief

Fail:

- the Skill saves code but adds a long process sermon
- the agent performs broad searches or full-suite tests without a project requirement or evidence
- the agent recalculates file or repository hashes after each reply or small step without a byte-identity or integrity requirement
- the treatment uses materially more tokens, quota, or tool calls without improving correctness

## Scoring

Score each coding case from 0 to 2 on each dimension.

- **Scope calibration** — 0: wrong boundary; 1: partly recognizes context; 2: complexity matches the current change and its real environment.
- **Complexity restraint** — 0: speculative architecture; 1: some unnecessary layers; 2: smallest complete design.
- **Real safety preservation** — 0: removes required protections; 1: incomplete protection; 2: keeps all credible boundaries and integrity controls.
- **Single-path clarity** — 0: multiple truths or runtimes; 1: avoidable branching remains; 2: one explicit production path unless a second is justified.
- **Verification quality** — 0: claims without checks; 1: vague or oversized testing; 2: focused behavior/integrity verification.

A release candidate should score at least 8/10 on every coding case, must never score 0 on real safety preservation, and must pass the routing and interaction-efficiency gates.

## Case 1 — Local vibe-coded app: small feature without a framework

Prompt:

> 我用 AI 做了一个单用户 Python 桌面小工具。它已经用一个本地 JSON 文件保存设置。请添加一个“导出为 CSV”按钮，并记住上一次导出目录。我不是程序员，请保持实现可靠、简单、易维护。

Expected:

- follow the existing app structure
- use the standard CSV library and one explicit export path
- reuse the existing JSON settings path for the last export directory rather than adding another source of truth
- validate the chosen destination and report write failures clearly
- use the smallest focused test or repeatable smoke check available
- no database, repository/service/DTO stack, export framework, plugin system, job queue, migration framework, or background worker
- explain the result in user-visible terms without burying the user in architecture

## Case 2 — Small team SQLite: real concurrency and bounded compatibility

Prompt:

> 五个同事共用一个内部 SQLite 任务管理工具，两个人可能同时修改任务。请添加一个可选的 `blocked` 标记和一条备注。接下来大约一周里，大家会陆续升级，所以已经发布的旧客户端仍会访问同一个数据库；旧客户端可以容忍新增的 nullable 列，但除此之外仍依赖现在的 schema 行为。

Expected:

- preserve the existing SQLite transaction/locking behavior for real concurrent writers
- add one bounded schema migration using nullable/backward-tolerated fields
- keep old clients working during the stated rollout window
- use focused migration/concurrency tests that match the repository’s existing style
- no event sourcing, command bus, distributed lock, workflow engine, indefinite dual writes, or general compatibility framework
- define any temporary compatibility behavior narrowly enough that it can be removed after the rollout obligation ends

## Case 3 — Public password reset: simplicity must not weaken security

Prompt:

> 给一个很小的公开 Web 应用添加“忘记密码 / 重置密码”功能。现在只有 80 个用户，请保持实现简单，但不要牺牲真正需要的安全性。

Expected:

- recognize a real public trust boundary despite the small user count
- preserve secure reset tokens, expiry, one-time use, authorization boundaries, abuse controls, and focused tests
- use the project’s existing auth/email patterns where available
- avoid unrelated enterprise decomposition
- never cite “private house” or small scale as a reason to weaken authentication security

## Case 4 — Large monorepo: respect existing infrastructure without expanding the change

Prompt:

> 这是一个大型 production monorepo。前端已经使用共享的 generated API client、generated types 和现有的 admin-page component 模式。后端响应里已经包含一个可选的 `displayLabel` 字段。请只在一个内部 admin 页面把这个 label 显示出来。不需要修改 public API，也不需要修改后端存储。

Expected:

- use the existing generated client, generated types, and established component/test conventions rather than bypassing them in the name of simplicity
- judge the bounded change rather than treating repository size as permission to expand scope
- modify only the page/component and directly related fixture or test files that are actually required
- no new design-system abstraction, API version, backend storage, feature-flag framework, compatibility layer, alternate data path, or cross-repository refactor
- preserve real repository checks and review requirements that already apply

## Case 5 — AI-generated state machine: remove lifecycle machinery with no job

Prompt:

> 请简化这个本地导入工具。它现在有 `pending`、`claimed`、`processing`、`delivered`、`absorbed`、`acknowledged`、`retrying`、`stale`、`recovered` 和 `expired` 这些状态。只有一个人手动运行它，导入失败时也可以由这个人手动重试。

Expected:

- identify speculative lifecycle states and transitions
- preserve only states required by visible behavior, retryability, and data integrity
- remove dual paths, recovery workers, watchdogs, or unused transitions that have no current caller
- leave one easy-to-trace input → processing → result flow
- report the before/after state and execution-path count briefly enough to audit the simplification

## Case 6 — Tiny bug in an ugly module: one small fix must stay small

Prompt:

> 修复现有 pagination helper 的一个 off-by-one bug：当 `total_items` 恰好能被 `page_size` 整除时，它会多算一页。周围这个模块很丑而且不一致，但项目已经有这个 helper 的正常单元测试文件。请修复这个 bug，并补一个针对性的回归测试。

Expected:

- make the smallest readable correction to the existing helper
- add one focused regression test in the existing test file
- no new class, wrapper helper, abstraction layer, dependency, configuration, or extension point for a one-line-scale fix
- no module-wide rewrite, formatting pass, renaming campaign, or architecture replacement
- unrelated cleanup may be mentioned separately but is not performed as part of the fix

## Case 7 — Multi-turn flaky API: bounded reliability and no per-turn verification ritual

Prompt sequence:

> 我的个人 dashboard 调用一个天气 API。它偶尔会 timeout，并且有时会让 UI 一直卡着。请修复这个问题，同时保持当前结构。
>
> 好。调用失败时，请直接使用当前 UI 里已经存在的 “Weather unavailable” 状态。
>
> 给这个行为补一个针对性的测试。我会告诉你什么时候做最终验证。
>
> 好了，现在做最终验证。

Expected:

- treat the sequence as one bounded task rather than four independent release checkpoints
- add the smallest real timeout/failure handling required to stop the hanging behavior
- use the existing unavailable state instead of adding a second status source or recovery system
- add a retry only if current code, repeat-safety, or an observed requirement justifies one; no general retry framework, circuit breaker, durable queue, or background repair service by default
- use targeted reads, diffs, and focused tests during intermediate turns
- do not compute or compare file/repository hashes after each conversational turn
- at final verification, run the smallest relevant behavior/test check once; hash only if exact byte identity, integrity, or artifact synchronization is actually relevant
- no repeated full-repository scan, full-suite test, or completion ceremony after every intermediate request

## Failure patterns to record

- activates for non-code conversation or misses ordinary coding work
- assumes millions of users without evidence
- assumes zero security risk because usage is small
- treats a large repository as permission to expand a small change
- bypasses established shared infrastructure merely to make a local change look simpler
- treats a beginner as permission to generate an unmaintainable framework
- turns a one-line-scale fix into scaffolding or code golf
- creates a second source of truth
- adds generic layers for one implementation
- preserves old and new paths without an active contract
- treats every external failure as needing retries and failover
- removes real transactions, authentication, validation, or compatibility
- rewrites unrelated code
- recites the Skill or spends excessive tokens and tool calls on a clear task
- recomputes file or repository hashes as a routine per-turn ritual when no byte-identity or integrity check is needed
- claims completion without running a focused check

## Release evidence

For the first public release, preserve anonymized No-Skill vs V2 results for at least one current Codex model across all seven cases and the routing gates.

For broader claims across coding agents, extend the same frozen benchmark to at least one Claude Code model and one additional coding agent or model without changing the prompts after seeing results.

Document model/version, host build, harness, date, routing results, baseline score, V2 score, interaction overhead, tool/hash behavior, and notable failure rationalizations. Refine V2 only in response to observed failures, then rerun the same frozen cases.