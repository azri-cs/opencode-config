---
description: Comprehensive code review of recent changes
agent: code-reviewer
temperature: 0.1
subtask: true
---

Perform a thorough code review of recent changes. Focus on code quality, security, performance, and best practices.

**Git Analysis:**
!`git diff --stat`
!`git diff --name-only`
!`git log --oneline -5`
!`git status`

## Review Scope
- **Files Changed**: [count]
- **Lines Added**: [count]
- **Lines Removed**: [count]
- **Commits**: [list]

## Code Quality Assessment

### 1. Readability & Maintainability
- [ ] Clear function/variable names
- [ ] Appropriate function length (< 30 lines)
- [ ] No deep nesting (> 3 levels)
- [ ] Proper code organization
- [ ] No duplicate code blocks

**Issues Found:**
- [List specific readability issues with line numbers]

### 2. Type Safety (where applicable)
- [ ] Type hints used
- [ ] Proper interfaces/contracts
- [ ] No implicit type conversions
- [ ] Generic types properly used

**Issues Found:**
- [List type safety issues]

### 3. Error Handling
- [ ] Proper exception handling
- [ ] Meaningful error messages
- [ ] No swallowed exceptions
- [ ] Appropriate logging

**Issues Found:**
- [List error handling issues with line numbers and suggestions]

### 4. Code Style
- [ ] Follows project conventions
- [ ] Consistent formatting
- [ ] Proper comments (why, not what)
- [ ] No commented-out code

**Issues Found:**
- [List style violations]

## Security Review

### 1. Input Validation
- [ ] All user inputs validated
- [ ] Proper sanitization
- [ ] SQL injection prevention
- [ ] XSS prevention

**Critical Issues:**
```[language]
[Insecure code example]
[Explanation of vulnerability]
[Secure alternative]
```

### 2. Authentication & Authorization
- [ ] Proper authentication checks
- [ ] Authorization verified
- [ ] Role-based access control
- [ ] Session management

**Critical Issues:**
```[language]
[Auth code issue]
[Explanation]
[Secure implementation]
```

### 3. Data Protection
- [ ] No hardcoded secrets
- [ ] Environment variables used
- [ ] Sensitive data masked in logs
- [ ] Encryption where needed

**Critical Issues:**
```[language]
[Secret exposure]
[Explanation]
[Secure approach]
```

### 4. Dependencies
- [ ] No known vulnerabilities
- [ ] Dependencies up to date
- [ ] Minimal dependency footprint

**Dependency Alerts:**
- [List vulnerable dependencies]

## Performance Review

### 1. Database Queries
- [ ] No N+1 queries
- [ ] Proper indexing used
- [ ] Efficient query structure
- [ ] Caching implemented

**Performance Issues:**
```[language]
[Problematic query]
[Impact: High/Medium/Low]
[Optimized version]
```

### 2. Algorithm Efficiency
- [ ] Appropriate data structures
- [ ] No unnecessary loops
- [ ] Proper complexity consideration
- [ ] Memoization/caching used

**Issues Found:**
- [List efficiency issues]

### 3. Resource Management
- [ ] Proper connection handling
- [ ] Memory efficient
- [ ] No memory leaks
- [ ] Stream/file handling correct

**Issues Found:**
- [List resource management issues]

## Best Practices Compliance

### Framework Conventions
**For Laravel:**
- [ ] Repository pattern
- [ ] Service layer
- [ ] Form requests
- [ ] Policies

**For Django:**
- [ ] CBV usage
- [ ] DRF serializers
- [ ] Manager/queryset patterns
- [ ] Django apps structure

**For React:**
- [ ] Hooks usage
- [ ] Proper state management
- [ ] Component composition
- [ ] TypeScript usage

### Testing Standards
- [ ] Test coverage adequate
- [ ] Tests are isolated
- [ ] Proper assertions
- [ ] Edge cases covered

### Documentation
- [ ] API documentation
- [ ] Function documentation
- [ ] README updated
- [ ] Breaking changes documented

## Summary & Recommendations

### Critical Issues (Must Fix)
1. [Issue Title]
   - **File**: [path]
   - **Lines**: [line numbers]
   - **Problem**: [description]
   - **Impact**: [severity: Critical]
   - **Solution**: [code example]

### Warnings (Should Fix)
1. [Issue Title]
   - **File**: [path]
   - **Problem**: [description]
   - **Recommendation**: [suggestion]

### Suggestions (Consider)
1. [Suggestion]
   - **Rationale**: [reasoning]
   - **Benefit**: [improvement]

### Praise (What's Good)
1. [Positive observations about the code]

## Action Items
- [ ] Fix critical security issues
- [ ] Address performance concerns
- [ ] Improve test coverage
- [ ] Update documentation
- [ ] Refactor complex sections

**Overall Review Status:** [APPROVED / NEEDS_CHANGES / REJECTED]
