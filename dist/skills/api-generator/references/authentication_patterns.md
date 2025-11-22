# Authentication Patterns

## JWT (JSON Web Tokens)

### Basic Implementation

**Token Structure**
```
header.payload.signature
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

**Payload Example**
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "email": "john@example.com",
  "role": "admin",
  "iat": 1678900800,
  "exp": 1678987200
}
```

### Login Endpoint
```javascript
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "securePassword123"
}

Response: 200 OK
{
  "access_token": "eyJhbGc...",
  "refresh_token": "eyJhbGc...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Token Refresh
```javascript
POST /api/auth/refresh
Content-Type: application/json

{
  "refresh_token": "eyJhbGc..."
}

Response: 200 OK
{
  "access_token": "newAccessToken...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Protected Endpoint Usage
```javascript
GET /api/users/profile
Authorization: Bearer eyJhbGc...

Response: 200 OK
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

## OAuth 2.0 Flows

### Authorization Code Flow

**Step 1: Authorization Request**
```
GET /oauth/authorize?
  response_type=code&
  client_id=CLIENT_ID&
  redirect_uri=https://app.example.com/callback&
  scope=read write&
  state=random_state_string
```

**Step 2: User Grants Permission**
User logs in and approves access

**Step 3: Authorization Code Response**
```
GET https://app.example.com/callback?
  code=AUTHORIZATION_CODE&
  state=random_state_string
```

**Step 4: Exchange Code for Token**
```
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&
code=AUTHORIZATION_CODE&
redirect_uri=https://app.example.com/callback&
client_id=CLIENT_ID&
client_secret=CLIENT_SECRET

Response:
{
  "access_token": "ACCESS_TOKEN",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "REFRESH_TOKEN"
}
```

### Client Credentials Flow

```
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&
client_id=CLIENT_ID&
client_secret=CLIENT_SECRET&
scope=api.read

Response:
{
  "access_token": "ACCESS_TOKEN",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Password Grant (Not Recommended for New Apps)

```
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=password&
username=user@example.com&
password=userPassword&
client_id=CLIENT_ID&
client_secret=CLIENT_SECRET

Response:
{
  "access_token": "ACCESS_TOKEN",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "REFRESH_TOKEN"
}
```

## API Key Authentication

### Header-Based
```
GET /api/users
X-API-Key: your-api-key-here
```

### Query Parameter (Less Secure)
```
GET /api/users?api_key=your-api-key-here
```

### API Key Management Endpoints

**Create API Key**
```
POST /api/keys
Authorization: Bearer jwt_token

{
  "name": "Production API Key",
  "scopes": ["users.read", "orders.write"],
  "expires_at": "2025-12-31T23:59:59Z"
}

Response:
{
  "id": "key_123",
  "key": "api_key_1234567890abcdef_example_not_real",
  "name": "Production API Key",
  "scopes": ["users.read", "orders.write"],
  "created_at": "2024-03-15T10:30:00Z",
  "expires_at": "2025-12-31T23:59:59Z"
}
```

**List API Keys**
```
GET /api/keys
Authorization: Bearer jwt_token

Response:
{
  "data": [
    {
      "id": "key_123",
      "name": "Production API Key",
      "scopes": ["users.read", "orders.write"],
      "last_used_at": "2024-03-15T09:00:00Z",
      "created_at": "2024-01-01T00:00:00Z"
    }
  ]
}
```

**Revoke API Key**
```
DELETE /api/keys/key_123
Authorization: Bearer jwt_token

Response: 204 No Content
```

## Session-Based Authentication

### Login
```
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}

Response: 200 OK
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict

{
  "message": "Login successful",
  "user": {
    "id": 123,
    "name": "John Doe"
  }
}
```

### Authenticated Request
```
GET /api/users/profile
Cookie: session_id=abc123

Response: 200 OK
{
  "id": 123,
  "name": "John Doe"
}
```

### Logout
```
POST /api/auth/logout
Cookie: session_id=abc123

Response: 200 OK
Set-Cookie: session_id=; Max-Age=0

{
  "message": "Logout successful"
}
```

## Multi-Factor Authentication (MFA)

### Enable MFA
```
POST /api/auth/mfa/enable
Authorization: Bearer jwt_token

Response:
{
  "qr_code": "data:image/png;base64,...",
  "secret": "JBSWY3DPEHPK3PXP",
  "backup_codes": [
    "12345678",
    "87654321"
  ]
}
```

### Verify MFA Setup
```
POST /api/auth/mfa/verify
Authorization: Bearer jwt_token
Content-Type: application/json

{
  "code": "123456"
}

Response:
{
  "success": true,
  "message": "MFA enabled successfully"
}
```

### Login with MFA
```
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}

Response: 200 OK
{
  "requires_mfa": true,
  "temp_token": "temp_abc123"
}

# Then verify MFA
POST /api/auth/mfa/authenticate
Content-Type: application/json

{
  "temp_token": "temp_abc123",
  "code": "123456"
}

Response:
{
  "access_token": "eyJhbGc...",
  "refresh_token": "eyJhbGc...",
  "token_type": "Bearer"
}
```

## Role-Based Access Control (RBAC)

### User Roles Structure
```json
{
  "user_id": 123,
  "roles": ["admin", "moderator"],
  "permissions": [
    "users.read",
    "users.write",
    "posts.read",
    "posts.write",
    "posts.delete"
  ]
}
```

### Permission Checking Middleware
```javascript
// Check if user has specific permission
requirePermission('users.write')

// Check if user has any of the permissions
requireAnyPermission(['users.write', 'admin.users'])

// Check if user has specific role
requireRole('admin')

// Check if user has any of the roles
requireAnyRole(['admin', 'moderator'])
```

### Endpoint Protection
```javascript
// Single permission
GET /api/admin/users
Requires: admin permission

// Multiple permissions (all required)
POST /api/posts
Requires: posts.create AND users.authenticated

// Any permission (one required)
GET /api/posts/123/edit
Requires: posts.edit OR posts.admin OR site.admin
```

## Token Refresh Strategies

### Automatic Refresh
```javascript
// Client automatically refreshes token before expiry
const TOKEN_REFRESH_THRESHOLD = 5 * 60 * 1000; // 5 minutes

if (tokenExpiresIn < TOKEN_REFRESH_THRESHOLD) {
  refreshAccessToken();
}
```

### Refresh on 401
```javascript
// Client refreshes token when receiving 401
fetch('/api/users', {
  headers: {
    Authorization: `Bearer ${accessToken}`
  }
})
.then(response => {
  if (response.status === 401) {
    return refreshAccessToken().then(() => retryRequest());
  }
  return response;
});
```

### Sliding Sessions
```javascript
// Extend session on each request
app.use((req, res, next) => {
  if (req.session) {
    req.session.touch(); // Reset expiry
  }
  next();
});
```

## Security Best Practices

### Token Storage

**DO**:
- Store tokens in httpOnly cookies (for web)
- Use secure flag for cookies
- Use SameSite attribute
- Store in secure storage (mobile)
- Encrypt sensitive tokens

**DON'T**:
- Store in localStorage (XSS risk)
- Store in sessionStorage (XSS risk)
- Include in URLs
- Log tokens
- Store unencrypted on disk

### Password Security

**Hashing**:
- Use bcrypt, Argon2, or scrypt
- Use high cost factor (10-12 rounds for bcrypt)
- Add salt automatically
- Never store plain text passwords

```javascript
const bcrypt = require('bcrypt');
const SALT_ROUNDS = 12;

// Hash password
const hashedPassword = await bcrypt.hash(password, SALT_ROUNDS);

// Verify password
const isValid = await bcrypt.compare(password, hashedPassword);
```

### Token Expiry

**Access Tokens**: Short-lived (15 minutes - 1 hour)
**Refresh Tokens**: Longer-lived (days to weeks)
**API Keys**: Long-lived or never expire (revocable)

### Rate Limiting

**Login Attempts**:
```javascript
// Limit to 5 attempts per 15 minutes
app.use('/api/auth/login', rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5,
  message: 'Too many login attempts'
}));
```

**API Requests**:
```javascript
// Limit to 100 requests per hour
app.use('/api', rateLimit({
  windowMs: 60 * 60 * 1000,
  max: 100,
  message: 'Rate limit exceeded'
}));
```

### HTTPS Only

Always require HTTPS in production:
```javascript
app.use((req, res, next) => {
  if (req.header('x-forwarded-proto') !== 'https' && process.env.NODE_ENV === 'production') {
    res.redirect(`https://${req.header('host')}${req.url}`);
  } else {
    next();
  }
});
```

### Security Headers

```javascript
// Helmet.js configuration
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"]
    }
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  }
}));
```

## Common Authentication Errors

### 401 Unauthorized
```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Authentication required"
  }
}
```

### 403 Forbidden
```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "Insufficient permissions"
  }
}
```

### Invalid Token
```json
{
  "error": {
    "code": "INVALID_TOKEN",
    "message": "Token is invalid or expired"
  }
}
```

### Invalid Credentials
```json
{
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Email or password is incorrect"
  }
}
```
