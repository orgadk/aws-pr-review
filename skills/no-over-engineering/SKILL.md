---
name: no-over-engineering
description: Flag unnecessary complexity, premature abstractions, and gold-plating in pull requests. Trigger phrases: "review PR", "check complexity", "simplify code", "over-engineered".
---

# no-over-engineering

Flag unnecessary complexity, premature abstractions, and gold-plating in PRs.

## Steps
1. For each new abstraction (class, interface, factory, strategy pattern), verify it is used in at least 2 places. Single-use abstractions are a warning.
2. Check for premature generalization: config flags with only one value, plugin systems with one plugin, base classes with one subclass.
3. Flag dependencies added for trivial tasks (e.g., a full date library for one date format, a DI framework for a 50-line Lambda).
4. Check Lambda functions: if a Lambda does more than one conceptual thing, flag for single-responsibility review.
5. Check IaC: CDK/CFN constructs should not add more than necessary. No "future-proofing" resources not in scope.
6. Summarize findings as **blocking** (significant maintenance burden) or **suggestion** (minor simplification available).

## Rules
- Simple > clever. If a 10-line solution exists, a 100-line solution needs justification.
- No frameworks for things the standard library handles.
- No scope-expanding TODOs — separate PR or ticket.
