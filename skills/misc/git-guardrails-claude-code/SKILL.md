---
name: git-guardrails-claude-code
description: 配置 Claude Code hook，在危险 git 命令（push、reset --hard、clean、branch -D 等）执行之前就把它们拦下。当用户想阻止破坏性的 git 操作、添加 git 安全 hook，或在 Claude Code 中拦下 git push/reset 时使用。
---

# 配置 Git 护栏

配置一个 PreToolUse hook，在 Claude 执行危险 git 命令之前拦截并阻止它们。

## 哪些命令会被拦下

- `git push`（包括 `--force` 在内的所有变体）
- `git reset --hard`
- `git clean -f` / `git clean -fd`
- `git branch -D`
- `git checkout .` / `git restore .`

被拦下时，Claude 会看到一条消息，告诉它自己无权访问这些命令。

## 步骤

### 1. 询问范围

询问用户：只为**这个项目**（`.claude/settings.json`）安装，还是为**所有项目**（`~/.claude/settings.json`）安装？

### 2. 复制 hook 脚本

随附的脚本在：[scripts/block-dangerous-git.sh](scripts/block-dangerous-git.sh)

按范围把它复制到目标位置：

- **项目**：`.claude/hooks/block-dangerous-git.sh`
- **全局**：`~/.claude/hooks/block-dangerous-git.sh`

用 `chmod +x` 让它可执行。

### 3. 把 hook 加进设置

加进对应的设置文件：

**项目**（`.claude/settings.json`）：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

**全局**（`~/.claude/settings.json`）：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

如果设置文件已经存在，就把这个 hook 合并进现有的 `hooks.PreToolUse` 数组。不要覆盖其他设置。

### 4. 询问定制

询问用户是否要从被拦下的清单里增加或删除模式。相应地修改复制出来的脚本。

### 5. 验证

跑一个快速测试：

```bash
echo '{"tool_input":{"command":"git push origin main"}}' | <path-to-script>
```

应当以退出码 2 退出，并向 stderr 打印一条 BLOCKED 消息。
