---
description: Run linters and fix code style issues
agent: build
temperature: 0.2
---

Run code linters and analyze code quality issues. Auto-fix where possible, provide manual fixes for others.

**Lint Execution:**
!`npm run lint 2>&1 || npm run lint -- --fix 2>&1 || true`
!`./vendor/bin/pint --test 2>&1 || ./vendor/bin/pint 2>&1 || true`
!`flake8 . --max-line-length=120 --exclude=vendor,node_modules,migrations 2>&1 || true`
!`eslint . --ext .ts,.tsx,.js,.jsx --max-warnings=0 2>&1 || true`

!`cat .eslintrc 2>/dev/null || cat .eslintrc.js 2>/dev/null || echo "No ESLint config found"`
!`cat pyproject.toml 2>/dev/null | grep -A 10 "\[tool.flake8\]" || echo "No Flake8 config found"`

## Linting Results Summary

### Tool Results
| Tool | Status | Issues | Warnings | Auto-fixable |
|------|--------|--------|----------|--------------|
| ESLint | [Pass/Fail] | [count] | [count] | [count] |
| Prettier | [Pass/Fail] | [count] | [count] | [count] |
| Flake8 | [Pass/Fail] | [count] | [count] | [count] |
| PHP CS Fixer | [Pass/Fail] | [count] | [count] | [count] |
| Pint | [Pass/Fail] | [count] | [count] | [count] |

### Overall Status
- **Total Issues**: [count]
- **Fixed Automatically**: [count]
- **Manual Fixes Needed**: [count]
- **Status**: [PASS/FAIL/WITH WARNINGS]

---

## Critical Issues (Must Fix)

### 1. Syntax Errors
**Files Affected**: [list]

```[language]
// Current (broken)
[syntax error code]

// Fixed
[corrected code]
```

**Fix Command:**
```bash
# Syntax errors must be fixed manually
# Check line numbers and fix accordingly
```

### 2. Security Issues
**Severity**: Critical

**Files Affected**:
- [file:line] - [issue description]
- [file:line] - [issue description]

```[language]
// Vulnerable code
[insecure code]

// Secure alternative
[secure code]
```

**Security Concerns**:
- [Concern 1]: [explanation and fix]
- [Concern 2]: [explanation and fix]

---

## Major Issues (Should Fix)

### 1. Code Style Violations

#### Naming Conventions
**Issue**: Inconsistent naming

```[language]
// Current (inconsistent)
const userName = 'John';
const user_age = 'Jane';
const UserAddress = '123 St';

// Fixed (consistent)
const userName = 'John';
const userAge = 'Jane';
const userAddress = '123 St';
```

**Convention Applied**: [camelCase/variableNaming/PascalCase]

#### Line Length
**Issue**: Lines exceeding 120 characters

**Files with Long Lines**:
- [file:line] - [excerpt]
- [file:line] - [excerpt]

**Auto-fix Command:**
```bash
# Prettier will auto-fix line length
npx prettier --write .
```

#### Indentation
**Issue**: Mixed indentation (spaces/tabs)

**Files Affected**:
- [file:line] - [current indentation]
- [file:line] - [current indentation]

**Auto-fix Command:**
```bash
# Fix indentation
npx prettier --write .
# Or for PHP
./vendor/bin/pint
```

### 2. Code Smells

#### Duplicate Code
**Issue**: Code duplication detected

**Locations**:
- [file:lines] - [code excerpt]
- [file:lines] - [code excerpt]

**Refactored Code**:
```[language]
// Extracted to function
function sharedLogic($param1, $param2) {
  // Common implementation
  return $result;
}

// Usage
$result1 = sharedLogic(arg1, arg2);
$result2 = sharedLogic(arg3, arg4);
```

#### Complex Functions
**Issue**: High cyclomatic complexity

**Files**:
- [file:function] - Complexity: [score] (max: 10)
- [file:function] - Complexity: [score] (max: 10)

**Recommendations**:
- Extract conditions into named variables
- Break into smaller functions
- Use early returns

### 3. Type Errors (TypeScript/JavaScript)

```typescript
// Current (type error)
const numbers = [1, 2, '3', 4];
const doubled = numbers.map(n => n * 2); // Error: '*' not valid for string

// Fixed (typed)
const numbers: number[] = [1, 2, 3, 4];
const doubled = numbers.map(n => n * 2);
```

### 4. Import/Export Issues

```[language]
// Current (problematic)
import { Component } from './path';
import { Another } from './path';
// Unused import detected

// Fixed
import { Component } from './path';
```

---

## Minor Issues (Consider Fixing)

### 1. Formatting Inconsistencies

**Auto-fix Command:**
```bash
# Auto-format all files
npx prettier --write .

# PHP files
./vendor/bin/pint

# Python files
black .
isort .
```

### 2. Unused Code

**Files with Unused Code**:
- [file:line] - Unused variable: `$unusedVar`
- [file:line] - Unused function: `unusedFunction()`
- [file:line] - Unused import: `import`

**Removal Suggestion**:
```[language]
// Remove unused code
// Instead of commenting out, delete entirely
// If needed later, use git history to recover
```

### 3. Comments Quality

**Issues**:
- [file:line] - Comment describes "what" instead of "why"
- [file:line] - TODO comment not addressed
- [file:line] - Commented-out code

**Recommendations**:
```[language]
// Bad comment
// increment counter by 1

// Good comment (explains WHY)
// Reset counter for new batch processing to ensure accurate tracking
counter = 0;
```

---

## Auto-Fix Commands

### Automatically Fix All Issues
```bash
# JavaScript/TypeScript
npm run lint:fix

# PHP
./vendor/bin/pint

# Python
black .
isort --recursive .
flake8 --fix .

# All (if configured)
npm run format
```

### Fix Specific Issues
```bash
# ESLint
npx eslint . --ext .ts,.tsx --fix

# Prettier
npx prettier --write "src/**/*.{ts,tsx,js,jsx}"

# PHP CS Fixer
./vendor/bin/pint -v

# Python Black
black src/ tests/
```

---

## Configuration Improvements

### Recommended ESLint Config
```javascript
// .eslintrc.js
module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier'
  ],
  rules: {
    '@typescript-eslint/explicit-function-return-type': 'error',
    '@typescript-eslint/no-unused-vars': 'error',
    'prefer-const': 'error',
    'no-console': 'warn'
  }
};
```

### Recommended Prettier Config
```javascript
// .prettierrc
module.exports = {
  semi: true,
  singleQuote: true,
  tabWidth: 2,
  trailingComma: 'es5',
  printWidth: 100,
  bracketSpacing: true,
  arrowParens: 'avoid'
};
```

---

## Quality Metrics

### Before Linting
| Metric | Value |
|--------|-------|
| Total Warnings | [count] |
| Total Errors | [count] |
| Code Coverage | [percentage] |
| Complexity Score | [score] |

### After Linting (Target)
| Metric | Value |
|--------|-------|
| Total Warnings | 0 |
| Total Errors | 0 |
| Code Coverage | [percentage] |
| Complexity Score | [score] |

---

## Compliance Checklist

- [ ] All critical issues resolved
- [ ] All major issues addressed
- [ ] Auto-fixes applied
- [ ] Manual fixes documented
- [ ] Configuration updated
- [ ] CI/CD pipeline updated
- [ ] Team notified of changes

---

## Next Steps

1. **Immediate Actions**:
   - Run auto-fix commands
   - Manually fix critical issues
   - Update configuration files

2. **Short-term Improvements**:
   - Add pre-commit hooks
   - Configure IDE auto-formatting
   - Update CI pipeline

3. **Long-term Goals**:
   - Establish linting culture
   - Regular code quality audits
   - Continuous improvement process
