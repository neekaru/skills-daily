
---
name: reasonable-code-writer
description: Write or refactor source code so that its intent, naming, control flow, and abstractions are easy for humans to understand without unnecessary complexity. Use for implementing simple maintainable solutions or applying readability findings while preserving behavior, security, and existing contracts.
---

# Reasonable Code Writer

> Make code as reasonable as the people writing and maintaining it.

## Purpose

Produce code that is **correct, understandable, and no more complex than necessary**. Prefer a straightforward design that a competent maintainer can trace and modify, rather than code optimized for line count, cleverness, or the appearance of architectural sophistication.

This skill **may edit code** when asked to implement or refactor. Do not expand the requested scope solely to make unrelated code more stylistically consistent.

## Principles

1. **Name by intent:** Prefer `calculateInvoiceTotal()` to `processData()` when that is the operation's actual purpose.
2. **Solve the actual problem:** Do not add speculative architecture for imagined future requirements.
3. **Justify abstractions:** Add a helper, class, interface, or layer only if it improves readability, reuse, a meaningful boundary, testability, or change isolation.
4. **Prefer obvious control flow:** Favor explicit steps and predictable errors over clever nesting and compressed expressions.
5. **Keep cohesive responsibilities:** Do not merge unrelated validation, authentication, persistence, and rendering merely to reduce function count.
6. **Do not worship short code:** A few extra lines are worthwhile when they make intent or behavior clearer.
7. **Preserve behavior:** Maintain public APIs, outputs, side effects, failure modes, and compatibility unless changes are explicitly requested.
8. **Preserve protections:** Never simplify away necessary authorization, sanitization, validation, transactions, logging, or error handling.
9. **Fit the project:** Prefer existing conventions and standard language facilities over introducing a new framework or dependency without need.
10. **Explain tradeoffs briefly:** Make clear why meaningful structure was retained or removed.

## When to Use

- Implement a feature with minimal necessary design.
- Simplify overengineered AI-generated code.
- Apply a Reasonable Code Inspector report.
- Rename unclear functions and variables while maintaining compatibility.
- Reduce unnecessary delegation, branching, or duplication.

## Writing and Refactoring Workflow

1. **Establish intent and constraints:** Understand the user requirement, expected behavior, public contracts, runtime, and project conventions.
2. **Read the existing code:** Review relevant definitions, callers, tests, and boundaries. If context is incomplete, make minimal assumptions and disclose them.
3. **Choose the simplest adequate design:** Prefer direct language features and a readable sequence of steps.
4. **Name clearly:** Use domain-specific, unambiguous names that distinguish actions from queries and indicate side effects where useful.
5. **Remove only unjustified complexity:** Simplify pass-through wrappers, nested conditionals, unused generalization, and needless scaffolding where safe.
6. **Retain meaningful seams:** Keep validation, domain logic, I/O, transactions, security, and error handling visibly separated when necessary.
7. **Preserve compatibility:** Update references carefully; avoid breaking public names, serialization formats, database interfaces, or external integrations.
8. **Verify:** Run available tests, static analysis, formatting, or targeted checks. If you cannot run them, state that explicitly. Never claim tests passed unless they did.
9. **Review the result as a reader:** Could a maintainer understand the main path without chasing a chain of trivial abstractions?
10. **Summarize:** State what changed, what remained deliberately separate, and how correctness was verified.

## Preferred Patterns

### Meaningful names

Avoid vague descriptions:

```php
function processData($a, $b) {
    return $a * $b;
}
```

Prefer intent and domain language:

```php
function calculateItemSubtotal(float $unitPrice, int $quantity): float {
    return $unitPrice * $quantity;
}
```

### Avoid redundant forwarding

Before:

```php
function getUser($id) {
    return fetchUser($id);
}

function fetchUser($id) {
    return UserRepository::find($id);
}
```

Possible simplification **only if callers and boundaries allow**:

```php
$user = UserRepository::find($id);
```

Do **not** remove `getUser()` if it is a stable public contract, a domain boundary, or useful encapsulation.

### Preserve meaningful steps

```php
function saveUserProfile(array $input): void {
    $profile = validateUserProfile($input);
    UserRepository::save($profile);
}
```

A separate `validateUserProfile()` can be reasonable because validation is an important responsibility. Do not inline it just to reduce the number of functions.

## Decision Rules

Before introducing or retaining an abstraction, ask:

- Does it give an operation a clearer domain name?
- Does it hide a genuinely complex implementation?
- Does it enforce an important boundary or invariant?
- Does it support actual reuse or variation, rather than hypothetical reuse?
- Does it reduce the effort needed to understand or safely change the code?

If all answers are **no**, prefer a more direct solution, provided behavior and compatibility are preserved.

Before removing an abstraction, ask the same questions again. **Do not collapse a useful boundary merely because it adds a function call.**

## Scope and Safety Rules

- Do not edit unrelated files for cosmetic consistency.
- Do not replace existing tests with weaker tests to make a refactor pass.
- Do not silently alter authorization, data handling, concurrency, retries, or error semantics.
- Do not introduce dependencies, design patterns, or frameworks without a demonstrated need.
- Do not replace a clear multi-step implementation with a cryptic one-liner.
- Do not claim a refactor is behavior-preserving if that has not been established.
- For high-risk or compatibility-sensitive changes, prefer smaller incremental refactors and explicit review.

## Output Format

When implementing or refactoring, summarize as:

```markdown
## Changes
- ...

## Why This Is More Reasonable
- ...

## Intentionally Preserved
- ...

## Verification
- Tests/checks run: ...
- Not verified / remaining risks: ...
```

For small requests, keep the summary proportionate. Do not create lengthy reports to justify a two-line change.

## Definition of Done

The code solves the requested problem, expresses its intent clearly, preserves the necessary behavior and protections, follows the project's conventions, and contains **no complexity without a defensible purpose**.
