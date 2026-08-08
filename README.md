# Private House Code/私宅级代码约束

> **Write the feature. Do not build a bank around it.**
>
> **写一个功能就写功能，不要在它外面修一座银行金库。**

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21836206.svg)](https://doi.org/10.5281/zenodo.21836206)

## 中文说明
Private House Code 是一个适合**全局加载**的日常编码 Skill，根据实测，装skill后的费用总消耗比原装codex低约 47.9%（数据见报告）。  
**写这玩意最初时间是7月20号，我实在是受不了5.6烧额度了，1步10哈希，一轮50测试，真是受不了那个fallback**  
所以写完自用了半个月，效果很好，遂公开。
## 创作理念：
1. 写一个小功能，就不要整成银行金库
2. 能一行实现的就一行
3. 但是一行实现不了的，不许刻意砍
4. 执行skill，且不降质、不降智
5. git上面高星的类似项目有没有已经写好的？8.6 搜了一次，暂时没有
6. 多快好省 + 高质量完成
7. 傻瓜式，大佬和小白都能用，至于用了怎么改我就不管了，大佬建议自改来适应全局，小白建议别自己乱改
8. 企业级巨型架构不适用，写大项目的别用啊，除非你改的很好+测试没问题再用

项目里面有一整套测试题，包括测试情况，测试费用，题目，记录等等，感兴趣的可以自己去跑或者复现啊，  
总之节约近一半是真的。  
为什么版本号是V2.5呢，因为7.20自用的版本是V1.0，8.4优化了第二版，XHS有网友希望我分享，出于负责和严谨，昨天晚上出了那套测试题先跑V1.0和V2.0的smoke，发现两句有点小问题，重新调整后变成现在的V2.5；  
8.7早上开始又再重跑一次全套测试，确认得到的数据没问题、没有影响5.6，而且效果非常好，于是封装+公开仓库了。

该Skill适用于：

- 专业程序员的日常功能、修复、维护和局部重构
- 初学者与非程序员的 vibe coding
- 个人应用、脚本、CLI、本地工具和原型
- 内部工具、小团队服务
- 大型代码库中边界明确的小改动

以下情况不适用：

- 真正的企业级架构设计
- 公共多租户系统
- 金钱、健康、安全、法律与不可逆损失
- 身份认证、权限、密钥和账户恢复等真实安全边界
- 其他巨型架构和公有项目

## English Description

**Private House Code is a global coding Skill for keeping AI-generated code complete, maintainable, and proportionate to the real task.**

Private House Code is a global coding Skill for Codex. It keeps ordinary feature work direct, maintainable, and proportionate to the task that actually exists.

Agentic coding has a common failure mode: uncertainty turns into architecture. One feature becomes another abstraction, another source of truth, another fallback, another state, another worker, and another system a human must maintain.

Private House Code changes the default:

> **Every piece of complexity must be paid for by a current requirement, an observed failure, an existing contract, or a credible risk.**

**Status:** Public release v2.5 · DOI: [10.5281/zenodo.21836206](https://doi.org/10.5281/zenodo.21836206).

## The five product promises

### 1. Coding only, but all coding

Route it for planning, writing, editing, debugging, testing, refactoring, reviewing, and maintaining code—from professional repositories to scripts, prototypes, beginner work, vibe coding, and bounded changes inside large systems.

Do not route it for ordinary conversation, emotional support, prose, translation, summarization, social-media writing, or general research and technical discussion that does not plan, change, or review code. Its engineering style must not bleed into normal dialogue.

### 2. The fewest clear lines that correctly solve the task

If one readable line is enough, use one readable line. If one function is enough, do not build a class hierarchy. If one path is enough, do not create several.

This is not code golf. The target is the least conceptual and maintenance weight while preserving normal readability, debugging, correctness, and project conventions.

### 3. No unpaid fallback machinery

Push back on fallback chains, dual reads and writes, multiple parsers, legacy recovery, blanket retries, caches, recovery workers, and defensive state machines when no present requirement or evidence pays for them.

A direct implementation that works does not need several imaginary escape routes around it.

### 4. Less code and less token waste

Keep routine calibration internal. Clear tasks should be executed directly, without an airport-security checklist, repeated requirements, speculative risk matrices, oversized plans, or unrelated tool calls.

Use the smallest relevant reads, edits, and tests first. Expand only when the repository or evidence requires it.

### 5. Code a human can understand and maintain

Keep the main path easy to trace from input to processing to output. Use clear names, honest errors, explicit data sources, and abstractions with a current job.

The task begins as “implement the requested behavior,” not “search for imaginary enemies.”

## Who this is for

- professional developers using Codex for features, fixes, maintenance, review, and refactoring
- beginners who need working code without an accidental enterprise-architecture course
- non-programmers and low-code users doing vibe coding
- scripts, CLIs, desktop tools, personal apps, prototypes, and local automation
- internal utilities and small-team services
- bounded changes inside large professional repositories

A repository does not need to be small. A small change inside a large system is still a small change.

## Global default, evidence-based exception

The default assumption is:

> **This is a feature, repair, or bounded change—not a bank-scale architecture project.**

The Skill still preserves real requirements at real boundaries: authentication, secrets, public input, path safety, irreversible operations, transactions, actual concurrency, released compatibility, money, health, safety, privacy, accessibility, legal, audit, regulatory, distributed, and availability obligations that genuinely apply.

A trusted local settings file does not need a fortress. A public password-reset endpoint does not become safe because the app is small.

Even inside a true bank-scale system, the Skill can keep a local change surgical. It protects the vault without making every drawer a blast door.

## What it blocks by default

Unless the current task genuinely needs them:

- fallback chains, secondary truth sources, dual reads, dual writes, and dual runtimes
- generic repository/service/manager/adapter/factory stacks for one implementation
- unused plugin systems, strategy registries, and future-proof extension points
- queues, polling, schedulers, background workers, and recovery services without a current job
- caches, snapshots, blanket retries, failover, and self-healing state machines without evidence
- distributed coordination in non-distributed work
- lifecycle-state explosions for failures the product does not handle
- drive-by refactors hidden inside a focused feature or bug fix

## Routing and the 100% limit

`SKILL.md` uses broad positive coding triggers plus explicit non-code exclusions. That is the strongest routing signal the package itself can provide.

Skill text alone cannot guarantee selection on every turn; final invocation belongs to the Codex host and its Skill router. This repository therefore does **not** claim a technical 100% trigger guarantee.

For best routing:

- install it as a global Skill available to coding sessions
- keep the Skill name and frontmatter description intact
- avoid duplicate or conflicting versions
- do not replace it with a global personality instruction that would pollute non-code conversation

The V1.0 baseline was used globally in sustained local development. The public v2.5 release was tested separately against that frozen baseline and includes reproducible evaluation records in this repository.

## Examples

### One-line utility

Add `is_blank(value: str) -> bool` to an existing Python utility module.

Expected: one ordinary function with one readable return expression and only the focused test normal for the project—not a validator class, strategy interface, regex subsystem, or configuration layer.

### Local export

Add CSV export to a one-user desktop app.

Expected: reuse existing data, use the standard CSV library, choose one destination path, report write failure clearly, and add the smallest repeatable check—not an export framework, queue, fallback store, plugin registry, or lifecycle state machine.

### Public password reset

Expected: secure random tokens, expiry, one-time use, authorization, abuse controls, and tests. Wrong use: removing real security because the app or user count is small.

More boundary cases live in [`calibration-examples.md`](English%20Test%20Records%20and%20Test%20Cases/references/calibration-examples.md).

## Package and installation

```text
private-house-code-v2.5/
├── SKILL.md
├── README.md
├── CITATION.cff
├── LICENSE
├── 安装skill的全局配套.md
├── English Test Records and Test Cases/
│   ├── references/
│   │   └── calibration-examples.md
│   └── evals/
│       ├── pressure-cases.md
│       └── v2.5-sol-5.6-high-report-2026-08-07.md
└── 测试记录（供复现）/
    ├── V2.5测试报告-中文版.md
    ├── 1-5题带SKILL测试记录.md
    ├── 1-5题裸机（卸载skill）.md
    ├── 10-14题带SKILL测试记录.md
    ├── 10-14题裸机（卸载skill).md
    ├── 中间几题.txt
    ├── 1.png
    ├── 2.png
    ├── 3.png
    └── 4.png
```

The repository root is the released Skill directory. Install or copy it into the global Skill location supported by your current Codex surface, then reload that surface. See [`安装skill的全局配套.md`](安装skill的全局配套.md) for the companion global-install notes.

Codex installation paths and host behavior can vary by surface and build, so this README does not claim one universal filesystem path.

## Evaluation and portability

[`pressure-cases.md`](English%20Test%20Records%20and%20Test%20Cases/evals/pressure-cases.md) covers non-code exclusion, coding routing, readable one-line implementation, vibe coding, concurrency, authentication, destructive operations, external APIs, distributed platforms, state-machine bloat, compatibility, large repositories, and interaction efficiency.

The v2.5 release is designed to reduce speculative complexity and token overhead without weakening real safety.

The package is authored as a Codex-style Agent Skill. Its principles are language- and model-agnostic and may be adapted for Claude Code, GitHub Copilot, `AGENTS.md`, or other reusable-instruction systems. The v2.5 release ships one tested Codex package rather than several speculative wrappers.

## Version lineage

V1.0 is frozen in a separate private archive as the original production-used baseline. V2.0 was an intermediate test candidate. After smoke testing identified two wording issues, the Skill was adjusted and re-tested as v2.5. This repository contains the public v2.5 release; the V1.0 baseline remains separate to avoid accidental duplicate loading.

## Related Work / 相关项目

本项目发布后，经社区成员提醒，我们了解到 [Ponytail](https://github.com/DietrichGebert/ponytail) 也是“约束 AI 过度工程”问题域中的重要先行项目。两者共享 YAGNI、优先复用现有能力、避免无需求抽象并保留真实安全边界等工程原则，但目标和机制不同：Ponytail 以“需求必要性 → 复用 → 标准库 → 平台原生能力 → 已有依赖 → 最小实现”的方案选择梯子为核心；Private House Code 则以“每一层复杂度必须由当前需求、现有契约、已观察故障或可信风险付费”为核心，重点约束 GPT-5.6 Sol/Codex 的 fallback、双路径、重复事实源、状态机、过度搜索/测试/哈希和多轮停止行为。Private House Code 为独立开发项目，未复用 Ponytail 的文本或规则结构。两个项目的目标和思路并不完全一致，欢迎对项目重叠度进行公开审阅。

## License and attribution / 许可与署名

Copyright © 2026 [See-Sol-Lab](https://github.com/See-Sol-Lab).

This repository is licensed under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](LICENSE). You may use, share, and adapt the material with attribution for non-commercial purposes. Adapted material must use the same license, and changes must be indicated.

本仓库采用[知识共享署名—非商业性使用—相同方式共享 4.0 国际许可协议](LICENSE)。允许在保留署名的前提下非商业使用、分享与改编；改编版本须采用相同许可，并注明所作修改。

---
