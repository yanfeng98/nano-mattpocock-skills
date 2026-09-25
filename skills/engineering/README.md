# 工程

我每天写代码都会用到的技能。

## 用户调用（User-invoked）

只有你亲手输入才能用到（Claude Code：`disable-model-invocation: true`；Codex：`agents/openai.yaml` 里的 `policy.allow_implicit_invocation: false`）。

- **[ask-matt](./ask-matt/SKILL.md)**：问它哪个技能或流程适合你现在的处境。它是架在本仓库用户调用技能之上的一个路由。
- **[grill-with-docs](./grill-with-docs/SKILL.md)**：拷问式访谈，同时建立你项目的领域模型，边谈边磨术语，并就地更新 `CONTEXT.md` 和 ADR。
- **[triage](./triage/SKILL.md)**：让 issue 在一台由分诊角色组成的状态机里流转。
- **[improve-codebase-architecture](./improve-codebase-architecture/SKILL.md)**：巡查代码库，找出深化机会，用一份可视化的 HTML 报告呈现，然后就你选中的那一个拷问到底。
- **[setup-matt-pocock-skills](./setup-matt-pocock-skills/SKILL.md)**：为工程类技能配置这个仓库（问题跟踪器、分诊标签、领域文档的目录结构）。每个仓库跑一次。
- **[to-spec](./to-spec/SKILL.md)**：把当前对话变成一份 spec 并发布到问题跟踪器。
- **[to-tickets](./to-tickets/SKILL.md)**：把任何计划、spec 或对话拆成一组曳光弹工单，每个工单都声明自己的阻塞边，既可以写成文本放进本地文件，也可以在真正的跟踪器上做成原生阻塞链接。
- **[implement](./implement/SKILL.md)**：实现 spec 或一组工单所描述的工作，在事先约定的接缝上驱动 `/tdd`，提交前用 `/code-review` 收尾。
- **[wayfinder](./wayfinder/SKILL.md)**：把一大块超出单个 agent 会话容量的工作，规划成问题跟踪器上一张由决策工单组成的共享地图，一张一张地解决，直到通往目的地的路清晰起来。

## 模型调用（Model-invoked）

模型或用户都能到达（写足触发措辞，让模型能自行取用它）。

- **[prototype](./prototype/SKILL.md)**：做一个用完就扔的原型来回答某个设计问题：状态或逻辑类问题产出一个可分享的 HTML 文件，UI 类问题给出几个可切换的变体。

- **[diagnosing-bugs](./diagnosing-bugs/SKILL.md)**：针对难缠 bug 和性能回退的严谨诊断循环：先搭一个能在这个 bug 上变红的反馈回路 → 最小化 → 提出假设 → 埋点 → 修复 → 回归测试。
- **[research](./research/SKILL.md)**：依据高可信的一手来源调研一个问题，把结论写成带引用的 Markdown 文件存进仓库，以后台 agent 的方式运行。
- **[tdd](./tdd/SKILL.md)**：红绿重构循环的测试驱动开发。一次一个垂直切片地做功能、修 bug。
- **[domain-modeling](./domain-modeling/SKILL.md)**：主动建立并磨利项目的领域模型：质疑术语、用场景做压力测试，并就地更新 `CONTEXT.md` 和 ADR。
- **[codebase-design](./codebase-design/SKILL.md)**：设计深模块的共用方法论和词汇：小接口、干净的接缝、通过接口可测。
- **[code-review](./code-review/SKILL.md)**：对自某个固定基点以来的 diff 做两个维度的评审：**标准**（是否遵守本仓库的编码标准，外加一份 Fowler 坏味道基线？）与 **Spec**（是否忠实实现了最初的 issue 或 spec？），以并行的子 agent 运行。
- **[resolving-merge-conflicts](./resolving-merge-conflicts/SKILL.md)**：在一个进行中的 git merge 或 rebase 冲突里逐块推进，每块都按追到双方各自一手来源的意图来解决，然后把这步操作做完，绝不 `--abort`。
- **[wizard](./wizard/SKILL.md)**：生成一个交互式 bash 向导，带一个真人走完只有本人才能做的步骤：开通基础设施，配置凭据或 CI secret，在一个陌生的第三方后台里熟悉一遍操作，或者执行一次性的迁移或切换。
