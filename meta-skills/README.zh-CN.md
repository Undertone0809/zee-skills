# Meta Skills

[English](README.md)

Meta skills 用来创建、演进、评估和打包其他 Agent Skills。它们适合把 skill 当作长期工程资产维护的人，而不是把 skill 当成一次性 prompt 的人。

## 为什么需要 Meta Skills

Agent Skill 不应该是写完就结束的静态说明。真正有用的 skill 会在真实任务里暴露边界：漏触发、误触发、假设不清、流程脆弱、工具依赖不稳、输出质量不够、反复被用户纠正。

Meta skills 的核心很简单：实践产生证据，证据推动修改。一次成功经验、一次返工、一次 review blocker，都可以变成 failure class、workflow patch、trigger eval、validation case 或可审阅的 skill 变更。

它们不是帮你写更长的 prompt，而是帮你维护一条 skill engineering loop：

```text
真实任务或会话
-> 观察成功经验、用户纠正和失败模式
-> 判断该创建新 skill 还是优化现有 skill
-> 产出可审阅的 skill 变更
-> 增加 eval / trigger case / validation case
-> 在下一次实践里继续验证
```

## 什么时候用

当某类 agent 行为值得复用、沉淀或系统性改进时，就可以用 meta skills。

- 从真实会话中提炼新的 reusable workflow。
- 把已经重复执行过多次的流程做成可发现、可加载的 Agent Skill。
- 修正触发不稳定、边界不清或输出不一致的现有 skill。
- 把一次失败、返工、review blocker 或成功经验写成可复用规则。
- 给 skill 增加 trigger eval、validation case、benchmark plan 或打包说明。
- 把经验整理成可审阅、可测试、可迭代的工程资产。

## 常见场景

最常见的用法，是根据真实实践维护 Agent Skill：

```text
看一下最近 30 个 Codex session，结合现有的 skills，有哪些新的需要 conversation-to-skill 吗？
```

用 [`conversation-to-skill`](conversation-to-skill/SKILL.md) 从近期会话里找出可复用流程，并生成新的 skill package。

```text
看一下最近 30 个 Codex session，结合现有的 skills，哪些 project skill 需要优化吗？
```

用 [`skill-optimizer`](skill-optimizer/SKILL.md) 从执行痕迹、用户纠正、失败模式和验证结果里找出现有 skill 的修改点。

也可以直接处理更具体的请求：

- “把这次成功的发布流程沉淀成一个 skill。”
- “这个 skill 经常误触发，帮我优化 description 和 trigger eval。”
- “基于这次失败，给现有 skill 加一组 validation cases。”
- “把这个 skill package 整理到可安装、可评估的状态。”

## 自迭代做法

Meta skills 最适合跑成一个持续的 skill-maintenance loop。你不需要很重的人工流程；每天从真实 session 里挑出少量高信号反馈，就足够让 skill 慢慢变好。

在 Claude Code、Codex、OpenClaw，或者任何支持 scheduled work 的 agent runtime 里，可以创建两个每日自动化任务：

```text
看一下最近 30 个 Codex session，结合现有的 skills，有哪些新的需要 Conversation to Skill 吗？
```

用 [`conversation-to-skill`](conversation-to-skill/SKILL.md) 把重复出现的成功 workflow、被用户纠正过的流程、逐渐稳定下来的 SOP，转成新的 skill package。

```text
看一下最近 30 个 Codex session，结合现有的 skills，哪些 project skill 需要优化吗 Skill Optimizer？
```

用 [`skill-optimizer`](skill-optimizer/SKILL.md) 把用户纠正、执行失败、误触发、漏触发、弱输出和 validation gap，转成小而可审阅的 patch。

每份报告都应该包含：

- 每个建议背后的真实 session 证据。
- 这个建议是新 skill candidate，还是现有 skill patch。
- 对应的 failure class 或重复成功模式。
- 建议修改哪个 skill，以及怎么改。
- 用什么 eval、trigger case 或 validation case 证明改动有效。

这样每天会有两份报告：一份发现新的 skill 机会，一份发现已有 project skills 的优化点。人只需要 review 高信号建议，批准值得合入的 patch，然后让第二天的真实 session 继续验证。

## Skills

- [`skill-creator`](skill-creator/SKILL.md)：创建新 skill、优化已有 skill、运行评估循环，并打包 skill artifact。
- [`conversation-to-skill`](conversation-to-skill/SKILL.md)：把有价值的会话或重复流程转成可复用的 skill package。
- [`skill-optimizer`](skill-optimizer/SKILL.md)：基于证据、失败、执行痕迹和用户纠正，优化、加固、benchmark 或重构已有 skill。

## 安装

安装 meta-skills collection：

```bash
npx skills add Undertone0809/zee-agent-skills/meta-skills
```

CLI 可以安装全部 meta skills，也可以只安装你需要的 skill。

## 怎么用

可以从两类入口开始：

1. 需要新 skill：用 [`conversation-to-skill`](conversation-to-skill/SKILL.md)，从会话、SOP、团队流程或成功实践中提炼新 skill。
2. 需要优化旧 skill：用 [`skill-optimizer`](skill-optimizer/SKILL.md)，从真实证据中诊断、patch、验证和 benchmark 现有 skill。

如果还不确定是创建还是优化，先让 agent 对照近期会话和已有 skill 做一次判断：哪些经验值得创建新 skill，哪些问题应该回写到现有 skill。
