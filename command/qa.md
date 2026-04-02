---
description: Perform frontend-oriented QA review for responsiveness, accessibility, interactions, and implementation quality
agent: frontend-qa
temperature: 0.1
subtask: true
---

Perform a frontend QA review for the feature, screen, or component described as $ARGUMENTS. Focus on visible quality, interaction correctness, responsiveness, accessibility, and regression risk.

**UI Analysis:**
!`git status`
!`git diff --name-only`
!`find . -type f \( -name "*.tsx" -o -name "*.jsx" -o -name "*.vue" -o -name "*.html" -o -name "*.css" \) ! -path "./node_modules/*" ! -path "./dist/*" | head -20`

## QA Review

- **Scope**: $ARGUMENTS
- **Likely UI surfaces**: [pages/components/layouts]
- **Primary concerns**: [responsiveness/a11y/interaction/visual regressions]

## Output Format

- QA summary
- Functional issues
- Accessibility or responsiveness concerns
- Visual or UX risks
- Recommended manual or automated checks
