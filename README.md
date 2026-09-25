<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 为真正的工程师准备的技能

我每天用来做真正的工程，而不是 vibe coding 的 agent 技能。

开发真实应用很难。GSD、BMAD、Spec-Kit 这类方法想通过接管流程来帮忙，但这么做也拿走了你的控制权，让流程里的问题变得难以解决。

这些技能刻意做得小巧、易改、可组合。它们不挑模型。它们来自几十年的工程经验。放手去折腾，把它们改成你自己的。享受吧。

## Installation (30-second setup)

This repo is installed from source: you clone it, and a script symlinks its skills into the directories your agent reads. Nothing updates behind your back, and `git pull` is the update.

### 1. Clone and link

```bash
git clone https://github.com/yanfeng98/nano-mattpocock-skills.git
cd nano-mattpocock-skills
bash scripts/link-skills.sh
```

It links each skill into `~/.claude/skills` (Claude Code) and `~/.agents/skills` (the cross-client path Codex reads, alongside its own native `~/.codex/skills`), one symlink into your clone per skill:

- An edit to a `SKILL.md` takes effect the next time you start a session.
- `git pull` updates the whole installed set at once.
- Adding or renaming a skill means re-running the script to relink it.
- `misc/` is skipped on purpose. `in-progress/` is linked, because the beta feedback loop runs on a local install.

The script replaces whatever already sits at a target path, file or directory, unless it is a symlink; and it never prunes, so a skill you renamed or removed leaves its old symlink behind: delete that by hand.

### 2. Run `/setup-matt-pocock-skills`

In your agent, run it once per repo. It will:

- Ask you which issue tracker you want to use (GitHub, Linear, or local files)
- Ask you what labels you apply to tickets when you triage them (`/triage` uses labels)
- Ask you where you want to save any docs we create

### 3. Bam - you're ready to go.

## 为什么会有这些技能

我做这些技能，是为了修掉我在 Claude Code、Codex 以及其他编码 agent 上常见的失败模式。

### #1：Agent 没有做我想要的事

> “没人确切知道自己想要什么”
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**。软件开发里最常见的失败模式是没有对齐。你以为开发者知道你想要什么，等看到他们做出来的东西，才发现他们根本没理解你。

到了 AI 时代也是如此。你和 agent 之间存在沟通鸿沟。解法是一次**拷问式访谈**（grilling session），让 agent 就你要做的东西问你具体细节。

**解法**是下面这两个：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md)：用于非代码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md)：和 [`/grill-me`](./skills/productivity/grill-me/SKILL.md) 一样，但多了些好东西（见下文）

这两个是我最受欢迎的技能。它们帮你在动手之前跟 agent 对齐，也深入想清楚你要做的改动。_每次_ 想做什么改动，都要用它们。

### #2：Agent 太啰嗦

> 有了统一语言（ubiquitous language），开发者之间的对话和代码的表达，都源自同一个领域模型。
>
> Eric Evans, [Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题**：项目刚开始时，开发者和软件要服务的人（领域专家）通常说着不同的语言。

我和我的 agent 之间也有同样的拉扯。agent 通常被丢进一个项目，然后就得一边干一边摸清里面的行话。结果一个词就能说清的事，它要用 20 个词。

**解法**是一套共享语言。它是一份文档，帮 agent 破译项目里用的行话。

<details>
<summary>
示例
</summary>

这里有一个例子 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md)，来自我的 `course-video-manager` 仓库。哪一个读起来更容易？

- **BEFORE**: “课程里某个章节中的一节课被做成‘真实’的（也就是在文件系统里有了位置）时会有问题”
- **AFTER**: “物化级联（materialization cascade）出了问题”

这种简洁会在一次又一次的会话里带来回报。

</details>

这一点内建在 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 里。它同样是一次拷问式访谈，但它会帮你和 AI 一起建立共享语言，并把难以解释的决策记进 ADR。

这有多强大，很难解释。它可能是这个仓库里最酷的一招。试试就知道了。

> [!TIP]
> 共享语言除了减少啰嗦，还有很多别的好处：
>
> - **变量、函数和文件的命名保持一致**，都使用共享语言
> - 结果是**代码库对 agent 来说更容易导航**
> - agent **花在思考上的 token 也更少**，因为它能用到更精炼的语言

### #3：代码不对

> “永远迈小步、迈慎重的步。反馈的速率就是你的速度上限。绝不要接太大的活。”
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**：假设你和 agent 已经就“要做什么”对齐了。可 agent _依然_ 做出了垃圾，怎么办？

这时候该检查你的反馈回路了。如果对它产出的代码实际运行情况没有反馈，agent 就是在摸黑前进。

**解法**：你需要常规的那套反馈回路，即静态类型、浏览器访问、自动化测试。

对自动化测试来说，红绿重构循环至关重要。也就是 agent 先写一个失败的测试，再让这个测试通过。这能给它稳定、一致的反馈，从而产出好得多的代码。

我做了**一个 [`/tdd`](./skills/engineering/tdd/SKILL.md) 技能**，可以插进任何项目。它鼓励红绿重构，并就什么算好测试、什么算坏测试给 agent 大量指导。

为了调试，我还做了 **[`/diagnosing-bugs`](./skills/engineering/diagnosing-bugs/SKILL.md)** 技能，它把最好的调试实践包成一个有章法的循环，一个阶段一个阶段地过关。

### #4：我们造出了一坨泥球

> “_每天_ 都要在系统设计上投入。”
>
> Kent Beck, [Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> “最好的模块都很深：大量功能都能通过一个简单的接口访问到。”
>
> John Ousterhout, [A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题**：用 agent 做出来的大多数应用都复杂、难改。因为 agent 能大幅提升写代码的速度，它们也加速了软件熵增。代码库变复杂的速度前所未有。

**解法**是对 AI 驱动的开发采取一种激进的新做法：重视代码的设计。

这一点内建在这些技能的每一层里：

- [`/to-spec`](./skills/engineering/to-spec/SKILL.md) 会在写 spec 之前，先追问你这次要动哪些模块

而最关键的是 [`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md)：它巡查代码库，找出可以做得更深的地方，把候选交到你手上。我建议每隔几天就在自己的代码库上跑一次。它是一次巡查，不是抢救：在真正老旧的代码库上，它会找出真实的候选，但它不会替你把这一坨泥球理清楚。

### 小结

软件工程的基本功比以往任何时候都更重要。这些技能是我把这些基本功浓缩成可重复实践的最好尝试，帮你交付职业生涯里最好的应用。享受吧。

## 参考

这些技能按一个维度划分：谁能调用它们。**用户调用（User-invoked）**的技能，只有你亲手输入才能用到（比如 `/grill-me`），它们的职责是编排。**模型调用（Model-invoked）**的技能既可以由你调用，_也_ 可以在任务合适时由 agent 自动使用，它们承载的是可复用的方法论。用户调用的技能可以调用模型调用的技能，但绝不调用另一个用户调用的。

### 工程

我每天写代码都会用到的技能。

**用户调用（User-invoked）**

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)**：问它哪个技能或流程适合你现在的处境。它是架在本仓库所有用户调用技能之上的一个路由。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)**：拷问式访谈，同时建立你项目的领域模型，边谈边磨术语，并就地更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.md)**：让 issue 沿着由分诊角色组成的状态机流转。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)**：巡查代码库，找出可以做得更深的地方，用一份可视化的 HTML 报告呈现，然后就你选中的那个拷问到底。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)**：为工程类技能配置这个仓库（问题跟踪器、分诊标签、领域文档的目录结构）。在用其他工程技能之前，每个仓库先跑一次。
- **[to-spec](./skills/engineering/to-spec/SKILL.md)**：把当前对话变成一份 spec 并发布到问题跟踪器。不做访谈，只是把你已经谈过的东西综合出来。
- **[to-tickets](./skills/engineering/to-tickets/SKILL.md)**：把任何计划、spec 或对话拆成一组曳光弹工单，每个工单都声明自己的阻塞边，既可以写成文本放进本地文件，也可以在真正的跟踪器上做成原生阻塞链接。
- **[implement](./skills/engineering/implement/SKILL.md)**：实现 spec 或一组工单所描述的工作，在事先约定的接缝上驱动 `/tdd`，提交前用 `/code-review` 收尾。
- **[wayfinder](./skills/engineering/wayfinder/SKILL.md)**：把一大块超出单个 agent 会话容量的工作，规划成问题跟踪器上一张由决策工单组成的共享地图，然后一张一张地解决，直到通往目的地的路清晰起来。

**模型调用（Model-invoked）**

- **[prototype](./skills/engineering/prototype/SKILL.md)**：做一个用完就扔的原型来回答某个设计问题：状态或逻辑类问题产出一个可分享的 HTML 文件，UI 类问题给出几个截然不同的变体，在同一个路由上切换。
- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)**：针对难缠 bug 和性能回退的严谨诊断循环：先搭一个能在这个 bug 上变红的反馈回路 → 最小化 → 提出假设 → 埋点 → 修复 → 回归测试。
- **[research](./skills/engineering/research/SKILL.md)**：依据高可信的一手资料调研一个问题，把结论写成带引用的 Markdown 文件存进仓库，以后台 agent 的方式运行。
- **[tdd](./skills/engineering/tdd/SKILL.md)**：红绿重构循环的测试驱动开发。一次一个垂直切片地做功能、修 bug。
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)**：主动建立并磨利项目的领域模型：拿术语表来检验用词，用边缘场景做压力测试，并就地更新 `CONTEXT.md` 和 ADR。
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)**：设计深模块的共用方法论和词汇：把大量行为放在一个小接口之后，落在干净的接缝上，并通过这个接口可测。
- **[code-review](./skills/engineering/code-review/SKILL.md)**：对自某个固定基准点以来的 diff 做两个维度的评审：**标准**（是否遵守本仓库的编码标准，外加一份 Fowler 坏味道基线？）与 **Spec**（是否忠实实现了最初的 issue 或 spec？），以并行的子 agent 运行，互不干扰。
- **[resolving-merge-conflicts](./skills/engineering/resolving-merge-conflicts/SKILL.md)**：在一个进行中的 git merge 或 rebase 冲突里逐块推进，每块都先追到双方各自的一手来源，弄清意图之后再决定怎么解，然后把这步操作做完（绝不 `--abort`）。
- **[wizard](./skills/engineering/wizard/SKILL.md)**：生成一个交互式 bash 向导，带一个真人走完只有本人才能做的步骤：开通基础设施，配置凭据或 CI secret，在一个陌生的第三方后台里熟悉一遍操作，或者执行一次性的迁移或切换。

### 生产力

通用工作流工具，不限于编码。

**用户调用（User-invoked）**

- **[grill-me](./skills/productivity/grill-me/SKILL.md)**：让 agent 就一个计划或设计对你刨根问底，直到设计树的每个分支都有结论。
- **[handoff](./skills/productivity/handoff/SKILL.md)**：把当前对话压缩成一份交接文档，好让另一个 agent 接着做。
- **[teach](./skills/productivity/teach/SKILL.md)**：用多次会话教你一项新技能或新概念，把当前目录当作一个有状态的教学工作区。
- **[to-questionnaire](./skills/productivity/to-questionnaire/SKILL.md)**：把一个你独自答不了的决策，变成一份 Markdown 问卷，交给唯一能答的那个人：异步填写，或者开会时一起填。它拷问的是发送这件事本身（发给谁、你要拿回什么），不是问卷的主题。
- **[wait-what](./skills/productivity/wait-what/SKILL.md)**：一条消息没听懂，就立刻用它。agent 会补上你缺的上下文，用你的 `CONTEXT.md` 词汇、用大白话重新讲一遍。

**模型调用（Model-invoked）**

- **[grilling](./skills/productivity/grilling/SKILL.md)**：就一个计划、决策或想法对用户刨根问底，直到设计树的每个分支都有结论。它是 `grill-me`、`grill-with-docs`、`triage`、`wayfinder` 和 `improve-codebase-architecture` 背后那个可复用的访谈原语。
- **[writing-for-agents](./skills/productivity/writing-for-agents/SKILL.md)**：写文档给 agent 看：技能、AGENTS.md/CLAUDE.md，以及任何 agent 顺着一条指引就能找到的文档。
