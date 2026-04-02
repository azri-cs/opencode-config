---
description: Generate comprehensive documentation for code components
agent: docs-writer
temperature: 0.3
---

Analyze the specified code and generate comprehensive documentation. Use the file path provided as $ARGUMENTS or the current context if no argument given. Prefer project terminology and stack-specific skills when they clarify behavior.

**File Analysis:**
!`find . -name "$ARGUMENTS" -o -name "$(echo $ARGUMENTS | sed 's/.*\///')" 2>/dev/null | head -5`

!`ls -la $ARGUMENTS 2>/dev/null || ls -la $(echo $ARGUMENTS | sed 's/.*\///') 2>/dev/null || echo "Checking current directory..."`

## Documentation Template

### Component Overview
- **File**: [path/to/file]
- **Type**: [Component/Class/Function/Module/Service]
- **Language**: [PHP/Python/JavaScript/TypeScript]
- **Framework**: [Laravel/Django/React/Node]
- **Last Modified**: [date]
- **Lines of Code**: [count]

### Purpose
[Clear description of what this component does and its role in the system]

### Prerequisites
- **Dependencies**: [external libraries or modules required]
- **Environment**: [specific environment requirements]
- **Configuration**: [configuration needed before use]

---

## API Reference

### Class/Component Name

#### Constructor/Initialization
```[language]
[constructor or initialization code]
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| [name] | [type] | [yes/no] | [value] | [description] |

**Throws:**
- [Exception type]: [when thrown]

#### Public Methods

##### methodName()
```[language]
[method signature]
```

**Description:** [what the method does]

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| [name] | [type] | [yes/no] | [value] | [description] |

**Returns:**
- [Return type]: [description]

**Throws:**
- [Exception type]: [when thrown]

**Example:**
```[language]
// Basic usage
const result = instance.methodName(param1, param2);

// Advanced usage
const result = instance.methodName({
  option1: value1,
  option2: value2
});
```

##### anotherMethod()
```[language]
[method signature]
```

**Description:** [what the method does]

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| [name] | [type] | [yes/no] | [value] | [description] |

**Returns:**
- [Return type]: [description]

**Throws:**
- [Exception type]: [when thrown]

---

#### Protected/Private Methods

##### _internalMethod()
```[language]
[method signature]
```

**Description:** [internal implementation details]

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| [name] | [type] | [yes/no] | [value] | [description] |

**Returns:**
- [Return type]: [description]

---

### Properties/Attributes

| Property | Type | Writable | Description |
|----------|------|----------|-------------|
| [name] | [type] | [yes/no] | [description] |
| [name] | [type] | [yes/no] | [description] |

---

## Usage Examples

### Basic Usage
```[language]
// Import or require
import { ComponentName } from './path';

// Initialize
const instance = new ComponentName({
  config1: 'value1',
  config2: 'value2'
});

// Use method
const result = instance.methodName('input');
console.log(result);
```

### Advanced Usage
```[language]
// Complex configuration
const instance = new ComponentName({
  config1: 'value1',
  config2: 'value2',
  callbacks: {
    onSuccess: (result) => {
      console.log('Success:', result);
    },
    onError: (error) => {
      console.error('Error:', error);
    }
  }
});

// Multiple method calls
const result1 = instance.method1();
const result2 = instance.method2(result1);
const finalResult = instance.method3(result2);
```

### Real-World Example
```[language]
// Practical implementation
async function practicalExample() {
  try {
    const userService = new UserService({
      baseUrl: 'https://api.example.com',
      timeout: 5000
    });
    
    const users = await userService.getUsers({
      page: 1,
      limit: 10,
      filters: {
        active: true
      }
    });
    
    return users;
  } catch (error) {
    console.error('Failed to fetch users:', error);
    throw error;
  }
}
```

---

## Integration Guide

### Dependency Injection
**For Laravel:**
```php
// Service Provider registration
$this->app->singleton(ComponentInterface::class, ComponentName::class);

// Constructor injection
public function __construct(ComponentInterface $component) {
    $this->component = $component;
}
```

**For Django:**
```python
# settings.py
INSTALLED_APPS = [
    ...
    'myapp',
]

# Dependency injection
service = ComponentName(config={'key': 'value'})
```

**For React:**
```tsx
// Context provider
import { ComponentProvider } from './context';

function App() {
  return (
    <ComponentProvider config={defaultConfig}>
      <ChildComponent />
    </ComponentProvider>
  );
}

// Hook usage
const { instance } = useComponent();
```

### Database Integration
**For Laravel/Eloquent:**
```php
// Model integration
class UserService {
    public function __construct(private UserRepository $repository) {}
    
    public function getActiveUsers() {
        return $this->repository->where('active', true)->get();
    }
}
```

**For Django:**
```python
# Model integration
class UserService:
    def __init__(self, user_model):
        self.user_model = user_model
    
    def get_active_users(self):
        return self.user_model.objects.filter(is_active=True)
```

### API Integration
```[language]
// HTTP client integration
import axios from 'axios';

class ApiService {
  constructor(private httpClient) {}
  
  async fetchData(endpoint) {
    const response = await this.httpClient.get(endpoint);
    return response.data;
  }
}
```

---

## Configuration Options

### Default Configuration
```javascript
const defaultConfig = {
  // Core settings
  debug: false,
  timeout: 30000,
  retries: 3,
  
  // Feature flags
  caching: true,
  logging: true,
  validation: true,
  
  // Security
  encryption: 'AES-256',
  authRequired: true,
  
  // Performance
  batchSize: 100,
  maxConcurrent: 5,
  
  // Callbacks
  onSuccess: null,
  onError: null,
  onProgress: null
};
```

### Environment Variables
```bash
# .env file
COMPONENT_DEBUG=false
COMPONENT_TIMEOUT=30000
COMPONENT_ENCRYPTION_KEY=your-encryption-key
COMPONENT_API_URL=https://api.example.com
```

---

## Best Practices

### Performance Tips
1. **Initialization**: [tip 1]
2. **Method Calls**: [tip 2]
3. **Memory Management**: [tip 3]

### Error Handling
```[language]
// Recommended error handling
try {
  const result = await instance.methodName();
  return result;
} catch (error) {
  if (error instanceof SpecificError) {
    // Handle specific error
    logger.error('Specific error:', error);
  } else {
    // Handle general error
    logger.error('General error:', error);
    throw error;
  }
}
```

### Security Considerations
1. **Input Validation**: Always validate input parameters
2. **Authentication**: Ensure proper auth before sensitive operations
3. **Data Protection**: Encrypt sensitive data in transit and at rest

### Common Pitfalls
1. **Pitfall 1**: [description] → [solution]
2. **Pitfall 2**: [description] → [solution]
3. **Pitfall 3**: [description] → [solution]

---

## Troubleshooting

### Common Issues

#### Issue: Configuration Not Applied
**Symptoms:** [symptoms]
**Cause:** [root cause]
**Solution:**
```[language]
// Correct configuration
const instance = new ComponentName({
  ...defaultConfig,
  ...customConfig  // This overrides defaults
});
```

#### Issue: Method Returns Unexpected Result
**Symptoms:** [symptoms]
**Cause:** [root cause]
**Solution:**
```[language]
// Add validation
const result = instance.methodName(input);
if (!result) {
  throw new Error('Unexpected result');
}
```

#### Issue: Performance Degradation
**Symptoms:** [symptoms]
**Cause:** [root cause]
**Solution:**
```[language]
// Implement caching
const cache = new Map();
async function cachedMethod(input) {
  if (cache.has(input)) {
    return cache.get(input);
  }
  const result = await instance.methodName(input);
  cache.set(input, result);
  return result;
}
```

---

## Related Components

### Dependencies
- [Component 1] - [relationship]
- [Component 2] - [relationship]

### Related Components
- [Component 3] - [relationship]
- [Component 4] - [relationship]

### See Also
- [Documentation Link 1]
- [Documentation Link 2]
- [API Reference Link]

---

## Changelog

### Version History
| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0.0 | [date] | Initial release | [name] |
| 1.1.0 | [date] | [changes] | [name] |
| 1.2.0 | [date] | [changes] | [name] |

### Migration Guide
**From v1.0 to v1.1:**
- [Breaking change]: [migration steps]
- [Deprecation]: [migration steps]
- [New feature]: [usage guide]

---

## Contributing

### Development Setup
```bash
# Clone repository
git clone [repository]

# Install dependencies
npm install  # or composer install

# Run tests
npm test  # or php artisan test

# Build documentation
npm run docs
```

### Testing
```bash
# Unit tests
npm run test:unit

# Integration tests
npm run test:integration

# E2E tests
npm run test:e2e
```

### Style Guide
- **Code Style**: [standard]
- **Documentation Style**: [standard]
- **Commit Messages**: [conventional commits]
