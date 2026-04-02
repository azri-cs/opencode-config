---
description: Investigate a bug, failing test, or broken flow and identify the most likely root cause
agent: debugger
temperature: 0.1
subtask: true
---

Investigate the bug, failure, or runtime issue described as $ARGUMENTS. Reproduce the issue with the smallest reliable path, gather evidence, and identify the most likely root cause without making code changes.

**Initial Analysis:**
!`git status`
!`git diff --stat`
!`git log --oneline -5`

## Debugging Brief

- **Problem**: $ARGUMENTS
- **Observed behavior**: [what is failing]
- **Expected behavior**: [what should happen]
- **Likely layer**: [input/state/logic/I-O/integration/environment]

## Investigation Steps

1. Restate the failure clearly.
2. Identify the smallest reproducible path.
3. Run the narrowest commands or inspections needed to test hypotheses.
4. Separate confirmed evidence from assumptions.

## Output Format

- Reproduction status
- Evidence gathered
- Most likely root cause
- Recommended fix path
- Suggested verification command(s)
