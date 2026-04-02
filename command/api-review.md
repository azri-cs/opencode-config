---
description: Review API endpoints, contracts, validation, errors, and integration safety
agent: api-reviewer
temperature: 0.1
subtask: true
---

Review the API surface described as $ARGUMENTS. Focus on contract clarity, validation, error handling, compatibility, authentication boundaries, and client integration safety.

**API Analysis:**
!`git status`
!`git diff --name-only`
!`find . -type f \( -name "*.php" -o -name "*.py" -o -name "*.ts" -o -name "*.js" \) ! -path "./vendor/*" ! -path "./node_modules/*" ! -path "./.git/*" | head -20`

## API Review

- **Scope**: $ARGUMENTS
- **API style**: [REST/GraphQL/internal service]
- **Primary concerns**: [validation/errors/versioning/auth/contracts]

## Output Format

- API summary
- Contract issues
- Error handling or validation issues
- Compatibility and consumer risk
- Recommended follow-up tests
