---
description: Create a proper commit with best practices and descriptive message
agent: release-helper
temperature: 0.1
---

Analyze staged changes and create a proper commit message with best practices.

**Git Analysis:**
!`git status`
!`git diff --cached --stat`
!`git diff --cached`

## Commit Analysis

### Staged Changes
- **Files**: [count]
- **Lines Added**: [count]
- **Lines Removed**: [count]
- **Type**: [feat/fix/refactor/docs/etc]

---

## Commit Message Format

### Format Structure
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type Definitions
| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `style` | Code style changes (formatting, etc.) |
| `refactor` | Code refactoring |
| `perf` | Performance improvements |
| `test` | Adding or modifying tests |
| `chore` | Maintenance tasks |
| `build` | Build system changes |
| `ci` | CI configuration changes |
| `revert` | Reverting commits |

---

## Commit Message Template

### Recommended Structure

```
feat(auth): add user registration with email verification

Implement user registration endpoint with email verification workflow.
Users can now sign up and receive a verification email to activate
their account. This feature addresses the requirement for user
authentication in the system.

- Add User model with email verification fields
- Create registration API endpoint
- Implement email verification service
- Add background job for sending verification emails
- Include unit and integration tests

Closes #123
Refs #456
```

### Generated Commit Message

**Subject Line**:
```
feat(api): [subject based on changes]
```

**Body**:
[Detailed description of changes]

**Footer**:
- [Breaking changes]
- [Related issues]
- [Co-authored-by]

---

## Commit Examples

### Feature Commit
```
feat(user): add password reset functionality

Implement password reset flow with secure token-based validation.
Users can request a password reset email and reset their password
using a time-limited token.

- Add PasswordReset model for tracking reset requests
- Create API endpoints for request and confirm reset
- Implement secure token generation and validation
- Add email template for reset instructions
- Include security measures against brute force attacks
- Add comprehensive test coverage

Closes #245
```

### Bug Fix Commit
```
fix(auth): resolve session timeout issue for API calls

Fixed an issue where valid sessions were timing out prematurely
during API requests. The problem was caused by incorrect session
lifetime configuration that wasn't being respected for API
authentication.

- Update session configuration to respect API timeout settings
- Fix session refresh logic for authenticated API calls
- Add integration tests for session persistence
- Update session management documentation

Fixes #789
```

### Refactor Commit
```
refactor(database): optimize user query performance

Refactor user queries to improve performance by implementing
eager loading and query optimization. This reduces database
load and improves response times for user-related endpoints.

- Replace N+1 queries with eager loading
- Add database indexes for frequently queried columns
- Implement query result caching for read operations
- Optimize complex queries with proper joins
- Add performance benchmarks

Related to #321
```

### Documentation Commit
```
docs(readme): update installation and configuration sections

Update README with current installation instructions and
environment configuration. Also add troubleshooting section
for common issues.

- Add PHP 8.2 minimum requirement
- Update database configuration examples
- Add environment variable documentation
- Include troubleshooting for common errors
- Add Docker setup instructions

Refs #654
```

### Breaking Change Commit
```
feat(api): upgrade to RESTful API v2

Migrate from v1 to v2 API with improved response formats
and authentication mechanism. This is a breaking change that
requires client updates.

BREAKING CHANGE: All v1 endpoints are deprecated and will
be removed in v3. Clients must update to use v2 endpoints.

Migration guide:
1. Update base URL from /api/v1/ to /api/v2/
2. Update response format (see documentation)
3. Update authentication header format
4. Update error response structure

Closes #111
```

---

## Best Practices

### 1. Use Imperative Mood
```
# Good
add user registration

# Bad
added user registration
adding user registration
```

### 2. Keep Subject Line Under 50 Characters
```
# Good
feat(auth): add user registration

# Bad
feat(auth): I added user registration functionality because we needed it for the new system
```

### 3. Use Body to Explain "Why"
```markdown
# Good
fix(auth): resolve session timeout issue

The session timeout was being calculated incorrectly due to
timezone differences between server and client. This fix
ensures consistent timeout behavior across different timezones.

# Bad  
fix(auth): fixed the thing
```

### 4. Reference Issues
```
Closes #123    # Closes issue when PR merges
Fixes #456     # Fixes issue
Refs #789      # Related to issue
Related to #321 # Just related
```

### 5. Use Conventional Commits
```
<type>(<scope>): <subject>
```

---

## Automated Commit Generation

### Current Changes Analysis

**Files Changed**:
```
[file1] - [status] - [description]
[file2] - [status] - [description]  
[file3] - [status] - [description]
```

**Generated Message**:

```
feat([main-scope]): [concise subject]

[Detailed description explaining:
- What changed
- Why it changed
- Any important notes]

[Bullet points if multiple changes]
- Change 1
- Change 2

[Footer with references]
Closes #[issue]
```

---

## Commit Checklist

### Before Committing
- [ ] Changes are staged correctly
- [ ] No unintended files included
- [ ] Code follows project conventions
- [ ] Tests pass locally
- [ ] Commit message follows format
- [ ] Subject line is clear and concise
- [ ] Body provides context
- [ ] Related issues referenced

### Commit Message Quality
- [ ] Imperative mood used
- [ ] Under 50 characters in subject
- [ ] Capitalized subject
- [ ] No trailing period
- [ ] Wrapped at 72 characters in body
- [ ] Clear "why" explained
- [ ] Breaking changes documented

---

## Git Commands

### Stage Changes
```bash
# Stage specific files
git add [file1] [file2]

# Stage all changes
git add .

# Check what will be committed
git diff --cached
```

### Create Commit
```bash
# Create commit with message
git commit -m "type(scope): subject"

# Create commit with editor
git commit

# Amend last commit
git commit --amend

# Amend without editing message
git commit --amend --no-edit
```

### Push Commit
```bash
# Push to remote
git push origin [branch]

# Force push (use carefully)
git push --force origin [branch]
```

---

## Summary

### Generated Commit Message

**Type**: [feat/fix/refactor/docs/style/test/chore]
**Scope**: [component/area]
**Subject**: [concise description]

**Body**: 
[Detailed explanation of what changed and why]

**Footer**:
- [References]
- [Breaking changes]

---

## Execute Commit

To create the commit, run:
```bash
git commit -m "[type]([scope]): [subject]

[Body]

[Footer]"
```

Would you like me to generate the exact commit command for your current changes?
