---
description: Create a comprehensive pull request with proper description
agent: release-helper
temperature: 0.2
---

Analyze recent changes and create a comprehensive pull request description.

**Git Analysis:**
!`git diff --stat`
!`git diff --name-only`
!`git log --oneline -10`
!`git status`

## Pull Request Creation

### PR Information
- **Branch**: [current branch]
- **Target Branch**: [main/develop]
- **Changes**: [X files changed, Y insertions, Z deletions]
- **Commits**: [count]

---

## PR Template

```markdown
## Summary
[Brief description of the changes]

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] This change requires a documentation update

## Motivation and Context
[Why is this change needed? What problem does it solve?]

## How Has This Been Tested?
- [ ] Unit tests
- [ ] Integration tests
- [ ] E2E tests
- [ ] Manual testing

## Screenshots (if applicable)
[Add screenshots to help explain your changes]

## Checklist
- [ ] My code follows the project's style guidelines
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] Any dependent changes have been merged and published in downstream modules

## Changes Made
### Added
- [Added file/feature]: [Description]

### Changed
- [Changed file/feature]: [Description of change]

### Deprecated
- [Deprecated file/feature]: [Reason and替代方案]

### Removed
- [Removed file/feature]: [Reason]

### Fixed
- [Fixed issue]: [Description of fix]

## Breaking Changes
- [Breaking change]: [Description and migration guide]

## Related Issues
- Fixes #[issue number]
- Related to #[issue number]

## Additional Notes
[Any additional information that reviewers should know]
```

---

## Detailed PR Description

### Executive Summary
[1-2 sentences describing the PR]

### Problem Statement
[What problem does this solve?]

### Solution Overview
[How does it solve the problem?]

### Technical Details
[Deep dive into the technical implementation]

### Architecture Changes
[If any architectural changes, describe them here]

### API Changes
[If API changed, document the changes]

### Database Changes
[If database changed, document migrations]

### Security Considerations
[If security affected, document implications]

### Performance Impact
[If performance affected, document metrics]

---

## Files Changed

### Backend Changes
| File | Changes | Impact |
|------|---------|--------|
| [file] | [add/remove/modify] | [High/Med/Low] |

### Frontend Changes
| File | Changes | Impact |
|------|---------|--------|
| [file] | [add/remove/modify] | [High/Med/Low] |

### Configuration Changes
| File | Changes | Impact |
|------|---------|--------|
| [file] | [add/remove/modify] | [High/Med/Low] |

---

## Testing

### Test Coverage
- **Unit Tests**: [X] new, [Y] modified
- **Integration Tests**: [X] new, [Y] modified
- **E2E Tests**: [X] new, [Y] modified

### Manual Testing Steps
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Test Results
```
[Paste test results here]
```

---

## Reviewer Checklist

### Code Quality
- [ ] Code follows style guidelines
- [ ] No hardcoded secrets
- [ ] Proper error handling
- [ ] No unnecessary complexity
- [ ] Meaningful variable names

### Functionality
- [ ] Works as expected
- [ ] Edge cases handled
- [ ] Error cases handled
- [ ] No regression in existing features

### Security
- [ ] Input validation
- [ ] Authentication/Authorization
- [ ] Data protection
- [ ] No security vulnerabilities

### Performance
- [ ] No obvious performance issues
- [ ] Database queries optimized
- [ ] No unnecessary operations

### Documentation
- [ ] Code comments updated
- [ ] API documentation updated
- [ ] README updated if needed

---

## Deployment Instructions

### Pre-deployment Checklist
- [ ] Tests passing
- [ ] Code reviewed
- [ ] Documentation updated
- [ ] Database migrations ready

### Deployment Steps
```bash
# 1. Pull latest changes
git pull origin main

# 2. Run migrations
php artisan migrate
# or
python manage.py migrate

# 3. Clear cache
php artisan cache:clear
# or
python manage.py cache clear

# 4. Build assets
npm run build
```

### Rollback Plan
```bash
# Rollback migration
php artisan migrate:rollback
# or
python manage.py migrate [previous]

# Revert code
git revert [commit]
```

---

## Questions for Reviewers
1. [Question 1]
2. [Question 2]
3. [Question 3]
