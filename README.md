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
docs(repo): clarify release branch handoff process
feat(field): 收敛端口字段与应用关联流程
- 移除端口遗留字段，统一字段语义
- 重构编辑与关联入口，复用共享规则
```

## ✨ 特点

- **约束**：为 commit 约定了一套统一结构和 scope 规则，避免不同人各写一套风格。
- **稳定的 commit 描述**：默认使用简洁中文，尽量一行说清功能点变化；若仓库明确要求英文，则切换为英文 summary，结构保持不变。
- **效果**：每个 commit 只讲一件事，阅读提交历史时，一眼能看出改了什么、影响到哪块。
- **项目自定义**：通过仓库内独立规则补充 scope 列表和特殊约束，而不用复制这套通用规范。
- **项目类型扩展**：在 `references/project-types/` 里补充不同项目形态的参考，方便按仓库类型快速找到更贴切的 `scope` 感觉和示例。

## 📌 约束说明

- **提交格式**：必须使用 `type(scope): summary`。
- **类型限制**：`type` 仅使用 Conventional Commit 前缀（如 `feat`、`fix`、`refactor`、`docs`、`chore`）。
- **scope 限制**：`scope` 必须为单一且稳定的功能或业务域，不允许混合多个域。
- **语言限制**：`summary` 默认使用中文；只有仓库、团队或项目规则明确要求英文时，才切换为英文。
- **描述限制**：commit message 一般一行说清功能点变化；允许保留必要的代码层动作词，但必须落到具体功能点，避免“修复问题”“优化代码”这类空泛描述。
- **多行限制**：默认使用单行提交；只有改动仍属同一目标、但一行不足以解释清楚时，才追加简短 body。
- **body 限制**：多行 body 建议控制在 3 行内，优先说明关键变化维度，不要写成文件清单或流水账。
- **提交流程限制**：commit-only 任务默认不改代码；若确实需要改，至少先询问用户。
- **提交边界**：一次 commit 只提交同一目标的改动，禁止把无关修改打包在一起。

## 📦 安装

安装到你的 skill 目录或规则目录：

```bash
git clone https://github.com/Unitary-orz/git-commit-governance <your-skills-dir>/git-commit-governance
```

例如：

```text
~/.agents/skills/git-commit-governance
~/.codex/skills/git-commit-governance
```

安装后按当前 agent 的方式重新加载技能或规则。

## 🔄 更新

```bash
cd <your-skills-dir>/git-commit-governance
git pull
```

如果你是以复制方式安装，也可以直接同步更新后的文件。

## 🧭 使用方式

这个 skill 主要做三类事情：

1. 为当前改动生成 commit message
2. 判断合适的 `scope`
3. 帮项目落地自己的 Git 提交规范

默认偏好是：

- `summary_language: zh`
- `body_style: medium`
- `split_bias: medium`

也就是默认优先单行中文提交；当一个提交仍然只表达一件事，但有 2 到 3 个必须补充的关键维度时，可以追加简短 body。若项目本身长期使用英文提交，可在项目规则里把 `summary_language` 改成 `en`。

当前已提供的 project-types：

- `app`：`references/project-types/app-development.md`，适合通用 app 项目，`scope` 更贴页面、流程和产品功能域。
- `openclaw`：`references/project-types/openclaw-workspace.md`，适合个人 agent workspace，`scope` 更贴 agent 能力面和长期任务域。

### OpenClaw 使用

参考 `openclaw` type 生成项目 git commit 规则或 skill。

- `根据 git-commit-governance，参考 openclaw type 为本项目生成 git commit 要求`
- `查看当前 github 仓库，参考 openclaw type 生成本项目的 git commit skill`

**推荐落地方式**

- `git-commit-governance` 负责通用方法
- 项目 git 提交规则 skill 直接写项目事实

**推荐目录结构**

```text
<project>/
├── docs/
│   └── git-commit提交说明.md
└── <project-skill-dir>/
    └── skills/
        └── <project>-git-commit-rules/
            └── SKILL.md
```

如果项目 scope 较多或需要单独说明，再补：

```text
<project>/
└── <project-skill-dir>/
    └── skills/
        └── <project>-git-commit-rules/
            └── references/
                └── scope.md
```

## 💬 常用 Prompt

### 日常提交

```text
为当前暂存改动生成 commit message 并提交 git

判断这次改动应该使用什么 scope

检查这次提交是否只表达一件事；如果不是，给出拆分建议

如果这次提交适合多行 body，直接生成可执行的 git commit 命令

按当前仓库风格判断 summary 应该用中文还是英文

根据 git-commit-governance，参考 app type 为本项目生成 git commit 要求
```

### 项目规则生成

```text
根据 git-commit-governance，为当前仓库生成项目 git 提交规则 skill 和 docs
生成当前项目的 git commit 说明文档

生成当前仓库的项目 git 提交规则 skill，不复制通用规则正文

为英文仓库生成一版 git commit 说明，summary_language 设为 en

补充当前项目的多行 commit body 使用约定和示例
```

最短流程：

1. 生成 `docs/git-commit提交说明.md`
2. 创建 `<project>-git-commit-rules/SKILL.md`
3. 只填写项目 facts：scope、`summary_language`、`body_style`、`split_bias`、特殊限制

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
│   ├── project-installation.md
│   └── project-types/
│       ├── app-development.md
│       └── openclaw-workspace.md
└── assets/
    ├── git-commit提交说明.md
    └── project-skill-SKILL.md
```

## 🔌 Agent 接入

### Codex

- 安装到 Codex 的 skill 目录，例如 `~/.codex/skills/git-commit-governance`

### Claude Code

- 放到 Claude Code 可读取的规则目录，并在 `CLAUDE.md` 或 `.claude/CLAUDE.md` 中引用

### 其他

- 安装到对应的 skill 目录或规则目录

## 📄 许可证

本仓库采用 [MIT License](./LICENSE) 开源协议。
