---
name: resolving-merge-conflicts
description: "当你需要解决进行中的 git merge/rebase 冲突（conflict）时使用。"
---

1. **看清这个 merge/rebase 的当前状态**。检查 git 历史，以及有冲突的文件。

2. **找到每一处冲突的一手来源（primary source）**。深入理解每处改动为什么做出，原来的意图是什么。读 commit message，查 PR，查原始的 issue/工单。

3. **解决每一个 hunk。** 尽可能保留双方的意图。无法兼容时，选符合这次 merge 既定目标的那一个，并记下取舍。**不要**发明新行为。一律解决；绝不 `--abort`。

4. 找出项目的**自动检查**并运行它们，通常是先 typecheck、再测试、最后 format。修复这次 merge 弄坏的任何东西。

5. **完成这次 merge/rebase。** 把所有东西 stage 并提交。如果在 rebase，就继续推进 rebase 过程，直到所有 commit 都完成 rebase。
