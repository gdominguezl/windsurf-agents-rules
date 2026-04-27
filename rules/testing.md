---
trigger: glob
globs: "**/*.test.ts"
description: Testing standards with strict no-mocks policy
---

# Testing Standards

## Naming

- Names in English, written as business rules, not implementation details.
- Descriptive: what is being tested and what is expected.
- No technical verbs (`returns`, `calls`, `throws`); use domain verbs (`considers`, `validates`, `accepts`, `calculates`).
- `describe`: `The [Subject]` — the subject is a domain concept, not a class name.
- `it`: `[action] [object] [condition]`. The `describe + it` sentence should read as a spec.

```typescript
describe('The Invoice Calculator', () => {
  it('applies a 10% discount for orders above 100€', () => { ... });
  it('does not allow negative quantities', () => { ... });
});
```

## Structure — AAA

- **Arrange**: set up context and data.
- **Act**: run the action under test.
- **Assert**: verify the outcome.
- Separate the three sections with a blank line.

## Mocks

- **Never use mocks without asking the Tech Lead first.**
- Mocks hide design problems and couple tests to implementation.
- Before proposing one, ask: "Can I solve this with a simpler design?"

## Example

```typescript
// ⚠️ Coupled to implementation
test('calculatePrice returns 90', () => {
  expect(calculatePrice(100, 10)).toBe(90);
});

// ✅ Describes the business rule
test('calculates price with discount applied to given product', () => {
  const originalPrice = 100;
  const discountPercentage = 10;

  const finalPrice = calculateDiscountedPrice(originalPrice, discountPercentage);

  expect(finalPrice).toBe(90);
});
```

## Non-Negotiable Rules

### Never

1. Delete an existing test — if it fails, the implementation is wrong.
2. Modify a test to make the implementation pass — tests define expected behavior.
3. Make tests depend on each other — keep them isolated.
4. Use mocks without asking the Tech Lead first.

### Always

1. Keep tests isolated and independent.
2. Fix the implementation when a test fails, not the test.
3. Ask the Tech Lead before deleting or significantly changing a test.
