# Code Style Rules

## General Principles

- Write code that is easy to read and understand
- Be consistent with existing codebase patterns
- Favor clarity over cleverness
- Keep functions and methods focused and small

## Naming Conventions

### Variables
- Use descriptive names that reveal intent
- Use camelCase for variables and functions (JavaScript, Java, C#)
- Use snake_case for variables and functions (Python, Ruby)
- Avoid single-letter names except for loop counters
- Boolean variables should start with `is`, `has`, `can`, or `should`

```javascript
// Good
const isUserAuthenticated = true;
const hasPermission = false;

// Bad
const auth = true;
const x = false;
```

### Functions/Methods
- Use verb phrases that describe what the function does
- Be specific about the action
- Keep names concise but clear

```python
# Good
def calculate_total_price(items):
    pass

def send_notification_email(user, message):
    pass

# Bad
def do_stuff(x):
    pass

def process(data):
    pass
```

### Classes
- Use PascalCase
- Use nouns or noun phrases
- Name should reflect what the class represents

```java
// Good
public class UserRepository {}
public class EmailService {}

// Bad
public class DoStuff {}
public class Manager {}
```

### Constants
- Use UPPER_SNAKE_CASE
- Place at top of file or in constants file

```python
MAX_RETRY_COUNT = 3
DEFAULT_TIMEOUT_SECONDS = 30
API_BASE_URL = "https://api.example.com"
```

## Code Organization

### File Structure
- One class per file (for OOP languages)
- Group related functions together
- Place imports at the top
- Organize in logical sections

### Function Length
- Keep functions under 50 lines when possible
- Extract complex logic into separate functions
- Each function should do one thing well

### Indentation and Spacing
- Use consistent indentation (2 or 4 spaces)
- Add blank lines between logical sections
- Don't exceed 80-120 characters per line

## Comments

### When to Comment
- Explain "why" not "what"
- Document complex algorithms
- Clarify non-obvious decisions
- Mark TODOs and FIXMEs

```javascript
// Good - explains why
// Using a Set here for O(1) lookup performance
// as this operation happens in a tight loop
const uniqueIds = new Set(ids);

// Bad - explains what (obvious from code)
// Create a new Set with ids
const uniqueIds = new Set(ids);
```

### Avoid
- Commented-out code (remove it, it's in version control)
- Redundant comments
- Obvious statements

## Error Handling

- Use try-catch blocks for operations that can fail
- Provide meaningful error messages
- Log errors with context
- Don't swallow exceptions silently

```python
# Good
try:
    result = process_payment(amount)
except PaymentError as e:
    logger.error(f"Payment processing failed for amount {amount}: {e}")
    raise

# Bad
try:
    result = process_payment(amount)
except:
    pass  # Silent failure
```

## Language-Specific Guidelines

### JavaScript/TypeScript
- Use `const` by default, `let` when needed, avoid `var`
- Use arrow functions for callbacks
- Use template literals for string interpolation
- Prefer async/await over raw promises

### Python
- Follow PEP 8
- Use list comprehensions for simple transformations
- Use context managers (`with` statement) for resources
- Type hints for function signatures

### Java
- Follow Java naming conventions
- Use appropriate access modifiers
- Leverage interfaces and abstract classes
- Use generics for type safety

### C#
- Follow .NET naming conventions
- Use LINQ for collections
- Implement IDisposable for resources
- Use async/await for I/O operations

## Don't

- Don't use magic numbers (use named constants)
- Don't nest too deeply (max 3-4 levels)
- Don't repeat yourself (DRY principle)
- Don't ignore warnings
- Don't commit dead code
