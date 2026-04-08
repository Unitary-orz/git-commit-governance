# Scope Selection

`scope` 只表达这次提交的主域，不表达实现层次。

## 选择顺序
- 先看仓库规范：如果 `AGENTS.md`、`docs/`、贡献指南里已经限定了可用 scope，优先复用仓库已有命名。
- 再看产品域：优先选择用户或维护者能直接理解的功能域，例如 `import`、`export`、`field`、`template`、`auth`、`release`。
- 最后才用仓库域：只有当改动明显属于仓库治理、脚本、配置、CI、构建链路时，才使用 `repo`、`build`、`ci` 一类 scope。

## 约束
- 一次提交只选 1 个 scope。
- scope 保持简短，通常 1 个词；必要时可用短短语，但不要拼接多个域。
- 不要优先使用纯技术层名，如 `ui`、`backend`、`api`、`db`，除非仓库明确这么要求。
- 不要把多个改动面直接串在一起，例如 `import-template-field`、`host-and-app`。

## 判定方法
- 如果用户首先感知的是某个功能入口或业务流程，就用该功能域。
- 如果改动横跨多层，但共同服务于同一个功能域，仍然用那个功能域。
- 如果改动没有明确功能域，只是仓库级维护，则用 `repo` 或仓库规定的等价 scope。

## 示例
- 导入流程映射修复：`fix(import): ...`
- 字段编辑与字段合并规则收敛：`refactor(field): ...`
- 模板配置说明更新：`docs(template): ...`
- 构建脚本或仓库配置调整：`chore(repo): ...`

## 反例
- `fix(ui): ...`，如果真正修的是导入流程，应改为 `fix(import): ...`
- `feat(import-template): ...`，一次提交不应混合两个功能域
- `chore(code): ...`，scope 过泛，无法帮助追踪
