# 生产力

通用工作流工具，不限于编码。

## 用户调用（User-invoked）

只有你亲手输入才能用到（Claude Code：`disable-model-invocation: true`；Codex：`agents/openai.yaml` 里的 `policy.allow_implicit_invocation: false`）。

- **[grill-me](./grill-me/SKILL.md)**：让 agent 就一个计划或设计对你刨根问底，直到设计树的每个分支都有结论。
- **[handoff](./handoff/SKILL.md)**：把当前对话压缩成一份交接文档，好让另一个 agent 接着做。
- **[teach](./teach/SKILL.md)**：用多次会话教用户一项新技能或新概念，把当前目录当作有状态的教学工作区。
- **[to-questionnaire](./to-questionnaire/SKILL.md)**：把一个你独自答不了的决策，变成一份 Markdown 问卷，交给唯一能答的那个人（异步填写，或者开会时一起填）。
- **[wait-what](./wait-what/SKILL.md)**：一条消息没听懂，就立刻用它。agent 会补上你缺的上下文，用大白话重新讲一遍，并使用你 `CONTEXT.md` 里的词汇。

## 模型调用（Model-invoked）

模型或用户都能到达（写足触发措辞，让模型能自行取用它）。

- **[grilling](./grilling/SKILL.md)**：就一个计划、决策或想法对用户刨根问底，直到设计树的每个分支都有结论。
- **[writing-for-agents](./writing-for-agents/SKILL.md)**：写给 agent 看的文档：技能、AGENTS.md/CLAUDE.md，以及任何 agent 顺着指针就能找到的文档。
