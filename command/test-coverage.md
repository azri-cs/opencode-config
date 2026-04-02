---
description: Analyze test coverage and identify gaps
agent: test-runner
temperature: 0.2
subtask: true
---

Analyze test coverage and identify areas needing additional tests.

**Coverage Analysis:**
!`npm test -- --coverage --coverage-reporters=json 2>&1 || vendor/bin/pest --coverage 2>&1 || python -m pytest --cov=. --cov-report=json 2>&1 || echo "Running coverage analysis..."`

!`find . -path "./vendor" -prune -o -path "./node_modules" -prune -o -path "./.git" -prune -o -name "*Test.php" -print 2>/dev/null | wc -l`

!`find . -path "./vendor" -prune -o -path "./node_modules" -prune -o -path "./.git" -prune -o -name "*.test.ts" -print 2>/dev/null | wc -l`

!`find . -path "./vendor" -prune -o -path "./node_modules" -prune -o -path "./.git" -prune -o -name "test_*.py" -print 2>/dev/null | wc -l`

## Test Coverage Analysis

### Summary
| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Line Coverage | [X%] | > 80% | [Pass/Fail] |
| Function Coverage | [X%] | > 85% | [Pass/Fail] |
| Branch Coverage | [X%] | > 75% | [Pass/Fail] |
| Statement Coverage | [X%] | > 80% | [Pass/Fail] |
| Complexity Coverage | [X%] | > 70% | [Pass/Fail] |

### Files by Coverage
| File | Lines | Functions | Branches | Status |
|------|-------|-----------|----------|--------|
| [file1] | [X%] | [X%] | [X%] | [Covered/Partial/Uncovered] |
| [file2] | [X%] | [X%] | [X%] | [Covered/Partial/Uncovered] |
| [file3] | [X%] | [X%] | [X%] | [Covered/Partial/Uncovered] |

---

## Uncovered Code Analysis

### 1. Critical Files with Low Coverage

#### File: [path/to/file]
**Current Coverage**: [X%]
**Lines Uncovered**: [list]

**Uncovered Code**:
```[language]
[uncovered code]
```

**Recommended Tests**:
```[language]
// Test case 1
test('should handle [scenario]', () => {
  // Arrange
  const input = [test data];
  
  // Act
  const result = functionUnderTest(input);
  
  // Assert
  expect(result).toEqual([expected]);
});

// Test case 2
test('should handle [edge case]', () => {
  // Arrange
  const input = [edge case data];
  
  // Act
  const result = functionUnderTest(input);
  
  // Assert
  expect(result).toEqual([expected]);
});
```

### 2. Uncovered Functions/Methods

#### Function: [class.functionName]
**Coverage**: 0%

**Missing Test Scenarios**:
1. **Normal Operation**
   - Input: [test data]
   - Expected: [result]
   
2. **Edge Case 1**
   - Input: [edge case data]
   - Expected: [result]
   
3. **Edge Case 2**
   - Input: [edge case data]
   - Expected: [result]
   
4. **Error Condition**
   - Input: [invalid data]
   - Expected: [error/exception]

**Test Implementation**:
```[language]
describe('ClassName.functionName', () => {
  it('should handle normal operation', () => {
    // Implementation
  });
  
  it('should handle edge case 1', () => {
    // Implementation
  });
  
  it('should handle edge case 2', () => {
    // Implementation
  });
  
  it('should throw error for invalid input', () => {
    // Implementation
  });
});
```

### 3. Uncovered Branches

#### Branch: [condition]
**Location**: [file:line]

**Uncovered Branches**:
- [condition] when [X] → [Y branches not taken]

**Test Cases Needed**:
```[language]
// Branch: if (isValid)
test('should execute when isValid is true', () => {
  const result = functionUnderTest({ isValid: true });
  expect(result).toEqual([expected]);
});

test('should execute when isValid is false', () => {
  const result = functionUnderTest({ isValid: false });
  expect(result).toEqual([expected]);
});

// Branch: switch (status)
test('should handle PENDING status', () => {
  const result = functionUnderTest({ status: 'PENDING' });
  expect(result).toEqual([expected]);
});

test('should handle APPROVED status', () => {
  const result = functionUnderTest({ status: 'APPROVED' });
  expect(result).toEqual([expected]);
});

test('should handle REJECTED status', () => {
  const result = functionUnderTest({ status: 'REJECTED' });
  expect(result).toEqual([expected]);
});
```

---

## Test Gap Analysis

### 1. Missing Test Types

#### Unit Tests
**Status**: [Complete/Incomplete]
**Missing Tests**:
- [ ] [Test for function X]
- [ ] [Test for function Y]

#### Integration Tests
**Status**: [Complete/Incomplete]
**Missing Tests**:
- [ ] [Test for API endpoint X]
- [ ] [Test for database operation Y]

#### End-to-End Tests
**Status**: [Complete/Incomplete]
**Missing Tests**:
- [ ] [User journey X]
- [ ] [User journey Y]

### 2. Missing Edge Cases

| Edge Case | File | Function | Priority |
|-----------|------|----------|----------|
| Null input | [file] | [function] | High |
| Empty array | [file] | [function] | High |
| Maximum values | [file] | [function] | Medium |
| Special characters | [file] | [function] | Medium |
| Network timeout | [file] | [function] | High |
| Permission denied | [file] | [function] | High |

### 3. Missing Error Handling Tests

**Error Cases Not Tested**:
- [ ] [Error type] in [file:function]
- [ ] [Error type] in [file:function]
- [ ] [Error type] in [file:function]

**Test Examples**:
```[language]
describe('Error Handling', () => {
  it('should throw ValidationError for invalid input', () => {
    expect(() => {
      functionUnderTest(invalidInput);
    }).toThrow(ValidationError);
  });
  
  it('should throw NetworkError on timeout', () => {
    jest.spyOn(api, 'fetch').mockRejectedValue(new TimeoutError());
    
    await expect(functionUnderTest(request))
      .rejects.toThrow(NetworkError);
  });
  
  it('should handle permission denied gracefully', () => {
    const result = functionUnderTest({ hasPermission: false });
    expect(result).toEqual({ allowed: false });
  });
});
```

---

## Recommended Test Improvements

### 1. High Priority Tests

#### Authentication Tests
```[language]
describe('Authentication', () => {
  describe('login', () => {
    it('should authenticate valid credentials', () => {
      // Test valid login
    });
    
    it('should reject invalid password', () => {
      // Test invalid password
    });
    
    it('should handle non-existent user', () => {
      // Test user not found
    });
    
    it('should handle account lockout', () => {
      // Test lockout after failed attempts
    });
    
    it('should generate JWT on successful login', () => {
      // Test token generation
    });
  });
  
  describe('authorization', () => {
    it('should allow access with valid token', () => {
      // Test authorized access
    });
    
    it('should deny access with invalid token', () => {
      // Test unauthorized access
    });
    
    it('should handle expired tokens', () => {
      // Test expired token
    });
  });
});
```

#### Database Operations Tests
```[language]
describe('Database Operations', () => {
  describe('create', () => {
    it('should create record with valid data', () => {
      // Test successful creation
    });
    
    it('should fail on duplicate key', () => {
      // Test unique constraint
    });
    
    it('should validate required fields', () => {
      // Test validation
    });
    
    it('should handle database connection failure', () => {
      // Test connection error
    });
  });
  
  describe('read', () => {
    it('should return single record by ID', () => {
      // Test find by ID
    });
    
    it('should return paginated results', () => {
      // Test pagination
    });
    
    it('should filter by criteria', () => {
      // Test filtering
    });
    
    it('should handle empty results', () => {
      // Test no results found
    });
  });
  
  describe('update', () => {
    it('should update existing record', () => {
      // Test successful update
    });
    
    it('should fail on non-existent record', () => {
      // Test update non-existent
    });
    
    it('should handle concurrent updates', () => {
      // Test optimistic locking
    });
  });
  
  describe('delete', () => {
    it('should soft delete record', () => {
      // Test soft delete
    });
    
    it('should prevent deletion with dependencies', () => {
      // Test foreign key constraint
    });
  });
});
```

### 2. API Endpoint Tests

```[language]
describe('API Endpoints', () => {
  describe('GET /api/users', () => {
    it('should return paginated users', () => {
      const response = await request(app)
        .get('/api/users')
        .query({ page: 1, limit: 10 });
      
      expect(response.status).toBe(200);
      expect(response.body.data).toHaveLength(10);
      expect(response.body.meta).toBeDefined();
    });
    
    it('should filter users by status', () => {
      const response = await request(app)
        .get('/api/users')
        .query({ status: 'active' });
      
      expect(response.status).toBe(200);
      response.body.data.forEach(user => {
        expect(user.status).toBe('active');
      });
    });
    
    it('should return 401 for unauthenticated request', () => {
      const response = await request(app)
        .get('/api/users');
      
      expect(response.status).toBe(401);
    });
    
    it('should handle pagination boundaries', () => {
      const response = await request(app)
        .get('/api/users')
        .query({ page: 9999, limit: 100 });
      
      expect(response.status).toBe(200);
      expect(response.body.data).toHaveLength(0);
    });
  });
  
  describe('POST /api/users', () => {
    it('should create new user', () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'securepassword',
      };
      
      const response = await request(app)
        .post('/api/users')
        .send(userData);
      
      expect(response.status).toBe(201);
      expect(response.body.data.email).toBe(userData.email);
    });
    
    it('should validate required fields', () => {
      const response = await request(app)
        .post('/api/users')
        .send({});
      
      expect(response.status).toBe(422);
      expect(response.body.errors).toBeDefined();
    });
    
    it('should reject duplicate email', () => {
      const userData = {
        name: 'John Doe',
        email: 'existing@example.com',
        password: 'securepassword',
      };
      
      const response = await request(app)
        .post('/api/users')
        .send(userData);
      
      expect(response.status).toBe(409);
    });
  });
});
```

### 3. Component Tests

```typescript
describe('UserCard Component', () => {
  it('should render user information', () => {
    const user = createTestUser();
    render(<UserCard user={user} />);
    
    expect(screen.getByText(user.name)).toBeInTheDocument();
    expect(screen.getByText(user.email)).toBeInTheDocument();
  });
  
  it('should call onClick when clicked', () => {
    const onClick = jest.fn();
    const user = createTestUser();
    
    render(<UserCard user={user} onClick={onClick} />);
    fireEvent.click(screen.getByRole('button'));
    
    expect(onClick).toHaveBeenCalledWith(user.id);
  });
  
  it('should show loading state', () => {
    render(<UserCard user={null} isLoading={true} />);
    
    expect(screen.getByRole('status')).toHaveTextContent(/loading/i);
  });
  
  it('should handle error state', () => {
    render(<UserCard user={null} error={new Error('Failed')} />);
    
    expect(screen.getByRole('alert')).toHaveTextContent(/failed/i);
  });
  
  it('should be accessible', () => {
    const user = createTestUser();
    render(<UserCard user={user} />);
    
    expect(screen.getByRole('article')).toBeInTheDocument();
    expect(screen.getByRole('button')).toHaveAttribute('aria-label');
  });
});
```

---

## Test Coverage Improvement Plan

### Current Coverage: [X%]

### Target Coverage: [90%]

### Coverage Improvement Roadmap

#### Phase 1: Critical Path (Week 1)
- [ ] Test all authentication functions → +[X]%
- [ ] Test main business logic → +[X]%
- [ ] Test API endpoints → +[X]%
- **Subtotal**: [X]%

#### Phase 2: Edge Cases (Week 2)
- [ ] Test all error handlers → +[X]%
- [ ] Test validation logic → +[X]%
- [ ] Test boundary conditions → +[X]%
- **Subtotal**: [X]%

#### Phase 3: Integration (Week 3)
- [ ] Test database operations → +[X]%
- [ ] Test external API calls → +[X]%
- [ ] Test queue/jobs → +[X]%
- **Subtotal**: [X]%

#### Phase 4: Complete Coverage (Week 4)
- [ ] Test remaining functions → +[X]%
- [ ] Add E2E tests → +[X]%
- [ ] Review and optimize → +[X]%
- **Subtotal**: [X]%

---

## Test Data Recommendations

### Test Fixtures
```[language]
// user.fixture.js
export const createTestUser = (overrides = {}) => ({
  id: 'user-123',
  name: 'Test User',
  email: 'test@example.com',
  role: 'user',
  status: 'active',
  createdAt: new Date('2024-01-01'),
  updatedAt: new Date('2024-01-01'),
  ...overrides,
});

export const createTestUsers = (count = 5) => 
  Array.from({ length: count }, (_, i) => 
    createTestUser({ id: `user-${i}`, email: `user${i}@example.com` })
  );
```

### Mock Data
```[language]
// Mock database
export const mockUserRepository = {
  findById: jest.fn(),
  findAll: jest.fn(),
  create: jest.fn(),
  update: jest.fn(),
  delete: jest.fn(),
};

// Mock API
export const mockApi = {
  get: jest.fn(),
  post: jest.fn(),
  put: jest.fn(),
  delete: jest.fn(),
};
```

---

## Quality Gates

### Minimum Coverage Requirements
| Component | Minimum Coverage |
|-----------|------------------|
| Core Business Logic | 95% |
| API Endpoints | 90% |
| Database Operations | 90% |
| Authentication/Authorization | 100% |
| Error Handling | 80% |
| Utility Functions | 85% |
| UI Components | 80% |

### Coverage Enforcement
```javascript
// package.json
{
  "scripts": {
    "test": "jest --coverage",
    "test:ci": "jest --coverage --coverageThreshold='{\"global\":{\"branches\":90,\"functions\":90,\"lines\":90,\"statements\":90}}'"
  }
}
```

---

## Testing Best Practices

### 1. Test Organization
```
tests/
├── unit/
│   ├── services/
│   ├── models/
│   └── utils/
├── integration/
│   ├── api/
│   └── database/
├── e2e/
│   ├── user-journeys/
│   └── admin-journeys/
├── fixtures/
│   ├── user.fixture.js
│   └── data.fixture.js
└── mocks/
    ├── api.mocks.js
    └── database.mocks.js
```

### 2. Test Naming Conventions
```
# Bad
test('test1')
test('should work')

# Good
test('should return user when valid ID provided')
test('should throw ValidationError when email is invalid')
test('should authenticate user with correct credentials')
```

### 3. Test Isolation
```javascript
// Each test should be independent
describe('UserService', () => {
  let service;
  let repository;
  
  beforeEach(() => {
    repository = new MockUserRepository();
    service = new UserService(repository);
  });
  
  afterEach(() => {
    jest.clearAllMocks();
  });
});
```

---

## Testing Tools & Commands

### Running Tests
```bash
# Run all tests with coverage
npm test

# Run specific test file
npm test -- user.service.test.js

# Run tests in watch mode
npm test -- --watch

# Run tests without coverage
npm test -- --coverage=false

# Run tests with specific reporter
npm test -- --coverage-reporters=html,lcov
```

### Coverage Reports
```bash
# Generate HTML report
npm test -- --coverageReporters=html

# Generate LCOV for CI
npm test -- --coverageReporters=lcov

# Generate JSON for analysis
npm test -- --coverageReporters=json
```

### CI Integration
```yaml
# GitHub Actions
- name: Run tests
  run: npm test
  
- name: Check coverage
  run: |
    if [ $(jq '.coverage.statements.pct' coverage/coverage-final.json) -lt 90 ]; then
      echo "Coverage below 90%"
      exit 1
    fi
```
