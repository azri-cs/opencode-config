---
description: Analyze codebase structure and dependencies
agent: explore
temperature: 0.2
---

Analyze the current project structure and provide comprehensive insights.

**Project Analysis:**
!`find . -type f \( -name "*.php" -o -name "*.py" -o -name "*.js" -o -name "*.ts" -o -name "*.jsx" -o -name "*.tsx" \) ! -path "./vendor/*" ! -path "./node_modules/*" ! -path "./.git/*" ! -path "./migrations/*" | head -30`

!`ls -la`
!`cat package.json 2>/dev/null | head -20 || cat composer.json 2>/dev/null | head -20 || echo "No package.json or composer.json found"`

!`find . -name "*.md" -maxdepth 2 ! -path "./vendor/*" ! -path "./node_modules/*" | head -10`

## Project Overview

### Technology Stack
- **Primary Language**: [PHP/Python/JavaScript]
- **Framework**: [Laravel/Django/React/Node]
- **Database**: [MySQL/PostgreSQL/MongoDB]
- **Package Manager**: [npm/Composer/Pip]
- **Key Dependencies**: [list major packages]

### Project Type
- [ ] REST API
- [ ] Single Page Application (SPA)
- [ ] Full Stack Application
- [ ] Microservice
- [ ] CLI Application
- [ ] Library/Package

### Architecture Pattern
- [ ] MVC (Model-View-Controller)
- [ ] MVVM (Model-View-ViewModel)
- [ ] Repository-Service Layer
- [ ] Domain-Driven Design
- [ ] Microservices
- [ ] Monolithic

## Directory Structure Analysis

### Source Code Organization
```
[project-root]/
├── [primary-directory]/  ([count] files)
├── [secondary-directory]/ ([count] files)
├── tests/                 ([count] files)
├── docs/                  ([count] files)
└── [config-files]
```

### Key Observations
- **Total Source Files**: [count]
- **Test Files**: [count]
- **Configuration Files**: [count]
- **Documentation Files**: [count]

## Dependency Analysis

### Production Dependencies
**Major Libraries/Frameworks:**
- [Dependency 1]: [version] - [purpose]
- [Dependency 2]: [version] - [purpose]
- [Dependency 3]: [version] - [purpose]

### Development Dependencies
- Testing: [frameworks used]
- Linting: [tools used]
- Building: [tools used]
- Formatting: [tools used]

### Dependency Health
- **Outdated Dependencies**: [list if any]
- **Vulnerable Dependencies**: [security alerts]
- **Unused Dependencies**: [suggestions]

## Code Quality Metrics

### File Statistics
- **Total Lines of Code**: [approximate]
- **Average File Size**: [lines per file]
- **Largest Files**: [top 5 files by size]
- **Most Complex Files**: [functions/classes with high complexity]

### Language Distribution
- [Language]: [percentage] ([file count] files)
- [Language]: [percentage] ([file count] files)
- [Language]: [percentage] ([file count] files)

### Test Coverage Estimate
- **Test Files**: [count]
- **Coverage**: [estimated percentage]
- **Test Types**: [unit/integration/e2e]

## Architecture Assessment

### Strengths
1. [Positive architectural decision]
2. [Good organization pattern]
3. [Proper separation of concerns]

### Weaknesses
1. [Architecture issue]
2. [Organization problem]
3. [Technical debt]

### Refactoring Opportunities
1. [Refactoring suggestion 1]
   - **Current**: [problem]
   - **Suggested**: [improvement]
   - **Effort**: [Low/Medium/High]
   - **Impact**: [High/Medium/Low]

2. [Refactoring suggestion 2]
   - **Current**: [problem]
   - **Suggested**: [improvement]
   - **Effort**: [Low/Medium/High]
   - **Impact**: [High/Medium/Low]

## Security Assessment

### Potential Vulnerabilities
- **Input Validation**: [status]
- **Authentication**: [status]
- **Authorization**: [status]
- **Data Protection**: [status]
- **Dependencies**: [status]

### Security Recommendations
1. [Security improvement]
2. [Security improvement]

## Performance Assessment

### Potential Bottlenecks
- **Database**: [observations]
- **Caching**: [status]
- **API Calls**: [observations]
- **Heavy Computations**: [identified areas]

### Performance Recommendations
1. [Performance optimization]
2. [Performance optimization]

## Modernization Opportunities

### Technology Upgrades
1. **Framework Version**: Current [version] → Latest [version]
   - **Breaking Changes**: [yes/no]
   - **Effort**: [Low/Medium/High]
   - **Benefits**: [list]

2. **Language Version**: Current [version] → Latest [version]
   - **Breaking Changes**: [yes/no]
   - **Effort**: [Low/Medium/High]
   - **Benefits**: [list]

### Best Practices Adoption
1. [Modern practice to adopt]
2. [Modern practice to adopt]

## Documentation Review

### Existing Documentation
- [x] README.md
- [ ] CONTRIBUTING.md
- [ ] Architecture documentation
- [ ] API documentation
- [ ] Deployment guide

### Documentation Gaps
- [Missing documentation areas]

## Recommendations Summary

### Immediate Actions (This Sprint)
1. [Action 1]
2. [Action 2]
3. [Action 3]

### Short-term Improvements (1-2 Months)
1. [Improvement 1]
2. [Improvement 2]
3. [Improvement 3]

### Long-term Goals (3-6 Months)
1. [Goal 1]
2. [Goal 2]
3. [Goal 3]

### Technical Debt Items
1. [Debt item] - [impact: High/Medium/Low]
2. [Debt item] - [impact: High/Medium/Low]
