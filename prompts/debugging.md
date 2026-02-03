# Debugging Prompt

## Purpose

Use AI to help identify, diagnose, and fix bugs in your code.

## Template

```
I'm experiencing a bug in my code. Help me debug and fix it.

**Bug Description:**
[Describe what's happening vs what should happen]

**Error Message (if any):**
```
[Paste error message and stack trace]
```

**Code:**
```[language]
[Paste relevant code]
```

**Steps to Reproduce:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Environment:**
- Language/Framework: [e.g., Python 3.9, Node.js 18]
- Dependencies: [relevant libraries and versions]
- OS: [if relevant]

**What I've Tried:**
- [Attempt 1]
- [Attempt 2]

**Additional Context:**
[Any other relevant information]
```

## Example

```
I'm experiencing a bug in my code. Help me debug and fix it.

**Bug Description:**
My REST API endpoint returns a 500 error when processing certain user requests, 
but works fine for others. Should return 200 with user data.

**Error Message:**
```
TypeError: Cannot read property 'toString' of null
    at getUserData (user-service.js:45:23)
    at processRequest (api-handler.js:12:15)
```

**Code:**
```javascript
// user-service.js
async function getUserData(userId) {
  const user = await db.users.findById(userId);
  const lastLogin = user.lastLogin.toString();
  
  return {
    id: user.id,
    name: user.name,
    lastLogin: lastLogin
  };
}

// api-handler.js
app.get('/api/users/:id', async (req, res) => {
  try {
    const data = await getUserData(req.params.id);
    res.json(data);
  } catch (error) {
    console.error(error);
    res.status(500).send('Internal Server Error');
  }
});
```

**Steps to Reproduce:**
1. Call GET /api/users/123
2. Server returns 500 error
3. Error appears in logs

**Environment:**
- Node.js 18.x
- Express 4.18
- MongoDB with Mongoose

**What I've Tried:**
- Added try-catch (still errors)
- Logged userId (it's valid)
- Checked database (user exists)

**Additional Context:**
Works for some users but not others. Started happening after a recent deployment.
```

## Expected Response Format

The AI should provide:

1. **Root Cause Analysis**
   - Identify the exact problem
   - Explain why it's happening

2. **Fix Recommendation**
   - Specific code changes
   - Why the fix works

3. **Prevention**
   - How to avoid similar issues
   - Best practices

4. **Testing Suggestions**
   - How to verify the fix
   - Additional test cases

## Tips for Better Debugging with AI

1. **Be Specific**: Provide exact error messages
2. **Include Context**: Show surrounding code
3. **Share Attempts**: What you've already tried
4. **Minimize Code**: Remove unrelated parts
5. **Provide Examples**: Show working and failing cases
6. **Include Logs**: Share relevant log output

## Variations

### Quick Debug
```
Getting error: [error message]
In this code: [code snippet]
What's wrong?
```

### Performance Issue
```
This code is slow:
[code]

Expected performance: [benchmark]
Actual performance: [benchmark]

Profile results:
[profiling data]

How can I optimize it?
```

### Intermittent Bug
```
Bug occurs randomly in this code:
[code]

Frequency: [e.g., 1 in 10 requests]
Conditions when it happens: [observations]

Possible race condition or timing issue?
```

### Logic Error
```
This function returns unexpected results:

Input: [example input]
Expected output: [what should happen]
Actual output: [what actually happens]

Code:
[code]

What's the logic error?
```

## Common Bug Categories

### Null/Undefined Issues
- Missing null checks
- Undefined variables
- Optional chaining needed

### Type Errors
- Type mismatches
- Incorrect conversions
- Type coercion issues

### Async Issues
- Missing await
- Race conditions
- Callback hell
- Promise rejection handling

### Off-by-One Errors
- Array index issues
- Loop boundary problems

### Side Effects
- Mutating shared state
- Unintended global changes
- Closure issues

### Logic Errors
- Incorrect conditions
- Wrong operators
- Faulty algorithms
