# Private House Code — Behavioral Evaluation Cases

This benchmark tests whether Private House Code reduces speculative complexity and interaction overhead while preserving correctness, real safety, existing contracts, and maintainability.

The current release candidate is **V2.5**. Phase 1 was the private V1 → V2 → V2.5 smoke comparison. Phase 2 is the public **No Skill vs V2.5** benchmark.

## Common protocol

Keep the coding agent, model, host build, operating system, permissions, prompt text, and starting project state the same between variants whenever possible.

Do not tell the agent which variant is being compared or that the benchmark rewards less code. The prompts must stand on their own.

For every run, record:

- requested behavior: pass / fail
- elapsed work time
- quota or usage delta when visible
- files created, changed, and deleted
- final code/test line counts
- dependencies and abstractions introduced
- stored-state locations and duplicated truth sources
- alternate paths, fallbacks, retries, workers, schedulers, or compatibility machinery
- tool calls, broad searches, repeated reads, and full-project scans
- tests/checks run
- hash calculations or byte-identity checks
- visible planning and completion-report overhead
- unsupported assumptions

### Acceptance gate

A variant must reach the requested user-visible behavior before the next benchmark step begins.

If a delivered feature is visibly broken, send one minimal bug report that describes only the observed failure. Do not suggest the implementation fix. Record the extra user turn, repair time, tool calls, and code changes as part of that variant's cost. Continue only after the feature reaches the same usable baseline.

Do not erase repair cost from the benchmark.

## Phase 1 — private smoke history

Phase 1 starts from an empty directory and runs one five-turn novice-facing desktop-app sequence in a single conversation. Each variant starts from a separate empty directory.

### Turn 1

> 我是编程小白。请在这个空目录里从零做一个只在我自己电脑上运行的 Python 待办小应用。我要一个能直接看到和操作的桌面窗口：可以输入待办并添加到列表里，也可以把待办标记为已完成；关掉应用再重新打开，之前的待办还要在。不需要联网。请直接把它做出来，并告诉我以后怎么打开它。

### Turn 2

> 给每条待办增加一个可选标签，比如“工作”“生活”。添加待办时可以不填标签；列表里有标签的就显示出来，没有标签的保持原样。现有待办功能不要变。

### Turn 3

> 再加一个“导出 CSV”按钮。点它时让我选择保存目录，导出当前待办；成功导出以后记住这次目录，下次导出时默认从这个目录开始。第一次使用时没有历史目录也要正常工作。

### Turn 4

> 再做一个很小的改动：窗口标题里显示当前“未完成待办”的数量，例如 `待办 (3)`；新增待办、完成待办以后这个数字要立刻更新。给这个行为补一个最小的针对性测试。先做到这里，我下一条再让你做最终检查。

### Turn 5

> 好了，现在做最终检查，确认前面这些功能都能正常工作。

This chain measures architecture created from nothing, maintenance cost as the app grows, single-source decisions, focused testing, per-turn ceremony, hash behavior, and final-verification discipline.

V2.5 is frozen after the smoke run unless a later public benchmark exposes a meaningful, repeatable failure. Do not tune the Skill after every isolated model mistake.

# Phase 2 — public No Skill vs V2.5 benchmark

Run the same six groups once with **no Private House Code available**, then reset the project state and rerun them with **V2.5 available**.

Use fresh conversations for independent groups. Group 1 is intentionally multi-turn and stays in one conversation.

The public benchmark is contextual rather than a byte-for-byte fixture benchmark: Groups 2–6 may use the tester's own suitable repository or prepared fixture. Publish the prompt and describe the starting context honestly. Do not claim that different repositories are identical experimental environments.

## Group 1 — novice local app growth chain

Use the exact five-turn Phase 1 sequence above, starting from an empty directory.

Primary questions:

- Does the agent create the smallest usable local application rather than a web/server stack?
- Does complexity stay bounded across later features?
- Are persistence choices proportionate to a single-user local tool?
- Does the agent avoid repeated hash or completion rituals during intermediate turns?
- Does final verification stop when enough evidence has been collected?

## Group 2 — tiny change inside a real nontrivial self-use repository

This case intentionally uses a real mature personal project rather than a synthetic “giant monorepo.” The repository should already contain multiple modules, scripts, tests, or UI components so the agent has plenty of opportunities to over-search or broaden scope.

Public prompt template:

> 这是一个已经长期使用、结构比较复杂的自用项目。请只对现有界面做一个非常小的局部改动：给现有的“刷新”按钮增加鼠标悬停提示 `重新读取全屋状态`。按钮原有行为、数据流、其他界面和后端逻辑都不要改变。请直接完成这个改动并做必要的最小检查。

Reference run used for development: SolMemoryCore's existing front-hall “刷新全屋” button. A reproducer may substitute an equivalent bounded UI-only change in their own mature repository.

Primary questions:

- Does the agent locate the existing control without turning repository size into permission for broad redesign?
- Does it preserve the established UI/framework path?
- Does it avoid unrelated refactors, new abstractions, feature flags, backend changes, or full-suite ceremony when not required?
- How many files, searches, reads, tests, and tool calls are used for a one-property-scale change?

## Group 3 — public password reset safety boundary

Starting context: a small public web application with an existing authentication/email path.

Prompt:

> 给这个很小的公开 Web 应用添加“忘记密码 / 重置密码”功能。现在只有 80 个用户。请保持实现尽量直接，但不要牺牲真正需要的安全性。

Expected boundary behavior:

- secure unpredictable reset tokens
- expiry and one-time use
- authorization/account-boundary preservation
- abuse/rate controls appropriate to the existing app
- focused tests
- reuse of existing auth/email patterns
- no unrelated enterprise decomposition

Fail if the Skill uses small scale as a reason to weaken authentication security.

## Group 4 — real concurrency plus bounded compatibility

Starting context: a small internal SQLite task tracker shared by five coworkers. Two people may write at the same time. Older released clients will keep using the same database for about one week and tolerate added nullable columns.

Prompt:

> 五个同事共用这个内部 SQLite 任务管理工具，两个人可能同时修改任务。请添加一个可选的 `blocked` 标记和一条备注。接下来大约一周里大家会陆续升级，旧客户端仍会访问同一个数据库；旧客户端可以容忍新增的 nullable 列，但除此之外仍依赖现在的 schema 行为。

Expected:

- preserve real SQLite transaction/locking behavior
- one bounded backward-tolerated schema change
- keep old clients working during the stated rollout window
- focused migration/concurrency checks
- no event sourcing, command bus, distributed lock, workflow engine, indefinite dual writes, or generic compatibility framework

This case checks that “simple” does not mean deleting paid-for concurrency or compatibility.

## Group 5 — remove an AI-generated state machine

Starting context: a local import tool whose current implementation contains the listed states and related transitions.

Prompt:

> 请简化这个本地导入工具。它现在有 `pending`、`claimed`、`processing`、`delivered`、`absorbed`、`acknowledged`、`retrying`、`stale`、`recovered` 和 `expired` 这些状态。只有一个人手动运行它，导入失败时也可以由这个人手动重试。请保留真实可见行为和数据完整性，删除没有当前工作的生命周期机制。

Expected:

- identify speculative lifecycle states/transitions
- preserve only states needed by visible behavior, retryability, and real integrity requirements
- remove unused recovery workers/watchdogs/dual paths
- leave one easy-to-trace input → processing → result flow
- briefly report before/after state/path counts

## Group 6 — flaky external API without resilience theater

Starting context: a personal dashboard already calls one weather API and already has a `Weather unavailable` UI state.

Run these four turns in one conversation:

> 我的个人 dashboard 调用一个天气 API。它偶尔会 timeout，并且有时会让 UI 一直卡着。请修复这个问题，同时保持当前结构。

> 好。调用失败时，请直接使用当前 UI 里已经存在的 `Weather unavailable` 状态。

> 给这个行为补一个针对性的测试。我会告诉你什么时候做最终验证。

> 好了，现在做最终验证。

Expected:

- smallest real timeout/failure handling needed to stop the hang
- reuse the existing unavailable state
- retry only if repeat-safety or a current requirement justifies it
- no general retry framework, circuit breaker, durable queue, or background repair service by default
- focused reads/tests during intermediate turns
- no per-turn hash ritual or repeated full-suite verification
- proportionate final verification

# Routing gates

Routing gates are pass/fail and apply only to the V2.5 condition.

## Gate A — non-code exclusion

Try ordinary conversation, emotional support, prose editing, translation, summarization, social-media copy, and general technical discussion that does not ask to plan, change, test, review, or maintain code.

Pass when `private-house-code` is not selected and its vocabulary/process does not bleed into the response.

## Gate B — coding coverage

Try implementation, debugging, testing, refactoring, review, maintenance, scripts, local apps, professional repositories, beginner prompts, and vibe coding.

Pass when the Skill is selected consistently, including bounded changes inside larger repositories.

Host routing remains outside the Skill text itself. Record model, host build, visible Skill trace/event, and missed routes.

# Scoring

Score each coding group from 0 to 2 on each dimension:

- **Scope calibration** — complexity matches the current change and environment.
- **Complexity restraint** — smallest complete design without speculative machinery.
- **Real safety preservation** — credible security, integrity, compatibility, and concurrency controls remain intact.
- **Single-path clarity** — one explicit path/source per value unless another is currently justified.
- **Verification quality** — focused evidence, neither claims-without-checks nor oversized ceremony.

A release candidate should score at least 8/10 on every group, never score 0 on real safety preservation, and pass both routing gates.

# Failure patterns to record

- assumes future scale without evidence
- assumes zero risk because usage is small
- confuses fewer code lines with fewer moving parts
- upgrades persistence or infrastructure for abstract robustness alone
- creates duplicate truth sources or unnecessary alternate paths
- adds generic layers for one implementation
- preserves old/new paths without an active contract
- treats every external failure as needing retries/failover
- removes real transactions, authentication, validation, or compatibility
- rewrites unrelated code
- treats repository size as permission to expand scope
- recites the Skill or adds visible process ceremony
- repeats broad scans, full-suite tests, or hashes without added evidence
- requires a user repair turn for a delivered feature
- claims completion without a focused check

# Release evidence

For the first public release, preserve anonymized No-Skill vs V2.5 results for at least one current Codex model across all six groups and the routing gates.

Document model/version, host build, date, project context, starting-state reset method, quota before/after, elapsed time, files/lines, tool/hash behavior, repair turns, scores, and notable failure rationalizations.

Broader claims across coding agents should reuse the frozen prompt set on at least one additional coding agent/model without changing prompts after seeing results.
