# API Design Principles

## RESTful API Design

### Resource Naming Conventions

**Use Nouns, Not Verbs**
```
✅ Good:
GET /api/users
POST /api/products
GET /api/orders/123

❌ Bad:
GET /api/getUsers
POST /api/createProduct
GET /api/fetchOrder/123
```

**Use Plural Nouns for Collections**
```
✅ Good:
GET /api/users
GET /api/products

❌ Bad:
GET /api/user
GET /api/product
```

**Use Hierarchical URLs for Relationships**
```
✅ Good:
GET /api/users/123/orders
GET /api/products/456/reviews
POST /api/orders/789/items

❌ Bad:
GET /api/orders?userId=123
GET /api/reviews?productId=456
```

### HTTP Methods Usage

**GET - Retrieve Resources**
- Idempotent and safe
- No side effects
- Cacheable
```
GET /api/users           # List all users
GET /api/users/123       # Get specific user
GET /api/users/123/orders # Get user's orders
```

**POST - Create Resources**
- Not idempotent
- Creates new resource
- Returns 201 Created with Location header
```
POST /api/users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}

Response: 201 Created
Location: /api/users/124
```

**PUT - Update/Replace Resources**
- Idempotent
- Replaces entire resource
- Returns 200 OK or 204 No Content
```
PUT /api/users/123
Content-Type: application/json

{
  "id": 123,
  "name": "John Updated",
  "email": "john.new@example.com"
}
```

**PATCH - Partial Update**
- Partially modifies resource
- Only send changed fields
- Returns 200 OK
```
PATCH /api/users/123
Content-Type: application/json

{
  "email": "john.new@example.com"
}
```

**DELETE - Remove Resources**
- Idempotent
- Removes resource
- Returns 204 No Content or 200 OK
```
DELETE /api/users/123

Response: 204 No Content
```

### HTTP Status Codes

**2xx Success**
- `200 OK` - Successful GET, PUT, PATCH, DELETE
- `201 Created` - Successful POST creating resource
- `202 Accepted` - Request accepted, processing async
- `204 No Content` - Successful but no content to return

**3xx Redirection**
- `301 Moved Permanently` - Resource permanently moved
- `304 Not Modified` - Cached version is still valid

**4xx Client Errors**
- `400 Bad Request` - Invalid request syntax
- `401 Unauthorized` - Authentication required
- `403 Forbidden` - Authenticated but not authorized
- `404 Not Found` - Resource doesn't exist
- `405 Method Not Allowed` - HTTP method not supported
- `409 Conflict` - Conflict with current state
- `422 Unprocessable Entity` - Validation errors
- `429 Too Many Requests` - Rate limit exceeded

**5xx Server Errors**
- `500 Internal Server Error` - Generic server error
- `502 Bad Gateway` - Invalid upstream response
- `503 Service Unavailable` - Server temporarily down
- `504 Gateway Timeout` - Upstream timeout

### Pagination Strategies

**Offset-Based Pagination**
```
GET /api/users?limit=20&offset=40

Response:
{
  "data": [...],
  "pagination": {
    "limit": 20,
    "offset": 40,
    "total": 150
  }
}
```

**Cursor-Based Pagination** (Recommended for large datasets)
```
GET /api/users?limit=20&cursor=eyJpZCI6MTIzfQ==

Response:
{
  "data": [...],
  "pagination": {
    "next_cursor": "eyJpZCI6MTQzfQ==",
    "has_more": true
  }
}
```

**Page-Based Pagination**
```
GET /api/users?page=3&per_page=20

Response:
{
  "data": [...],
  "pagination": {
    "page": 3,
    "per_page": 20,
    "total_pages": 8,
    "total_items": 150
  }
}
```

### Filtering, Sorting, and Field Selection

**Filtering**
```
GET /api/products?category=electronics&price_min=100&price_max=500
GET /api/users?status=active&role=admin
GET /api/orders?created_after=2024-01-01&status=pending
```

**Sorting**
```
GET /api/products?sort=price          # Ascending
GET /api/products?sort=-price         # Descending
GET /api/products?sort=category,-price # Multiple fields
```

**Field Selection** (Sparse Fieldsets)
```
GET /api/users?fields=id,name,email
GET /api/products?fields=id,name,price,images
```

**Include Related Resources**
```
GET /api/users/123?include=orders,profile
GET /api/products/456?include=reviews,category
```

### Versioning Strategies

**URL Versioning** (Most Common)
```
/api/v1/users
/api/v2/users
```
Pros: Clear, easy to route
Cons: URL changes

**Header Versioning**
```
GET /api/users
API-Version: 2.0
```
Pros: Clean URLs
Cons: Less visible

**Content Negotiation**
```
GET /api/users
Accept: application/vnd.myapi.v2+json
```
Pros: RESTful
Cons: Complex

**Query Parameter**
```
GET /api/users?version=2
```
Pros: Simple
Cons: Can be missed in routing

### Response Structure

**Successful Response**
```json
{
  "data": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com"
  },
  "meta": {
    "timestamp": "2024-03-15T10:30:00Z"
  }
}
```

**Collection Response**
```json
{
  "data": [
    { "id": 1, "name": "User 1" },
    { "id": 2, "name": "User 2" }
  ],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 150
  },
  "meta": {
    "timestamp": "2024-03-15T10:30:00Z"
  }
}
```

**Error Response**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ],
    "timestamp": "2024-03-15T10:30:00Z",
    "request_id": "abc-123"
  }
}
```

## GraphQL API Design

### Schema Design Principles

**Type Definitions**
```graphql
type User {
  id: ID!
  name: String!
  email: String!
  createdAt: DateTime!
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  content: String!
  author: User!
  comments: [Comment!]!
}

type Comment {
  id: ID!
  text: String!
  author: User!
  post: Post!
}
```

**Query Design**
```graphql
type Query {
  # Singular queries
  user(id: ID!): User
  post(id: ID!): Post

  # List queries with pagination
  users(
    limit: Int = 20
    offset: Int = 0
    filter: UserFilter
    sort: UserSort
  ): UserConnection!

  posts(
    first: Int
    after: String
    filter: PostFilter
  ): PostConnection!
}

type UserConnection {
  edges: [UserEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}

type UserEdge {
  node: User!
  cursor: String!
}

type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}
```

**Mutation Design**
```graphql
type Mutation {
  createUser(input: CreateUserInput!): CreateUserPayload!
  updateUser(input: UpdateUserInput!): UpdateUserPayload!
  deleteUser(id: ID!): DeleteUserPayload!
}

input CreateUserInput {
  name: String!
  email: String!
  password: String!
}

type CreateUserPayload {
  user: User
  errors: [Error!]
  success: Boolean!
}

type Error {
  field: String
  message: String!
  code: String!
}
```

**Subscription Design**
```graphql
type Subscription {
  userCreated: User!
  postUpdated(postId: ID!): Post!
  commentAdded(postId: ID!): Comment!
}
```

### Naming Conventions

**Fields**: camelCase
```graphql
type User {
  firstName: String
  lastName: String
  emailAddress: String
}
```

**Types**: PascalCase
```graphql
type UserProfile { }
type PostComment { }
```

**Enums**: UPPER_CASE
```graphql
enum UserRole {
  ADMIN
  MODERATOR
  USER
  GUEST
}
```

**Input Types**: suffix with "Input"
```graphql
input CreateUserInput { }
input UpdatePostInput { }
```

**Payload Types**: suffix with "Payload"
```graphql
type CreateUserPayload { }
type UpdatePostPayload { }
```

## API Security Best Practices

### Authentication Patterns

**JWT (JSON Web Tokens)**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**API Keys**
```
X-API-Key: your-api-key-here
```

**OAuth 2.0**
```
Authorization: Bearer {access_token}
```

### Input Validation

**Always Validate**:
- Data types
- Required fields
- String lengths
- Number ranges
- Email formats
- URL formats
- Date formats
- Enum values

**Sanitize Input**:
- Remove HTML tags
- Escape special characters
- Prevent SQL injection
- Prevent XSS attacks
- Validate file uploads

### Rate Limiting

**Response Headers**
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1678900800
```

**Rate Limit Exceeded Response**
```
HTTP/1.1 429 Too Many Requests
Retry-After: 3600

{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Try again later."
  }
}
```

### CORS Configuration

```javascript
{
  origin: ['https://example.com'],
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  exposedHeaders: ['X-Total-Count'],
  credentials: true,
  maxAge: 3600
}
```

## Performance Optimization

### Caching Strategies

**HTTP Caching Headers**
```
Cache-Control: public, max-age=3600
ETag: "abc123"
Last-Modified: Wed, 15 Mar 2024 10:30:00 GMT
```

**Conditional Requests**
```
GET /api/users/123
If-None-Match: "abc123"

Response: 304 Not Modified
```

### Compression

**Enable gzip/brotli**
```
Accept-Encoding: gzip, deflate, br
Content-Encoding: gzip
```

### Database Optimization

- Use database indexes
- Implement query result caching
- Use connection pooling
- Avoid N+1 queries
- Use pagination
- Implement field selection

### Response Optimization

- Only return requested fields
- Use compression
- Implement caching
- Optimize JSON serialization
- Use CDN for static assets

## Documentation Best Practices

### OpenAPI/Swagger

- Document all endpoints
- Include request/response examples
- Document all parameters
- Specify required vs optional fields
- Include error responses
- Add descriptions and summaries
- Version your documentation
- Keep it up-to-date

### Code Examples

Provide examples in multiple languages:
- JavaScript/TypeScript
- Python
- PHP
- Ruby
- Java
- Go
- C#

### Changelog

Maintain version changelog:
- Breaking changes
- New features
- Bug fixes
- Deprecations
- Migration guides
