# OWASP Top 10 2021 Security Risks

## A01:2021 – Broken Access Control

### Description
Restrictions on what authenticated users are allowed to do are not properly enforced. Attackers can exploit these flaws to access unauthorized functionality and/or data.

### Common Vulnerabilities
- Bypassing access control checks by modifying the URL, internal application state, or HTML page
- Allowing the primary key to be changed to another user's record (IDOR)
- Accessing API with missing access controls for POST, PUT and DELETE
- Elevation of privilege (acting as a user without being logged in or acting as an admin when logged in as a user)
- Metadata manipulation, such as replaying or tampering with JWT tokens
- CORS misconfiguration allowing unauthorized API access

### Examples
```javascript
// ❌ Insecure - No authorization check
app.get('/api/users/:id', (req, res) => {
  const user = await User.findById(req.params.id);
  res.json(user);
});

// ✅ Secure - Verify user owns the resource
app.get('/api/users/:id', authenticateToken, (req, res) => {
  if (req.user.id !== req.params.id && !req.user.isAdmin) {
    return res.status(403).json({ error: 'Forbidden' });
  }
  const user = await User.findById(req.params.id);
  res.json(user);
});
```

### Prevention
- Implement access control mechanisms and reuse them throughout the application
- Model access controls to enforce record ownership
- Deny by default
- Implement rate limiting for API and controller access
- Log access control failures and alert admins when appropriate
- Invalidate JWT tokens after logout

## A02:2021 – Cryptographic Failures

### Description
Failures related to cryptography (or lack thereof) which often lead to exposure of sensitive data.

### Common Vulnerabilities
- Transmitting data in clear text (HTTP, SMTP, FTP)
- Using old or weak cryptographic algorithms
- Using default crypto keys or weak keys
- Missing encryption of sensitive data
- Not enforcing encryption (e.g., missing security directives or headers)
- Not using authenticated encryption

### Examples
```javascript
// ❌ Insecure - Plain text password storage
const user = {
  password: req.body.password
};

// ✅ Secure - Hashed password
const bcrypt = require('bcrypt');
const saltRounds = 12;
const hashedPassword = await bcrypt.hash(req.body.password, saltRounds);

// ❌ Insecure - Weak encryption
const crypto = require('crypto');
const cipher = crypto.createCipher('aes192', 'password'); // Deprecated

// ✅ Secure - Strong encryption with IV
const algorithm = 'aes-256-gcm';
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);
const cipher = crypto.createCipheriv(algorithm, key, iv);
```

### Prevention
- Classify data processed, stored, or transmitted
- Encrypt all sensitive data at rest
- Enforce encryption in transit (TLS with perfect forward secrecy)
- Disable caching for sensitive data
- Use strong adaptive and salted hashing functions (Argon2, scrypt, bcrypt, PBKDF2)
- Use authenticated encryption rather than just encryption
- Store keys securely and implement proper key management

## A03:2021 – Injection

### Description
Hostile data is used within a query or command, which can lead to unintended command execution.

### Common Vulnerabilities
- SQL Injection
- NoSQL Injection
- OS Command Injection
- LDAP Injection
- Expression Language (EL) Injection

### Examples

**SQL Injection**:
```javascript
// ❌ Insecure - SQL Injection vulnerability
const query = `SELECT * FROM users WHERE id = ${req.params.id}`;

// ✅ Secure - Parameterized query
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [req.params.id]);

// Or with ORM
const user = await User.findByPk(req.params.id);
```

**NoSQL Injection**:
```javascript
// ❌ Insecure - NoSQL Injection
db.users.find({ username: req.body.username, password: req.body.password });
// Attack: { username: 'admin', password: { $ne: null } }

// ✅ Secure - Proper validation
const username = String(req.body.username);
const password = String(req.body.password);
db.users.find({ username, password });
```

**Command Injection**:
```javascript
// ❌ Insecure - Command Injection
const { exec } = require('child_process');
exec(`ping ${req.query.ip}`);

// ✅ Secure - Input validation and allowlist
const ip = req.query.ip;
if (!/^(\d{1,3}\.){3}\d{1,3}$/.test(ip)) {
  return res.status(400).json({ error: 'Invalid IP' });
}
exec(`ping ${ip}`);
```

### Prevention
- Use parameterized queries or prepared statements
- Use ORM frameworks
- Validate input using positive/allowlist validation
- Escape special characters
- Use LIMIT and other SQL controls within queries
- Never concatenate user input directly into commands

## A04:2021 – Insecure Design

### Description
Missing or ineffective control design, representing different weaknesses in security requirements and architecture.

### Common Issues
- Lack of threat modeling
- Missing security requirements
- Insecure design patterns
- Lack of security principles (defense in depth, least privilege)

### Examples
```javascript
// ❌ Insecure Design - No rate limiting on password reset
app.post('/api/auth/reset-password', async (req, res) => {
  await sendPasswordResetEmail(req.body.email);
  res.json({ message: 'Reset email sent' });
});

// ✅ Secure Design - Rate limiting implemented
const rateLimit = require('express-rate-limit');

const resetLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 3, // 3 requests per window
  message: 'Too many reset requests'
});

app.post('/api/auth/reset-password', resetLimiter, async (req, res) => {
  await sendPasswordResetEmail(req.body.email);
  res.json({ message: 'If the email exists, a reset link was sent' });
});
```

### Prevention
- Establish and use a secure development lifecycle
- Establish and use a library of secure design patterns
- Use threat modeling for critical authentication, access control, business logic, and key flows
- Integrate security language and controls into user stories
- Integrate plausibility checks at each tier
- Write unit and integration tests to validate critical flows
- Limit resource consumption by user or service

## A05:2021 – Security Misconfiguration

### Description
Security misconfiguration is the most commonly seen issue, often resulting from insecure default configurations, incomplete configurations, or misconfigured HTTP headers.

### Common Issues
- Missing security hardening
- Unnecessary features enabled
- Default accounts and passwords
- Overly informative error messages
- Missing or misconfigured security headers
- Out-of-date software

### Examples
```javascript
// ❌ Insecure - Verbose error messages
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.stack });
});

// ✅ Secure - Generic error messages
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Internal server error' });
});

// ✅ Secure - Security headers
const helmet = require('helmet');
app.use(helmet());

// ✅ Secure - HTTPS enforcement
app.use((req, res, next) => {
  if (req.header('x-forwarded-proto') !== 'https' && process.env.NODE_ENV === 'production') {
    res.redirect(`https://${req.header('host')}${req.url}`);
  } else {
    next();
  }
});
```

### Prevention
- Implement a repeatable hardening process
- Remove unused features and frameworks
- Review and update configurations
- Implement segmented application architecture
- Send security directives to clients (security headers)
- Automate verification of configurations

## A06:2021 – Vulnerable and Outdated Components

### Description
Using components with known vulnerabilities can undermine application defenses and enable attacks.

### Common Issues
- Not knowing versions of all components used
- Software is vulnerable, unsupported, or out of date
- Not scanning for vulnerabilities regularly
- Not fixing or upgrading dependencies in a timely fashion

### Detection
```bash
# Check for vulnerabilities
npm audit
npm audit --audit-level=high

# Update dependencies
npm update
npm outdated

# Use automated tools
snyk test
```

### Prevention
- Remove unused dependencies, features, components, files
- Continuously inventory versions of client-side and server-side components
- Monitor sources like CVE and NVD for vulnerabilities
- Only obtain components from official sources over secure links
- Monitor for unmaintained libraries and components
- Implement a patch management process

## A07:2021 – Identification and Authentication Failures

### Description
Confirmation of the user's identity, authentication, and session management is critical to protect against authentication-related attacks.

### Common Issues
- Permits automated attacks (credential stuffing, brute force)
- Permits default, weak, or well-known passwords
- Uses weak credential recovery processes
- Uses plain text, encrypted, or weakly hashed passwords
- Missing or ineffective multi-factor authentication
- Exposes session IDs in the URL
- Does not properly invalidate session IDs

### Examples
```javascript
// ❌ Weak password policy
if (password.length < 6) {
  return res.status(400).json({ error: 'Password too short' });
}

// ✅ Strong password policy
const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{12,}$/;
if (!passwordRegex.test(password)) {
  return res.status(400).json({
    error: 'Password must be at least 12 characters with uppercase, lowercase, number and special character'
  });
}

// ❌ Session in URL
res.redirect(`/dashboard?session=${sessionId}`);

// ✅ Session in HttpOnly cookie
res.cookie('sessionId', sessionId, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict',
  maxAge: 3600000
});
```

### Prevention
- Implement multi-factor authentication
- Do not ship or deploy with default credentials
- Implement weak password checks
- Align password length, complexity, and rotation policies with NIST 800-63b
- Harden against account enumeration
- Limit or increasingly delay failed login attempts
- Use a server-side, secure session manager
- Session IDs should not be in the URL
- Invalidate session IDs after logout

## A08:2021 – Software and Data Integrity Failures

### Description
Software and data integrity failures relate to code and infrastructure that does not protect against integrity violations.

### Common Issues
- Insecure deserialization
- Auto-update without verification
- CI/CD pipeline without integrity verification
- Relying on libraries from untrusted sources

### Examples
```javascript
// ❌ Insecure deserialization
const userData = JSON.parse(req.cookies.user);
// If user modifies cookie, can inject malicious data

// ✅ Secure - Verify integrity
const crypto = require('crypto');
const [data, signature] = req.cookies.user.split('.');
const expectedSignature = crypto
  .createHmac('sha256', SECRET_KEY)
  .update(data)
  .digest('hex');

if (signature !== expectedSignature) {
  return res.status(400).json({ error: 'Invalid signature' });
}
```

### Prevention
- Use digital signatures to verify software
- Ensure libraries and dependencies from trusted repositories
- Use software supply chain security tools
- Ensure CI/CD pipeline has proper configuration and access control
- Ensure unsigned or unencrypted serialized data is not sent to untrusted clients

## A09:2021 – Security Logging and Monitoring Failures

### Description
Without logging and monitoring, breaches cannot be detected and responded to effectively.

### Common Issues
- Insufficient logging
- Logs not monitored for suspicious activity
- Logs only stored locally
- Alerting thresholds and response escalation not in place
- Penetration testing does not trigger alerts

### Examples
```javascript
// ❌ No security logging
app.post('/api/login', async (req, res) => {
  const user = await authenticate(req.body);
  res.json({ token: generateToken(user) });
});

// ✅ Proper security logging
const winston = require('winston');
const logger = winston.createLogger({
  transports: [new winston.transports.File({ filename: 'security.log' })]
});

app.post('/api/login', async (req, res) => {
  try {
    const user = await authenticate(req.body);
    logger.info('Successful login', { userId: user.id, ip: req.ip });
    res.json({ token: generateToken(user) });
  } catch (error) {
    logger.warn('Failed login attempt', { email: req.body.email, ip: req.ip });
    res.status(401).json({ error: 'Invalid credentials' });
  }
});
```

### Prevention
- Log all login, access control, and server-side input validation failures
- Ensure logs are in a format that log management solutions can consume
- Ensure log data is encoded correctly
- Ensure high-value transactions have audit trails
- Establish effective monitoring and alerting
- Establish an incident response and recovery plan

## A10:2021 – Server-Side Request Forgery (SSRF)

### Description
SSRF flaws occur when a web application fetches a remote resource without validating the user-supplied URL.

### Common Issues
- Fetching URLs from user input without validation
- Allowing access to internal services
- Not implementing network segmentation
- Lack of allowlist for remote resources

### Examples
```javascript
// ❌ SSRF Vulnerability
app.get('/fetch', async (req, res) => {
  const url = req.query.url;
  const response = await fetch(url);
  res.send(await response.text());
});
// Attack: /fetch?url=http://localhost:8080/admin

// ✅ Secure - URL validation and allowlist
const allowedDomains = ['api.example.com', 'cdn.example.com'];

app.get('/fetch', async (req, res) => {
  const url = new URL(req.query.url);

  if (!allowedDomains.includes(url.hostname)) {
    return res.status(400).json({ error: 'Domain not allowed' });
  }

  if (url.protocol !== 'https:') {
    return res.status(400).json({ error: 'Only HTTPS allowed' });
  }

  const response = await fetch(url.toString());
  res.send(await response.text());
});
```

### Prevention
- Sanitize and validate all client-supplied input data
- Enforce URL schema, port, and destination with positive allowlist
- Do not send raw responses to clients
- Disable HTTP redirections
- Implement network segmentation to separate resource access
