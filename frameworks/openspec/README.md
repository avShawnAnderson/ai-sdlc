# OpenAPI Specification (OpenSpec)

This directory contains examples and best practices for using AI with OpenAPI Specification (formerly Swagger).

## About OpenAPI

OpenAPI Specification is a standard for describing RESTful APIs. It enables:

- API documentation
- Code generation
- API testing
- Client SDK generation
- Mock server creation

## AI Integration

AI tools excel at working with OpenAPI specifications:

1. **Spec Generation**: Create OpenAPI specs from requirements or existing APIs
2. **API Design**: Design RESTful APIs following best practices
3. **Documentation**: Generate human-readable API documentation
4. **Code Generation**: Create server stubs and client SDKs
5. **Validation**: Check specs for errors and inconsistencies

## Using AI with OpenAPI

### Generate OpenAPI Specification

Create spec from requirements:

```
Create an OpenAPI 3.0 specification for a [API description].

Requirements:
- [requirement 1]
- [requirement 2]

Include:
- All endpoints
- Request/response schemas
- Authentication
- Error responses
- Examples
```

Example output:
```yaml
openapi: 3.0.0
info:
  title: User Management API
  description: API for managing user accounts
  version: 1.0.0
  contact:
    email: api@example.com

servers:
  - url: https://api.example.com/v1
    description: Production server
  - url: https://staging-api.example.com/v1
    description: Staging server

security:
  - bearerAuth: []

paths:
  /users:
    get:
      summary: List all users
      description: Returns a paginated list of users
      tags:
        - Users
      parameters:
        - name: page
          in: query
          description: Page number
          required: false
          schema:
            type: integer
            default: 1
            minimum: 1
        - name: limit
          in: query
          description: Number of items per page
          required: false
          schema:
            type: integer
            default: 20
            minimum: 1
            maximum: 100
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/User'
                  pagination:
                    $ref: '#/components/schemas/Pagination'
        '401':
          $ref: '#/components/responses/UnauthorizedError'
        '500':
          $ref: '#/components/responses/InternalServerError'

    post:
      summary: Create a new user
      description: Creates a new user account
      tags:
        - Users
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          $ref: '#/components/responses/BadRequestError'
        '401':
          $ref: '#/components/responses/UnauthorizedError'
        '409':
          description: User already exists
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          $ref: '#/components/responses/InternalServerError'

  /users/{userId}:
    get:
      summary: Get user by ID
      description: Returns a single user
      tags:
        - Users
      parameters:
        - name: userId
          in: path
          required: true
          description: User ID
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          $ref: '#/components/responses/NotFoundError'
        '401':
          $ref: '#/components/responses/UnauthorizedError'
        '500':
          $ref: '#/components/responses/InternalServerError'

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
          format: uuid
          description: Unique user identifier
        email:
          type: string
          format: email
          description: User email address
        firstName:
          type: string
          description: User first name
        lastName:
          type: string
          description: User last name
        createdAt:
          type: string
          format: date-time
          description: Account creation timestamp
        updatedAt:
          type: string
          format: date-time
          description: Last update timestamp
      required:
        - id
        - email
        - firstName
        - lastName
        - createdAt

    CreateUserRequest:
      type: object
      properties:
        email:
          type: string
          format: email
          description: User email address
        firstName:
          type: string
          minLength: 1
          maxLength: 50
          description: User first name
        lastName:
          type: string
          minLength: 1
          maxLength: 50
          description: User last name
        password:
          type: string
          format: password
          minLength: 8
          description: User password
      required:
        - email
        - firstName
        - lastName
        - password

    Pagination:
      type: object
      properties:
        page:
          type: integer
          description: Current page number
        limit:
          type: integer
          description: Items per page
        total:
          type: integer
          description: Total number of items
        pages:
          type: integer
          description: Total number of pages

    Error:
      type: object
      properties:
        code:
          type: string
          description: Error code
        message:
          type: string
          description: Error message
        details:
          type: array
          items:
            type: string
          description: Additional error details
      required:
        - code
        - message

  responses:
    BadRequestError:
      description: Bad request
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    
    UnauthorizedError:
      description: Unauthorized
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    
    NotFoundError:
      description: Resource not found
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    
    InternalServerError:
      description: Internal server error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

### Enhance Existing Spec

Add details to an existing spec:

```
Enhance this OpenAPI specification by adding:
- More detailed descriptions
- Request/response examples
- Validation rules
- Additional error responses
- Security definitions

[PASTE SPEC]
```

### Generate Code from Spec

```
Generate [language] server code from this OpenAPI spec:

[PASTE SPEC]

Include:
- Route handlers
- Request validation
- Error handling
- Response formatting
```

### Create Client SDK

```
Generate a [language] client SDK from this OpenAPI spec:

[PASTE SPEC]

Include:
- All API methods
- Type definitions
- Error handling
- Authentication
```

## Best Practices

1. **Use OpenAPI 3.0+**: Latest version with better features
2. **Comprehensive Schemas**: Define all request/response schemas
3. **Examples**: Include examples for all endpoints
4. **Error Responses**: Document all possible error responses
5. **Security**: Define authentication/authorization clearly
6. **Versioning**: Include API version in the spec
7. **Descriptions**: Add clear descriptions for all elements
8. **Validation**: Use JSON Schema validation rules

## AI Tips for OpenAPI

- Provide complete API requirements
- Specify authentication method
- Include data models and relationships
- Request examples for each endpoint
- Ask for validation rules
- Specify error handling approach

## Example Prompts

### Convert Existing API
```
Analyze this API implementation and generate an OpenAPI 3.0 specification:

[PASTE CODE]
```

### API Design Review
```
Review this OpenAPI specification for:
- REST best practices
- Missing error responses
- Security issues
- Schema completeness

[PASTE SPEC]
```

### Mock Server Setup
```
Create a mock server configuration from this OpenAPI spec for testing purposes.

[PASTE SPEC]
```

## Tools and Resources

- [Swagger Editor](https://editor.swagger.io/)
- [OpenAPI Generator](https://openapi-generator.tech/)
- [Swagger UI](https://swagger.io/tools/swagger-ui/)
- [Postman](https://www.postman.com/) - API testing
- OpenAPI Specification documentation

## Use Cases

- **API-First Development**: Design API before implementation
- **Documentation**: Auto-generate API documentation
- **Testing**: Generate test cases from spec
- **Client Generation**: Create SDKs for multiple languages
- **Contract Testing**: Ensure API matches specification
- **Mock Servers**: Create mock APIs for frontend development
