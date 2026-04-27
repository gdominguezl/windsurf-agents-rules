---
trigger: always_on
description: Coding standards for names, functions, and classes
---

# Coding Standards

## Names

- Pronounceable English, no technical abbreviations (abbreviations only in narrow-scope lambdas).
- No redundant prefixes/suffixes (`I`, `Impl`, `Abstract`).
- Concrete; avoid catch-all words (helper, util, manager) unless the domain requires them.
- No type information in the name — the IDE shows it.
- One concept, one name — no synonyms for the same thing.
- Fit the grammar: `isPaidInvoice`, `sumOfNumbersIn(expression)`.
- Encode negation in the name (`doesNotExist`) rather than using `!`.
- Nouns for classes/modules, verbs for methods/functions, adjectives only for interfaces.
- Allowed generic suffixes: `DTO`, `Repository`, `Factory`, `Mapper`, `UseCase`, `Service`.
- Prefer a self-explanatory name over a comment; comment only when code cannot express intent.
- Constants: `camelCase`, not `SCREAMING_SNAKE_CASE`.
- No underscore prefix for private members.
- No magic strings — use enums or literal types for fixed sets.

```typescript
// ⚠️
const d = 42;
const usrCtrl = new UsrCtrl();

// ✅
const daysUntilExpiration = 42;
const userAuthenticator = new UserAuthenticator();
```

---

## Functions

1. **Single responsibility**. The function does exactly what its name says. 10–15 lines is a smell, not a rule.
2. **Verb names** that precisely describe the action.
3. **Arity 0–3**. If higher, group parameters in a typed object.
4. **No boolean/config flags**. Split into specific functions (`show()` / `hide()`).
5. **At most one optional parameter**; avoid if possible.
6. **Guard clauses and early returns**. Keep indentation and complexity low.
7. **Readable conditions**: extract compound booleans into explanatory variables or functions; prefer affirmative conditions; avoid `else`.
8. **Separate control flow from business logic**. Delegate calculations to dedicated functions. Avoid `for`.
9. **Declarative style** (`map` / `filter` / `reduce`) when it improves readability — not dogma.
10. **Prefer pure functions**; avoid side effects when possible.
11. **CQS**: commands mutate and return nothing; queries return and don't mutate. Know the justified exceptions (e.g. insert-returning-id).
12. **Readability beats micro-optimization**. Optimize only when measured.
13. **No function-level comments**.
14. **No mutation of collections** (`push`, `splice`, …). Return new arrays.
15. **Constants live next to their usage**, inside the function that uses them.

```typescript
// ⚠️ Query that mutates
function totalWithDiscount(pct: number): number {
  this.appliedDiscount = pct;
  return this.total * (1 - pct / 100);
}

// ✅ Command + Query
function applyDiscount(pct: number): void { this.appliedDiscount = pct; }
function calculateTotal(): number {
  return this.baseTotal * (1 - this.appliedDiscount / 100);
}
```

---

## Classes and Modules

1. **Minimum scope, maximum cohesion**. A constant used only by one function lives inside it.
2. **Simple constructors**. Move validation to a factory method and make the constructor private; skip the factory when there's no validation.
3. **Class layout**: public constructors → private constructor → public API → private methods. In TypeScript, auto-inject properties via the constructor.
4. **Encapsulation by default** (`private`, no unnecessary exports).
5. **Law of Demeter + Tell, Don't Ask**.
6. **No anemic models**. Classes hold behavior (DTOs only at boundaries).
7. **Complete objects at construction**. Avoid setters; limit getters.
8. **No singletons**. Global-state instances belong to the app's factory module.
9. **Composition over inheritance**.
10. **Domain-specific types** with behavior, especially in the domain layer.

```typescript
// ⚠️ Ask
if (order.customer().address().city() === "Madrid") {
  order.applyDiscount(10);
}

// ✅ Tell
order.applyDiscountForCity("Madrid", 10);
```
