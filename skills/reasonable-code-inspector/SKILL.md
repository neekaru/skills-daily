---
name: reasonable-code-inspector
description: Read-only review of code readability and AI-associated overengineering patterns, including wrapper explosion, generic naming, redundant defensive checks, speculative abstractions, and misleading complexity. Detect unnecessary complexity without claiming to identify AI authorship; report evidenced findings and preserve meaningful boundaries.
---

# Reasonable Code Inspector

> Correct code is not necessarily understandable code.

## Purpose

Inspect code for **reasonableness to a human maintainer**, not merely syntax correctness or compliance with a formatting convention. Find code that appears functional but forces readers to spend unnecessary effort understanding its intent, flow, and effects.

This skill is **read-only**. Do not modify files, refactor code, or quietly apply fixes. Propose targeted changes for a separate implementation step.

## Principles

1. **Intent first.** Function, variable, and type names should communicate what they mean in the project's domain.
2. **Complexity must earn its place.** Every wrapper, class, interface, helper, and indirection should have a concrete benefit.
3. **Shorter is not automatically simpler.** Avoid rewarding dense one-liners, clever tricks, or compressed control flow.
4. **Small functions are not automatically better.** Splitting logic into tiny functions can make a reader jump between files unnecessarily.
5. **Respect boundaries.** Security checks, validation, transactional behavior, error handling, domain rules, and stable APIs may justify separation.
6. **Evaluate in context.** Follow call sites, implementations, tests, conventions, and relevant docs before declaring an abstraction redundant.
7. **Do not manufacture issues.** Reporting "No substantive readability issues found" is a valid outcome.
8. **Prefer evidence over taste.** Explain the reader's actual difficulty and the expected improvement; do not enforce one personal style.
9. **Uncertainty is explicit.** If only a snippet is visible, describe limitations rather than infer hidden implementations.
10. **Detect complexity, not authorship.** A pattern common in AI-assisted coding is not proof that AI wrote it, and human-written code can show the same pattern.
11. **Patterns are leads, not verdicts.** Confirm whether each suspicious structure creates genuine comprehension cost in this codebase before flagging it.

## When to Use

- Review AI-generated or human-written code that is technically valid but hard to follow.
- Review pull requests for cognitive complexity or overengineering.
- Audit naming, control flow, hidden side effects, and responsibility boundaries.
- Decide whether proposed refactoring would genuinely improve understanding.
- Scan files and cross-file call chains for patterns often seen in AI-generated or AI-assisted code, without attempting to attribute code authorship.

## Inspection Workflow

1. **Understand the goal:** Determine what the code is supposed to do and who maintains it.
2. **Read the relevant context:** Inspect the implementation, callers, tests, contracts, and existing project conventions when available.
3. **Trace the happy path and important failure paths:** Identify where a reader must jump, infer state, or decode generic terminology.
4. **Scan for AI-associated coding patterns:** Use the detection catalog below as a heuristic, not an authorship detector. Trace implementations across files before flagging.
5. **Look for reasonable-code issues:** Use the checklist below. Distinguish demonstrated issues from possible issues.
6. **Check preservation constraints:** Note security, performance, API compatibility, and business behavior that any refactor must preserve.
7. **Prioritize findings:** Report the few changes with the greatest likely readability benefit. Do not nitpick.
8. **Recommend, do not rewrite:** Give an actionable direction, optionally a very small illustrative snippet, but leave code changes to the Writer.

## Review Checklist

| Dimension | Diagnostic question | Common warning signs |
| --- | --- | --- |
| Intent clarity | Can a reader tell what this operation does? | `processData`, `doStuff`, `handleThing` |
| Domain naming | Do names reflect the actual business concept? | `data`, `item`, `result2` used where a precise name exists |
| Indirection | How many layers must a reader follow to see the behavior? | Chains of pass-through wrappers |
| Abstraction value | Does this abstraction isolate variability or express an important boundary? | Interfaces with no meaningful use; speculative factories |
| Flow clarity | Can the reader trace success and failure paths? | Deep nesting; hidden branching; tangled early mutations |
| Responsibility | Does each unit have a cohesive purpose? | Functions mixing validation, persistence, formatting, and transport |
| State and effects | Are mutations, I/O, and errors visible or easy to find? | Surprise side effects; misleading pure-sounding names |
| Duplication | Is repeated code truly the same rule? | Copy-paste with identical semantics, or premature consolidation of different rules |
| Conventions | Is the code consistent with the local codebase? | New architectural pattern for one trivial operation |
| Testability | Can the behavior be verified without unnatural setup? | Heavy scaffolding for a simple pure operation |

## AI-Associated Coding Pattern Detection

The purpose of this scan is **to find unnecessary complexity, not determine whether a human or AI authored the code**. Do not output an "AI probability," "AI-written" label, or claim of provenance based on stylistic traits.

| ID | Pattern | Signal to investigate | Important false-positive check |
| --- | --- | --- | --- |
| RC-AI-001 | Wrapper Explosion | Several functions only forward parameters through layers | Do the wrappers preserve a stable API, authorization boundary, telemetry, or domain vocabulary? |
| RC-AI-002 | Abstraction Inflation | Factory + interface + class hierarchy for a single simple behavior | Does the codebase need plugins, multiple implementations, dependency inversion, or testing seams? |
| RC-AI-003 | Generic Naming | `processData`, `executeTask`, `handleThing`, `data2` | Is the generic name actually an established framework or domain convention? |
| RC-AI-004 | Defensive Coding Overload | Repeated checks, fallback chains, or null guards on guaranteed values | Are these checks guarding a genuine trust boundary or unstable external input? |
| RC-AI-005 | Premature Extensibility | Strategy/plugin/config registry for one fixed current case | Is variability already required or explicitly planned? |
| RC-AI-006 | Comment Noise | Comments paraphrase the very next line or narrate obvious syntax | Do they instead explain non-obvious *why*, constraints, or tradeoffs? |
| RC-AI-007 | Unnecessary Async | Promise/async or parallelism added to a simple synchronous path | Is concurrency improving throughput, latency, or integration with an async API? |
| RC-AI-008 | Pattern-for-Pattern's-Sake | Design patterns added without a concrete problem solved | Is the pattern mandated by architecture or beneficial for modularity? |
| RC-AI-009 | Excessive Error Handling | Catch-and-rethrow chains and repeated logging with no value added | Is the layer translating errors, adding context, cleaning up, or preserving an invariant? |
| RC-AI-010 | Indirection Maze | A reader must cross many files to find one simple action | Are separate files meaningful architectural or security boundaries? |
| RC-AI-011 | Configuration Theater | Many options that are never used or always fixed | Are these contractual settings or supported deployment variants? |
| RC-AI-012 | Test Theater | Tests assert implementation details or only mirror mocks without checking behavior | Are they legitimate characterization, contract, or regression tests? |

### Detection Procedure

1. Identify entry points, call sites, implementations, and relevant tests. Inspect multiple files when the project permits it; single-file scans have limited confidence.
2. Mark candidate patterns with IDs and concrete code evidence. Do not treat frequency alone as proof of harm.
3. Ask what purpose the structure serves, including performance, security, extensibility, compatibility, accessibility, failure handling, and team conventions.
4. Compare the actual code with a **simpler plausible alternative**. If the alternative would be less clear, less safe, or more coupled, do not report a problem.
5. Report only justified findings, with their reader impact, confidence, location, and preservation constraints.
6. Group related manifestations of one root problem rather than flooding the report with repeated warnings.
7. If context is missing, mark the item **Needs context** rather than guessing. If nothing material remains, state **No substantive readability issues found**.

### Reporting Rules

- Give each substantiated pattern a stable ID such as `RC-AI-002`; IDs indicate *pattern categories*, not provenance.
- Include evidence (function names, call chain, or snippet), the cognitive cost, a realistic refactoring direction, and what must not change.
- Use `High`, `Medium`, or `Low` for **impact**, and `High`, `Medium`, or `Low` for **confidence** independently.
- Never infer that code is AI-generated from comments, formatting, variable names, or repetitive architecture alone.
- Never recommend removing validation, logging, error propagation, authorization, or abstractions without evaluating why they exist.

### Example Pattern Finding

```yaml
id: RC-AI-002
pattern: Abstraction Inflation
severity: medium
confidence: high
location: src/Services/UserFormatter.php:18
observed: "One factory creates the only implementation of an interface that only calls trim()."
reader_cost: "Readers must inspect three types to understand one normalization operation."
recommendation: "Consider using a direct, aptly named operation if there is no stable interface contract."
preserve: "Public API compatibility and any planned alternative implementations."
authorship: "Not assessed"
```

The example path and line number are illustrative only; use actual locations in real reports.

## Severity

- **High:** Confusing structure obscures consequential behavior, security checks, error propagation, or state changes.
- **Medium:** Meaningful cognitive burden or unjustified abstraction likely to impede maintenance.
- **Low:** Minor naming or readability issue worth changing only when already working nearby.
- **No finding:** No material issue supported by the visible code.

Severity is about **maintainability and comprehension impact**, not an invented numeric readability score.

## Output Format

Use this structure (omit empty sections):

```markdown
# Reasonable Code Inspection

## Context
- Scope: ...
- Assumptions/limitations: ...

## Findings
### [Medium] RC-AI-001 Wrapper Explosion — `src/Users.php:42`
- **Evidence:** `getUser()` only delegates to `fetchUser()`, which delegates again.
- **Pattern:** `RC-AI-001` (not a claim of AI authorship).
- **Why this is confusing:** Readers must traverse multiple names to discover one operation.
- **Suggested direction:** Consider calling the meaningful boundary directly.
- **Preserve:** Existing error handling and public API consumers.
- **Confidence:** High / Medium / Low

## What Should Stay
- Explain boundaries or explicit steps that are justified.

## Verdict
- No substantive issues / Targeted cleanup recommended / Structural simplification recommended
```

When no issue is found, say so clearly and briefly. Use real locations only when available; never invent file paths or line numbers.

## Anti-Patterns in the Review Itself

- Calling every one-line function bad.
- Calling every long function bad.
- Counting classes/functions/lines as the sole basis for a verdict.
- Removing a domain-level abstraction because it looks like a wrapper.
- Assuming correct behavior without checking tests or contracts.
- Recommending changes that conceal errors or weaken input validation.
- Treating preference-based formatting as a high-severity finding.
- Automatically modifying code during inspection.

## Definition of Done

A maintainer can understand **what is confusing, why it matters, and what change would help**, without the review introducing unsupported claims or changing code.
