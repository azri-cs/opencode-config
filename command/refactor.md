---
description: Refactor code for improved quality, performance, and maintainability
agent: legacy-modernizer
temperature: 0.2
---

Analyze the specified code and provide comprehensive refactoring recommendations. Use the file path provided as $ARGUMENTS or analyze the most recently modified files.

**Code Analysis:**
!`git diff --name-only`
!`find . -name "$ARGUMENTS" -o -name "$(echo $ARGUMENTS | sed 's/.*\///')" 2>/dev/null | head -5`

!`wc -l $ARGUMENTS 2>/dev/null || find . -name "*.php" -o -name "*.py" -o -name "*.js" -o -name "*.ts" | head -5 | xargs wc -l 2>/dev/null | tail -1`

## Refactoring Analysis

### File Information
- **File**: [path/to/file]
- **Language**: [PHP/Python/JavaScript/TypeScript]
- **Size**: [lines of code]
- **Complexity**: [estimated complexity score]
- **Last Modified**: [date]

### Current Issues Identified

#### 1. Code Structure Issues
**Problem**: [description of structural issue]

**Location**: [line numbers]

**Current Code**:
```[language]
[problematic code]
```

**Issues**:
- [Issue 1]
- [Issue 2]
- [Issue 3]

**Refactored Code**:
```[language]
[improved code]
```

**Rationale**:
- [Why this is better]
- [Benefits]
- [Trade-offs]

#### 2. Naming Convention Issues
**Problem**: [description of naming issues]

**Location**: [line numbers]

**Current Code**:
```[language]
[poorly named code]
```

**Issues**:
- Variables: [issues]
- Functions: [issues]
- Classes: [issues]
- Files: [issues]

**Refactored Code**:
```[language]
[well-named code]
```

**Naming Guidelines Applied**:
- [Guideline 1]
- [Guideline 2]
- [Guideline 3]

#### 3. Function/Method Issues
**Problem**: [description of function issues]

**Location**: [line numbers]

**Current Code**:
```[language]
[problematic function]
```

**Issues Detected**:
- Length: [too long/short]
- Parameters: [too many/missing types]
- Responsibility: [multiple responsibilities]
- Return: [inconsistent return]

**Refactored Code**:
```[language]
[improved function]
```

**Extracted Functions**:
```[language]
// Helper function 1
function helper1() {
  // Implementation
}

// Helper function 2  
function helper2() {
  // Implementation
}
```

#### 4. Loop & Iteration Issues
**Problem**: [description of loop issues]

**Location**: [line numbers]

**Current Code**:
```[language]
[inefficient loop]
```

**Issues**:
- Performance: [issue]
- Readability: [issue]
- Maintainability: [issue]

**Refactored Code**:
```[language]
[optimized loop]
```

**Improvements**:
- Performance: [X% faster]
- Readability: [improved]
- Memory: [reduction]

#### 5. Conditional Logic Issues
**Problem**: [description of conditional issues]

**Location**: [line numbers]

**Current Code**:
```[language]
[complex conditional]
```

**Issues**:
- Nesting depth: [level]
- Complexity score: [score]
- Readability: [poor]

**Refactored Code**:
```[language]
[simplified conditional]
```

**Patterns Applied**:
- Early returns
- Guard clauses
- Strategy pattern
- Null coalescing
- Optional chaining

#### 6. Error Handling Issues
**Problem**: [description of error handling issues]

**Location**: [line numbers]

**Current Code**:
```[language]
[poor error handling]
```

**Issues**:
- Swallowed exceptions
- Generic catch blocks
- Missing error context
- No logging

**Refactored Code**:
```[language]
[improved error handling]
```

**Best Practices Applied**:
- Specific exception types
- Error context preservation
- Proper logging
- User-friendly messages

#### 7. Code Duplication Issues
**Problem**: [description of duplication]

**Locations**: [line numbers]

**Current Code**:
```[language]
[duplicate code 1]
// ... some code ...
[duplicate code 2]
```

**Extracted Solution**:
```[language]
// Reusable function
function reusableFunction($param1, $param2) {
  // Common implementation
  return $result;
}

// Usage
const result1 = reusableFunction(arg1, arg2);
const result2 = reusableFunction(arg3, arg4);
```

**Benefits**:
- DRY principle followed
- Single source of truth
- Easier maintenance
- Consistent behavior

---

## Performance Optimization

### 1. Database Query Optimization
**Problem**: [N+1, missing indexes, etc.]

**Current Query**:
```[language]
[inefficient query]
```

**Optimized Query**:
```[language]
[efficient query]
```

**Performance Impact**:
- Query time: [X]ms → [Y]ms
- Database load: [X]% → [Y]%

### 2. Algorithm Optimization
**Problem**: [inefficient algorithm]

**Current Implementation**:
```[language]
[slow implementation]
```

**Optimized Implementation**:
```[language]
[fast implementation]
```

**Complexity Analysis**:
- Before: O([complexity])
- After: O([complexity])
- Speedup: [X]x faster

### 3. Memory Optimization
**Problem**: [memory leak/inefficient usage]

**Current Code**:
```[language]
[memory-intensive code]
```

**Optimized Code**:
```[language]
[memory-efficient code]
```

**Memory Impact**:
- Peak memory: [X]MB → [Y]MB
- Garbage collection: [improved]

### 4. Caching Implementation
**Problem**: [repeated expensive operations]

**Solution**:
```[language]
[cache implementation]
```

**Cache Strategy**:
- Cache type: [in-memory/Redis/file]
- TTL: [time]
- Invalidation: [strategy]

**Performance Gain**:
- Response time: [X]ms → [Y]ms
- Database queries: [X] → [Y]

---

## Modernization Opportunities

### 1. Language Features
**Current**: [old language version/feature]

**Modern Approach**:
```[language]
[modern code]
```

**Benefits**:
- [Benefit 1]
- [Benefit 2]
- [Benefit 3]

### 2. Framework Patterns
**Current**: [outdated pattern]

**Modern Approach**:
```[language]
[modern pattern]
```

**Migration Steps**:
1. [Step 1]
2. [Step 2]
3. [Step 3]

### 3. Architecture Improvements
**Current**: [current architecture]

**Proposed Architecture**:
```[language]
[improved architecture]
```

**Benefits**:
- [Benefit 1]
- [Benefit 2]
- [Benefit 3]

---

## Code Quality Metrics

### Before Refactoring
| Metric | Value | Rating |
|--------|-------|--------|
| Cyclomatic Complexity | [score] | [Good/Fair/Poor] |
| Lines of Code | [count] | [Good/Fair/Poor] |
| Code Duplication | [percentage] | [Good/Fair/Poor] |
| Test Coverage | [percentage] | [Good/Fair/Poor] |
| Documentation | [percentage] | [Good/Fair/Poor] |

### After Refactoring (Estimated)
| Metric | Value | Rating |
|--------|-------|--------|
| Cyclomatic Complexity | [score] | [Good/Fair/Poor] |
| Lines of Code | [count] | [Good/Fair/Poor] |
| Code Duplication | [percentage] | [Good/Fair/Poor] |
| Test Coverage | [percentage] | [Good/Fair/Poor] |
| Documentation | [percentage] | [Good/Fair/Poor] |

---

## Refactoring Priority Matrix

### High Priority (Should Do)
1. **Security Issues**
   - [Issue]: [description]
   - Risk: [high/medium/low]
   - Effort: [small/medium/large]
   - Code: [file:line]

2. **Performance Issues**
   - [Issue]: [description]
   - Impact: [high/medium/low]
   - Effort: [small/medium/large]
   - Code: [file:line]

### Medium Priority (Nice to Do)
1. **Readability Improvements**
   - [Issue]: [description]
   - Impact: [medium]
   - Effort: [small]
   - Code: [file:line]

2. **Maintainability Improvements**
   - [Issue]: [description]
   - Impact: [medium]
   - Effort: [medium]
   - Code: [file:line]

### Low Priority (Consider Later)
1. **Style Improvements**
   - [Issue]: [description]
   - Impact: [low]
   - Effort: [small]
   - Code: [file:line]

2. **Modernization**
   - [Issue]: [description]
   - Impact: [low]
   - Effort: [large]
   - Code: [file:line]

---

## Implementation Plan

### Phase 1: Safety First (Day 1)
1. **Add Tests**
   - Write unit tests for existing functionality
   - Ensure 80% coverage before refactoring
   - Set up CI pipeline

2. **Version Control**
   - Create feature branch
   - Commit frequently
   - Use atomic commits

### Phase 2: Incremental Refactoring (Days 2-3)
1. **Simple Refactoring**
   - Rename variables/functions
   - Extract constants
   - Simplify conditionals

2. **Function Extraction**
   - Break large functions
   - Create helper functions
   - Improve single responsibility

### Phase 3: Complex Refactoring (Days 4-5)
1. **Architecture Changes**
   - Implement design patterns
   - Improve separation of concerns
   - Add abstraction layers

2. **Performance Optimization**
   - Optimize queries
   - Add caching
   - Improve algorithms

### Phase 4: Validation (Day 6)
1. **Test Validation**
   - Run full test suite
   - Verify functionality
   - Performance testing

2. **Code Review**
   - Peer review
   - Security review
   - Performance review

---

## Risk Assessment

### High Risk Changes
| Change | Risk Level | Mitigation Strategy |
|--------|------------|---------------------|
| [Change 1] | [High/Medium/Low] | [Mitigation] |
| [Change 2] | [High/Medium/Low] | [Mitigation] |

### Rollback Plan
1. **Git Checkpoint**: [branch name]
2. **Backup**: [location]
3. **Restore Command**: [command]

---

## Verification Checklist

### Functionality
- [ ] All tests pass
- [ ] No regression in existing features
- [ ] Edge cases handled
- [ ] Error cases handled

### Performance
- [ ] Response time improved
- [ ] Memory usage optimized
- [ ] Database queries optimized

### Code Quality
- [ ] No new warnings
- [ ] Code style consistent
- [ ] Documentation updated

### Security
- [ ] No new vulnerabilities
- [ ] Security tests pass
- [ ] Dependencies updated

---

## Time Estimation

### Total Estimated Time: [X hours/days]

### Breakdown
- Analysis: [X hours]
- Phase 1: [X hours]
- Phase 2: [X hours]
- Phase 3: [X hours]
- Phase 4: [X hours]
- Buffer: [X hours]

### Dependencies
- [Dependency 1]: [impact]
- [Dependency 2]: [impact]
