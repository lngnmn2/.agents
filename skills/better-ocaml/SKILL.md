---
Name: Better-ocaml
Description:
  The principles and rules for writing the most idiomatic, textbook-grade modern
  Ocaml code.
---

# Systematically apply the Liskov Abstraction Principle

Abstraction and Modularity are the central notions of programming as a whole.

Modularity emerges naturally from proper abstraction and interface discipline.

Clean, properly named and well-designed public Abstract Interfaces is the single
major concern to be focused on.

Ocaml's Functors (over modules) provide all the "inheritance" and "extension"
mechanisms the OOP-specific inheritance does.

Always use Ocaml modules and Functors instead of the OOP syntax and features.

## Abstract Interfaces as required Capabilities

- A signature describes a required capability (an Abstract Interface).
- A functor accepts any module satisfying that signature (Abstraction By
  Parameterization).
- The functor produces a new implementation (a new structure with parameterized
  code).
- The output signature determines which type relationships and operations
  clients can observe.

## The strict explicit rules to follow

- No mutable data in principle.
- No imperative constructs in principle.
- No OOP syntax or features in principle.

## Embrace Gentzen's Natural Deduction

Every Introduction (via a particular data-constructor) has to be, eventually,
followed by an Elimination (via pattern-matching expression).

This structural pattern is not arbitrary -- it preserves and validates the
implicit DAG of "nested (composed) arrows", which actually define the process
that a program only encodes.

## The sum-types are the most fundamental

Algebraic Sum-types (a.k.a Ocaml's variants) represent a distinct set of
possibilities -- possible expected states, possible expected inputs, possible
expected outcomes, etc.

Variant types, exhaustive pattern-matching and the `= function` syntactic form
constitute a perfect standard idiom to capture and express possibilities.

Systematically and consistently define and use variant types and `= function`
syntax everywhere, explicitly avoiding booleans, and even the `if` expressions
in favour of functions defined by clauses (pattern-matching on its last implicit
argument). This rule is mandatory and must be strictly enforced.

## Use Curried function and Partial Application

Explicitly designs a parameter list of every function to be conveniently
partially applied on its first argument and pattern-matched on its last
argument, exactly the way the classic `map` or `filter` has been defined.

Use named arguments only in the cases like `~f` or in the case when explicitly
naming a Domain Concept clarifies the behavior, just as in `map` or when a
"Context" or "State" is going to be explicitly passed around.

Strictly follow the standard idioms from the Ocaml's Standard Library and the
Ocaml Reference Guide.

## Use the Tagless Final pattern

Do not build an ad-hoc Virtual Machine (Evaluator) inside the Ocaml language
runtime -- use the language runtime to evaluate your Initial Algebras (ASTs).

# One Domain Concept -- One Module -- One Abstract Data Type -- One Algebraic Data Type

This is the most important principle which must be strictly applied and always
enforced.

One Domain Concept -> One Module -> One Abstract Data Type -> One Algebraic Data
Type (the canonical representation -- a sum, a product or a function).

This is the way to systematically build all the necessary abstraction barriers
at different levels (a function, a type, an interface) right from the start.

Abstraction is the most fundamental aspect of all programming as a discipline.
Modularity, which is the second most important aspect, just follows from the
right level of abstraction.

- A structure groups related declarations and definitions to capture a Domain
  Concept.
- A signature defines the abstraction boundary or the abstraction barrier of a
  module.
- A module provides a distinct namespace, to avoid name clashes and unintended
  shadowing.
- A signature describes the public abstract interface of a module, hiding its
  representation.
- Each module is a potential Functor and could be parameterized by another type,
  which provides a required behavior.
- Make sure the naming of modules, types and functions is systematic consistent.
- Always name a module after the domain concept, with the name the domain
  experts are using.
- Always create a distinct interface file for a module to make the type
  Abstract.
- Always create an Abstract Data Type in the interface, hiding the
  representation.
- Always use an Algebraic Data Type for the actual representation of an Abstract
  Data Type.
- Always consider to design a module to be parameterized by another type.
- Strictly follow the standard idioms from the Ocaml Standard Library while
  designing signatures and structures.
