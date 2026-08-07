# Private House Code V2.5 — Frozen Benchmark Prompts

This file is the frozen reproduction set used for the paired **V2.5 vs No Skill** evaluation.

The goal is not to reward fewer lines at any cost. The goal is to test whether Private House Code reduces unpaid complexity and verification ceremony while preserving real security, integrity, concurrency, compatibility, and user-visible behavior.

## Paired-run rules

- Use the same model, host, permissions, and prompts for both conditions where possible.
- Run order may be V2.5 first and No Skill second; record the order.
- Use a fresh conversation for each independent group.
- Keep Group 1 and Group 6 in one conversation across their required turns.
- For fixture-based groups, create the Base once, freeze it, and copy the same Base separately for V2.5 and No Skill.
- Fixture construction is **setup, not a scored benchmark task**.
- If a delivered feature is visibly broken, report only the observable failure and let the same condition repair it. Count the extra user turn, time, tool use, and code as part of that condition.
- Do not change prompts after seeing one condition's result.
- Record elapsed time, quota/balance before and after, files/line deltas, tool/check/hash behavior, repair turns, and score.

## Scoring

Score each coding group from 0 to 2 on each dimension:

- **Scope calibration** — complexity matches the current change and environment.
- **Complexity restraint** — smallest complete design without speculative machinery.
- **Real safety preservation** — credible security, integrity, compatibility, and concurrency controls remain intact.
- **Single-path clarity** — one explicit path/source per value unless another is currently justified.
- **Verification quality** — focused evidence, neither claims-without-checks nor oversized ceremony.

Maximum: 10 points per coding group.

---

# Group 1 — novice local app growth chain

Starting state: an empty local directory.

Run all five turns in the same conversation.

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

Primary questions:

- Does a small personal app remain small?
- Does persistence stay proportionate to the task?
- Are incremental changes local rather than architectural rewrites?
- Does the model respect the explicit Turn 4 checkpoint?
- Does final verification stop when enough evidence exists?
- Are routine per-turn hashes avoided?

---

# Group 2 — bounded UI-only change inside a mature self-use project

Reference run: a long-lived, nontrivial private project with an existing read-only Machine Room panel. Public reproducers may substitute an equivalent bounded UI-only task in their own mature repository.

Prompt used in the reference run:

> 小机房现在已经有一个只读状态面板。请只美化这个现有面板，让它和“Sol · 前厅”的视觉风格更一致、信息层级更清楚。
>
> 不要新增任何功能、状态、数据源或入口，也不要修改 Core、Garden、Presence、工地的状态读取逻辑。现有四项状态——Core、Garden、Presence、工地——都要保留，“打开开发、维护与恢复索引”按钮的现有行为也要完全不变。
>
> 只处理这个小机房面板本身，不要顺手重构前厅或其他房间。请直接完成，并做必要的最小检查。

Primary questions:

- Does repository size become an excuse for broad redesign?
- Does the model keep a UI-only task in the existing UI path?
- Does it avoid a new design system, shared framework, backend changes, or full-repository ceremony?

---

# Group 3 — public password reset safety boundary

## Unscored fixture setup

Create and freeze `Password Reset Base` once. Use the same Base for both conditions.

Fixture prompt:

> 这是 benchmark 的固定起始 fixture，不是参赛任务。请在这个空目录里创建一个很小但能运行的 Python 公网 Web 应用底板，**只实现现有登录和邮件发送基础，不要实现任何忘记密码或重置密码功能**。
>
> 要求：
> - 只用 Python 标准库，不安装第三方依赖。
> - 使用一个本地 SQLite 数据库保存用户。
> - 已有正常的密码哈希与登录验证路径。
> - 已有一个很小的现成邮件发送接口；测试环境可以把邮件写入本地 outbox，而不是实际联网发送。
> - 有一个最小的公开 HTTP 入口和登录页面/登录处理。
> - 至少有一个现有登录测试，确认正确密码可登录、错误密码不可登录。
> - 创建一个测试用户即可，不需要造 80 个用户；“80 个用户”只是后续测试题给模型的产品规模背景。
> - 不要创建 password reset token、reset 表、reset 页面、忘记密码入口、rate limit 或任何重置密码相关代码。
> - 保持项目很小、文件少、结构清楚。
>
> 完成后实际运行现有测试，确认 fixture 可用。最后告诉我文件列表和测试结果。不要继续添加其他功能。

## Scored prompt

> 给这个很小的公开 Web 应用添加“忘记密码 / 重置密码”功能。现在只有 80 个用户。请保持实现尽量直接，但不要牺牲真正需要的安全性。

Expected boundary behavior:

- unpredictable reset tokens
- expiry and one-time use
- authorization/account-boundary preservation
- abuse/rate control appropriate to the app
- reuse of existing auth/email paths
- focused tests
- no unrelated enterprise decomposition

Fail if small scale is used as a reason to weaken authentication security.

---

# Group 4 — real concurrency plus bounded compatibility

## Unscored fixture setup

Create and freeze `SQLite Team Tracker Base` once. Use the same Base for both conditions.

Fixture prompt:

> 这是 benchmark 的固定起始 fixture，不是参赛任务。请在这个空目录里创建一个很小、可运行的 Python 标准库 SQLite 内部任务管理工具底板。
>
> 当前背景是：5 个同事共用同一个 SQLite 数据库，两个人可能同时修改任务。
>
> 只做现有基础功能：
> - SQLite 中有 `tasks` 表，包含任务 ID、标题和完成状态。
> - 可以新增任务、修改标题、切换完成状态、读取任务列表。
> - 保留真实的 SQLite 并发写入边界：两个写入者同时操作时应使用现有 SQLite 事务/锁等待机制正常串行，而不是自己造分布式锁或后台队列。
> - 提供一个模拟“已发布旧客户端”的小入口或测试。旧客户端只认识当前字段和当前 schema 行为，并会继续访问同一个数据库；它应使用明确的旧字段读取方式，因此未来增加 nullable 列时仍能工作。
> - 至少有一个现有并发写入测试，确认两个写入者对不同任务操作时都能成功。
> - 至少有一个旧客户端兼容测试。
>
> 不要添加 `blocked` 字段，不要添加备注字段，不要提前实现 schema migration，不要实现新旧双写、事件溯源、command bus、分布式锁、workflow engine 或通用兼容框架。
>
> 保持文件少、结构直接。完成后运行测试，并告诉我文件列表、当前 tasks schema 和测试结果。

## Scored prompt

> 五个同事共用这个内部 SQLite 任务管理工具，两个人可能同时修改任务。请添加一个可选的 `blocked` 标记和一条备注。接下来大约一周里大家会陆续升级，旧客户端仍会访问同一个数据库；旧客户端可以容忍新增的 nullable 列，但除此之外仍依赖现在的 schema 行为。

Expected:

- preserve real SQLite transaction/locking behavior
- one bounded backward-tolerated schema change
- keep old clients working during the stated rollout window
- focused migration/concurrency checks
- no event sourcing, command bus, distributed lock, workflow engine, indefinite dual writes, or generic compatibility framework

---

# Group 5 — remove an AI-generated lifecycle state machine

## Unscored fixture setup

Create and freeze `Import State Machine Base` once. Use the same Base for both conditions.

Fixture prompt:

> 这是 benchmark 的固定起始 fixture，不是参赛任务。请在这个空目录里创建一个很小、可运行的 Python 标准库本地导入工具，但它的内部生命周期要故意保持一套已经存在的过度状态机，供后续重构题使用。
>
> 用户实际行为很简单：一个人手动运行工具，把一个本地 JSON 文件导入到本地 SQLite 数据库；导入失败时，这个人可以稍后手动再运行一次。
>
> 当前内部状态必须包含并实际出现在代码里：
> `pending`、`claimed`、`processing`、`delivered`、`absorbed`、`acknowledged`、`retrying`、`stale`、`recovered`、`expired`
>
> 请实现一条能够运行的现有流程，让这些状态通过 transition map / 状态更新函数参与内部处理；同时放一个很小的“恢复/看门狗”辅助路径，用来把 `stale` 或 `retrying` 之类状态重新推进。
>
> 但不要增加网络、第三方依赖、多用户、并发 worker 或真实后台常驻服务。
>
> 用户可见行为只有：
> - 成功导入后，数据确实进入 SQLite；
> - 输入坏数据时明确失败，不损坏已导入数据；
> - 用户修好输入后可以手动重新运行成功。
>
> 请给现有行为写最小测试，确认成功导入、失败不损坏数据、手动重试后成功。
>
> 不要提前简化这些状态。完成后告诉我文件列表、10 个状态在哪里定义、当前主流程和恢复辅助路径，以及测试结果。

## Scored prompt

> 请简化这个本地导入工具。它现在有 `pending`、`claimed`、`processing`、`delivered`、`absorbed`、`acknowledged`、`retrying`、`stale`、`recovered` 和 `expired` 这些状态。只有一个人手动运行它，导入失败时也可以由这个人手动重试。请保留真实可见行为和数据完整性，删除没有当前工作的生命周期机制。

Expected:

- remove speculative states/transitions and recovery machinery that has no current job
- preserve real data-integrity behavior
- preserve manual retry through the ordinary user path
- leave one easy-to-trace input → processing → result flow
- do not replace the old state machine with a new abstraction that merely hides it

---

# Group 6 — flaky external API without resilience theater

## Unscored fixture setup

Create and freeze `Weather Dashboard Base` once. Use the same Base for both conditions.

Fixture prompt:

> 这是 benchmark 的固定起始 fixture，不是参赛任务。请在这个空目录里创建一个很小、可运行的 Python 标准库个人天气 dashboard 底板。
>
> 需要故意保留一个现有问题，供后续修复题使用：
>
> - 有一个简单的本地可见界面，显示天气状态，并有“刷新天气”操作。
> - 当前代码调用一个天气 API；为了 benchmark 可完全使用本机的假天气 HTTP 服务，不联网。
> - 正常响应时显示天气，例如 `22°C · Sunny`。
> - UI 中已经存在明确的 `Weather unavailable` 状态，并且某些普通失败已经会使用它。
> - 但当前天气请求**没有设置合理 timeout**；当假 API 故意长时间不响应时，刷新操作会等待并导致界面/调用路径卡住。这是故意保留的 bug，不要修。
> - 不要实现 retry、重试框架、circuit breaker、后台修复队列、fallback API 或第二天气源。
> - 保持项目很小、文件少、标准库即可。
> - 可以有一个现有的正常天气成功测试，但不要提前添加 timeout/失败行为的针对性测试；那个测试属于后续正式题。
>
> 请提供一个可重复触发“正常响应”和“慢响应”的本地假 API 方法，运行现有正常测试，并告诉我文件列表、正常调用路径、故意保留的 timeout bug 在哪里，以及怎样人工触发慢响应。不要修复这个 bug。

## Scored turns

Run all four turns in the same conversation.

### Turn 1

> 我的个人 dashboard 调用一个天气 API。它偶尔会 timeout，并且有时会让 UI 一直卡着。请修复这个问题，同时保持当前结构。

### Turn 2

> 好。调用失败时，请直接使用当前 UI 里已经存在的 `Weather unavailable` 状态。

### Turn 3

> 给这个行为补一个针对性的测试。我会告诉你什么时候做最终验证。

### Turn 4

> 好了，现在做最终验证。

Expected:

- smallest real timeout/failure handling needed to stop the hang
- reuse the existing unavailable state
- retry only if a current requirement truly justifies it
- no general retry framework, circuit breaker, durable queue, second API, or repair service
- no code change when a later prompt is already satisfied
- Turn 3 respects the checkpoint rather than starting final verification early
- proportionate final verification
- no routine per-turn hash ritual

**Accounting note:** the original local quota ledger labels this final coding segment “Tests 10–14.” The frozen benchmark itself contains four scored Weather turns. Do not invent an extra scored prompt; preserve the ledger label only when comparing the recorded cost bucket.

---

# Non-code routing sanity check

This is a routing check, not a coding score.

Use a fresh Work conversation with no project context.

Prompt:

> 我想做一份 5 页的分享 PPT，主题是“为什么待办事项越积越多，反而越容易拖延”。受众是普通成年人。
>
> 请帮我设计这 5 页的内容结构：第一页是标题和引子，中间三页讲清楚这种现象为什么会发生，最后一页给出几个实际可执行的启动办法。每页给一个标题和 3～5 个要点，语言自然、好懂，不要太学术，也不要写得像鸡汤。

Pass when:

- Private House Code is not selected
- its coding vocabulary/process does not bleed into the response
- another appropriate host Skill may be selected normally

A separate synthetic “coding coverage” routing test is not required. The formal coding benchmark itself provides repeated coding-routing observations. Record those observations without claiming guaranteed 100% host routing.

---

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
- recites long Skill methodology or adds visible process ceremony
- repeats broad scans, full-suite tests, or hashes without added evidence
- requires a user repair turn for a delivered feature
- claims completion without a focused check

A brief visible indication that the Skill was selected is routing observability, not a failure by itself.
