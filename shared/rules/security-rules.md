# Security Rules

## General Security Principles

- Never trust user input
- Validate and sanitize all data
- Use principle of least privilege
- Keep dependencies updated
- Follow secure coding guidelines
- Regular security audits

## Input Validation

### Always Validate
```javascript
// Good
function processUserInput(input) {
  if (typeof input !== 'string') {
    throw new Error('Input must be a string');
  }
  
  if (input.length > MAX_LENGTH) {
    throw new Error('Input too long');
  }
  
  const sanitized = sanitizeInput(input);
  return processData(sanitized);
}

// Bad
function processUserInput(input) {
  return processData(input); // No validation!
}
```

### Sanitize Input
- Remove or escape special characters
- Use allowlists, not denylists
- Apply context-specific sanitization

## SQL Injection Prevention

### Use Parameterized Queries
```python
# Good - Parameterized query
cursor.execute(
    "SELECT * FROM users WHERE email = %s",
    (user_email,)
)

# Bad - String concatenation
cursor.execute(
    "SELECT * FROM users WHERE email = '" + user_email + "'"
)
```

### Use ORM Features
```javascript
// Good - ORM
const user = await User.findOne({ email: userEmail });

// Bad - Raw query with concatenation
const user = await db.query(
  `SELECT * FROM users WHERE email = '${userEmail}'`
);
```

## XSS (Cross-Site Scripting) Prevention

### Escape Output
```javascript
// Good - Escaped output
res.send(escapeHtml(userInput));

// Bad - Unescaped output
res.send(userInput);
```

### Use Template Engines with Auto-Escaping
```html
<!-- Good - Auto-escaped in most modern frameworks -->
<div>{{ userInput }}</div>

<!-- Bad - Raw HTML -->
<div>{{{ userInput }}}</div>
```

### Content Security Policy
```javascript
// Set CSP headers
app.use((req, res, next) => {
  res.setHeader(
    'Content-Security-Policy',
    "default-src 'self'; script-src 'self'"
  );
  next();
});
```

## Authentication & Authorization

### Secure Password Handling
```javascript
// Good - Hashed passwords
const bcrypt = require('bcrypt');
const saltRounds = 10;

async function hashPassword(password) {
  return await bcrypt.hash(password, saltRounds);
}

async function verifyPassword(password, hash) {
  return await bcrypt.compare(password, hash);
}

// Bad - Plain text passwords
function savePassword(password) {
  db.save({ password: password }); // Never do this!
}
```

### Use Strong Password Requirements
- Minimum 8 characters
- Mix of uppercase, lowercase, numbers, symbols
- Check against common password lists
- Implement account lockout after failed attempts

### Secure Session Management
```javascript
// Good - Secure session config
app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: true,      // HTTPS only
    httpOnly: true,    // No JavaScript access
    sameSite: 'strict', // CSRF protection
    maxAge: 3600000    // 1 hour
  }
}));
```

## Secrets Management

### Never Commit Secrets
```bash
# Good - Use environment variables
DB_PASSWORD=${DB_PASSWORD}

# Bad - Hardcoded secrets
DB_PASSWORD=supersecretpassword123
```

### Use Secret Management Services
- AWS Secrets Manager
- Azure Key Vault
- HashiCorp Vault
- Environment variables

### Rotate Secrets Regularly
- Change passwords periodically
- Rotate API keys
- Update certificates before expiration

## API Security

### Use HTTPS
```javascript
// Good - Enforce HTTPS
app.use((req, res, next) => {
  if (!req.secure && req.get('x-forwarded-proto') !== 'https') {
    return res.redirect('https://' + req.get('host') + req.url);
  }
  next();
});
```

### Implement Rate Limiting
```javascript
// Good - Rate limiting
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);
```

### Validate JWT Tokens
```javascript
// Good - Verify tokens
const jwt = require('jsonwebtoken');

function authenticateToken(req, res, next) {
  const token = req.headers['authorization']?.split(' ')[1];
  
  if (!token) {
    return res.sendStatus(401);
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user;
    next();
  });
}
```

## File Upload Security

### Validate File Types
```javascript
// Good - Validate file type
const allowedTypes = ['image/jpeg', 'image/png', 'image/gif'];

if (!allowedTypes.includes(file.mimetype)) {
  throw new Error('Invalid file type');
}
```

### Limit File Size
```javascript
// Good - Size limit
const upload = multer({
  limits: {
    fileSize: 5 * 1024 * 1024 // 5MB
  }
});
```

### Store Files Outside Web Root
```javascript
// Good - Separate storage
const uploadPath = '/var/app/uploads/';

// Bad - In public directory
const uploadPath = './public/uploads/';
```

## Error Handling

### Don't Expose Sensitive Information
```javascript
// Good - Generic error message
res.status(500).json({
  error: 'An error occurred'
});

// Bad - Detailed error exposure
res.status(500).json({
  error: err.message,
  stack: err.stack,
  query: sqlQuery
});
```

### Log Errors Securely
```javascript
// Good - Log without sensitive data
logger.error('Login failed', {
  userId: user.id,
  timestamp: Date.now()
});

// Bad - Log sensitive data
logger.error('Login failed', {
  password: password,
  creditCard: user.creditCard
});
```

## Dependency Security

### Keep Dependencies Updated
```bash
# Regular updates
npm audit
npm update

# Fix vulnerabilities
npm audit fix
```

### Use Lock Files
- package-lock.json (npm)
- yarn.lock (Yarn)
- Pipfile.lock (Python)

### Review Dependencies
- Check for known vulnerabilities
- Audit before adding new dependencies
- Remove unused dependencies

## CORS Configuration

### Configure Properly
```javascript
// Good - Specific origins
app.use(cors({
  origin: ['https://yourdomain.com'],
  credentials: true
}));

// Bad - Allow all origins
app.use(cors({
  origin: '*'
}));
```

## Regular Security Practices

1. **Code Reviews**: Security-focused reviews
2. **Security Testing**: Regular penetration testing
3. **Monitoring**: Watch for suspicious activity
4. **Updates**: Keep all software updated
5. **Training**: Security awareness for team
6. **Incident Response**: Have a plan ready

## Security Checklist

- [ ] All input validated and sanitized
- [ ] Parameterized queries used
- [ ] Output properly escaped
- [ ] Passwords hashed with strong algorithm
- [ ] HTTPS enforced
- [ ] Rate limiting implemented
- [ ] CORS properly configured
- [ ] Secrets not in code
- [ ] Dependencies up to date
- [ ] Error messages don't leak info
- [ ] File uploads validated
- [ ] Authentication implemented
- [ ] Authorization checked
- [ ] Logging configured securely
- [ ] Security headers set
