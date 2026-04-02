---
description: Perform comprehensive security audit
agent: security-auditor
temperature: 0.1
subtask: true
---

Perform a comprehensive security audit of the codebase. Focus on OWASP Top 10 vulnerabilities and secure coding practices.

**Project Analysis:**
!`git diff --name-only`
!`find . -type f \( -name "*.php" -o -name "*.py" -o -name "*.js" -o -name "*.ts" \) ! -path "./vendor/*" ! -path "./node_modules/*" ! -path "./.git/*" | head -20`

!`cat package.json 2>/dev/null | grep -A 5 '"dependencies"' || echo "Checking dependencies..."`
!`grep -r "password\|secret\|api_key\|apikey" . --include="*.php" --include="*.py" --include="*.js" --include="*.ts" ! -path "./vendor/*" ! -path "./node_modules/*" ! -path "./.git/*" 2>/dev/null | head -10 || echo "No obvious secrets found"`

## Security Audit Report

### Scope
- **Files Changed**: [count]
- **Lines Analyzed**: [count]
- **Focus Areas**: Authentication, Authorization, Data Protection, Input Validation

---

## 1. Authentication Security

### Current Implementation Review
- **Authentication Method**: [JWT/Session/OAuth/Other]
- **Password Hashing**: [bcrypt/argon2/PBKDF2]
- **Session Management**: [status]

### Findings

#### ✅ Strengths
1. [Positive authentication practice]

#### ⚠️ Issues Found

##### [Issue Title]
- **Severity**: [Critical/High/Medium/Low]
- **Location**: [file:line]
- **Current Code**:
```[language]
[insecure code]
```
- **Vulnerability**: [explanation]
- **Impact**: [what could happen]
- **Recommendation**:
```[language]
[secure code]
```
- **Fix Priority**: [P0/P1/P2/P3]

---

## 2. Authorization & Access Control

### Current Implementation Review
- **Authorization Model**: [RBAC/ABAC/Other]
- **Permission Checks**: [where implemented]
- **Role Hierarchy**: [if applicable]

### Findings

#### ✅ Strengths
1. [Positive authorization practice]

#### ⚠️ Issues Found

##### [Issue Title]
- **Severity**: [Critical/High/Medium/Low]
- **Location**: [file:line]
- **Current Code**:
```[language]
[insecure code]
```
- **Vulnerability**: [explanation]
- **Impact**: [what could happen]
- **Recommendation**:
```[language]
[secure code]
```
- **Fix Priority**: [P0/P1/P2/P3]

---

## 3. Input Validation & Sanitization

### Current Implementation Review
- **Validation Framework**: [framework used]
- **Sanitization Methods**: [methods used]
- **File Upload Security**: [status]

### Findings

#### ✅ Strengths
1. [Positive validation practice]

#### ⚠️ Issues Found

##### SQL Injection Vulnerabilities
- **Severity**: [Critical/High/Medium/Low]
- **Location**: [file:line]
- **Vulnerable Code**:
```[language]
[unsafe query construction]
```
- **Risk**: Database compromise, data theft
- **Safe Alternative**:
```[language]
[parameterized query]
```
- **Fix Priority**: P0

##### Cross-Site Scripting (XSS)
- **Severity**: [Critical/High/Medium/Low]
- **Location**: [file:line]
- **Vulnerable Code**:
```[language]
[unsafe output]
```
- **Risk**: Account hijacking, data theft
- **Safe Alternative**:
```[language]
[escaped output]
```
- **Fix Priority**: P0

##### Command Injection
- **Severity**: [Critical/High/Medium/Low]
- **Location**: [file:line]
- **Vulnerable Code**:
```[language]
[unsafe system call]
```
- **Risk**: Server compromise
- **Safe Alternative**:
```[language]
[safe approach]
```
- **Fix Priority**: P0

---

## 4. Data Protection

### Current Implementation Review
- **Encryption at Rest**: [status]
- **Encryption in Transit**: [status]
- **Sensitive Data Handling**: [status]
- **Logging Practices**: [status]

### Findings

#### ⚠️ Issues Found

##### Hardcoded Secrets
- **Severity**: Critical
- **Locations Found**:
  - [file:line] - [secret type]
  - [file:line] - [secret type]
- **Risk**: Credential compromise, unauthorized access
- **Recommendation**: Use environment variables or secrets management

##### Sensitive Data in Logs
- **Severity**: High
- **Locations Found**:
  - [file:line] - [logged data type]
- **Risk**: Data exposure through logs
- **Recommendation**: Mask sensitive data before logging

---

## 5. Dependency Security

### Vulnerability Scan
- **Total Dependencies**: [count]
- **Known Vulnerabilities**: [count]
- **Critical Severity**: [count]
- **High Severity**: [count]

### Dependency Issues

#### Critical Vulnerabilities
| Dependency | Version | Vulnerability | CVE | Fix Available |
|------------|---------|---------------|-----|---------------|
| [name] | [ver] | [desc] | [cve] | [yes/no] |

#### High Severity Issues
| Dependency | Version | Vulnerability | CVE | Fix Available |
|------------|---------|---------------|-----|---------------|
| [name] | [ver] | [desc] | [cve] | [yes/no] |

---

## 6. Security Headers & Configuration

### HTTP Security Headers
| Header | Current | Recommended |
|--------|---------|-------------|
| X-Frame-Options | [value] | DENY/SAMEORIGIN |
| X-Content-Type-Options | [value] | nosniff |
| X-XSS-Protection | [value] | 1; mode=block |
| Strict-Transport-Security | [value] | max-age=31536000 |
| Content-Security-Policy | [value] | [policy] |

### CORS Configuration
- **Origin**: [allowed origins]
- **Methods**: [allowed methods]
- **Headers**: [allowed headers]
- **Credentials**: [status]

---

## 7. File & Resource Security

### File Upload Security
- **Allowed Types**: [file extensions]
- **Size Limits**: [max size]
- **Storage**: [location]
- **Virus Scanning**: [status]

### Path Traversal
- **Status**: [Protected/Vulnerable]
- **Affected Code**: [locations]

---

## 8. API Security

### Current Implementation
- **Authentication**: [method]
- **Rate Limiting**: [status]
- **Input Validation**: [status]
- **Output Encoding**: [status]

### Findings

#### ⚠️ Issues Found

##### Missing Rate Limiting
- **Severity**: Medium
- **Location**: [file/endpoint]
- **Risk**: Brute force attacks, DoS
- **Recommendation**: Implement rate limiting

##### Missing Input Validation
- **Severity**: High
- **Location**: [file/endpoint]
- **Risk**: Injection attacks
- **Recommendation**: Add validation

---

## 9. Error Handling & Logging

### Current Implementation
- **Error Display**: [production/dev mode]
- **Stack Traces**: [status]
- **Log Location**: [path]
- **Log Level**: [level]

### Findings

#### ⚠️ Issues Found

##### Information Disclosure
- **Severity**: Medium
- **Location**: [file]
- **Issue**: Stack traces visible in [production/error messages]
- **Risk**: Information leakage
- **Recommendation**: Disable debug mode in production

---

## 10. Session Security

### Current Implementation
- **Session Timeout**: [duration]
- **Cookie Security**: [flags]
- **Session Regeneration**: [status]

### Findings

#### ⚠️ Issues Found

##### Insecure Session Configuration
- **Severity**: Medium
- **Location**: [configuration file]
- **Issue**: [description]
- **Recommendation**: [secure configuration]

---

## Security Score

### Overall Assessment
- **Current Score**: [X/100]
- **Critical Issues**: [count] (P0)
- **High Issues**: [count] (P1)
- **Medium Issues**: [count] (P2)
- **Low Issues**: [count] (P3)

### Grade: [A/B/C/D/F]

---

## Remediation Plan

### Immediate Actions (P0 - Critical)
1. [ ] **SQL Injection Fix**
   - File: [file]
   - Lines: [line numbers]
   - Effort: [time estimate]
   
2. [ ] **XSS Vulnerability Fix**
   - File: [file]
   - Lines: [line numbers]
   - Effort: [time estimate]

3. [ ] **Remove Hardcoded Secrets**
   - Files: [list]
   - Effort: [time estimate]

### Short-term Actions (P1 - High)
1. [ ] **Authorization Fixes**
   - Files: [list]
   - Effort: [time estimate]

2. [ ] **Update Vulnerable Dependencies**
   - Dependencies: [list]
   - Effort: [time estimate]

### Medium-term Actions (P2 - Medium)
1. [ ] **Security Headers**
   - Effort: [time estimate]

2. [ ] **Rate Limiting**
   - Effort: [time estimate]

### Long-term Improvements (P3 - Low)
1. [ ] **Code Refactoring**
   - Effort: [time estimate]

2. [ ] **Enhanced Logging**
   - Effort: [time estimate]

---

## Testing Recommendations

### Security Test Cases
1. [ ] Authentication bypass tests
2. [ ] Authorization escalation tests
3. [ ] SQL injection tests
4. [ ] XSS tests
5. [ ] CSRF tests
6. [ ] Rate limiting tests

### Tools to Use
- **Static Analysis**: [SAST tools]
- **Dynamic Analysis**: [DAST tools]
- **Dependency Scanning**: [SCA tools]
- **Penetration Testing**: [tools/methodology]
