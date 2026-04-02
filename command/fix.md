---
description: Fix a GitHub issue with comprehensive analysis and implementation
agent: legacy-modernizer
temperature: 0.2
---

Analyze and fix the GitHub issue. Use the issue number or description provided as $ARGUMENTS.

**Issue Analysis:**
!`gh issue view $ARGUMENTS 2>/dev/null || echo "Checking for issue context..."`

!`git log --oneline -10`

!`git diff --stat`

## Issue Resolution Plan

### Issue Information
- **Issue**: [title/summary from $ARGUMENTS]
- **Status**: [open/closed/assignee]
- **Priority**: [high/medium/low]
- **Labels**: [bug/feature/refactor/etc]

### Understanding the Problem

**Current Behavior**:
- [Description of current behavior]

**Expected Behavior**:
- [Description of expected behavior]

**Root Cause Analysis**:
- **Primary Cause**: [technical explanation]
- **Contributing Factors**: [list]
- **Impact**: [severity and scope]

---

## Implementation Strategy

### Step 1: Reproduce the Issue

**Test Environment Setup**:
- Environment: [development/staging]
- Test Data: [required test data]
- Reproduction Steps:
  1. [Step 1]
  2. [Step 2]
  3. [Step 3]

**Verification**:
```bash
# Commands to verify the issue
$ARGUMENTS

# Expected error output
[error description]

# Actual behavior
[current behavior]
```

### Step 2: Analyze Root Cause

**Code Analysis**:
- **Affected Files**: [list]
- **Affected Functions**: [list]
- **Dependencies**: [list]

**Technical Details**:
```[language]
// Problematic code
[problematic code]

// Issue explanation
[explanation of why this doesn't work]
```

### Step 3: Implement Fix

**Solution Design**:
```[language]
// Fixed code
[corrected code]

// Explanation
[why this fixes the issue]
```

**Changes Required**:

#### File 1: [path/to/file]
**Lines Changed**: [X-Y]
**Change Type**: [fix/add/remove]

```[language]
// Before
[old code]

// After
[new code]
```

#### File 2: [path/to/file]
**Lines Changed**: [X-Y]
**Change Type**: [fix/add/remove]

```[language]
// Before
[old code]

// After
[new code]
```

### Step 4: Add Tests

**Test Cases Needed**:
1. **Test for Fix Verification**
   ```[language]
   test('should fix [issue description]', () => {
     // Arrange
     const input = [test data];
     
     // Act
     const result = functionUnderTest(input);
     
     // Assert
     expect(result).toEqual([expected after fix]);
   });
   ```

2. **Test for Edge Cases**
   ```[language]
   test('should handle [edge case] after fix', () => {
     // Test edge case
   });
   ```

3. **Test for Regression Prevention**
   ```[language]
   test('should not break existing functionality', () => {
     // Test existing behavior still works
   });
   ```

### Step 5: Update Documentation

**Documentation Updates**:
- [ ] README.md - [what to update]
- [ ] API documentation - [what to update]
- [ ] Code comments - [what to update]
- [ ] User guide - [what to update]

---

## Implementation Checklist

### Pre-Implementation
- [ ] Created feature branch: `fix/issue-$ARGUMENTS`
- [ ] Pulled latest changes from main
- [ ] Understood the problem fully
- [ ] Identified all affected files
- [ ] Prepared test data

### Implementation
- [ ] Implemented the fix in [file 1]
- [ ] Implemented the fix in [file 2]
- [ ] Added necessary tests
- [ ] Updated documentation
- [ ] Verified fix works locally

### Verification
- [ ] All tests pass
- [ ] Linting passes
- [ ] Type checking passes
- [ ] Manual testing completed
- [ ] No regressions detected

### Commit & Push
- [ ] Committed changes with descriptive message
- [ ] Pushed branch to remote
- [ ] Created pull request
- [ ] Added reviewers

---

## Detailed Implementation

### File: [main affected file]

**Problem**: [description of the issue in this file]

**Solution**: [description of the fix]

**Code Changes**:
```[language]
[Complete code section with fix]
```

**Key Changes**:
1. [Change 1]: [explanation]
2. [Change 2]: [explanation]
3. [Change 3]: [explanation]

### File: [secondary affected file]

**Problem**: [description of the issue in this file]

**Solution**: [description of the fix]

**Code Changes**:
```[language]
[Complete code section with fix]
```

---

## Testing Strategy

### Unit Tests
```[language]
describe('Issue Fix Verification', () => {
  beforeEach(() => {
    // Setup test environment
  });
  
  it('should fix the reported issue', () => {
    // Test the specific fix
  });
  
  it('should handle [scenario]', () => {
    // Additional test
  });
  
  it('should not affect existing functionality', () => {
    // Regression test
  });
});
```

### Integration Tests
```[language]
describe('Integration Tests', () => {
  it('should work correctly in integration', () => {
    // Test how the fix works with other components
  });
});
```

### Manual Testing Checklist
- [ ] Tested in development environment
- [ ] Tested with different user roles
- [ ] Tested edge cases
- [ ] Tested error conditions
- [ ] Tested performance impact

---

## Risk Assessment

### Risks Identified
| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| [Risk 1] | [High/Med/Low] | [High/Med/Low] | [Mitigation] |
| [Risk 2] | [High/Med/Low] | [High/Med/Low] | [Mitigation] |

### Rollback Plan
- **Branch**: [backup branch name]
- **Command**: `git checkout backup-branch`
- **Files to revert**: [list]

---

## Quality Assurance

### Code Review Checklist
- [ ] Code follows project conventions
- [ ] Tests are comprehensive
- [ ] Documentation is updated
- [ ] No security vulnerabilities introduced
- [ ] Performance impact considered
- [ ] Edge cases handled

### Final Validation
- [ ] All tests passing
- [ ] No linting errors
- [ ] Type checking passes
- [ ] Manual testing complete
- [ ] Code review approved

---

## Communication

### Pull Request Description
```
## Issue Fixed
- Fixes #[issue number]

## Summary
[Brief description of the fix]

## Changes Made
- [File 1]: [description]
- [File 2]: [description]
- [Tests]: [description]

## Testing
- Unit tests added/updated
- Integration tests passed
- Manual testing completed

## Screenshots/Demo
[If applicable]

## Checklist
- [ ] Tests pass
- [ ] Linting passes
- [ ] Documentation updated
- [ ] No breaking changes
```

### Team Notification
- [ ] Tagged reviewers
- [ ] Added to standup
- [ ] Updated issue status
