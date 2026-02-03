# Code Review Prompt

## Purpose

Request a comprehensive code review from AI to identify issues, suggest improvements, and ensure code quality.

## Template

```
Please review the following code for:

1. **Bugs and Logic Errors**
   - Identify any potential bugs
   - Check for logic errors
   - Flag edge cases that aren't handled

2. **Performance Issues**
   - Identify inefficient algorithms
   - Suggest optimizations
   - Point out potential bottlenecks

3. **Security Vulnerabilities**
   - Check for common security issues
   - Identify potential injection points
   - Review authentication/authorization

4. **Code Quality**
   - Assess readability
   - Check naming conventions
   - Evaluate code organization
   - Suggest refactoring opportunities

5. **Best Practices**
   - Language-specific best practices
   - Framework conventions
   - Design patterns

Code to review:
```[language]
[PASTE YOUR CODE HERE]
```

Context:
[DESCRIBE THE PURPOSE AND CONTEXT]

Focus areas:
[ANY SPECIFIC CONCERNS OR AREAS TO FOCUS ON]
```

## Example

```
Please review the following code for bugs, performance issues, security vulnerabilities, code quality, and best practices.

Code to review:
```python
def process_user_input(user_id, data):
    query = "SELECT * FROM users WHERE id = " + user_id
    result = db.execute(query)
    
    if result:
        user = result[0]
        user['data'] = data
        db.save(user)
        return True
    return False
```

Context:
This function processes user input and updates the database. It's called from a web API endpoint.

Focus areas:
I'm particularly concerned about security and data validation.
```

## Expected Output

The AI should provide:
- List of identified issues with severity
- Specific code examples of problems
- Suggested fixes
- Explanation of why changes are needed

## Tips

- Provide enough context about how the code is used
- Mention your tech stack and frameworks
- Be specific about concerns or focus areas
- Ask for prioritized feedback if code is large
- Request specific examples in suggestions

## Variations

### Quick Review
```
Quick code review - check for obvious bugs and security issues:
[CODE]
```

### Security-Focused Review
```
Security audit of this code. Check for:
- SQL injection
- XSS vulnerabilities
- Authentication issues
- Data exposure
[CODE]
```

### Performance Review
```
Performance review of this code. Identify:
- Time complexity issues
- Memory leaks
- Inefficient algorithms
- Caching opportunities
[CODE]
```
