---
trigger: always_on
description: TDD cycle (RED-GREEN-REFACTOR) with TPP for the GREEN step
---

# Test-Driven Development

Before starting: clarify requirements with the Tech Lead, list cases as a TODO in the test file, and order them from simplest (happy path) to hardest (edge cases).

## 🔴 RED — write a failing test

- Pick the simplest pending case.
- Write the test; it won't compile.
- Add the minimum stub (empty function, `return null`) to compile.
- Run the test and confirm it fails for the right reason.

## 🟢 GREEN — make it pass

- Use **TPP** (see below) to pick the simplest transformation that turns the test green.
- No premature optimization; just make it work.

## 🔵 REFACTOR — clean up

- Remove duplication (apply Rule of Three before abstracting).
- Check names, clarity, and function responsibility.
- Follow `@.windsurf/rules/coding-standards.md`.
- Keep all tests green.
- Mark the case done and re-evaluate: is the next pending case still the simplest? If not, reorder.

---

## Transformation Priority Premise (TPP)

Pick the lowest-numbered transformation that makes the test pass:

1. `{}` → `nil`
2. `nil` → constant
3. constant → richer constant
4. constant → scalar (variable)
5. statement → statements
6. unconditional → `if`
7. scalar → array
8. array → container
9. statement → recursion
10. `if` → `while`
11. expression → function
12. variable → assignment (mutation)

---

## Example

```typescript
// Cases: 1) empty list  2) one price  3) multiple prices

// Test 1
test('calculates total of empty price list', () => {
  expect(calculateTotal([])).toBe(0);
});

// GREEN — TPP {} → constant
function calculateTotal(prices) { return 0; }

// Test 2
test('calculates total of a single price', () => {
  expect(calculateTotal([100])).toBe(100);
});

// GREEN — TPP constant → scalar (guard clause)
function calculateTotal(prices) {
  if (prices.length === 0) return 0;
  return prices[0];
}

// Test 3
test('calculates total of multiple prices', () => {
  expect(calculateTotal([100, 50, 25])).toBe(175);
});

// GREEN — declarative style (coding-standards §9) clearer than recursion/loop
function calculateTotal(prices) {
  return prices.reduce((sum, price) => sum + price, 0);
}
```

---

## Simple Design (Kent Beck)

In order:
1. Passes all tests.
2. Expresses intent clearly.
3. No duplication of knowledge (wait for 3rd occurrence before abstracting).
4. Minimum number of elements.
