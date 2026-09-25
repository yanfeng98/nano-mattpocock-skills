# 进行中

Beta。这些技能是刻意公开的：试试它们，告诉我哪里坏了。在升格到稳定桶之前，它们不进顶层 README，也不进已推广的集合，没有对应的 docs 页，而且可能毫无预告地改动或消失。

`scripts/link-skills.sh` 会把它们和其他技能一起链接，所以本地安装里已经有了。只想手动拿其中一个，在仓库根目录跑：

```bash
mkdir -p ~/.claude/skills
ln -sfn "$PWD/skills/in-progress/<name>" ~/.claude/skills/<name>
```

`-f` 会替换已有的链接，而不是写进它里面，所以重复跑是安全的。Codex 和其他 Agent Skills 的 harness 请改用 `~/.agents/skills`。

- **[loop-me](./loop-me/SKILL.md)**：用多次会话把自己拷问成可落地的工作流 spec，把当前目录当作有状态的工作区。用户调用。
- **[writing-beats](./writing-beats/SKILL.md)**：把一篇文章塑造成由节拍构成的旅程，自选冒险式的。先挑一个起始节拍，只写这一个节拍，再转向下一个，直到文章走到自然的结尾。
- **[writing-fragments](./writing-fragments/SKILL.md)**：拷问式访谈：从你身上挖出片段（形形色色的写作小块），把它们追加进同一份文档，作为以后写文章的原始素材。
- **[writing-shape](./writing-shape/SKILL.md)**：拿一份装着原始素材的 markdown 文件，一段一段地把它塑造成文章，每一步都为格式选择争一争。
- **[claude-handoff](./claude-handoff/SKILL.md)**：把当前对话交给一个全新的后台 agent，让它立刻接手工作，用 `claude --bg` 把交接摘要喂给它。用户调用。
- **[setup-ts-deep-modules](./setup-ts-deep-modules/SKILL.md)**：把 dependency-cruiser 接进 TypeScript 仓库，让每个包都成为一个深模块：实现藏在子文件夹里，只能通过它的入口点文件抵达，测试也通过这些入口点锻炼它。用户调用。
- **[implement-spec](./implement-spec/SKILL.md)**：在一个分支上实现整份 spec。把工单当作任务图而不是清单来推进，在就绪的前沿上并行跑实现者子 agent 以求最大并发，最后把结果作为一个 PR 落地。用户调用。
- **[pr](./pr/SKILL.md)**：一份 pull request 正文该长什么样的参考：来自一手来源（primary source）而不是 diff 的摘要、展示这次改动的最小可视化、一组前后对照的证据、刻意排除在外的内容，以及一次单向门/双向门的判断。模型调用。
- **[retro](./retro/SKILL.md)**：在一次会话之后，给编码 agent 的环境（引导文件、编码标准、自动化检查、工具链）提改进建议。STUB：只有设计笔记，还不能用。用户调用。
