---
name: setup-ts-deep-modules
description: 把 dependency-cruiser 接进 TypeScript 仓库，让每个包都成为一个深模块（deep module）：实现藏在子文件夹里，只能通过它的入口点（entry point）文件抵达。仅由用户调用。
disable-model-invocation: true
---

# 配置 TS 深模块

把这个仓库里的每个包都做成**深模块**：大量行为放在一个小接口之后。一个包的公开表面是它的**入口点**（包根目录下的那些文件），子文件夹里的一切都是隐藏的。这个技能会安装 [dependency-cruiser](https://github.com/sverweij/dependency-cruiser)，以及那套让入口点成为唯一入口的规则，然后证明这些规则真的会咬人。

关于这套词汇（深模块、接口、接缝（seam）、深度），用 "codebase-design" 调用 Skill 工具，并通篇使用它的用语。

## 它强制出的结构

```
src/packages/
  <name>/
    index.ts        ← an entry point (public). Import this from outside.
    client.ts       ← another entry point. Packages may expose SEVERAL.
    lib/            ← implementation: hidden from outside, free to import each other.
    tests/          ← co-located tests + fixtures (a subfolder, so private).
```

公开表面是包的**根文件**，而不是某个指定的 `index.ts`。按约定，实现放在 `lib/`，测试放在 `tests/`，让每个包都有同样的双文件夹结构。不过规则本身是通用的：*任何*子文件夹里的*任何东西*都是私有的，所以你绝不需要为了新增一个文件夹去改配置。

四条规则，全部是 `error`：

1. **入口点边界**：包外的代码（应用代码或另一个包）只能导入那个包的入口点（它的根文件），绝不导入它子文件夹里的任何东西。
2. **包内自由**：包自己的文件之间可以自由互相导入。
3. **测试穿过入口点**：`<pkg>/tests/` 下的文件可以导入任意包的入口点，以及自己 `tests/` 里的夹具（fixture），但绝不导入任何包的子文件夹内部（连自己的也不行）。跨包的集成测试没问题；深层导入则不行。
4. **无循环**：没有依赖循环。

**要入口点，不要 barrel。** 因为公开表面是*每个*根文件，一个包可以暴露好几个小入口点（`index.ts`、`client.ts`、`server.ts`），而不必把一切都塞进一个巨大的 `index.ts`。不鼓励写那种重新导出整棵子树的 barrel 文件；把入口点保持小，把实现藏进子文件夹。

分层（哪些包可以依赖哪些包）是*另一个*关注点，在配置里作为一个注释掉的桩留着，交给这个仓库自己填。

## 步骤

### 1. 检测环境

- **包管理器**：`pnpm-lock.yaml` → pnpm，`yarn.lock` → yarn，`bun.lockb` → bun，否则用 npm。下面每条命令都用它（`pnpm`/`yarn`/`npm run`/`bunx`）。
- **包根目录**：如果 `src/` 存在就用 `src/packages`，否则用 `packages`。如果仓库已经有了另一套明显的约定，就和用户确认这个选择。
- **已有配置**：检查有没有 `.dependency-cruiser.*` 文件。如果存在，**绝不**覆盖它：把这四条规则和选项合并进去，并告诉用户你加了什么。

**完成判据：** 包管理器、包根目录，以及是否已有配置，这三项都已确认。

### 2. 安装 dependency-cruiser

用检测到的包管理器把 `dependency-cruiser` 作为 devDependency 安装。

**完成判据：** `dependency-cruiser` 出现在 `devDependencies` 里。

### 3. 写配置文件

把 [`dependency-cruiser.config.cjs`](./dependency-cruiser.config.cjs) 复制到仓库根目录，命名为 `.dependency-cruiser.cjs`。把 `PACKAGES_ROOT` 设成第 1 步检测到的根目录。规则基于路径深度，且不区分文件扩展名，所以其他什么都不用改。

**完成判据：** `.dependency-cruiser.cjs` 存在且 `PACKAGES_ROOT` 正确，四条禁止规则也都在。

### 4. 把它接进检查

- 加一个 `lint:boundaries` 脚本：`depcruise <packages-root>`（或者 `depcruise src`）。
- 把它并进仓库的总检查命令，也就是那条已经在跑类型检查的命令（例如 `check` / `ci` / `validate` 脚本）。**绝不**去动 `tsconfig`，也不加路径别名。
- 如果没有总检查命令，就加上 `lint:boundaries`，并告诉用户把它纳入 CI。

**完成判据：** `lint:boundaries` 存在，并且和类型检查在同一条命令里执行。

### 5. 为示例包搭脚手架

创建并提交一个 `<packages-root>/example/`，作为可以照抄的模板：

- `index.ts` 是一个入口点。导出一个函数，由它委派给一个内部文件（这样这个包一眼就能看出是*深*的，而不是透传层）。
- `lib/impl.ts`：**子文件夹**里的内部文件，被 `index.ts` 导入，从外面无法触及。
- `tests/example.test.ts` **只**导入 `../index`（一个入口点），并针对公开函数做断言。

告诉用户这是一个起步模板，可以照抄，也可以删掉。

**完成判据：** 示例包已存在，通过一个根入口点暴露自己的行为，并把 `impl` 藏在子文件夹里。

### 6. 证明规则真的会咬人

这是整个技能的完成判据：一份在违规时不报错的配置毫无价值。

1. 跑 `lint:boundaries`。在干净的示例上它必须**通过**。
2. 临时给 `tests/example.test.ts` 加一个深层导入（例如 `import { thing } from "../lib/impl"`）。再跑一次 `lint:boundaries`；它必须**失败**，报出 `tests-through-entrypoints`。
3. 撤掉这个深层导入。再跑一次，它必须**通过**。

**完成判据：** 你看到一次通过、一次因深层导入而失败，然后又一次通过。如果第 2 步不失败，说明规则没接对，所以要先修好再收工。

### 7. 把约定写进文档

写一个 `README.md`，**放在包所在的文件夹里**（`<packages-root>/README.md`，就在它管辖的那些包旁边），内容涵盖：`src/packages/<name>/` 的布局（入口点在根目录，`lib/` 放实现，`tests/` 放测试）、「只通过包的入口点（它的根文件）导入」，以及怎么跑 `lint:boundaries`。明确**不鼓励 barrel 文件**：暴露几个小入口点，而不是用一个 index 重新导出整棵子树。内容只保留这份照抄片段，加上四条规则、每条各一段。

然后从仓库的 agent 指令文件（`CLAUDE.md` 存在就用它，否则用 `AGENTS.md`；两者都没有就创建 `AGENTS.md`）里给它加一个**上下文指针（context pointer）**。一行就够，例如 `Packages are deep modules: see [src/packages/README.md](./src/packages/README.md) before adding or importing one.` 正是这一行让 agent 发现边界规则，而不是一头撞上去。

**完成判据：** `<packages-root>/README.md` 存在且不鼓励 barrel，并且仓库的 `CLAUDE.md`/`AGENTS.md` 链到它。

## 备注

- 配置里的 `$1` 反向引用（dependency-cruiser 的分组匹配）正是让一个包能触及自己内部、而外部做不到的原因。不要把它们摊平成每个包各写一条的规则。
- 公开还是私有由**深度**决定：包的根文件是入口点；子文件夹里的任何东西都是私有的。约定俗成的子文件夹是 `lib/`（实现）和 `tests/`，但规则并没有把它们写死：任何子文件夹都是私有的，所以新增文件夹永远不需要改配置。加入口点就是加一个根文件（不用 barrel）。
- 包是**平铺**的：根目录下只有一层直接子项。包的内部想嵌套多深都行；但一个包里不能包含另一个包。
- 用 `.cjs`（不要用 `.js`），这样即使在 `"type": "module"` 的仓库里，配置里的 `module.exports` 也能工作。
