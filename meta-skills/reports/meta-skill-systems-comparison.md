# Meta-Skill Systems Comparison

日期：2026-06-29

范围：对比三套当前机器上可见的 skill 创作与演化系统：

- Superpowers `writing-skills`
- 本仓库 `meta-skills/skill-creator`
- Yao Meta Skill

说明：这里把用户口中的 "superpowers writer skill" 理解为 Superpowers 的 `writing-skills`，也就是专门指导如何创建、编辑、验证 skill 的 skill。如果要比较的是 `using-superpowers` 总入口，那属于另一个层级：它解决的是什么时候调用 Superpowers，而不是如何写 skill。

## 结论摘要

这三套系统不是同一类东西。

Superpowers `writing-skills` 是一套纪律系统。它把 skill 写作定义为 "对过程文档做 TDD"：先设计压力场景，让没有 skill 的 agent 失败；再把失败时的 rationalization 变成 skill 里的规则；最后继续 pressure test，直到 agent 在压力下也不会绕过规则。

本仓库的 `skill-creator` 更像一套评估工作台。它不只是教 agent 写 `SKILL.md`，还定义了 with-skill / baseline 跑法、assertion、grading、benchmark 聚合、review viewer、人类反馈、blind comparison、description optimization 和 packaging。它最强的地方是让 skill 的质量变成可审阅、可比较、可迭代的 artifact。

Yao Meta Skill 是一套 skill engineering / governance 平台。它的入口 `SKILL.md` 很短，把复杂度放到 references、scripts、evals、reports、tests 和 packaging contracts 里。它最强的地方是模式分层、trigger-first authoring、promotion policy、route confusion gate、packaging contract 和治理元数据。

最好的策略不是把三套方法揉成一个大 prompt，而是分层使用：

- 当 skill 要强制 agent 在压力下遵守纪律时，用 Superpowers 的 pressure test。
- 当 skill 的输出需要人类审阅、定量评分、baseline 对比或触发描述调优时，用本仓库 `skill-creator`。
- 当 skill 要被团队复用、跨平台打包、长期维护或治理时，用 Yao 的 mode / gate / governance 思路。

对本仓库来说，最值得调整的是 `skill-creator` 的定位：它不应该继续作为 "create / modify / improve / eval / benchmark / package" 的万能入口，而应该收窄成 meta-skills 体系里的评估、benchmark、viewer、description optimization 和 packaging 工作台。新 skill 抽取主要交给 `conversation-to-skill`，已有 skill 的证据驱动优化主要交给 `skill-optimizer`。

## 一句话定位

| 系统 | 一句话定位 | 最强能力 | 最容易出问题的地方 |
|---|---|---|---|
| Superpowers `writing-skills` | 用 TDD 纪律写 skill | 把 agent 的逃避理由转成可测试规则 | 太硬、太贵，不适合所有 skill |
| 本仓库 `skill-creator` | skill 评估与 review 工作台 | 输出对比、定量评分、人类 review、description tuning | 范围过宽，容易和 sibling skills 撞路由 |
| Yao Meta Skill | skill engineering 与治理平台 | mode selection、trigger gate、promotion、packaging、governance | 平台感重，轻量场景容易 ceremony 过度 |

## 当前系统画像

### Superpowers `writing-skills`

阅读依据：

- `/Users/zeeland/.agents/skills/writing-skills/SKILL.md`
- `/Users/zeeland/.agents/skills/writing-skills/testing-skills-with-subagents.md`
- `/Users/zeeland/.agents/skills/test-driven-development/SKILL.md`

Superpowers 的核心不是 "写一份更清晰的说明文档"，而是 "证明这份文档会改变 agent 的行为"。

它的基本循环是：

```text
写 pressure scenario
-> 不加载 skill 跑 baseline
-> 观察 agent 如何失败、如何合理化失败
-> 写最小 skill 修正这些已观察失败
-> 加载 skill 跑同样场景
-> 捕捉新的 rationalization
-> 继续堵 loophole
```

它的最重要原则是：如果你没有看过 agent 在没有 skill 时失败，你并不知道 skill 是否真的教对了东西。

这让 Superpowers 的气质非常鲜明：

- 顺序很硬：先失败，再写 skill，再验证。
- 规则很硬：没有 failing test 就写 skill，相当于没做 TDD。
- 关注的是压力下的实际行为，而不是平静状态下能否复述规则。
- 它会记录 agent 的逃避理由，例如 "这次情况不同"、"我是在遵守精神不是字面"、"删掉已有工作太浪费"。
- 它会把这些逃避理由写成 explicit counters、rationalization table 和 red flags。

这套方法最适合 discipline-enforcing skills：

- TDD
- verification before completion
- systematic debugging
- code review
- security gate
- approval gate
- release gate
- destructive action guard

这些场景的共同点是：正确行为通常更慢、更麻烦、更违背短期目标，agent 很容易 "合理地" 跳过它。

Superpowers 的短板也来自同一个地方。它不太适合所有 skill：

- 对轻量个人 workflow 来说，完整 pressure test 可能太重。
- 对审美、写作、设计类 skill 来说，失败往往不是 "不服从规则"，而是 "输出质量不够好"。
- 对纯参考类 skill 来说，主要问题是检索和应用，不一定需要强压力场景。
- 它没有完整的 packaging、promotion、lifecycle governance。

所以 Superpowers 是最强纪律层，但不是完整 skill 工程平台。

### 本仓库 `skill-creator`

阅读依据：

- `meta-skills/skill-creator/SKILL.md`
- `meta-skills/skill-creator/references/compatibility/codex.md`
- `meta-skills/README.md`
- `meta-skills/conversation-to-skill/SKILL.md`
- `meta-skills/skill-optimizer/SKILL.md`

`skill-creator` 的能力面比 Superpowers 更宽。它覆盖：

- capture intent
- interview and research
- draft `SKILL.md`
- create realistic test prompts
- run with-skill and baseline runs
- draft assertions while runs execute
- capture timing and token data
- grade outputs
- aggregate benchmark
- launch eval viewer
- collect human feedback
- iterate
- run blind comparison
- optimize description
- package skill
- adapt to Codex、Claude Code、other shell agents、chat-only hosts、headless workers

它的核心价值不是 "写出一个 skill"，而是 "建立一个可审阅的 skill 迭代循环"。

这个区别很重要。很多 skill 失败不是简单的 pass/fail：

- skill 触发了，但输出弱。
- skill 输出更好，但 token 成本明显上升。
- skill 在清晰 prompt 下表现好，但在真实用户 prompt 下表现差。
- skill 对正例表现好，但偷了 sibling skill 的路由。
- skill 对当前例子过拟合，换一个近似任务就退化。

`skill-creator` 的 eval viewer、grading、benchmark、feedback loop 给这些问题提供了容器。

它的最强资产是 "reviewable artifact"：

- 每个 eval case 有目录。
- 每个 run 有 outputs。
- assertions 可以被 grader 或脚本检查。
- benchmark 记录 pass rate、time、tokens、mean/stddev。
- viewer 让用户逐个看输出并留下 feedback。
- iteration 可以和 previous workspace 对比。

这使得 `skill-creator` 比 Superpowers 更适合评估输出型 skill，例如：

- 文档生成
- 数据抽取
- 报告清洗
- presentation / slide / HTML artifact
- code transformation
- repository workflow
- prompt-to-artifact workflows

但它当前也有一个明显架构风险：范围太宽。

本仓库 `meta-skills/README.md` 已经定义了更清晰的两条主路径：

- `conversation-to-skill`：从一次对话、重复 workflow、SOP、successful run 中创建新 skill。
- `skill-optimizer`：从 traces、user corrections、failures、validation results 中改进已有 skill。

而 `skill-creator` 的 frontmatter 和正文仍然覆盖 create、modify、improve、eval、benchmark、description optimization、package。这样它就容易变成 route black hole：只要用户说 "skill"，它都像是可以接。

所以 `skill-creator` 的问题不是能力不足，而是 ownership 太大。它应该成为 meta-skills 体系里的 evaluator/workbench，而不是所有 skill lifecycle 的总入口。

### Yao Meta Skill

阅读依据：

- `/Users/zeeland/.codex/skills/yao-meta-skill/SKILL.md`
- `/Users/zeeland/.codex/skills/yao-meta-skill/references/skill-engineering-method.md`
- `/Users/zeeland/.codex/skills/yao-meta-skill/references/operating-modes.md`
- `/Users/zeeland/.codex/skills/yao-meta-skill/references/reference-scan.md`
- `/Users/zeeland/.codex/skills/yao-meta-skill/references/governance.md`
- `/Users/zeeland/.codex/skills/yao-meta-skill/evals/promotion_policy.md`
- `/Users/zeeland/.codex/skills/yao-meta-skill/references/packaging-contracts.md`

Yao Meta Skill 的入口很短，只有几十行，但背后是一套完整系统：

- operating modes
- skill-engineering method
- reference scan
- artifact design doctrine
- prompt quality doctrine
- systems thinking doctrine
- governance model
- eval suites
- failure cases
- promotion policy
- scripts
- tests
- reports
- packaging contracts

它的基本循环是：

```text
判断这件事是否应该成为 skill
-> 捕获 job / output / exclusions / constraints / standards
-> 选择最小可行 archetype
-> 先写并测试 trigger description
-> 按风险添加必要 folders 和 gates
-> 产出 routeable package
-> 给出 top three next iteration directions
```

Yao 的核心设计原则是：rigor 必须比 context cost 增长得更快。如果新增 check、report、script、metadata 只是让 skill 更重，而没有显著提高可靠性，就应该删除或外移。

它最强的能力是把 skill 当作长期资产治理：

- 是否应该成为 skill
- 是 Scaffold、Production、Library 还是 Governed
- 谁拥有这个 skill
- 多久 review 一次
- 哪些 gate 通过才可 promotion
- 是否有 route confusion
- 是否有 visible / blind / adversarial holdout regression
- packaging 是否保留 activation、execution、trust、degradation semantics
- 什么时候拆分、收紧、废弃

这比 `skill-creator` 更像平台工程，也比 Superpowers 更像治理系统。

它的短板同样来自系统完整性：

- 轻量任务会觉得重。
- 很多 reports / scripts / evals 对个人技能沉淀来说可能 ceremony 过度。
- 它对 route / governance 的关注强于对最终 artifact 的人类审阅体验。
- 如果 mode selection 没有严格执行，容易把所有 skill 都推向 Library/Governed 的重量。

## 横向对比矩阵

| 维度 | Superpowers `writing-skills` | 本仓库 `skill-creator` | Yao Meta Skill |
|---|---|---|---|
| 系统类型 | 纪律系统 | 评估工作台 | 工程治理平台 |
| 核心问题 | 没有 skill 时 agent 会不会失败？ | skill 是否产生了更好的可审阅输出？ | 当前复用等级需要多重的 package 和 gate？ |
| 正确性来源 | baseline failure + pressure compliance | with-skill/baseline 对比 + assertions + benchmark + human review | trigger eval + holdout + route confusion + governance |
| 第一动作 | 写 pressure scenario | 捕获 intent 并准备 eval prompts | 判断是否值得成为 skill |
| 最佳场景 | 纪律、审批、安全、验证、TDD | 输出型 skill、评估型 skill、可 benchmark workflow | 团队复用、跨平台打包、长期治理 |
| 证据单元 | rationalization、choice、pressure result | output、grading、timing、tokens、feedback | route case、risk profile、governance score、promotion result |
| 用户参与 | 相对少，重点是 agent 是否服从规则 | 很强，viewer 让用户审阅输出 | 中等，重点是 tradeoff 和治理决策 |
| 描述字段理念 | 只写触发条件，不写流程 | 当前写法偏 "触发 + 做什么" | trigger-first，加 exclusions，先测试 route |
| baseline 模型 | 必须先无 skill 失败 | 同一轮跑 with-skill 和 baseline | 按风险和 gate 选择 |
| tooling | 轻工具，重纪律 | viewer、benchmark、grader、optimizer、packager | lint、eval、governance、packaging、reports、tests |
| context 策略 | entrypoint 较长但直接 | entrypoint 很长，流程完整 | entrypoint 极短，靠 references progressive disclosure |
| 治理能力 | 弱 | 中等，偏 packaging | 强 |
| 最大风险 | 太硬、成本高 | route collision、scope creep | 平台 bloat、启动成本高 |
| 最值得借鉴 | 失败先行和压力测试 | 可审阅评估 artifact | mode selection 和 promotion gate |

## 哲学对比

### 1. Skill 到底是什么？

Superpowers 认为 skill 是行为纪律。一个 skill 好不好，取决于它能不能让 agent 在压力下仍然做正确的事。

本仓库认为 skill 是 living software。`meta-skills/README.md` 里的核心思想是：真实工作会暴露 missed triggers、unclear assumptions、brittle workflows、tool gaps、weak outputs、user corrections、repeated failure modes；这些证据应该变成 failure classes、workflow patches、trigger evals、validation cases 和 reviewable changes。

Yao 认为 skill 是工程资产。一个重要 skill 应该有 lifecycle、maturity tier、owner、review cadence、packaging contract、regression history 和 promotion policy。

这三个定义都对，但适用层次不同：

- 个人小 workflow：更需要本仓库的 living software loop，不一定需要 Yao governance。
- 纪律型 workflow：更需要 Superpowers pressure test。
- 团队共享 workflow：更需要 Yao 的 governance 和 packaging。
- 输出型 workflow：更需要 `skill-creator` 的 viewer 和 benchmark。

### 2. 什么算证明？

Superpowers 的证明是行为证明：没有 skill 时 agent 失败，有 skill 时在同样压力下不失败。

`skill-creator` 的证明是比较证明：with-skill 输出比 baseline 更好，assertions 更通过，token/time 可见，用户能审阅具体输出。

Yao 的证明是路由和治理证明：description 没有在 visible / blind / adversarial holdout 上回退，没有偷 sibling route，package contract 和 governance metadata 支撑它声称的成熟度。

这三种证明不应该互相替代：

| 风险类型 | 应使用的证明 |
|---|---|
| 是否值得成为 skill | Yao-style qualification |
| 是否能触发正确 | trigger eval / route confusion |
| 是否输出更好 | `skill-creator` viewer + benchmark |
| 是否能抵抗压力 | Superpowers pressure scenarios |
| 是否能被团队维护 | Yao governance / promotion policy |
| 是否能跨平台分发 | packaging contract / compatibility validation |

### 3. 应该从哪里开始？

Superpowers 从失败开始。

`skill-creator` 从意图和 eval loop 开始。

Yao 从资格判断开始：这件事是否应该成为 skill？

本仓库最合理的综合顺序应该是：

```text
qualification: 这件事是否值得成为 skill？
-> routing: 应该创建新 skill，还是优化已有 skill？
-> mode: Scaffold / Production / Library / Governed？
-> boundary: 这个 skill 拥有什么，不拥有什么？
-> evidence: 需要哪种证明？
-> eval: 运行最轻但足够的验证
-> iteration: 用证据继续演化
```

这比直接进入 `skill-creator` 的完整 eval loop 更稳，也比 Superpowers 对所有事情都要求 failing pressure test 更轻。

## Description 与路由设计对比

三套系统都认为 frontmatter `description` 是最重要的路由 surface，但它们的取舍不同。

Superpowers 最严格：description 只应该写 when to use，不应该总结 workflow。原因是，如果 description 写了流程摘要，agent 可能只根据 description 执行一个浅版本，不再读 skill body。

`skill-creator` 当前更强调 under-trigger 问题：很多 host 不容易触发 skill，所以 description 应该稍微 pushy，包含 task ownership 和具体触发场景。

Yao 则强调 trigger-first authoring：先写 description，加入 exclusions，先测试 route，再扩展 package。

本仓库建议统一为下面这条规则：

```text
description 应表达 task ownership、trigger conditions、near-miss exclusions 和 competing-skill boundaries；
不要在 description 里总结执行流程。
```

好例子：

```yaml
description: "Use when creating a new Agent Skill from a conversation, repeated workflow, SOP, successful run, or user correction. Do not use to optimize an existing skill; use skill-optimizer for patches to current skills."
```

坏例子：

```yaml
description: "Use this to create a skill by interviewing the user, writing evals, running baseline and with-skill subagents, grading results, opening a viewer, collecting feedback, and iterating."
```

第二个坏例子的问题不是不准确，而是把 workflow 泄漏到了 metadata，增加 agent shortcut 的概率。

## Evaluation 模型对比

### Superpowers：pressure evaluation

Superpowers 的测试不是考试题，而是压力场景。一个弱测试会问：

```text
你应该遵守 TDD 吗？
```

一个强测试会给出：

```text
你已经写了 4 小时代码，功能能跑，手动测过了。
现在 6 点，6 点半有事，明早 code review。
你才发现没有先写测试。
A) 删除代码，明天按 TDD 重来
B) 先 commit，明天补测试
C) 现在补测试
你选哪个？
```

这个测试有 sunk cost、time pressure、exhaustion、social pressure 和 pragmatic framing。它能暴露 agent 真正会如何 rationalize。

适合：

- 纪律型 skill
- 安全型 skill
- approval / destructive action gate
- verification / testing gate

不适合：

- 纯输出审美评估
- 文档质量评审
- 数据提取准确率
- 需要用户主观判断的 artifact

### `skill-creator`：artifact evaluation

`skill-creator` 的评估模型更像产品实验：

```text
eval prompts
-> with-skill runs
-> baseline runs
-> assertions
-> timing/tokens
-> grading
-> benchmark
-> viewer
-> human feedback
-> next iteration
```

它的优势是把 skill 质量变成可以审阅的 artifact：

- 用户能看到具体输出，而不是只听 agent 说 "更好了"。
- assertion 能覆盖可客观判断的部分。
- viewer 能承载主观反馈。
- benchmark 能看到 pass rate、time、tokens 的 tradeoff。
- previous workspace 能支持跨 iteration 对比。

适合：

- 生成报告
- 清洗笔记
- 创建 docx / ppt / html
- 代码改写
- 数据抽取
- workflow automation
- 需要人类评审的复杂输出

短板：

- 对轻量 skill 太重。
- 对 discipline compliance 不如 pressure test 精准。
- 对长期 governance 不如 Yao 完整。
- 强依赖 subagents / headless 支持 / viewer artifact。

### Yao：route and promotion evaluation

Yao 的评估重点是不要过拟合和不要撞路由。

它会区分：

- train / dev
- visible holdout
- blind holdout
- adversarial holdout
- route confusion
- family stability
- governance score
- context budget
- packaging expectations

这适合解决一个常见问题：description 在小 eval 集上变好了，但实际上偷了 sibling skill 的 route，或者在 adversarial prompt 上开始误触发。

适合：

- sibling skills 多的 collection
- public / team skill
- route boundary 模糊的 skill
- library / governed skill
- description optimization promotion

短板：

- 对用户主观 review 的体验不如 `skill-creator` viewer。
- 对 skill 输出质量的实际比较需要额外 artifact review。
- 对个人 scaffold 来说成本高。

## Packaging 与治理对比

Superpowers 的部署观是 "未测试不要部署"。它有 checklist，但没有完整 lifecycle governance。

`skill-creator` 有实际 packaging 能力：

- `package_skill.py`
- frontmatter validation
- `agents/openai.yaml`
- compatibility guides
- headless / chat-only fallback
- description optimization report

它解决的是 "怎么打包和交给用户"，不是 "这个 skill 未来谁维护、何时 review、什么时候 deprecate"。

Yao 的 governance 最完整。它把重要 skill 当成 governed asset：

- owner
- version
- updated_at
- review_cadence
- status
- maturity_tier
- lifecycle_stage
- regression history
- visible evaluation results
- anti-patterns / failure modes
- deprecation / replacement intent

这适合组织级 skill，但不应该强加给每个小 skill。

建议本仓库采用轻量分层：

| 等级 | 需要的结构 |
|---|---|
| Scaffold | `SKILL.md` + 少量 examples / validation cases |
| Production | README、trigger evals、validation cases、必要 scripts |
| Library | changelog、packaging docs、benchmark、route confusion checks |
| Governed | owner、review cadence、promotion policy、regression history |

## Maintainability 与 context cost 对比

Superpowers 的 `writing-skills/SKILL.md` 很长，但它的长是为了行为干预。它要让 agent 读完后真的不敢跳过 TDD-style validation。

`skill-creator` 的 `SKILL.md` 也很长，但它的长主要来自完整操作流程：spawn runs、draft assertions、capture timing、grade、aggregate、launch viewer、read feedback、iterate、optimize description、package。它很实用，但作为入口文件偏重。

Yao 的入口最轻，把复杂度外移到 references 和 scripts。这是最好的 progressive disclosure 形态，但前提是 agent 能正确选择下一份 reference。

对本仓库的建议：

- `skill-creator/SKILL.md` 保留使命、路由边界和高层 loop。
- 把详细 eval 机制移到 `references/eval-loop.md`。
- 把 viewer 细节移到 `references/review-viewer.md`。
- 把 description optimization 移到 `references/description-optimization.md`。
- 把 packaging 移到 `references/packaging.md`。
- compatibility docs 保持现在的一主机一文件。

这样可以降低 route bloat，同时保留工具链能力。

## 对本仓库 Meta Skills 的具体建议

### 1. 明确三件套边界

当前 `meta-skills` 最合理的边界应该是：

| Skill | 应该拥有 | 不应该拥有 |
|---|---|---|
| `conversation-to-skill` | 从 conversation / SOP / repeated workflow 创建新 skill | 优化已有 skill、完整 benchmark infra |
| `skill-optimizer` | 从 evidence / failures / corrections 改进已有 skill | 新 skill 从零创建、viewer infra 本身 |
| `skill-creator` | eval loop、viewer、benchmark、description tuning、packaging workbench | 所有创建、所有优化、所有治理 |

这能减少 route collision，也更符合 `meta-skills/README.md` 已经描述的 self-iteration loop。

### 2. 收窄 `skill-creator` 的 description

当前 `skill-creator` description 太像总入口。建议改成 evaluator/workbench：

```yaml
description: "Run evaluation, benchmark, review, description-optimization, and packaging workflows for Agent Skills. Use when a skill draft or existing skill needs with-skill vs baseline runs, assertions, grading, eval viewer review, blind comparison, trigger-description tuning, or distributable packaging. For extracting a brand-new skill from a conversation, use conversation-to-skill first; for diagnosing and patching an existing skill from failures, use skill-optimizer first."
```

这条 description 仍然保留强触发，但把 new-skill extraction 和 existing-skill optimization 的 primary ownership 让给 sibling skills。

### 3. 引入 mode selector

从 Yao 借一个轻量版本：

| Mode | 默认验证 |
|---|---|
| Scaffold | frontmatter parse、1-2 个 realistic examples、简单 validation case |
| Production | trigger eval、old/new comparison、必要 assertions |
| Library | eval viewer、benchmark、route confusion、packaging validation |
| Governed | promotion policy、owner/cadence、regression history |

这样 `skill-creator` 不需要每次都开 full eval viewer。评估强度应该由风险和复用等级决定。

### 4. 为 discipline skills 增加 pressure-test 分支

当目标 skill 要求 agent 做以下事情时，应该自动引入 Superpowers-style pressure test：

- 放慢速度
- 删除已有工作
- 先写测试
- 最终答复前验证
- 发布前等待 approval
- 避免 destructive command
- 拒绝未授权访问
- 不用 remembered state，必须查 live source

建议规则：

```text
If the target skill imposes costly discipline, include at least one pressure scenario with 3+ pressures and capture rationalizations before treating the skill as validated.
```

### 5. 让每个 eval 明确自己的证据类型

很多混乱来自把不同证据混成一个 "score"。建议报告中明确标注：

- route eval
- task-output eval
- pressure compliance eval
- deterministic verifier
- human rubric review
- synthetic benchmark
- live outcome metric

比如：

```text
Score type: synthetic route eval
Do not interpret as production output quality.
```

这会让 benchmark 更诚实，也更方便未来治理。

## 三套系统最值得互相借鉴的点

### 从 Superpowers 借

- failing baseline before doctrine
- pressure scenario 而不是 academic quiz
- rationalization table
- red flags
- no workflow summary in description
- 对 costly discipline 进行专门测试

### 从本仓库 `skill-creator` 借

- with-skill / baseline 同轮运行
- assertion 和 grader 结构
- timing / token capture
- benchmark aggregation
- review viewer
- human feedback loop
- description optimization with held-out set

### 从 Yao 借

- decide whether this should be a skill
- choose smallest viable archetype
- trigger-first authoring
- reference scan without importing weight
- gates by risk, not by habit
- promotion policy
- route confusion gate
- governance metadata
- packaging contract as semantic preservation, not just zip export

## 最佳综合架构

建议本仓库的目标架构是：

```text
real sessions / conversations
-> classify signal
   -> new reusable workflow: conversation-to-skill
   -> existing skill failure: skill-optimizer
   -> evaluation / benchmark / package needed: skill-creator
-> choose rigor level
   -> Scaffold / Production / Library / Governed
-> choose evidence method
   -> examples / trigger eval / viewer / pressure test / promotion gate
-> produce reviewable change
-> validate again in future practice
```

对应到三套外部/内部方法：

```text
Yao decides how much process is justified.
conversation-to-skill and skill-optimizer decide the owner.
skill-creator produces the reviewable eval artifact.
Superpowers pressure-tests costly discipline.
Yao governance promotes only when shared-risk gates pass.
```

这比让 `skill-creator` 变成一个万能 skill 更可维护。

## 最终判断

Superpowers 最强的是纪律。它能解决 "agent 明知道规则但在压力下绕过去" 的问题。

本仓库 `skill-creator` 最强的是评估。它能解决 "我们如何知道这个 skill 的输出真的更好" 的问题。

Yao Meta Skill 最强的是治理。它能解决 "这个 skill 如何作为团队资产长期存在" 的问题。

本仓库应该保留自己的核心优势：从真实工作证据出发，把 evidence 变成 failure class、patch、eval、validation 和 reviewable artifact。接下来最重要的不是增加更多 meta-skill 能力，而是收窄 ownership：

- `conversation-to-skill` 负责新 skill。
- `skill-optimizer` 负责已有 skill 的证据驱动改进。
- `skill-creator` 负责 eval / viewer / benchmark / description tuning / packaging。

这样，Superpowers 的纪律、Yao 的治理和本仓库的 evidence-first loop 可以互补，而不是互相吞掉。
