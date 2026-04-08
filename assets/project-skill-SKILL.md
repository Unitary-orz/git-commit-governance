---
name: <project>-git-commit-rules
description: Use this skill when a project has its own commit facts, such as scope lists, summary language requirements, or release-specific constraints.
---

# <project> Git Commit Rules

## Role
- This skill records the commit facts and special constraints for the current project.
- If project docs or `AGENTS.md` are updated later, follow the latest project rule.

## Project facts
- `summary` language: `<例如：统一中文>`
- Allowed scopes: `<列出项目 scope 白名单>`
- Avoided scopes: `<列出不推荐或禁止的 scope>`
- Special constraints: `<例如：发布相关改动必须独立 commit>`

## Examples
- `feat(<scope>): <新增的功能点>`
- `fix(<scope>): <修复的功能点>`
- `refactor(<scope>): <围绕某个功能点的整理动作>`
