# Git Commit Governance

## 简介

面向 agent 的 Git 提交规范。

更适合非程序员的 Vibe Coding 场景，让 commit message 聚焦于「功能怎么变了」，不要停留在「代码怎么改了」。

目标是用简短直观的话，说明这次改动对用户或业务带来的实际变化。

**示例**

```text
feat(list): 新增应用列表批量继承入口
fix(import): 修复导入时字段映射兜底顺序
refactor(field): 收敛字段编辑流程的校验与保存逻辑
docs(repo): 更新提交流程说明
```

## ✨ 特点

- **约束**：为 commit 约定了一套统一结构和 scope 规则，避免不同人各写一套风格。
- **稳定的 commit 描述**：统一用简洁中文，尽量一行说清功能点变化，避免空泛描述。
- **效果**：每个 commit 只讲一件事，阅读提交历史时，一眼能看出改了什么、影响到哪块。
- **项目自定义**：通过仓库内独立规则补充 scope 列表和特殊约束，而不用复制这套通用规范。

## 📌 约束说明

- **提交格式**：必须使用 `type(scope): summary`。
- **类型限制**：`type` 仅使用 Conventional Commit 前缀（如 `feat`、`fix`、`refactor`、`docs`、`chore`）。
- **scope 限制**：`scope` 必须为单一且稳定的功能或业务域，不允许混合多个域。
- **描述限制**：commit message 使用简洁中文，一般一行说清功能点变化；允许保留必要的代码层动作词，但必须落到具体功能点，避免“修复问题”“优化代码”这类空泛描述。
- **提交流程限制**：commit-only 任务默认不改代码；若确实需要改，至少先询问用户。
- **提交边界**：一次 commit 只提交同一目标的改动，禁止把无关修改打包在一起。

## 📦 安装

如果你使用默认 Codex skill 目录：

```bash
git clone https://github.com/Unitary-orz/git-commit-governance ~/.codex/skills/git-commit-governance
```

如果你使用自定义 `CODEX_HOME`，安装到：

```text
$CODEX_HOME/skills/git-commit-governance
```

安装后建议重启 Codex。

## 🔌 其他 Agent 接入

这套仓库的核心是可复用的规则文件，而不是某个单独平台的专有格式。对不同 code agent，差异主要在“规则挂载入口”，不是规则正文。

**最小可用文件集**

- `SKILL.md`
- 项目内的 `docs/git-commit提交说明.md`

`references/scope.md` 是可选文件，仅在 scope 较多或需要单独解释时再使用。

如果目标 agent 支持专门的规则文件、workspace instructions 或自定义系统提示词，可以直接引用上述文件内容。

### Claude Code

**建议做法**

1. 先保留本仓库原始结构，作为统一规则源。
2. 在 Claude Code 官方支持的项目规则入口中引用本仓库规则，例如 `CLAUDE.md` 或 `.claude/CLAUDE.md`。
3. 如果项目内存在 `AGENTS.md`、`docs/git-commit提交说明.md` 或项目增量规则，优先遵循项目规则。

## 🔄 更新

```bash
cd ~/.codex/skills/git-commit-governance
git pull
```

如果你使用自定义 `CODEX_HOME`，在对应目录执行 `git pull`。

## 🧭 使用方式

这个 skill 主要做三类事情：

1. 为当前改动生成 commit message
2. 判断合适的 `scope`
3. 帮项目落地自己的 Git 提交规范

**推荐落地方式**

- `git-commit-governance` 负责通用方法
- 项目 git 提交规则 skill 直接写项目事实

**推荐目录结构**

```text
<project>/
├── docs/
│   └── git-commit提交说明.md
└── .codex/
    └── skills/
        └── <project>-git-commit-rules/
            └── SKILL.md
```

如果项目 scope 较多或需要单独说明，再补：

```text
<project>/
└── .codex/
    └── skills/
        └── <project>-git-commit-rules/
            └── references/
                └── scope.md
```

## 💬 常用 Prompt

### 日常提交

```text
提交 git
```

```text
判断这次改动的 scope
```

```text
检查这次提交是否只表达一件事
```

### 项目规范安装

```text
根据 git-commit-governance 安装项目 skill
```

```text
生成 docs/git-commit提交说明.md
```

```text
创建项目增量 skill，不复制公共 skill 正文
```

项目 git 提交规则 skill 可直接基于模板生成：

- `assets/project-skill-SKILL.md`

## 🗂️ 仓库结构

这个仓库按单 skill 仓库维护，仓库根目录就是 skill 根目录：

```text
git-commit-governance/
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── scope.md
│   └── project-installation.md
└── assets/
    ├── git-commit提交说明.md
    └── project-skill-SKILL.md
```

## 📄 许可证

本仓库采用 [MIT License](./LICENSE) 开源协议。
