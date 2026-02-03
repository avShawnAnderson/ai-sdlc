# Test Generation Skill

## Description

Automatically generate comprehensive unit tests for your code using AI assistance.

## When to Use

- After writing new functions or classes
- When test coverage is low
- When refactoring existing code
- For regression testing

## Prerequisites

- Existing code to test
- Understanding of your testing framework
- Test file structure in place

## Step-by-Step Instructions

### Step 1: Identify Code to Test

Select the function, class, or module you want to test.

### Step 2: Provide Context to AI

Include:
- The code to test
- Your testing framework (Jest, pytest, xUnit, etc.)
- Expected behavior
- Edge cases to consider

### Step 3: Request Tests

Example prompt:
```
Generate unit tests for this function using [testing framework].
Include:
- Happy path tests
- Edge cases
- Error handling
- Null/undefined inputs

[Paste your code here]
```

### Step 4: Review and Refine

- Verify test coverage
- Ensure tests are independent
- Check assertions are meaningful
- Add additional tests if needed

### Step 5: Run and Validate

Run the generated tests to ensure they:
- Pass with correct implementations
- Fail with incorrect implementations
- Cover the intended scenarios

## Example

### Original Code (JavaScript)
```javascript
function calculateDiscount(price, discountPercent) {
  if (price < 0 || discountPercent < 0) {
    throw new Error('Values must be positive');
  }
  if (discountPercent > 100) {
    throw new Error('Discount cannot exceed 100%');
  }
  return price * (1 - discountPercent / 100);
}
```

### Generated Tests (Jest)
```javascript
describe('calculateDiscount', () => {
  test('calculates discount correctly', () => {
    expect(calculateDiscount(100, 10)).toBe(90);
  });

  test('handles zero discount', () => {
    expect(calculateDiscount(100, 0)).toBe(100);
  });

  test('throws error for negative price', () => {
    expect(() => calculateDiscount(-100, 10)).toThrow('Values must be positive');
  });

  test('throws error for negative discount', () => {
    expect(() => calculateDiscount(100, -10)).toThrow('Values must be positive');
  });

  test('throws error for discount over 100%', () => {
    expect(() => calculateDiscount(100, 101)).toThrow('Discount cannot exceed 100%');
  });

  test('handles 100% discount', () => {
    expect(calculateDiscount(100, 100)).toBe(0);
  });
});
```

## Tips

- Start with simple tests and add complexity
- Use descriptive test names
- Test one thing per test
- Include both positive and negative cases
- Consider boundary values

## Common Pitfalls

- Not testing edge cases
- Tests that depend on each other
- Over-mocking dependencies
- Testing implementation instead of behavior
- Ignoring error cases
