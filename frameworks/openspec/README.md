# OpenSpec

This directory contains examples and best practices for using AI with OpenSpec.

## About OpenSpec

OpenSpec is a standard for describing AI agents and their capabilities. It enables:

- Agent documentation
- Capability definition
- Skill specification  
- Agent discovery
- Integration documentation

Learn more at [https://github.com/Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec)

## AI Integration

AI tools excel at working with OpenSpec specifications:

1. **Spec Generation**: Create OpenSpec definitions from agent requirements
2. **Agent Design**: Design AI agents following best practices
3. **Documentation**: Generate human-readable agent documentation
4. **Capability Mapping**: Define and organize agent capabilities
5. **Validation**: Check specs for errors and inconsistencies

## Using AI with OpenSpec

### Generate OpenSpec Specification

Create spec from requirements:

```
Create an OpenSpec specification for an [agent description].

Requirements:
- [requirement 1]
- [requirement 2]

Include:
- Agent metadata
- Capabilities
- Skills
- Input/output schemas
- Examples
```

Example output:
```yaml
openspec: 1.0.0
info:
  title: User Management Agent
  description: AI agent for managing user accounts
  version: 1.0.0
  contact:
    email: agent@example.com

agent:
  type: assistant
  capabilities:
    - user_management
    - authentication
    - data_processing

capabilities:
  user_management:
    description: Manage user accounts and profiles
    skills:
      - create_user
      - update_user
      - delete_user
      - list_users
      - get_user
    
  authentication:
    description: Handle user authentication
    skills:
      - login
      - logout
      - verify_token
      
skills:
  create_user:
    description: Create a new user account
    input:
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
    output:
      type: object
      properties:
        id:
          type: string
          format: uuid
          description: Unique user identifier
        email:
          type: string
          format: email
        firstName:
          type: string
        lastName:
          type: string
        createdAt:
          type: string
          format: date-time
      required:
        - id
        - email
        - firstName
        - lastName
        - createdAt
    errors:
      - code: USER_EXISTS
        message: User already exists
      - code: INVALID_INPUT
        message: Invalid input data
      - code: INTERNAL_ERROR
        message: Internal server error
        
  get_user:
    description: Get user by ID
    input:
      type: object
      properties:
        userId:
          type: string
          format: uuid
          description: User ID
      required:
        - userId
    output:
      type: object
      properties:
        id:
          type: string
          format: uuid
        email:
          type: string
          format: email
        firstName:
          type: string
        lastName:
          type: string
        createdAt:
          type: string
          format: date-time
      required:
        - id
        - email
        - firstName
        - lastName
        - createdAt
    errors:
      - code: USER_NOT_FOUND
        message: User not found
      - code: UNAUTHORIZED
        message: Unauthorized access
        
security:
  type: bearer
  scheme: jwt
  description: JWT token authentication
```

### Enhance Existing Spec

Add details to an existing spec:

```
Enhance this OpenSpec specification by adding:
- More detailed descriptions
- Request/response examples
- Validation rules
- Additional error responses
- Security definitions

[PASTE SPEC]
```

### Generate Code from Spec

```
Generate [language] agent implementation from this OpenSpec:

[PASTE SPEC]

Include:
- Capability handlers
- Input validation
- Error handling
- Response formatting
```

### Create Integration

```
Generate integration code for this OpenSpec agent:

[PASTE SPEC]

Include:
- All capability methods
- Type definitions
- Error handling
- Authentication
```

## Best Practices

1. **Use Latest OpenSpec Version**: Latest version with better features
2. **Comprehensive Schemas**: Define all input/output schemas
3. **Examples**: Include examples for all capabilities
4. **Error Responses**: Document all possible error responses
5. **Security**: Define authentication/authorization clearly
6. **Versioning**: Include agent version in the spec
7. **Descriptions**: Add clear descriptions for all elements
8. **Validation**: Use JSON Schema validation rules

## AI Tips for OpenSpec

- Provide complete agent requirements
- Specify authentication method
- Include capability models and relationships
- Request examples for each capability
- Ask for validation rules
- Specify error handling approach

## Example Prompts

### Convert Existing Agent
```
Analyze this agent implementation and generate an OpenSpec specification:

[PASTE CODE]
```

### Agent Design Review
```
Review this OpenSpec specification for:
- Best practices
- Missing capability definitions
- Security issues
- Schema completeness

[PASTE SPEC]
```

### Test Configuration
```
Create a test configuration from this OpenSpec for validation purposes.

[PASTE SPEC]
```

## Tools and Resources

- [OpenSpec Repository](https://github.com/Fission-AI/OpenSpec)
- OpenSpec documentation
- Agent validation tools

## Use Cases

- **Agent-First Development**: Design agents before implementation
- **Documentation**: Auto-generate agent documentation
- **Testing**: Generate test cases from spec
- **Integration**: Create integrations for multiple platforms
- **Contract Testing**: Ensure agent matches specification
- **Capability Discovery**: Enable dynamic agent discovery
