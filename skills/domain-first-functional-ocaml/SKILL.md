---
name: domain-first-functional-ocaml
description:
  Design and review OCaml code from domain concepts, algebraic types, explicit
  interfaces, and pure functions.
---

# Domain-first functional OCaml

Use this guide when designing or reviewing OCaml code. Respect the repository's
conventions and the user's explicit constraints.

## Governing rule

Derive the model from the domain, functions from types, abstraction from
contracts, and confidence from executable laws.

## Design sequence

1. **Understand the domain.** Start from requirements and examples. Collect its
   terms, actors, events, states, units, and rules. Keep concepts distinct when
   the domain distinguishes them, even if their representations match. Use the
   domain's language in code.

2. **Model the algebra.** Use sums for alternatives, products for facts that
   hold together, and functions for transformations. Use recursion for recursive
   structures. Give a value a named type when its meaning, unit, or invariant
   differs from a primitive. Make invalid states unrepresentable when the domain
   has a real constraint; do not encode speculative constraints.

3. **Specify abstractions.** Give each module one coherent abstraction or hidden
   design decision. Do not force one module per noun or one type per module. Use
   signatures as client contracts. Add an `.mli` when a module is a public or
   meaningful abstraction boundary; keep internal representations private where
   clients must not depend on them. Name a module's principal abstract type `t`.
   Name capability signatures `S`.

4. **Derive functions from types.** Follow the shape of the data with structural
   recursion and pattern matching. Match finite variants exhaustively. Prefer
   `= function` when clauses inspect the final argument. Use conditionals when
   they state a simple Boolean choice more clearly. Order curried arguments to
   support natural reuse and partial application.

5. **Represent outcomes and effects.** Use `option` for unexplained absence. Use
   `result` with a domain-specific error type when callers must distinguish
   failure cases. Reserve exceptions for failures the interface cannot represent
   meaningfully, and handle them at a deliberate boundary. Do not use sentinels
   or free-form strings in place of domain outcomes.

6. **Keep decisions pure; isolate effects.** Express domain decisions and state
   transitions as pure functions where practical. Keep I/O and other effects at
   explicit boundaries. Prefer immutable values. Confine mutation to a small,
   owned algorithm when it makes the implementation clearer or materially more
   efficient. The compiler checks types; it does not prove referential
   transparency.

7. **Choose abstraction when it earns its cost.** Start with ordinary variants,
   records, functions, and module signatures. Use phantom types or GADTs when
   they enforce a meaningful static constraint. Use an initial algebra when
   commands need inspection, transformation, or replay. Use tagless-final
   interfaces when abstract capabilities and multiple interpreters clarify the
   design. Use direct functions when neither pattern adds value. Parameterize
   modules with functors only for a real reusable contract.

8. **Implement and check the contract.** Write examples and laws before
   implementation where they clarify the design. Test observable behavior and
   public guarantees. Use property-based tests when a law or invariant can be
   stated generally. Follow the repository's test tools. Tests support
   confidence; they do not prove the absence of defects.

9. **Refine the result.** Let the compiler check type constraints and exhaustive
   matches. Reread names, interfaces, and functions. Remove accidental layers,
   duplicated rules, and details that clients do not need.

## Idioms

- Prefer qualified names. Use a narrow local `let open` only when it improves
  clarity; avoid global opens that obscure where names come from.
- Use `let%map` and `let%bind` consistently when the project uses `ppx_let` for
  `option` and `result`. Otherwise follow the project's established composition
  style.
- Use ordinary variants for domain errors and expected outcomes. Do not reach
  for GADTs merely to make each error a constructor.
- Keep module, type, and function names stable, descriptive, and consistent. One
  concept should have one clear owner, even when its implementation spans
  several declarations.
