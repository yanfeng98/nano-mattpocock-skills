---
name: setup-pre-commit
description: 在当前仓库里配置 Husky pre-commit hook，接入 lint-staged（Prettier）、类型检查与测试。当用户想添加 pre-commit hook、配置 Husky、配置 lint-staged，或加上提交时的格式化 / 类型检查 / 测试时使用。
---

# 配置 pre-commit hook

## 这个技能会配好什么

- **Husky** 的 pre-commit hook
- **lint-staged** 对所有已暂存文件跑 Prettier
- **Prettier** 配置（缺失时创建）
- pre-commit hook 里的 **typecheck** 与 **test** 脚本

## 步骤

### 1. 检测包管理器

检查 `package-lock.json`（npm）、`pnpm-lock.yaml`（pnpm）、`yarn.lock`（yarn）、`bun.lockb`（bun）。用存在的那个。不清楚时默认用 npm。

### 2. 安装依赖

作为 devDependencies 安装：

```
husky lint-staged prettier
```

### 3. 初始化 Husky

```bash
npx husky init
```

这会创建 `.husky/` 目录，并把 `prepare: "husky"` 加进 package.json。

### 4. 创建 `.husky/pre-commit`

写入这个文件（Husky v9+ 不需要 shebang）：

```
npx lint-staged
npm run typecheck
npm run test
```

**按需调整**：把 `npm` 换成检测到的包管理器。如果仓库的 package.json 里没有 `typecheck` 或 `test` 脚本，就略去这几行并告诉用户。

### 5. 创建 `.lintstagedrc`

```json
{
  "*": "prettier --ignore-unknown --write"
}
```

### 6. 创建 `.prettierrc`（缺失时）

只有在没有 Prettier 配置时才创建。用这些默认值：

```json
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": false,
  "trailingComma": "es5",
  "semi": true,
  "arrowParens": "always"
}
```

### 7. 验证

- [ ] `.husky/pre-commit` 存在且可执行
- [ ] `.lintstagedrc` 存在
- [ ] package.json 里的 `prepare` 脚本是 `"husky"`
- [ ] `prettier` 配置存在
- [ ] 跑 `npx lint-staged` 验证它能工作

### 8. 提交

把所有改动过或新建的文件加入暂存区，用这条提交信息提交：`Add pre-commit hooks (husky + lint-staged + prettier)`

这会走一遍新的 pre-commit hook：一次不错的冒烟测试，验证一切都能工作。

## 备注

- Husky v9+ 不需要在 hook 文件里写 shebang
- `prettier --ignore-unknown` 会跳过 Prettier 无法解析的文件（图片等）
- pre-commit 先跑 lint-staged（快，只针对已暂存文件），再跑完整的类型检查与测试
