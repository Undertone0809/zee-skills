# Zee Skills

[English](README.md)

Zee Skills 是一组 Agent Skills，用来把重复出现的工作沉淀成可复用的 agent 能力。

这个仓库基于一个简单的判断：skills 需要通过实践来变强。真实任务会暴露一个 skill 能做什么、哪里会失败、应该如何触发、需要什么证据、输出质量应该长什么样。好的 skill 维护，会把这些观察转成更清晰的指令、validation cases、evals、examples 和 package。

```text
practice
-> evidence
-> skill change
-> eval or validation case
-> better next run
```

## 这个仓库有什么

- [`meta-skills`](meta-skills/README.zh-CN.md)：创建、提炼、优化、评估和打包 Agent Skills。
- [`flomo-skills`](flomo-skills/README.md)：搜索、创建、编辑、导出和分析 flomo memos。
- [`gstack-style-doc`](gstack-style-doc/README.md)：生成 gstack 风格的技术文档。
- [`screenshot`](screenshot/README.md)：截取桌面、应用、窗口、区域或全屏截图。

每个文件夹的 README 会介绍具体 skill 列表和使用方式。

## 为什么有这个仓库

大多数 agent workflow 一开始都来自真实实践：一次成功的 session、一次用户纠正、一个重复的 repo 操作、一个有效的 review 模式，或者一个有价值的研究习惯。这些流程不应该只停留在一次对话里。

这个仓库用来把这些模式变成长期资产：

- 把重复流程沉淀成可安装的 skills。
- 基于真实执行痕迹和用户纠正优化已有 skills。
- 增加 trigger tests 和 validation cases，让 skill 行为可审阅。
- 把领域知识放在使用它的 workflow 附近。
- 打包 skills，让另一个 agent 之后也能发现并使用。

其中 `meta-skills` 是这套循环的中心。你想从近期实践里创建新 skill，或者基于真实证据优化 project skills 时，优先使用它。

## 安装

安装你需要的文件夹：

```bash
npx skills add Undertone0809/zee-agent-skills/meta-skills
npx skills add Undertone0809/zee-agent-skills/flomo-skills
npx skills add Undertone0809/zee-agent-skills/gstack-style-doc
npx skills add Undertone0809/zee-agent-skills/screenshot
```

对于包含多个 skills 的文件夹，比如 `meta-skills` 和 `flomo-skills`，CLI 可以让你选择安装一个、多个或全部 skills。

## 常见工作流

从真实实践中创建新 skill：

```text
看一下最近 30 个 Codex session，结合现有的 skills，有哪些新的 workflow 应该变成 skill？
```

从使用证据中优化已有 skill：

```text
看一下最近 30 个 Codex session，结合现有的 skills，哪些 project skill 需要优化？
```

安装一个聚焦领域的 collection：

```bash
npx skills add Undertone0809/zee-agent-skills/meta-skills
```

## 仓库约定

- 顶层文件夹是维护中的 skill collections。
- 每个 collection 都有自己的 `README.md`。
- 对嵌套 collection 使用文件夹级安装命令，不使用 repo root 安装命令。
- 优先维护小而聚焦的 skills，并写清楚 triggers、workflow、边界、examples 和 validation cases。
