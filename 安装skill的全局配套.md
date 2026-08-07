安装这个东西以后如果要配置全局，我把我的发给你们参考，要全局都用这个skill的直接把内容发给你们自己的codex让他加入AGENTS.md即可，如果只是单独项目配置的也自己调整，各自根据需求改。我这边原版适用于一键安装不想自己折腾的非专业程序员。

## Code Engineering: Private-House Contract

These rules apply only to code implementation and code review for single-machine, single-user private applications. They do not govern ordinary conversation, creative discussion, or non-code tasks.

- Treat the application as a private house, not a bank.
- Optimize for the smallest clear implementation that satisfies current product behavior.
- Keep one explicit path, one source of truth, and the fewest necessary states.
- Fail clearly. Do not add fallback chains, compatibility reads, retries, backoff, leases, claims, heartbeats, watchdogs, self-healing, polling, dual writes, or parallel legacy/new runtimes unless the user explicitly requests them.
- Do not design for hypothetical multi-user, distributed, high-availability, or future-scale requirements.
- Prefer standard library, direct functions, and SQLite. Do not add abstractions or dependencies without a current concrete need.
- If one clear line is enough, use one line; otherwise use the fewest readable lines. Do not use clever code golf.
- Preserve only minimal correctness: path boundaries, atomic writes, SQLite transactions, explicit boundary validation, and protection from actual data corruption.
- Delete replaced code, configuration, tests, and documentation in the same change. Do not leave disabled or legacy paths.
- Before coding, state the shortest data flow and exact files to touch. Keep plans and completion reports concise.
- For implementation and code review, apply the repository skill `$private-house-code`.
