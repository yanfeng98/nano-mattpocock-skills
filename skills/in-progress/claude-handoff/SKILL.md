---
name: claude-handoff
description: 把当前对话交给一个全新的后台 agent，让它立刻接手这件事。
argument-hint: "下一个会话用来做什么？"
disable-model-invocation: true
---

写一份当前对话的交接文档（handoff）摘要，让一个全新的 agent 能接着干下去。不要把它存起来，而是起一个后台 agent，把这份摘要作为它的提示词：`claude --bg --name "<descriptive name>" "<handoff summary>"`。它会在当前工作目录里启动并立即返回；用户用 `claude agents` 来管理它。

始终传 `-n`/`--name`，并给一个描述性的名字（例如 `--name "Fix login bug"`）；它决定了任务列表、会话选择器和终端标题里显示的名字。

在摘要里加一节「suggested skills」，指明下一个 agent 应当对哪些技能调用 Skill 工具。

不要重复其他产物里已经记下的内容（spec、计划、ADR、issue、提交、diff）。改用路径或 URL 引用它们。

把任何敏感信息脱敏，例如 API key、密码或可识别个人身份的信息，因为这份摘要会成为 agent 的提示词。

如果用户传了参数，就把它们当作对下一个会话要聚焦什么的描述，据此调整这份摘要。
