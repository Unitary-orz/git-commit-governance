---
name: git-commit-governance
description: Use this skill when preparing a git commit, drafting or reviewing a commit message, choosing a commit scope, splitting unrelated changes into separate commits, handling Git signing failures, and producing project git rule skills and docs.

---

# Git Commit Governance

## Scope
- Applies to Git commits in the current repository.
- Trigger when the task includes `git commit`, commit message drafting, staged diff review, or splitting commits.
- Read [references/scope.md](references/scope.md) before choosing `scope`.
- Default `summary` to Chinese unless the repository explicitly requires another language.

## Hard rules
- Commit messages must follow `type(scope): summary`.
- `type` must use English conventional prefixes: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `build`, `ci`, `revert`.
- `scope` must be a single short noun or domain phrase. Prefer product/domain scope over technical layer scope unless the repository already mandates otherwise.
- `summary` should be concise and directly describe the changed feature point, user-visible result, or clear business effect. Engineering action words are allowed, but they must stay attached to a concrete feature point.
- Avoid vague summaries such as `修复问题`, `调整逻辑`, `优化代码`, or other text that does not say which feature changed.
- Default to a single-line commit message (`type(scope): summary`).
- Only use multi-line commit bodies for larger but coherent functional changes; most commits should remain one line.
- If a body is needed, keep it brief (up to 3 bullet lines) and explain the main change dimensions instead of listing files.
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
- 重构相关页面编辑与关联入口，复用共享规则
- 同步测试与文档，保证新字段模型前后端一致"
```

## Recommended workflow
1. Inspect repository-local commit rules, if any, then read [references/scope.md](references/scope.md).
2. Inspect working tree.
3. Stage only the intended files.
4. Re-check staged diff and split unrelated changes.
5. For commit-only tasks, do not edit code unless the user asks or confirms.
6. Choose exactly one concrete `scope` for the staged change.
7. Run plain `git commit -m "type(scope): summary"`.
8. If signing fails, stop and surface the error.

## Project installation
- Use this skill to produce two repository outputs:
- `docs/git-commit提交说明.md` from [assets/git-commit提交说明.md](assets/git-commit提交说明.md)
- `<project>-git-commit-rules/SKILL.md` from [assets/project-skill-SKILL.md](assets/project-skill-SKILL.md)
- Keep this skill focused on shared commit method and default writing style.
- Keep the project rule skill limited to project facts: scope lists, summary language, avoided scopes, and special constraints.
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
