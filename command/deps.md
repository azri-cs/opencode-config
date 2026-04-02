---
description: Review dependencies for outdated packages, security exposure, and upgrade risk
agent: dependency-upgrader
temperature: 0.1
subtask: true
---

Review dependency health for the current project or for the area described as $ARGUMENTS. Identify outdated packages, risky upgrades, and the safest upgrade order.

**Dependency Analysis:**
!`git status`
!`cat package.json 2>/dev/null | grep -E '"dependencies"|"devDependencies"' -n || echo "No package.json found"`
!`cat composer.json 2>/dev/null | grep -E '"require"|"require-dev"' -n || echo "No composer.json found"`
!`cat pyproject.toml 2>/dev/null | grep -E '\[tool.poetry.dependencies\]|\[project\]' -n || echo "No pyproject.toml found"`

## Dependency Review

- **Scope**: $ARGUMENTS
- **Package managers detected**: [npm/pnpm/yarn/composer/pip/poetry/cargo/go]
- **Primary concerns**: [outdated/security/major-version churn]

## Output Format

- Dependency summary
- Safe upgrades
- Risky or breaking upgrades
- Security-sensitive packages
- Recommended upgrade order
- Verification needed after upgrade
