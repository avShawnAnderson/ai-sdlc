# Code Refactoring Skill

## Description

Use AI to refactor code for better maintainability, readability, and performance while preserving functionality.

## When to Use

- Code is difficult to understand or maintain
- Functions are too long or complex
- Code violates DRY principle
- Performance optimization needed
- Technical debt reduction

## Prerequisites

- Working code with known behavior
- Test coverage (recommended)
- Understanding of desired improvements

## Step-by-Step Instructions

### Step 1: Analyze Current Code

Review the code and identify issues:
- Long functions
- Repeated code
- Complex conditionals
- Poor naming
- Missing abstractions

### Step 2: Define Refactoring Goals

Be specific about what you want:
- Extract methods
- Simplify logic
- Improve naming
- Add design patterns
- Reduce complexity

### Step 3: Request Refactoring

Example prompt:
```
Refactor this code to improve [specific aspect]:

Goals:
- [goal 1]
- [goal 2]

Maintain:
- Current functionality
- Existing API/interface
- Test compatibility

[PASTE CODE]
```

### Step 4: Review Changes

Check that refactoring:
- Preserves functionality
- Improves readability
- Follows best practices
- Doesn't add unnecessary complexity

### Step 5: Test Thoroughly

Run existing tests to verify:
- All tests still pass
- No functionality broken
- Performance acceptable
- Edge cases handled

## Example

### Before Refactoring

```javascript
function processOrder(order) {
  // Validate
  if (!order.id || !order.items || order.items.length === 0) {
    throw new Error('Invalid order');
  }
  
  // Calculate total
  let total = 0;
  for (let i = 0; i < order.items.length; i++) {
    if (order.items[i].quantity && order.items[i].price) {
      total += order.items[i].quantity * order.items[i].price;
    }
  }
  
  // Apply discount
  if (order.discountCode) {
    if (order.discountCode === 'SAVE10') {
      total = total * 0.9;
    } else if (order.discountCode === 'SAVE20') {
      total = total * 0.8;
    } else if (order.discountCode === 'SAVE30') {
      total = total * 0.7;
    }
  }
  
  // Apply tax
  total = total * 1.08;
  
  // Save to database
  db.orders.insert({
    id: order.id,
    items: order.items,
    total: total,
    date: new Date()
  });
  
  // Send email
  email.send({
    to: order.customerEmail,
    subject: 'Order Confirmation',
    body: 'Your order total is $' + total
  });
  
  return total;
}
```

### After Refactoring

```javascript
class OrderProcessor {
  constructor(db, emailService, discountService) {
    this.db = db;
    this.emailService = emailService;
    this.discountService = discountService;
  }

  processOrder(order) {
    this.validateOrder(order);
    const subtotal = this.calculateSubtotal(order.items);
    const discounted = this.applyDiscount(subtotal, order.discountCode);
    const total = this.applyTax(discounted);
    
    this.saveOrder(order, total);
    this.sendConfirmation(order.customerEmail, total);
    
    return total;
  }

  validateOrder(order) {
    if (!order?.id) {
      throw new Error('Order ID is required');
    }
    if (!order.items?.length) {
      throw new Error('Order must contain items');
    }
  }

  calculateSubtotal(items) {
    return items.reduce((sum, item) => {
      if (!item.quantity || !item.price) {
        throw new Error('Invalid item: quantity and price required');
      }
      return sum + (item.quantity * item.price);
    }, 0);
  }

  applyDiscount(amount, discountCode) {
    if (!discountCode) return amount;
    
    const discount = this.discountService.getDiscount(discountCode);
    return amount * (1 - discount);
  }

  applyTax(amount, taxRate = 0.08) {
    return amount * (1 + taxRate);
  }

  saveOrder(order, total) {
    this.db.orders.insert({
      id: order.id,
      items: order.items,
      total,
      date: new Date()
    });
  }

  sendConfirmation(email, total) {
    this.emailService.send({
      to: email,
      subject: 'Order Confirmation',
      body: `Your order total is $${total.toFixed(2)}`
    });
  }
}
```

## Benefits of Refactoring

- **Readability**: Clear, single-purpose methods
- **Testability**: Easy to test individual methods
- **Maintainability**: Changes isolated to specific methods
- **Reusability**: Methods can be reused
- **Extensibility**: Easy to add new features

## Tips

- Refactor in small steps
- Run tests after each change
- Keep commits small and focused
- Use meaningful names
- Extract constants and magic numbers
- Reduce nesting depth

## Common Pitfalls

- Changing functionality while refactoring
- Over-engineering simple code
- Breaking existing tests
- Ignoring performance implications
- Not testing after refactoring
