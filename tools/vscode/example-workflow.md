# VS Code with GitHub Copilot - Example Workflow

## Scenario: Building a REST API Endpoint

This example demonstrates using GitHub Copilot in VS Code to build a user authentication endpoint.

## Step 1: Start with a Comment

```javascript
// POST endpoint for user login
// Accepts email and password
// Returns JWT token on success
// Returns 401 on invalid credentials
```

**Result**: Copilot suggests the basic endpoint structure.

## Step 2: Accept and Refine

```javascript
// POST endpoint for user login
// Accepts email and password
// Returns JWT token on success
// Returns 401 on invalid credentials
app.post('/api/auth/login', async (req, res) => {
  // Copilot suggests implementation
  const { email, password } = req.body;
  
  // Validate input
  if (!email || !password) {
    return res.status(400).json({ error: 'Email and password required' });
  }
  
  // Find user
  const user = await User.findOne({ email });
  if (!user) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  // Verify password
  const isValid = await bcrypt.compare(password, user.password);
  if (!isValid) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  // Generate token
  const token = jwt.sign(
    { userId: user.id, email: user.email },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
  
  res.json({ token });
});
```

## Step 3: Generate Tests with Copilot Chat

Open Copilot Chat (Ctrl+Shift+I or Cmd+Shift+I) and ask:

```
Generate Jest tests for this login endpoint including:
- Successful login
- Invalid credentials
- Missing email
- Missing password
- User not found
```

**Result**: Copilot generates comprehensive test suite.

```javascript
describe('POST /api/auth/login', () => {
  beforeEach(async () => {
    await User.deleteMany({});
  });

  test('successful login returns token', async () => {
    const user = await User.create({
      email: 'test@example.com',
      password: await bcrypt.hash('password123', 10)
    });

    const response = await request(app)
      .post('/api/auth/login')
      .send({
        email: 'test@example.com',
        password: 'password123'
      });

    expect(response.status).toBe(200);
    expect(response.body).toHaveProperty('token');
    
    const decoded = jwt.verify(response.body.token, process.env.JWT_SECRET);
    expect(decoded.userId).toBe(user.id);
  });

  test('invalid password returns 401', async () => {
    await User.create({
      email: 'test@example.com',
      password: await bcrypt.hash('password123', 10)
    });

    const response = await request(app)
      .post('/api/auth/login')
      .send({
        email: 'test@example.com',
        password: 'wrongpassword'
      });

    expect(response.status).toBe(401);
    expect(response.body.error).toBe('Invalid credentials');
  });

  test('missing email returns 400', async () => {
    const response = await request(app)
      .post('/api/auth/login')
      .send({
        password: 'password123'
      });

    expect(response.status).toBe(400);
    expect(response.body.error).toBe('Email and password required');
  });

  test('missing password returns 400', async () => {
    const response = await request(app)
      .post('/api/auth/login')
      .send({
        email: 'test@example.com'
      });

    expect(response.status).toBe(400);
    expect(response.body.error).toBe('Email and password required');
  });

  test('non-existent user returns 401', async () => {
    const response = await request(app)
      .post('/api/auth/login')
      .send({
        email: 'nonexistent@example.com',
        password: 'password123'
      });

    expect(response.status).toBe(401);
    expect(response.body.error).toBe('Invalid credentials');
  });
});
```

## Step 4: Generate Documentation

Select the function and ask Copilot Chat:

```
Generate JSDoc documentation for this function
```

**Result**: Copilot adds comprehensive JSDoc comments.

```javascript
/**
 * Authenticates a user and returns a JWT token
 * 
 * @route POST /api/auth/login
 * @param {Object} req.body - The request body
 * @param {string} req.body.email - User's email address
 * @param {string} req.body.password - User's password
 * @returns {Object} 200 - JWT token
 * @returns {Object} 400 - Missing credentials
 * @returns {Object} 401 - Invalid credentials
 * @returns {Object} 500 - Server error
 * 
 * @example
 * // Request
 * POST /api/auth/login
 * {
 *   "email": "user@example.com",
 *   "password": "securepassword"
 * }
 * 
 * // Response
 * {
 *   "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
 * }
 */
app.post('/api/auth/login', async (req, res) => {
  // ... implementation
});
```

## Step 5: Refactor with Copilot

Select the code and ask in Copilot Chat:

```
Refactor this to use a separate authentication service
```

**Result**: Copilot suggests extracting logic to a service.

```javascript
// auth.service.js
class AuthService {
  async login(email, password) {
    if (!email || !password) {
      throw new ValidationError('Email and password required');
    }
    
    const user = await User.findOne({ email });
    if (!user) {
      throw new AuthenticationError('Invalid credentials');
    }
    
    const isValid = await bcrypt.compare(password, user.password);
    if (!isValid) {
      throw new AuthenticationError('Invalid credentials');
    }
    
    const token = jwt.sign(
      { userId: user.id, email: user.email },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    return { token, user };
  }
}

// auth.controller.js
app.post('/api/auth/login', async (req, res) => {
  try {
    const { email, password } = req.body;
    const result = await authService.login(email, password);
    res.json(result);
  } catch (error) {
    if (error instanceof ValidationError) {
      return res.status(400).json({ error: error.message });
    }
    if (error instanceof AuthenticationError) {
      return res.status(401).json({ error: error.message });
    }
    res.status(500).json({ error: 'Internal server error' });
  }
});
```

## Tips for VS Code + Copilot

1. **Write Clear Comments**: Guide Copilot with descriptive comments
2. **Use Copilot Chat**: Ask questions about your code
3. **Accept Partially**: Use Tab to accept full suggestion, or Ctrl+→ for word-by-word
4. **Multiple Suggestions**: Use Alt+] to cycle through suggestions
5. **Context Matters**: Keep relevant files open
6. **Slash Commands**: Use `/explain`, `/fix`, `/tests` in chat
7. **Inline Chat**: Press Ctrl+I (Cmd+I) for inline suggestions

## Keyboard Shortcuts

- **Copilot Suggestions**: Suggestions appear automatically
- **Accept Suggestion**: Tab
- **Reject Suggestion**: Esc
- **Next Suggestion**: Alt+]
- **Previous Suggestion**: Alt+[
- **Open Copilot Chat**: Ctrl+Shift+I (Cmd+Shift+I)
- **Inline Chat**: Ctrl+I (Cmd+I)

## Common Use Cases

### Generate Boilerplate
Write a comment describing what you need, Copilot generates the code.

### Complete Functions
Start typing a function signature, Copilot completes the implementation.

### Write Tests
Ask Copilot Chat to generate tests for your functions.

### Explain Code
Select code and ask Copilot to explain what it does.

### Fix Bugs
Highlight problematic code and ask Copilot for fixes.

### Convert Code
Ask Copilot to convert code between languages or frameworks.
