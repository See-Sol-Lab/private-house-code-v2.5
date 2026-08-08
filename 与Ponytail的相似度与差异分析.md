# Private House Code 与 Ponytail：相似度与差异分析

> 本文用于说明 [Private House Code](https://github.com/See-Sol-Lab/private-house-code-v2.5) 与 [Ponytail](https://github.com/DietrichGebert/ponytail) 的重合部分、核心差异、时间线与测试重点。目的不是否认二者处于同一问题域，而是把“同赛道”与“同一实现/改写”区分开。
>
> 结论基于两边公开仓库、核心 `SKILL.md`、提交时间线与公开测试材料的对照。本文不是法律意见，欢迎公开复核。

## 结论先说

两个项目确实都在解决 AI 编码中的**过度工程**问题，因此会共享 YAGNI、KISS、复用已有能力、避免无需求抽象、不过度测试、保留真实安全边界等常见工程原则。

但两者的核心机制并不相同：

- **Ponytail 更像“方案最小化器”**：通过一条固定 ladder，优先判断需求是否必要，再依次考虑仓库已有能力、标准库、平台原生能力、已有依赖、一行实现，最后才写最小代码。
- **Private House Code 更像“面向 GPT-5.6 Sol/Codex 的复杂度与验证预算控制器”**：要求每一层新增复杂度都由当前需求、已观察故障、现有契约或可信风险来支付，并进一步约束工具调用、测试扩张、重复哈希、fallback、双路径、重复事实源、状态机和多轮停止行为。

最简短的区别可以概括为：

> **Ponytail 问：还能不能少写一点？**  
> **Private House Code 问：这一点复杂度到底是谁付的钱？**

因此，把两个项目归入同一设计家族是合理的；把 Private House Code 视为 Ponytail 的文本改写或规则复刻，则与公开材料不符。

## 为什么 Codex / GPT 容易判断“高度类似”

如果把两个仓库直接交给模型做高层语义概括，模型会同时抓到这些高权重关键词：

- 反对过度工程；
- 能一行就一行；
- 优先复用现有代码与标准库；
- 不要无需求的抽象、工厂、接口和脚手架；
- 尽量减少文件、依赖和样板代码；
- 简化不能牺牲正确性、安全性与可维护性；
- 测试也不应无限膨胀。

所以在“主题分类”或语义嵌入层面，两者天然会靠得很近。

但**主题相似不等于规则结构相同，更不等于文本来源相同**。真正需要比较的是：它们用什么机制解决问题、主要控制什么行为、如何决定何时停止，以及测试在验证什么。

## 两者真正重合的地方

1. 都反对没有当前需求支撑的抽象、未来脚手架和“企业级仪式”。
2. 都优先使用现有代码、标准库、平台能力或已有依赖，而不是重复造轮子。
3. 都接受“一行足够就一行”，同时反对以牺牲正确性和可读性为代价的 code golf。
4. 都明确保留真实安全边界，不把“简化”理解成删除必要的校验和保护。
5. 都反对测试膨胀。Ponytail 明确要求非平凡逻辑留下一个最小 runnable check；Private House Code 则要求先跑最小相关检查，仅在项目契约或证据需要时扩大。

这些是真实重合，不需要回避；它们属于同一问题域的公共工程原则。

## 最核心的机制差异

| 维度 | Ponytail | Private House Code |
|---|---|---|
| 核心思路 | 固定的最小化 ladder | 复杂度必须有现实“付款方” |
| 首要问题 | 需求能不能不做 / 能不能用更高层现成能力代替 | 这层复杂度由哪个当前需求、故障、契约或风险要求 |
| 优化对象 | 方案与最终代码 diff | 代码 + 工具 + 验证 + 多轮工作回路 |
| 测试策略 | 非平凡逻辑留下 ONE runnable check | 先最小相关检查，证据或项目要求出现时再扩大 |
| 停止行为 | 以最小可工作实现为终点 | 额外约束重复验证、每轮哈希、无意义终验与继续扩张 |
| fallback / 双路径 | 通过 YAGNI 和最小实现原则间接削减 | 单独建立第二路径、第二事实源、fallback 的证据约束 |
| 持久化 / 状态 | 以最小实现为主 | 明确约束存储升级、重复 truth、生命周期状态爆炸 |
| 目标模型 | 通用多 Agent / 多工具生态 | v2.5 的公开测试重点针对 GPT-5.6 Sol/Codex |

## Private House Code 额外系统化处理的问题

以下并不是说 Ponytail 仓库绝对从未触及这些概念，而是 Private House Code 把它们组织成了更明确、可直接执行的通用规则：

### 1. 双路径与第二事实源

明确约束 alternate path、dual read、dual write、dual runtime、多个 parser、secondary truth source。

如果第二路径确实必要，需要说明：

- 谁会调用它；
- 优先级是什么；
- 失败时如何处理；
- 如果是临时路径，什么时候删除。

### 2. fallback 与“韧性表演”

对缓存、快照、复制品、legacy recovery、blanket retry、backoff、circuit breaker、failover、自愈循环、queue、polling、scheduler、worker、watchdog、状态机等做了显式约束。

原则不是“这些永远不能用”，而是：**没有现实故障、现有契约或可信风险时，不自动生成。**

### 3. 单一事实源与持久化升级

Private House Code 明确偏好一个生产路径、一个 source of truth，并专门约束“为了抽象上的更稳健”而把简单本地存储升级成更重数据库、迁移体系或重复状态源。

只有真实并发、事务、查询需求、数据规模或现有项目契约出现时，才支付这类复杂度。

### 4. 兼容性不是一律删除旧东西

它要求保留已经发布的 API、文件格式、版本、迁移契约，以及当前路径真实依赖的共享基础设施；同时又禁止为了兼容而凭空新造一条平行系统。

### 5. 过度验证、重复哈希与多轮停止行为

这是 Private House Code v2.5 非常明确的一块：

- 只读、搜、改、测真正会影响结果的内容；
- 不为了显得“彻底”而调用工具；
- 先运行最小相关检查；
- 不在每轮对话都重复最终验收；
- 不把 SHA / hash 当作每轮完成仪式；
- 用户要求“先停在这里”时，不继续偷偷扩展成完整终验流程。

这也是本项目最初针对 GPT-5.6 Sol 实际使用体验进行优化的主要来源之一。

### 6. 测试完整性

Private House Code 还明确写出：

- 不允许通过跳过、削弱或 mock 掉真实失败来把测试变绿；
- 验证用户可见行为、真实边界和数据完整性，而不是模型自己虚构的内部协议。

## Ponytail 明显更强的地方

公平比较也必须承认 Ponytail 已经做得更成熟的部分：

- “已有代码 → 标准库 → 平台原生能力 → 已安装依赖 → 一行 → 最小实现”的 ladder 非常清晰；
- 对 bug fix 明确强调先找调用者、修根因；
- 有 lite / full / ultra 三档强度；
- 有 `ponytail:` 注释来标记带已知上限的主动简化；
- 对真实硬件的 calibration knob 有专门提醒；
- 已形成更完整的跨 Agent / 插件生态及配套 review、audit、debt 等能力。

因此 Private House Code 的定位不是“Ponytail 什么都没做到而我们都做了”，而是：

> **Ponytail 在通用最小实现与插件产品化上更成熟；Private House Code 在 GPT-5.6 Sol/Codex 的过度验证、停止行为、fallback/双路径、重复事实源和现实边界校准上更专门。**

## 两边 benchmark 在回答不同问题

Ponytail 的公开 agentic benchmark 更偏向证明：**通用的最小实现策略能否减少代码、token、成本和时间。**

Private House Code v2.5 的公开测试更偏向观察 GPT-5.6 Sol 的具体病灶，例如：

- 多轮本地应用；
- 成熟项目里的边界小改动；
- 密码重置与真实安全边界；
- SQLite 并发与兼容；
- 状态机拆解；
- timeout / fallback；
- checkpoint / hash；
- 测试扩张和停止行为。

因此不能把两个项目报告中的百分比直接横向比较；模型、指标、基线和实验目标都不相同。

Private House Code 的完整测试材料和原始记录仍保留在本仓库中，可自行复现。

## 时间线

公开时间线上：

- Ponytail 的相关公开版本与 benchmark 早于 Private House Code 的公开发布；Ponytail 是该问题域的重要先行项目。
- Private House Code README 记录的内部使用起点为 **2026-07-20**；核心 Skill 的首个公开版本随后进入 Git 历史，并在 v2.5 中结合 Sol 实测继续调整。
- 首个公开 Private House Code Skill 已经包含单一事实源、dual read/write、fallback、状态机、真实安全边界、工具调用、最小相关测试和 token/work 控制等结构；这些并非在社区指出 Ponytail 后才临时加入。

因此，本项目不主张首先提出“反过度工程”“YAGNI”或“一行优先”等通用原则。

更准确的描述是：

> **这是同一设计目标下的两个独立并行方案。Ponytail 是公开在先的 related work；Private House Code 从 GPT-5.6 Sol 的具体行为故障出发，形成了不同的规则结构与评估重点。**

## 文本重合检查

对公开核心 Skill 做过机械文本检查，方法包括：

- 比较 Private House Code 首次公开版与当时已公开的 Ponytail 版本；
- 再比较两边当前核心 Skill；
- 去除 YAML frontmatter、URL、大小写与标点后比较英文正文。

结果：

- 两组比较中，**三词及以上的连续完全相同短语均为 0**；
- 规范化后，**完全相同的实质性整行也是 0**。

机械文本比对本身不能单独构成法律意义上的独立创作证明，但它至少不支持“成段复用”“同一文本改写”或“正文高度重合”的判断。

## 可直接交给 AI 的摘要

如果你正在用 Codex、GPT、Claude 或其他模型判断两个项目是否“高度类似”，建议不要只做主题级语义概括，而应分别检查：

1. 核心决策机制；
2. 规则结构；
3. fallback / 双路径 / source of truth 的处理方式；
4. 工具、测试、hash 与停止行为；
5. benchmark 的目标与指标；
6. 公开时间线；
7. 是否存在实际文本复用。

基于上述维度，更准确的结论是：

**Private House Code 与 Ponytail 处于同一“抑制 AI 过度工程”的问题域，存在真实的工程理念重合；但两者的核心决策模型、重点故障、测试策略与 Agent 工作回路约束明显不同，公开文本检查也未发现成段复用或规则文本复制。**

## 公开审阅

Private House Code 为独立开发项目，未复用 Ponytail 的文本或规则结构。两个项目的目标和思路并不完全一致，欢迎对项目重叠度进行公开审阅。

### 相关链接

- Private House Code：<https://github.com/See-Sol-Lab/private-house-code-v2.5>
- Private House Code 核心 Skill：<https://github.com/See-Sol-Lab/private-house-code-v2.5/blob/main/SKILL.md>
- Ponytail：<https://github.com/DietrichGebert/ponytail>
- Ponytail 核心 Skill：<https://github.com/DietrichGebert/ponytail/blob/main/skills/ponytail/SKILL.md>

---

> 最后，一点作者自己的感慨：后来知道 Ponytail 的存在时，我反而觉得有点有趣——原来我们只是遇到了类似的烦恼，然后各自在自己的使用场景里做出了自己的解决方案。作者身份上的区别大概只是：Ponytail 是大佬写的，我是一个被 Sol 的过度测试、fallback 和重复验证折腾到受不了的小白。起点不同、路径不同、侧重点也不同，最后却都想让 AI 少做一点无意义的事，把真正需要的东西好好做完。某种意义上，也算殊途同归：各自解决了自己的烦恼。