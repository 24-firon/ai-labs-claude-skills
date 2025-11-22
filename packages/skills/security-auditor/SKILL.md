---
name: security-auditor
description: This skill should be used when the user requests security audits, vulnerability scanning, OWASP Top 10 checks, code security analysis, penetration testing guidance, or security best practices implementation. It provides comprehensive security assessment frameworks and remediation strategies.
---

# Security Auditor

## Overview

This skill enables comprehensive security auditing and vulnerability assessment for applications. It analyzes code for common security vulnerabilities including OWASP Top 10, implements security best practices, and provides actionable remediation strategies. The skill covers web applications, APIs, mobile apps, and infrastructure security.

## When to Use This Skill

Use this skill when the user requests:
- Security audit or vulnerability assessment
- OWASP Top 10 vulnerability checking
- Code security analysis
- Penetration testing preparation
- Security best practices implementation
- Dependency vulnerability scanning
- Authentication/authorization security review
- Data protection and encryption audit
- API security assessment
- Infrastructure security review

## Core Workflow

### Step 1: Determine Audit Scope

Identify what type of security audit is needed:

**A. Full Application Security Audit**
- Comprehensive security review
- All OWASP Top 10 categories
- Infrastructure and code review
- Output: Complete security assessment report

**B. Code Security Review**
- Focus on application code
- Common vulnerability patterns
- Secure coding practices
- Output: Code security findings report

**C. API Security Audit**
- API endpoint security
- Authentication/authorization
- Input validation and sanitization
- Output: API security assessment

**D. Infrastructure Security Review**
- Server configuration
- Network security
- Container/cloud security
- Output: Infrastructure hardening report

**E. Dependency Security Scan**
- Third-party library vulnerabilities
- Outdated dependencies
- License compliance
- Output: Dependency security report

### Step 2: OWASP Top 10 Security Checks

Systematically check for OWASP Top 10 vulnerabilities using `references/owasp_top_10.md`:

**1. Broken Access Control**
- Missing authorization checks
- Insecure direct object references (IDOR)
- Path traversal vulnerabilities
- Missing function-level access control

**2. Cryptographic Failures**
- Weak encryption algorithms
- Hardcoded secrets
- Insecure random number generation
- Missing encryption for sensitive data

**3. Injection**
- SQL injection
- NoSQL injection
- Command injection
- LDAP injection
- XPath injection

**4. Insecure Design**
- Missing rate limiting
- Lack of security requirements
- Insecure architecture patterns
- Missing threat modeling

**5. Security Misconfiguration**
- Default credentials
- Unnecessary features enabled
- Verbose error messages
- Missing security headers
- Outdated software

**6. Vulnerable and Outdated Components**
- Outdated dependencies
- Known CVEs in libraries
- Unsupported software versions

**7. Identification and Authentication Failures**
- Weak password policies
- Missing multi-factor authentication
- Session fixation
- Credential stuffing vulnerabilities

**8. Software and Data Integrity Failures**
- Insecure deserialization
- Missing integrity checks
- Untrusted CI/CD pipeline
- Auto-update without verification

**9. Security Logging and Monitoring Failures**
- Insufficient logging
- Missing security event detection
- No alerting mechanisms
- Logs not protected

**10. Server-Side Request Forgery (SSRF)**
- Unvalidated URL redirects
- Missing URL validation
- Network access from user input

### Step 3: Code Security Analysis

Analyze codebase for security vulnerabilities:

**Input Validation**
- Missing validation
- Inadequate sanitization
- Type confusion
- Length/range validation

**Output Encoding**
- XSS vulnerabilities
- Missing HTML encoding
- JavaScript injection
- CSS injection

**Authentication Security**
- Password storage (bcrypt, Argon2)
- Session management
- Token security
- Logout functionality

**Authorization Security**
- Role-based access control
- Permission checks
- Resource ownership validation
- Privilege escalation risks

**Data Protection**
- Sensitive data exposure
- Encryption at rest
- Encryption in transit
- Secure data deletion

**Error Handling**
- Information disclosure
- Stack trace exposure
- Verbose error messages
- Graceful degradation

### Step 4: API Security Assessment

Evaluate API security using `references/api_security_checklist.md`:

**Authentication**
- Token security (JWT)
- API key management
- OAuth implementation
- Token expiration

**Authorization**
- Endpoint protection
- Resource-level authorization
- Role validation
- Scope checking

**Input Validation**
- Request validation
- Parameter sanitization
- File upload security
- Content-Type validation

**Rate Limiting**
- Request throttling
- IP-based limiting
- User quotas
- DDoS protection

**API Security Headers**
- CORS configuration
- Content-Security-Policy
- X-Frame-Options
- X-Content-Type-Options

### Step 5: Infrastructure Security Review

Assess infrastructure security:

**Server Security**
- OS hardening
- Firewall configuration
- SSH key management
- Unnecessary services disabled

**Network Security**
- Network segmentation
- TLS/SSL configuration
- Certificate management
- VPN security

**Container Security**
- Image vulnerabilities
- Container isolation
- Secrets management
- Resource limits

**Cloud Security**
- IAM configuration
- Storage bucket permissions
- Security groups
- Encryption settings

### Step 6: Generate Security Report

Create comprehensive security report using `assets/templates/security_audit_report.md`:

**Report Sections**:
1. Executive Summary
2. Vulnerability Summary (by severity)
3. Detailed Findings
4. Risk Assessment
5. Remediation Recommendations
6. Compliance Status
7. Action Plan

**Severity Levels**:
- **Critical**: Immediate action required
- **High**: Fix within 7 days
- **Medium**: Fix within 30 days
- **Low**: Fix when possible
- **Informational**: Best practice recommendations

**File Naming**:
- `security-audit-[project]-[date].md`
- `vulnerability-report-[date].md`
- `penetration-test-findings-[date].md`

### Step 7: Remediation Guidance

Provide specific remediation steps for each vulnerability:

**For Each Finding**:
1. Vulnerability description
2. Affected code/configuration
3. Security impact
4. Proof of concept (if applicable)
5. Remediation steps
6. Code examples (before/after)
7. Verification method
8. Prevention strategies

## Advanced Features

### Automated Vulnerability Scanning

**Tools to Recommend**:
- **SAST**: SonarQube, Semgrep, Bandit
- **DAST**: OWASP ZAP, Burp Suite
- **Dependency Scanning**: Snyk, npm audit, OWASP Dependency-Check
- **Container Scanning**: Trivy, Clair, Anchore
- **Secret Scanning**: TruffleHog, GitLeaks

### Security Testing Scenarios

**SQL Injection Testing**:
```sql
' OR '1'='1
' UNION SELECT NULL--
'; DROP TABLE users--
```

**XSS Testing**:
```javascript
<script>alert('XSS')</script>
<img src=x onerror=alert('XSS')>
javascript:alert('XSS')
```

**Authentication Testing**:
- Brute force attempts
- Session fixation
- Token manipulation
- Password reset flaws

**Authorization Testing**:
- Horizontal privilege escalation
- Vertical privilege escalation
- IDOR vulnerabilities
- Missing function-level access control

### Compliance Checks

**GDPR Compliance**:
- Data minimization
- Consent management
- Right to be forgotten
- Data portability
- Breach notification

**PCI DSS** (if handling payments):
- Cardholder data protection
- Encryption requirements
- Access control
- Network security

**HIPAA** (if handling health data):
- PHI protection
- Access controls
- Audit logging
- Encryption requirements

## Usage Examples

### Example 1: Full Web Application Security Audit

**User Request**: "Perform a complete security audit of our Node.js e-commerce application."

**Execution**:
1. Review application architecture
2. Check all OWASP Top 10 vulnerabilities
3. Analyze authentication/authorization code
4. Review payment processing security
5. Check for hardcoded secrets
6. Scan dependencies for vulnerabilities
7. Review API endpoint security
8. Test input validation
9. Check security headers
10. Generate comprehensive report with findings
11. Provide remediation plan with priorities
12. Save as `security-audit-ecommerce-2024-03-15.md`

### Example 2: API Security Assessment

**User Request**: "Audit the security of our REST API."

**Execution**:
1. Review API authentication (JWT implementation)
2. Check authorization on all endpoints
3. Test input validation and sanitization
4. Verify rate limiting implementation
5. Check for injection vulnerabilities
6. Review error handling
7. Assess CORS configuration
8. Check security headers
9. Test for IDOR vulnerabilities
10. Generate API security report
11. Save as `api-security-audit-2024-03-15.md`

### Example 3: Dependency Vulnerability Scan

**User Request**: "Check our project for vulnerable dependencies."

**Execution**:
1. Run npm audit or equivalent
2. Identify all vulnerable packages
3. Check for known CVEs
4. Assess severity of vulnerabilities
5. Check for available patches
6. Review transitive dependencies
7. Create remediation plan
8. Provide update commands
9. Save as `dependency-security-report-2024-03-15.md`

## Reference Files

### `references/owasp_top_10.md`
Comprehensive OWASP Top 10 guide:
- Detailed vulnerability descriptions
- Real-world examples
- Detection methods
- Remediation strategies
- Prevention techniques
- Testing approaches

**When to load**: For any security audit or vulnerability assessment.

### `references/api_security_checklist.md`
API security best practices:
- Authentication patterns
- Authorization checks
- Input validation
- Rate limiting
- Security headers
- Testing methods

**When to load**: When auditing API security.

### `references/secure_coding_practices.md`
Secure coding guidelines:
- Input validation patterns
- Output encoding
- Authentication best practices
- Cryptography usage
- Error handling
- Logging security

**When to load**: When reviewing code security.

## Asset Templates

### `assets/templates/security_audit_report.md`
Professional security audit report template:
- Executive summary
- Findings by severity
- Detailed vulnerability descriptions
- Remediation recommendations
- Compliance status
- Action plan with timeline

### `assets/templates/vulnerability_disclosure.md`
Responsible disclosure template:
- Vulnerability details
- Affected versions
- Impact assessment
- Proof of concept
- Remediation
- Timeline

## Best Practices

### Security Testing
- Test in isolated environment
- Never test on production without permission
- Document all findings
- Provide proof of concepts carefully
- Verify fixes after remediation

### Reporting
- Prioritize by risk, not just severity
- Provide actionable recommendations
- Include code examples
- Explain business impact
- Set realistic timelines

### Remediation
- Fix critical issues immediately
- Address root causes, not just symptoms
- Implement defense in depth
- Add security tests
- Document security decisions

## Common Security Anti-Patterns

### Authentication
- Storing passwords in plain text
- Using weak hashing algorithms (MD5, SHA1)
- Missing rate limiting on login
- Predictable password reset tokens
- Session tokens in URLs

### Authorization
- Trusting client-side validation
- Missing server-side permission checks
- Relying on security through obscurity
- Inadequate separation of privileges

### Data Protection
- Storing sensitive data unencrypted
- Logging sensitive information
- Transmitting data over HTTP
- Inadequate key management
- Missing data sanitization

### Configuration
- Using default credentials
- Exposing .env files
- Verbose error messages
- Unnecessary features enabled
- Missing security headers

## Security Tools Integration

### CI/CD Pipeline
```yaml
# Example security scanning in CI/CD
security-scan:
  - npm audit --audit-level=high
  - snyk test
  - semgrep --config=auto
  - trivy image myapp:latest
```

### Pre-commit Hooks
```bash
# Prevent committing secrets
git secrets --scan
truffleHog --regex --entropy
```

### Automated Monitoring
- Security event logging
- Intrusion detection
- Anomaly detection
- Vulnerability tracking
- Compliance monitoring
