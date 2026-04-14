# Project Generation

本流程用于把 `git-commit-governance` 落到具体仓库，并生成为一套可执行、可维护的项目提交规则。

## 生成后的目标结构
- 全局规则仓库：可放在任意便于复用的 skill 目录或规则目录，例如 `~/.agents/skills/git-commit-governance`
- 项目文档：`docs/git-commit提交说明.md`
- 项目增量规则：建议保存在项目内，例如 `<project-skill-dir>/skills/<project>-git-commit-rules/SKILL.md`
- 可选项目参考：例如 `<project-skill-dir>/skills/<project>-git-commit-rules/references/*.md`

说明：
- 如果当前 agent 支持 skill 目录，直接放进项目 skill 目录即可。
- 如果当前 agent 不支持 skill 目录，也应保留一份等价的项目增量规则，并挂载到该 agent 的规则入口。
- 不同 agent 可以共用同一份项目文档与项目增量规则，不要分别维护多套正文。

## 生成原则
- `git-commit-governance` 负责通用提交方法和默认写法。
- 项目 git 提交规则 skill 直接写项目已知事实，例如 scope 白名单、偏好覆盖项、禁止项、发布限制。
- 开发者看项目文档，agent 参考仓库内可用的规则文件。
- 如项目文档或 `AGENTS.md` 有更新，以项目最新规则为准。
- 安装过程中，模型应结合仓库现状、历史提交风格和用户表达习惯，推断偏好覆盖项；生成结果时要明确告知用户本次选择了哪一档 `summary_language`、`body_style` 和 `split_bias`，以及为什么这样判断。

## 生成顺序

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
- 多行 body 在这个项目里是常态、补充项，还是很少使用
- 用户或团队更习惯默认打包提交，还是更愿意拆成多条 commit

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
- 项目偏好覆盖项，例如 `summary_language`、`body_style`、`split_bias`
- 提交前的检查目标
- 发布、版本、脚本、签名等特殊限制

### 3. 生成项目 git 提交规则 skill
在项目内新建项目 git 提交规则 skill，命名建议为 `<project>-git-commit-rules`。

如果当前 agent 支持 skill 目录，建议放在 `<project-skill-dir>/skills/<project>-git-commit-rules/`。
如果不支持，也至少应保留同等内容，并在该 agent 的项目规则入口中引用。

可以直接从模板起步：
- [assets/project-skill-SKILL.md](../assets/project-skill-SKILL.md)

项目 git 提交规则 skill 推荐只写这几类内容：
- 项目允许的 `scope`
- 偏好覆盖项，例如 `summary_language`、`body_style`、`split_bias`
- 不推荐或禁止的 `scope`
- 发布、版本、桌面端、迁移脚本等特殊限制
- 本项目的明确例外

项目 git 提交规则 skill 不要重复这些内容：
- Conventional Commit 基本格式
- 通用 `type` 规则
- 通用 `scope` 选择方法
- 提交检查的通用目标
- GPG 不绕过原则

### 4. 在项目 git 提交规则 skill 中写清作用
开头建议直接写清：
- 这份 skill 用于补充当前项目的提交事实和特殊限制
- 如项目文档或 `AGENTS.md` 有更新，以项目最新规范为准

### 5. 校对项目文档与项目规则
完成生成后，检查两类内容是否一致：
- `scope` 白名单
- 禁止 scope
- `summary_language`
- `body_style`
- `split_bias`
- 发布与签名限制

项目文档和项目规则可以表达方式不同，但规则不能相互矛盾。
同时，交付给用户时应明确说明：
- 本次推断出的 `summary_language`
- 本次推断出的 `body_style`
- 本次推断出的 `split_bias`
- 触发该判断的主要依据，例如历史提交风格、当前项目节奏、用户在本次对话中的偏好表达

## 推荐输出

### 项目文档
给开发者使用，建议落在：
- `docs/git-commit提交说明.md`

### 项目 git 提交规则 skill
给支持 skill 目录的 agent 使用，建议结构如下：

```text
<project-skill-dir>/skills/<project>-git-commit-rules/
└── SKILL.md
```

其中：
- `SKILL.md` 写适用范围、项目事实、特殊限制、规则作用
- `references/scope.md` 为可选文件，仅在项目 scope 较多或需要单独解释时再补充

建议先直接复制模板，再按项目实际情况替换占位内容：

```bash
mkdir -p <project-skill-dir>/skills/<project>-git-commit-rules
cp <your-skills-dir>/git-commit-governance/assets/project-skill-SKILL.md <project-skill-dir>/skills/<project>-git-commit-rules/SKILL.md
```

如果项目 scope 较多或命名容易漂移，再额外补一个可选的 `references/scope.md`。

如果当前 agent 不支持 skill 目录：
- 仍然建议保留这套目录作为规则源文件。
- 再把同样的约束同步到该 agent 的项目规则入口，而不是重写另一套规则正文。

## 不要这样生成
- 不要把公共 skill 整份复制到项目里继续维护。
- 不要把公共规则和项目规则各写一份完整版。
- 不要只创建项目 git 提交规则 skill，不生成开发者可读的项目文档。
- 不要完全依赖历史 commit 反推规则，历史只能用于校准，不是最高优先级来源。

## 完成交付的判断标准
- 项目已经有开发者可直接阅读的 `docs/git-commit提交说明.md`
- 项目已经有仅包含增量规则的 `<project>-git-commit-rules` 或等价规则文件
- 项目文档、项目规则、`AGENTS.md` 对核心提交规则表述一致
