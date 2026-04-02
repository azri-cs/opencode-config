---
description: Draft release-oriented output including commit summaries, PR notes, and release notes from recent changes
agent: release-helper
temperature: 0.1
subtask: true
---

Analyze recent changes and draft release-oriented output for $ARGUMENTS. Focus on concise, reviewer-friendly summaries and user-visible impact.

**Git Analysis:**
!`git status`
!`git diff --stat`
!`git diff --name-only`
!`git log --oneline -10`

## Release Draft

- **Scope**: $ARGUMENTS
- **Change type**: [feature/fix/refactor/perf/security/docs]
- **User impact**: [high/medium/low]
- **Risk**: [high/medium/low]

## Output Format

- Release summary
- Proposed commit message
- Proposed PR title
- PR body draft
- Release notes or changelog bullets
- Verification and risk notes
