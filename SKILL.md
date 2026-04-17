---
name: git-commit-governance
description: Use this skill when preparing a git commit, drafting or reviewing a commit message, choosing a commit scope, splitting unrelated changes into separate commits, handling Git signing failures, and producing project git rule skills and docs.

---

# Git Commit Governance

## Scope
- Applies to Git commits in the current repository.
- Trigger when the task includes `git commit`, commit message drafting, staged diff review, or splitting commits.
- Read [references/scope.md](references/scope.md) before choosing `scope`.
- When the repository clearly matches a known project type, also read the relevant file under [references/project-types/](references/project-types/).
- Follow repository or project preference overrides first; if `summary_language` is not specified, default `summary` to Chinese.

## Hard rules
- Commit messages must follow `type(scope): summary`.
- `type` must use English conventional prefixes: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `build`, `ci`, `revert`.
- `scope` must be a single short noun or domain phrase. Prefer product/domain scope over technical layer scope unless the repository already mandates otherwise.
- `summary` should be concise and directly describe the changed feature point, user-visible result, or clear business effect. Engineering action words are allowed, but they must stay attached to a concrete feature point.
- Avoid vague summaries such as `修复问题`, `调整逻辑`, `优化代码`, or other text that does not say which feature changed.
- Commit messages must always start with `type(scope): summary`; after that, use repository or project preferences such as `summary_language` and `body_style` to choose the language and whether the commit should stay single-line or include a short body.
- Multi-line bodies are valid when the change is still one coherent story and a short body helps explain the main impact, boundary, compatibility, or major change dimensions.
- If a body is used, keep it brief (up to 3 bullet lines) and explain the main change dimensions instead of listing files.
- Do not use `--no-gpg-sign`.
- If Git signing fails, stop and report the signing problem to the user. Do not bypass signing.

## Commit hygiene
- Confirm the staged change tells one coherent story.
- Verify the working tree and staged set do not silently include unrelated files.
- Review the staged change at the right level: summary by default, full diff when the change is broad, risky, or still unclear.
- Split unrelated changes into separate commits instead of bundling them for convenience.
- During a commit-focused task, do not modify code by default. If you find a problem in the changes, ask the user before editing because the code may be intentionally left for the user to change.
- If repository docs or `AGENTS.md` define commit rules, follow them before falling back to this skill's defaults.
- For project onboarding or rule installation, read [references/project-installation.md](references/project-installation.md).

## Project Types
- `references/project-types/*.md` holds optional project-type context for this skill.
- When a repository clearly feels like one of those shapes, read the matching file and use it to sharpen `scope` choice, summary tone, and examples.
- Keep it lightweight: repository-local rules still come first.
- Current references:
  - `app`: [references/project-types/app-development.md](references/project-types/app-development.md) for general app projects where `scope` should stay close to pages, flows, and product-facing domains.
  - `openclaw`: [references/project-types/openclaw-workspace.md](references/project-types/openclaw-workspace.md) for personal agent workspaces where `scope` can come from agent capabilities and long-lived task domains.

## Preference Defaults
- This skill keeps three default preference knobs:
  - `summary_language`: `zh`
  - `body_style`: `medium`
  - `split_bias`: `medium`
- These are preferences, not hard constraints. Repository-local rules may override them.
- `summary_language` affects the default language of `summary`:
  - `zh`: use Chinese summaries by default
  - `en`: use English summaries by default
- `body_style` is the main preference for how one coherent commit should be written:
  - `low`: prefer single-line commits unless a short body is needed to avoid ambiguity
  - `medium`: choose single-line or short-body form based on what best explains the coherent change
  - `high`: prefer adding a short body whenever it helps clarify the main dimensions of one coherent change
- `split_bias` only affects whether a change should stay in one commit or be split:
  - `low`: default to one commit and do not actively ask to split
  - `medium`: default to one commit, but suggest splitting when the staged change clearly contains multiple independent stories
  - `high`: default to splitting when the change can be cleanly expressed as separate stories
- Repository-local rules and project rule skills can override these defaults when needed.

## Message patterns

### Normal code change
```bash
git commit -m "fix(import): 修复导入时字段映射兜底顺序"
```

### Refactor
```bash
git commit -m "refactor(field): 收敛字段编辑流程的校验与保存逻辑"
```

### Feature
```bash
git commit -m "feat(list): 新增应用列表批量继承入口"
```

### Docs only
```bash
git commit -m "docs(repo): 更新提交流程说明"
```

### Repo or coordination changes
```bash
git commit -m "chore(repo): 同步仓库脚本与配置"
```

### Large but coherent feature change
```bash
git commit -m "feat(field): 收敛端口字段与应用关联流程" \
  -m "- 移除端口遗留字段，统一暴露语义表达
- 重构相关页面编辑与关联入口，复用共享规则"
```

## Recommended workflow
1. Inspect repository-local commit rules, if any, then read [references/scope.md](references/scope.md).
2. If the repository clearly matches a known project type, read the relevant file under [references/project-types/](references/project-types/).
3. Inspect working tree.
4. Stage only the intended files.
5. Re-check staged diff and decide whether the staged change still reads as one story or should be split.
6. For commit-only tasks, do not edit code unless the user asks or confirms.
7. Choose exactly one concrete `scope` for the staged change.
8. Read applicable preference overrides, especially `summary_language` and `body_style`, then decide the final language and whether this one coherent change should stay single-line or include a short body.
9. Run the commit in the form that matches the selected structure.
10. If signing fails, stop and surface the error.

## Project installation
- Use this skill to produce two repository outputs:
- `docs/git-commit提交说明.md` from [assets/git-commit提交说明.md](assets/git-commit提交说明.md)
- `<project>-git-commit-rules/SKILL.md` from [assets/project-skill-SKILL.md](assets/project-skill-SKILL.md)
- Keep this skill focused on shared commit method and default writing style.
- Keep the project rule skill limited to project facts: scope lists, avoided scopes, preference overrides, and special constraints.
- For the detailed installation flow, read [references/project-installation.md](references/project-installation.md).

### Steps
1. Inspect `AGENTS.md`, `docs/`, and recent good commits.
2. Generate or update `docs/git-commit提交说明.md`.
3. Create `<project>-git-commit-rules/SKILL.md`.
4. Fill only project facts and constraints.

## Anti-patterns
- `git commit --no-gpg-sign ...`
- One commit containing backend fix, frontend UI change, docs update, and release files without justification.
- Scope text that mixes multiple domains such as `import-template-field`.
- Summary text that only says `update`, `修改`, `调整一下`, or repeats filenames.
- Vague summaries such as `修复问题`, `优化代码`, `逻辑调整`, without naming the affected feature point.
