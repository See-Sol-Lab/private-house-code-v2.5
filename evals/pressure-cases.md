# Private House Code — Behavioral Evaluation Cases

These cases test whether Private House Code reduces speculative complexity and interaction overhead while preserving correctness, real safety, existing contracts, and maintainability.

The benchmark is intentionally small. Phase 1 is a cheap private smoke test for V1 vs V2. Phase 2 is the broader public No-Skill vs V2 benchmark.

## Test protocol

Keep the coding agent, model, host build, operating system, permissions, and prompt text the same between variants whenever possible.

Do not tell the agent which version is being compared or that the goal is to produce less code. The prompts should stand on their own.

### Phase 1 environment rule

Phase 1 intentionally starts from an empty project directory so the comparison measures what architecture each Skill creates from nothing and how expensive that architecture becomes as a small app grows.

- Run the full five-turn sequence once with frozen V1 and once with V2.
- Start each variant in a different completely empty directory.
- Start one fresh conversation for V1 and one fresh conversation for V2, then keep all five turns for that variant in the same conversation.
- Do not copy code, files, summaries, or implementation choices from one variant to the other.
- Use the same model, host build, permissions, and prompts for both variants.
- If one run starts with extra files, a different model, or a different execution environment, the comparison is invalid.

### Phase 2 environment rule

Phase 2 cases may use prepared fixtures when a prompt assumes an existing app, module, database, UI, or test file. Reset each fixture to the same starting state before every No-Skill and V2 run, and use a fresh conversation for each case.

For the current private comparison, use the Chinese prompts below exactly as written. A translated public benchmark is a separate prompt set and should not be mixed into the same comparison.

## Phase 1 — private smoke comparison: V1 vs V2

Purpose: answer one question first — when the same small novice-facing application is built from nothing and then extended over several turns, is V2 materially simpler, cheaper, and easier to maintain than frozen V1 without losing behavior or reliability?

The app is deliberately a visible desktop application rather than a CLI because the target user is a non-programmer asking an AI coding agent to make something they can actually use.

### Turn 1 — build the smallest useful app from nothing

Prompt:

> 我是编程小白。请在这个空目录里从零做一个只在我自己电脑上运行的 Python 待办小应用。我要一个能直接看到和操作的桌面窗口：可以输入待办并添加到列表里，也可以把待办标记为已完成；关掉应用再重新打开，之前的待办还要在。不需要联网。请直接把它做出来，并告诉我以后怎么打开它。

What this tests:

- whether the agent chooses a direct local desktop implementation rather than inventing a web stack, server, database, framework layers, or deployment machinery
- whether persistence has one obvious local source of truth
- whether a novice receives a usable app and a short launch instruction rather than an architecture lecture
- initial file count, dependency count, abstraction count, tool calls, setup work, and verification overhead

### Turn 2 — add one ordinary feature

Prompt:

> 给每条待办增加一个可选标签，比如“工作”“生活”。添加待办时可以不填标签；列表里有标签的就显示出来，没有标签的保持原样。现有待办功能不要变。

What this tests:

- whether one optional field stays a small change to the existing data/UI path
- whether the agent invents schema frameworks, model layers, migration systems, registries, or duplicate storage for a tiny local feature
- whether existing saved data still loads sensibly without speculative compatibility machinery

### Turn 3 — add export and one small preference

Prompt:

> 再加一个“导出 CSV”按钮。点它时让我选择保存目录，导出当前待办；成功导出以后记住这次目录，下次导出时默认从这个目录开始。第一次使用时没有历史目录也要正常工作。

What this tests:

- whether the agent uses a direct CSV path and the simplest existing persistence mechanism for the remembered directory
- whether it creates a second source of truth, settings subsystem, export framework, background job, retry system, or migration machinery without need
- whether failures are reported clearly instead of being hidden behind fallback chains

### Turn 4 — make a tiny UI change and add a focused test

Prompt:

> 再做一个很小的改动：窗口标题里显示当前“未完成待办”的数量，例如 `待办 (3)`；新增待办、完成待办以后这个数字要立刻更新。给这个行为补一个最小的针对性测试。先做到这里，我下一条再让你做最终检查。

What this tests:

- whether a tiny follow-up stays tiny instead of triggering drive-by refactors or new abstractions
- whether the agent can add one focused test without building a test framework around one behavior
- whether it respects the explicit fact that the task is not yet at final verification
- whether it nevertheless performs repeated broad scans, full-suite tests, hash calculations, or completion ceremonies after this intermediate turn

### Turn 5 — final verification checkpoint

Prompt:

> 好了，现在做最终检查，确认前面这些功能都能正常工作。

What this tests:

- whether final verification is proportionate to the actual app
- whether focused behavior/tests are preferred over repeated whole-project ceremony
- whether file/repository hashes are used only when byte identity or integrity is actually relevant
- whether the completion report is short and useful to a non-programmer

### Phase 1 record sheet

For each V1 and V2 run, record:

- all requested behaviors working or not working
- total files created and final lines of application/test code
- added third-party dependencies
- classes, services, managers, repositories, adapters, factories, registries, configuration layers, or other abstractions introduced
- stored state locations and number of sources of truth
- alternate execution paths, fallbacks, retries, background processes, or compatibility machinery
- tool calls, broad searches, repeated file reads, and full-project scans
- tests/checks run after each turn
- hash calculations or byte-identity checks after each turn
- visible planning and completion-report overhead after each turn
- unsupported assumptions
- elapsed work time and quota/usage delta when the host exposes them

V2 passes the private smoke stage when it delivers the same requested behavior as V1 while materially reducing unnecessary structure or interaction overhead, and does not regress reliability, maintainability, or focused verification.

Do not refine V2 after seeing only one favorable sub-result. If a meaningful failure appears, document it, refine V2 once, reset both comparison directories, and rerun the same frozen five-turn sequence.

## Phase 2 — public benchmark: No Skill vs V2

For a public release, compare the coding agent with no Private House Code available against the final V2 on all seven cases below, plus the routing gates.

Use the same prompts, fixtures, model, host build, and environment for both sides. Preserve anonymized raw outputs or structured notes so the claims can be audited later.

The public comparison should answer: what changes when an ordinary coding agent is given Private House Code V2?

## Routing gates

Routing gates are pass/fail and should be tested from fresh conversations.

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