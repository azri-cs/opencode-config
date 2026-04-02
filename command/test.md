---
description: Run test suite with comprehensive coverage analysis
agent: build
temperature: 0.2
---

Run the complete test suite and analyze results. Support for PHPUnit (PHP), pytest (Python), Jest (JavaScript/TypeScript).

**Test Execution:**
!`npm test 2>&1 || vendor/bin/pest --coverage 2>&1 || python -m pytest --cov 2>&1 || echo "No test command found"`

**If Laravel project:**
!`php artisan test --coverage 2>&1 || vendor/bin/pest --coverage 2>&1`

**If Django project:**
!`python manage.py test --verbosity=2 2>&1 || python -m pytest --cov=. --cov-report=term-missing 2>&1`

**If React/Node project:**
!`npm test -- --coverage --watchAll=false 2>&1 || yarn test --coverage 2>&1`

Analyze the results and provide:

## Test Results Summary
- Total tests: [number]
- Passed: [number]
- Failed: [number]  
- Skipped: [number]
- Pending: [number]
- Coverage: [percentage]

## Detailed Failing Tests Analysis
For each failing test:

### [Test Name/Class]
- **File**: [path/to/test.php/py/js]
- **Line**: [line number]
- **Error Message**: [actual error]
- **Expected**: [what should happen]
- **Actual**: [what happened]

### Root Cause Analysis
- **Primary Cause**: [technical explanation]
- **Affected Code**: [files/modules impacted]
- **Fix Strategy**: [recommended approach]

### Suggested Fix (Code Example)
```[language]
[specific code fix with explanation]
```

## Coverage Analysis
- **Overall Coverage**: [percentage]
- **Files with Low Coverage**: [list files < 80%]
- **Uncovered Lines**: [specific line numbers]
- **Missing Test Cases**: [suggestions for additional tests]

## Performance Impact
- **Slow Tests**: [tests taking > 1s]
- **Database Queries**: [if N+1 detected]
- **External Calls**: [API calls, network requests]

## Recommendations
1. **Critical Fixes**: [must fix before deployment]
2. **High Priority**: [should fix soon]
3. **Medium Priority**: [consider fixing]
4. **Technical Debt**: [tests to improve later]

## Additional Test Suggestions
Suggest 3-5 additional test cases that would improve coverage and robustness for the code being tested.
