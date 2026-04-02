# Global OpenCode Instructions

**Language-Agnostic Development Guidelines**

---

## Core Principles

### 1. Security-First Development
- **Never expose secrets, API keys, or credentials in code**
  - Use environment variables for all sensitive data
  - Implement proper secrets management (e.g., AWS Secrets Manager, HashiCorp Vault)
  - Audit code for hardcoded credentials before committing

- **Input Validation & Sanitization**
  - Validate all user inputs on the server side
  - Use parameterized queries to prevent SQL injection
  - Sanitize outputs to prevent XSS attacks
  - Implement proper output encoding

- **Authentication & Authorization**
  - Use established authentication frameworks (Laravel Sanctum, Django Auth, Passport.js)
  - Implement role-based access control (RBAC)
  - Apply principle of least privilege
  - Secure session management with proper expiration

- **OWASP Top 10 Awareness**
  - Be aware of and defend against OWASP Top 10 vulnerabilities
  - Keep dependencies updated to patch known vulnerabilities
  - Use security linters and scanners in CI/CD pipelines

### 2. Code Quality Standards

- **Readability & Maintainability**
  - Write self-documenting code with meaningful names
  - Keep functions small and focused (single responsibility)
  - Limit function length to 20-30 lines where possible
  - Use clear, descriptive variable and function names
  - Avoid deep nesting (max 3-4 levels)

- **Code Organization**
  - Follow project-specific conventions and patterns
  - Keep related code together (cohesion)
  - Separate concerns through proper module/namespace organization
  - Use consistent project structure across similar projects

- **Comments & Documentation**
  - Comment WHY, not WHAT (code should be self-explanatory)
  - Document complex algorithms and business logic
  - Keep comments up-to-date with code changes
  - Write API documentation for public interfaces
  - Use docblocks/type hints for function signatures

- **Error Handling**
  - Never swallow exceptions without logging
  - Provide meaningful error messages (not just "Error occurred")
  - Implement proper exception hierarchy
  - Use try-catch blocks for expected failure points
  - Log errors with sufficient context for debugging
  - Return appropriate HTTP status codes in APIs

### 3. Performance Optimization

- **Database Performance**
  - Use eager loading to prevent N+1 queries
  - Add appropriate indexes for frequently queried columns
  - Optimize queries (avoid SELECT *, use LIMIT)
  - Implement caching strategies (Redis, Memcached)
  - Use database query builders ORMs efficiently

- **Application Performance**
  - Implement lazy loading for heavy resources
  - Use pagination for large data sets
  - Optimize images and static assets
  - Implement connection pooling for databases
  - Profile code to identify bottlenecks

- **Frontend Performance**
  - Minimize bundle sizes (code splitting, tree shaking)
  - Use efficient caching strategies
  - Optimize asset delivery (CDN, compression)
  - Implement efficient state management
  - Reduce render cycles in UI frameworks

### 4. Testing Excellence

- **Test-Driven Development (TDD)**
  - Write tests before implementation when possible
  - Red-Green-Refactor workflow
  - Focus on behavior, not implementation

- **Test Coverage Goals**
  - Minimum 80% coverage on business logic
  - 100% coverage on critical paths (auth, payments, data validation)
  - Integration tests for API endpoints
  - E2E tests for critical user journeys

- **Testing Best Practices**
  - Use descriptive test names (should_return_error_when_invalid_input)
  - Follow Arrange-Act-Assert pattern
  - Test edge cases and boundary conditions
  - Mock external dependencies
  - Keep tests fast and isolated
  - Use factories/fixtures for test data

### 5. Git Workflow & Collaboration

- **Branch Naming Conventions**
  - `feature/` for new features (feature/user-authentication)
  - `bugfix/` for bug fixes (bugfix/login-error-on-logout)
  - `hotfix/` for urgent production fixes (hotfix/security-patch)
  - `refactor/` for code improvements (refactor/database-queries)
  - `docs/` for documentation updates

- **Commit Best Practices**
  - Write descriptive commit messages
  - Start with imperative mood ("Add feature" not "Added feature")
  - Limit first line to 50 characters
  - Explain WHAT and WHY in body
  - Reference issue numbers when applicable
  - Keep commits atomic (one purpose per commit)

- **Code Review Guidelines**
  - Review every PR before merging
  - Focus on logic, security, and architecture
  - Be constructive and specific in feedback
  - Check for test coverage and documentation
  - Verify no sensitive data in commits

### 6. Development Environment

- **Local Development**
  - Use Docker or environment-specific setups
  - Maintain consistency across team members
  - Use environment variables for configuration
  - Document setup procedures

- **IDE & Tools**
  - Use linters and formatters consistently
  - Configure IDE for project-specific settings
  - Use static analysis tools (PHPStan, mypy, ESLint)
  - Enable auto-format on save

- **Command Aliases (for your setup)**
  - `npm test` or `vendor/bin/pest` - Run tests
  - `npm run lint` or `./vendor/bin/pint` - Format code
  - `npm run typecheck` or `phpstan analyze` - Type checking
  - `php artisan migrate` or `python manage.py migrate` - Database migrations
  - `npm run dev` or `php artisan serve` - Start development server

---

## Language-Specific Notes

### PHP (Laravel)
- Follow PSR-12 coding standards
- Use Laravel's built-in features (Eloquent, Policies, Form Requests)
- Implement Repository pattern for complex database logic
- Use dependency injection and service containers
- Leverage Laravel's testing features (Pest, PHPUnit)

### Python (Django)
- Follow PEP 8 and PEP 20 (Zen of Python)
- Use type hints and mypy for type checking
- Leverage Django's built-in features (ORM, Admin, Auth)
- Use Django REST Framework for APIs
- Implement proper settings management (local vs production)

### JavaScript/TypeScript (React/Node)
- Use TypeScript strict mode
- Follow React best practices (hooks, functional components)
- Use ESLint and Prettier for consistent formatting
- Implement proper state management (React Query, Redux, Zustand)
- Use proper async/await patterns with error handling

---

## Common Patterns

### API Design
- Use RESTful conventions (GET, POST, PUT, DELETE)
- Implement proper versioning (/api/v1/*)
- Return consistent response formats
- Use proper HTTP status codes
- Implement pagination for lists
- Document APIs with OpenAPI/Swagger

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The given data was invalid",
    "details": {
      "email": ["The email field is required."]
    }
  }
}
```

### Success Response Format
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Example"
  },
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 100
  }
}
```

---

## Testing Framework Equivalents

| Task | Laravel/PHP | Django/Python | React/Node |
|------|-------------|---------------|------------|
| Unit Tests | PHPUnit, Pest | pytest, unittest | Jest, Vitest |
| E2E Tests | Laravel Dusk | Playwright, Cypress | Playwright, Cypress |
| Feature Tests | PHPUnit | pytest | Jest |
| Code Coverage | PHP_Coverage | coverage.py | NYC, c8 |
| Mocking | Mockery, Prophecy | unittest.mock | Jest mocks |

---

## Performance Monitoring

### Key Metrics to Track
- Response time (p95, p99)
- Error rates
- Database query performance
- Cache hit ratios
- Memory usage
- Queue processing times

### Tools
- Laravel: Telescope, Horizon, Clockwork
- Django: Django Debug Toolbar, Sentry
- Node: PM2, New Relic, Datadog
- General: APM tools, logging aggregators

---

## Security Checklist for Every PR

- [ ] No hardcoded secrets or API keys
- [ ] Input validation implemented
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS prevention (output encoding)
- [ ] Authentication/authorization checks
- [ ] Sensitive data not logged
- [ ] Dependencies updated
- [ ] Security scanning tools pass
- [ ] No debug code in production
- [ ] Proper error handling (no stack traces exposed)

---

## Additional Resources

- [OWASP Top 10](https://owasp.org/Top10/)
- [PHP-FIG PSR Standards](https://www.php-fig.org/)
- [PEP 8 Python Style Guide](https://pep8.org/)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Laravel Best Practices](https://github.com/alexeymezenin/laravel-best-practices)
- [Django Best Practices](https://docs.djangoproject.com/en/stable/topics/)
