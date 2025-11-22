---
name: api-generator
description: This skill should be used when the user requests API development, REST/GraphQL API generation, OpenAPI/Swagger documentation, API endpoint creation, or backend service scaffolding. It provides comprehensive frameworks for designing, implementing, and documenting APIs with industry best practices.
---

# API Generator

## Overview

This skill enables comprehensive API development and documentation. It generates REST and GraphQL APIs with proper structure, validation, authentication, error handling, and complete OpenAPI/Swagger documentation. The skill follows industry standards including RESTful principles, GraphQL best practices, and OpenAPI 3.0 specification.

## When to Use This Skill

Use this skill when the user requests:
- REST API development or generation
- GraphQL API creation
- OpenAPI/Swagger documentation
- API endpoint scaffolding
- Backend service structure
- API authentication setup
- API rate limiting and security
- API versioning strategy
- Microservices API design
- API testing and validation

## Core Workflow

### Step 1: Determine API Type and Requirements

Identify what type of API work is needed:

**A. REST API Development**
- Traditional RESTful services
- CRUD operations
- Resource-based architecture
- Output: REST API with OpenAPI docs

**B. GraphQL API Development**
- Query and mutation based API
- Type-safe schema
- Flexible data fetching
- Output: GraphQL schema with resolvers

**C. Hybrid Approach**
- REST for simple operations
- GraphQL for complex queries
- Best of both worlds
- Output: Combined API architecture

**D. API Documentation Only**
- Document existing API
- Generate OpenAPI spec
- Create API client SDKs
- Output: Complete API documentation

### Step 2: Gather Requirements

Collect essential information for API design:

**Essential Information**:
- API purpose and business domain
- Expected endpoints/operations
- Data models and relationships
- Authentication requirements
- Performance requirements
- Scalability needs

**Technical Requirements**:
- Programming language/framework
- Database type
- Hosting environment
- Rate limiting needs
- Caching strategy
- API versioning approach

**Security Requirements**:
- Authentication method (JWT, OAuth2, API Keys)
- Authorization levels
- Data encryption needs
- CORS policies
- Rate limiting rules

**Documentation Needs**:
- OpenAPI/Swagger specification
- API client examples
- Integration guides
- Error response documentation

### Step 3: Design API Architecture

Design the API structure using `references/api_design_principles.md`.

**REST API Design**:
1. Define resources and collections
2. Map HTTP methods to operations
3. Design URL structure and naming
4. Plan status codes and responses
5. Define request/response schemas
6. Design error handling

**GraphQL API Design**:
1. Define types and interfaces
2. Design queries and mutations
3. Plan subscriptions (if needed)
4. Define resolvers structure
5. Design input types and arguments
6. Plan error handling and validation

**Common Considerations**:
- Pagination strategy (offset, cursor-based)
- Filtering and sorting
- Field selection and projection
- Versioning approach (URL, header, content negotiation)
- Rate limiting and throttling
- Caching headers

### Step 4: Generate API Implementation

Create the API implementation using templates from `assets/templates/`.

**REST API Structure**:
```
api/
├── routes/
│   ├── users.js
│   ├── products.js
│   └── orders.js
├── controllers/
│   ├── userController.js
│   ├── productController.js
│   └── orderController.js
├── models/
│   ├── User.js
│   ├── Product.js
│   └── Order.js
├── middleware/
│   ├── auth.js
│   ├── validation.js
│   └── errorHandler.js
├── utils/
│   ├── logger.js
│   └── response.js
└── app.js
```

**GraphQL API Structure**:
```
api/
├── schema/
│   ├── types/
│   ├── queries/
│   ├── mutations/
│   └── schema.graphql
├── resolvers/
│   ├── userResolvers.js
│   ├── productResolvers.js
│   └── orderResolvers.js
├── models/
├── middleware/
└── server.js
```

### Step 5: Implement Security & Validation

Add security layers and validation:

**Authentication**:
- JWT token generation and validation
- OAuth2 flows
- API key management
- Session handling

**Authorization**:
- Role-based access control (RBAC)
- Permission checking middleware
- Resource ownership validation

**Input Validation**:
- Request schema validation
- Parameter sanitization
- File upload validation
- SQL injection prevention
- XSS protection

**Rate Limiting**:
- Request throttling
- IP-based limiting
- User-based quotas
- Distributed rate limiting

### Step 6: Generate OpenAPI Documentation

Create comprehensive API documentation using `assets/templates/openapi_template.yaml`.

**Documentation Includes**:
- API information and metadata
- Server configurations
- All endpoints with descriptions
- Request/response schemas
- Authentication schemes
- Error responses
- Code examples in multiple languages
- Try-it-out functionality

**File Naming Convention**:
- OpenAPI: `openapi-spec-[version]-[date].yaml`
- Postman Collection: `api-collection-[version].json`
- README: `API_DOCUMENTATION.md`

### Step 7: Add Testing and Monitoring

Implement testing and monitoring solutions:

**Testing**:
- Unit tests for controllers
- Integration tests for endpoints
- Contract testing
- Load testing scenarios
- Security testing

**Monitoring**:
- Request logging
- Error tracking
- Performance metrics
- Usage analytics
- Health check endpoints

### Step 8: Create Deployment Configuration

Generate deployment configs using `assets/templates/deployment_template.yaml`:

**Deployment Artifacts**:
- Docker configuration
- Docker Compose for local development
- Kubernetes manifests
- CI/CD pipeline configuration
- Environment variable templates

## Advanced Features

### API Versioning Strategies

**URL Versioning**:
```
/api/v1/users
/api/v2/users
```

**Header Versioning**:
```
API-Version: 1.0
Accept: application/vnd.api.v2+json
```

**Content Negotiation**:
```
Accept: application/json; version=1.0
```

### Pagination Patterns

**Offset-Based**:
```
GET /api/users?limit=20&offset=40
```

**Cursor-Based**:
```
GET /api/users?limit=20&cursor=eyJpZCI6MTIzfQ==
```

**Page-Based**:
```
GET /api/users?page=3&per_page=20
```

### Error Response Format

Consistent error structure:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request parameters",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ],
    "timestamp": "2024-03-15T10:30:00Z",
    "request_id": "abc123"
  }
}
```

### HATEOAS Implementation

Hypermedia as the Engine of Application State:
```json
{
  "id": 123,
  "name": "John Doe",
  "links": {
    "self": "/api/users/123",
    "orders": "/api/users/123/orders",
    "update": "/api/users/123",
    "delete": "/api/users/123"
  }
}
```

## Usage Examples

### Example 1: Generate REST API for E-commerce

**User Request**: "Create a REST API for an e-commerce platform with products, users, and orders."

**Execution**:
1. Gather requirements (authentication, payment integration, inventory)
2. Design resources: users, products, categories, orders, reviews
3. Use `assets/templates/rest_api_express.js` as base
4. Generate endpoints with CRUD operations
5. Implement JWT authentication
6. Add input validation with Joi/Yup
7. Generate OpenAPI specification
8. Create Postman collection
9. Add rate limiting and caching
10. Save as structured project

### Example 2: GraphQL API for Social Network

**User Request**: "Build a GraphQL API for a social networking app."

**Execution**:
1. Define schema: Users, Posts, Comments, Likes, Follows
2. Use `assets/templates/graphql_api_apollo.js`
3. Create type definitions
4. Implement resolvers with DataLoader for N+1 prevention
5. Add subscriptions for real-time updates
6. Implement authentication context
7. Add query complexity limiting
8. Generate schema documentation
9. Create GraphQL Playground config
10. Save complete API structure

### Example 3: Document Existing API

**User Request**: "Create OpenAPI documentation for my existing Node.js API."

**Execution**:
1. Analyze existing code structure
2. Extract endpoint definitions
3. Identify request/response patterns
4. Use `assets/templates/openapi_template.yaml`
5. Document all endpoints with examples
6. Add authentication schemes
7. Include error responses
8. Generate from code comments if available
9. Create interactive documentation
10. Export Postman collection

## Reference Files

This skill includes detailed reference documentation:

### `references/api_design_principles.md`
Comprehensive guide covering:
- RESTful API design principles
- Resource naming conventions
- HTTP method usage
- Status code selection
- Versioning strategies
- Security best practices
- Performance optimization
- GraphQL schema design

**When to load**: For any API design or architecture decisions.

### `references/authentication_patterns.md`
Complete authentication guide:
- JWT implementation
- OAuth2 flows
- API key management
- Session-based auth
- Multi-factor authentication
- Token refresh patterns
- Security considerations

**When to load**: When implementing authentication or security features.

### `references/api_testing_guide.md`
Testing strategies:
- Unit testing approaches
- Integration testing patterns
- Contract testing
- Load testing
- Security testing
- Mocking strategies
- Test data management

**When to load**: When adding tests to the API.

## Asset Templates

This skill includes professional templates in `assets/templates/`:

### `rest_api_express.js`
Complete Express.js REST API template with:
- Router configuration
- Controller pattern
- Middleware setup
- Error handling
- Validation
- Authentication

### `graphql_api_apollo.js`
Apollo Server GraphQL template with:
- Schema definitions
- Resolver structure
- Context setup
- DataLoader integration
- Subscription handling
- Error formatting

### `openapi_template.yaml`
OpenAPI 3.0 specification template:
- Complete structure
- Common patterns
- Example schemas
- Security schemes
- Response templates

### `api_middleware.js`
Reusable middleware collection:
- Authentication
- Authorization
- Validation
- Rate limiting
- Logging
- Error handling
- CORS configuration

## Best Practices

### API Design
- Use nouns for resources, not verbs
- Implement consistent naming conventions
- Version your API from the start
- Use appropriate HTTP methods and status codes
- Design for backward compatibility
- Keep responses predictable and consistent

### Security
- Always use HTTPS
- Implement proper authentication
- Use rate limiting
- Validate all inputs
- Sanitize user data
- Implement CORS properly
- Use security headers
- Log security events

### Performance
- Implement caching where appropriate
- Use pagination for list endpoints
- Support field filtering
- Implement compression
- Optimize database queries
- Use CDN for static assets
- Monitor and optimize slow endpoints

### Documentation
- Keep documentation up-to-date
- Provide code examples
- Document error responses
- Include rate limits
- Explain authentication
- Provide changelog
- Make documentation searchable

## Common Scenarios

### Scenario 1: New REST API
**Requirements**: Fresh API from scratch
**Approach**: Design → Generate structure → Implement → Document → Test
**Output**: Complete REST API with OpenAPI docs

### Scenario 2: Add GraphQL to Existing REST API
**Requirements**: GraphQL layer over REST
**Approach**: Analyze REST → Design GraphQL schema → Map to REST → Implement resolvers
**Output**: GraphQL gateway with REST backend

### Scenario 3: Microservices API
**Requirements**: Multiple interconnected services
**Approach**: Design service boundaries → API contracts → Gateway pattern → Implementation
**Output**: Microservices with API gateway

### Scenario 4: API Modernization
**Requirements**: Update legacy API
**Approach**: Document existing → Design v2 → Implement migration → Support both versions
**Output**: Modern API with migration path

## Tips for Effective API Development

1. **Start with Design**: Plan before coding
2. **Think Consumer-First**: Design for API consumers
3. **Be Consistent**: Maintain patterns throughout
4. **Version Early**: Don't wait to add versioning
5. **Document Everything**: Make it easy to understand
6. **Test Thoroughly**: Cover all scenarios
7. **Monitor Usage**: Track and optimize
8. **Handle Errors Gracefully**: Clear error messages
9. **Optimize Performance**: Fast responses matter
10. **Secure by Default**: Security is not optional
