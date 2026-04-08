# Project Installation

本流程用于把公共 `git-commit-governance` 落到具体仓库，并安装为一个合理的项目提交规范体系。

目标不是复制一份公共 skill 到项目里，而是形成两层结构：
- 公共 skill：负责通用提交格式、检查目标、scope 选择方法、签名底线
- 项目 skill：只负责该仓库自己的增量规则

## 安装后的目标结构
- 全局 skill：`$CODEX_HOME/skills/git-commit-governance`
- 项目文档：`docs/git-commit提交说明.md`
- 项目增量 skill：`.codex/skills/<project>-git-commit-rules/SKILL.md`
- 可选项目参考：`.codex/skills/<project>-git-commit-rules/references/*.md`

## 安装原则
- 公共 skill 保持通用，不写项目私有 scope 白名单。
- 项目规则只写增量，不重复公共 skill 的正文。
- 开发者看项目文档，agent 同时参考公共 skill 与项目增量 skill。
- 项目文档、项目 skill、`AGENTS.md` 三者表述必须一致；若冲突，以项目最新书面规范为准。

## 安装顺序

### 1. 先采集项目现状
在生成项目规则前，先确认仓库现在真实采用什么规范，优先级如下：

1. `AGENTS.md`
2. 现有 `docs/`、贡献指南、发布文档
3. 历史高质量 commit 样本

这里读历史 git 的目的不是“照抄过去”，而是校准项目当前真实风格：
- 当前 `summary` 通常用什么语言
- 团队实际使用哪些 `scope`
- 哪些 `scope` 命名稳定、可复用
- 提交粒度是偏细还是偏粗

如果历史提交质量混乱、命名漂移严重，或项目书面规范已经足够明确，则不要让历史 commit 覆盖书面规范。

### 2. 生成开发者可读文档
从 [assets/git-commit提交说明.md](../assets/git-commit提交说明.md) 生成项目 `docs/git-commit提交说明.md`。

处理原则：
- 如果项目还没有这份文档，直接基于模板生成。
- 如果项目已经有文档，优先在原文上整体修订，使其贴合模板结构与当前规范。
- 文档应面向开发者阅读，不要写成 skill 指令。

文档至少要明确这些内容：
- 提交格式
- `type` 集合
- 项目 `scope` 白名单
- 禁止或不推荐的 `scope`
- `summary` 语言要求
- 提交前的检查目标
- 发布、版本、脚本、签名等特殊限制

### 3. 安装项目增量 skill
在项目 `.codex/skills/` 下新建项目 skill，命名建议为 `<project>-git-commit-rules`。

项目 skill 只保留项目特有内容，例如：
- `summary` 是否固定中文
- `scope` 白名单与命名解释
- 不推荐或禁止的 `scope`
- 发布、版本、桌面端、迁移脚本等特殊限制
- 与项目文档、`AGENTS.md` 的优先级关系

项目 skill 不应重复这些公共内容：
- Conventional Commit 基本格式
- 通用 `type` 规则
- 通用 `scope` 选择方法
- 提交检查的通用目标
- GPG 不绕过原则

### 4. 在项目 skill 中声明依赖关系
项目 skill 开头应明确：
- 先使用公共 `git-commit-governance`
- 再叠加本项目规则
- 如与 `docs/git-commit提交说明.md` 或 `AGENTS.md` 冲突，以项目最新规范为准

### 5. 校对项目文档与项目 skill
完成安装后，检查两类内容是否一致：
- `summary` 语言
- `scope` 白名单
- 禁止 scope
- 发布与签名限制

项目文档和项目 skill 可以表达方式不同，但规则不能相互矛盾。

## 推荐输出

### 项目文档
给开发者使用，建议落在：
- `docs/git-commit提交说明.md`

### 项目 skill
给 agent 使用，建议结构如下：

```text
.codex/skills/<project>-git-commit-rules/
├── SKILL.md
└── references/
    └── scope.md
```

其中：
- `SKILL.md` 写适用范围、增量规则、优先级关系
- `references/scope.md` 写项目 scope 白名单与示例

## 不要这样安装
- 不要把公共 skill 整份复制到项目里继续维护。
- 不要把公共规则和项目规则各写一份完整版。
- 不要只创建项目 skill，不生成开发者可读的项目文档。
- 不要完全依赖历史 commit 反推规则，历史只能用于校准，不是最高优先级来源。

## 完成交付的判断标准
- 项目已经有开发者可直接阅读的 `docs/git-commit提交说明.md`
- 项目已经有仅包含增量规则的 `<project>-git-commit-rules`
- agent 可以明确区分“公共规则”和“项目规则”
- 项目文档、项目 skill、`AGENTS.md` 对核心提交规则表述一致
