---
name: ocaml
description:
  The rules to apply and the principles to follow to write idiomatic Ocaml code
---

# Explicit Functional OCaml

Build OCaml programs whose structure is a faithful refinement of the problem
domain. Make domain values, alternatives, invariants, state transitions,
outcomes, dependencies, and effects visible in types and module interfaces. Keep
stable decisions public and representation choices private so that the
implementation can change without spreading change to its clients.

The rules should prioritize readability and maintainability over writeability We
expect our code to be around for long periods of time, and we consider a little
bit of extra effort applied during code writing to be worth it in the long run
if the style rule improves readability.

The goals: consistency, readability, and encouraging good API design. Regardless
of the specific choices made, it helps everyone to have these choices made
beforehand.

To optimize for readability we enforce `let%bind` and `let%map` preprocessor
syntactic forms from JaneStreet for any result/option type function composition.

When a module exports ( `>>=` ) and `return` functions, these shall be used
systematically and consistently with the corresponding let syntax (extensions).

We also optimize for a proper, "just right" level of abstraction, and to stay at
this optimal high level with declarative DSLs based on a set of operators for a
given type, exactly as mathematical abstract notions (Set, Group, etc.) were
captured and defined.

The main goal is to maintain an one-to-one correspondence between the layers of
abstractions of the Problem Domain and the layers of code that represents and
implements them. Never reach to the lower layers of data representation.

This begins with consistently naming things, using the vocabulary which has been
collected from the terminology the domain experts are using for naming,
describing and communicating the things and processes within their domain.

Write down a pure mathematical model of the Problem Domain, using Algebraic Data
Types and Finite State Machines (explicit State Transitions) using the
programming language to define executable specifications. This is all it takes.

Prefer small, focused modules; ideally, but not necessarily, a single file
(focusing on one abstraction). If your code can be split up into smaller
modules, do so.

"One Concept -- one Abstract Data Type (Abstract Interface) -- One Module" is
the most important rule, from which everything else (extreme Separation of
Concerns, extreme Loose Coupling, extreme Modifiability, extreme Testability)
simply follows.

## The rules to systematically and consistently apply

- follow the established idealistic tradition of pure functional programming.
- referential transparency is the ability to replace an expression with its
  value without affecting the result of a computation.
- the code you write must ALWAYS be verified to be referentially transparent by
  the compiler.
- OCaml is beautiful. OCaml is elegant, simple, and graceful. Aesthetics do
  matter (it is a sign of using Just Right types in the design).
- The OCaml code you write MUST be elegant, stylish and tasteful.
- systematically use modules are used to encapsulate implementations behind
  interfaces.
- apply the practice of avoiding repetition by factoring out commonality into
  Abstract Data types (DRY).
- package each ADT into a small, orthogonal, loose-coupled, self-contained
  module (Liskov).
- optimize for readability, extreme modularity and thus testability.
- modules decouple the system design decisions and implementation details into
  separate tasks that can be carried out largely independently.
- modules are abstracted by giving specifications of what they are supposed to
  do.
- interface informs clients of the available functionality without revealing the
  details of the implementation.
- the module interface is a contract between the client and the implementer.
- design a module interface as an abstract description of what the module can
  do, explicitly stating and verifying the algebraic laws in the tests.
- modules should be used only through their declared interfaces, according to
  the algebraic laws stated in the docstrings.
- always write a separate `.mli` file, never place interfaces with the code.
- strictly follow the `Module.t` naming convention for types and `Module type S`
- think hard about module structure and interfaces early, right from the start.
- idiomacity, uniformity, consistency of the code are the highest priorities.
- same "things" must always look the same, different "things" differently.
- design function arguments for partial application and for pattern-matching.
- systematically and consistently use the `= function` syntax for variants.
- always use the `let module M = ... in ...` instead of opening a module.
- isolate side-effects code into specialized loosely coupled modules.
- reread, simplify, and clarify at every stage of creation. Use your head!
- always use the type system to specify and check constraints and invariants.
- make illegal states unrepresentable by making constraints explicit with types.
- when designing an interface always specify representation and FSM invariants
  with Algebraic data types.
- use Sum-types to specify possible states and Product-types to group values.
- the Universal algebraic pattern is a sum-of-products, possible of size one.
- systematically use Phantom types to encode explicit state transitions of FSMs.
- avoid imperative constructs and mutation as much as possible, in principle.
- systematically and consistently use explicit error sum-types as expected
  "unhappy" paths through the code.
- use GADTs as indexed types to specify and encode additional Domain
  constraints.
- modules, abstraction barriers (interfaces), modularity are the main concerns.
- mimic the clean and idiomatic Standard Library code as much as possible.
- write validation tests using `qcheck` and `alcotest` right from the start.
- validate the public interfaces only, NOT the implementation details.
- define the interfaces first, then validation and law tests, before any code.
- a program is written once, modified ten times, and read 100 times.
- elegance is not optional, it is a sign of the right algebraic design.
- factor out snippets of repeated code by defining them in separate functions
- usage must appear in the module's interface that exports it, not in the
  program that implements it.
- encode constraints directly in the types instead of docstrings.
- data types is the most important aspect, they define the shape of the code.
- the more type-constraints (expressed in types) -- the better.
- use assertions as much as possible, they let you avoid verbose comments.
- an assertion is preferable to a comment because it's more trustworthy.
- use exceptions ONLY when there is no way to return an expected result.
- in all other cases consistently use `result` and `option` types.
- systematically and consistently use the JaneStreet ppx_let package for
  `option` and `result` types.
- all error handling must be explicit using the clauses of the variant types.
- avoid naked primitive type values (strings, ints), use domain-specific types.
- use the syntactic forms to mimic traditional mathematical notation.
- local identifiers must be brief and should be reused from one function to
  another.
- use the `x::xs` naming convention systematically and consistently.
- always give the same name to function arguments which have the same meaning.
- use destructuring let bindings systematically and consistently.
- the destructuring let must be exhaustive.
- avoid `open` directives, using instead the qualified identifier notation.
- use only `let open Module ... in` where appropriate.
- the use of unqualified identifiers is ambiguous and gives rise to
  difficult-to-detect semantic errors.
- it would be less readable if you had to qualify all the identifiers.
- use only Algebraic Data Type -- variants, records/tuples, GADTs.
- each distinct Domain Concept must have a distinct module.
- each module must define a proper fully Abstract Data Type.
- each Abstract Data Type is an encapsulated Algebraic Data Type.
- never leak any representation or implementations details.
- systematically and consistently use the `Module.t` naming convention.
- export only smart-constructors and functions following algebraic laws.
- define functions by clauses using exhaustive pattern-matching.
- Never be afraid of overusing pattern-matching!
- avoid nonexhaustive pattern-matching constructs in principle.
- systematically and consistently use the `= function` syntax.
- always order the function arguments for meaningful partial application.
- always use exhaustive pattern-matching instead of conditionals.
- you must always subdivide your programs into coherent modules.
- for each module, you must always explicitly write an interface.
- for each interface, you must document the things defined by the module
- always write a separate `.mli` file, never place interfaces with the code.
- always define an abstract interface for each module in an `.mli` file.
- always write a docstring of the public functions in an `.mli` file.

- Use variants/datatype declarations and exhaustive pattern matching for
  alternatives; use records/products for simultaneous facts. Prefer
  `result`/`option` for expected outcomes and translate exceptions at a
  deliberate outer boundary.
- Use GADTs to mimic dependent types or specialize the constructors. Use GADTs
  to model domain-specific errors (one clause -- one error). Constraints on type
  parameters may change depending on the value constructor.
- Put each coherent abstraction behind an `.mli` interface or an ML signature.
  Keep a validated representation abstract/private and expose constructor and
  observer functions. Use functors for a real parameterised module contract, not
  to create indirection around a single use.
- Prefer immutable bindings and values. Confine `ref`, arrays, and imperative
  loops to small owned algorithms where mutation is the clearer or measured
  choice; do not mix their changing invariant with domain decision logic.
- Use qualified module names or narrow local opens by default. Names in a module
  interface are documentation and should be stable, descriptive, and consistent.

## Pure functional modeling and Software Architecture

- use the ubiquitous language the domain experts are using to name things.
- a word may legitimately mean different things in different bounded contexts.
- keep the naming consistent with the language of the domain and literature.
- make sure the levels of abstraction are as high as in expert's mental models.
- avoid "Services", "Managers", "Factories" and other unrelated nomenclature.
- everything in the Domain Model is mathematics - sums, products, functions.
- each Domain Concept is either a sum-type, a product-type or a function.
- use FSMs with explicit state transitions, states defined defined as sum-types.
- FSMs state transitions are defined as GADTs -- the Interpreter pattern.
- state transitions are pure functions on the Algebra of the corresponding FSM.
- use the Interpreter pattern systematically and consistently.
- define the algebras of Intents, FSM state transitions and Algebraic Effects.
- always begin with defining an Initial Algebra (as a pure AST) to interpret.
- produce pure descriptions of intended Algebraic Effects to be performed.
- use the Hexagonal/Clean architecture implicitly -- without naming it.
- avoid "adapters", "ports" and other polluting names, but use them implicitly.
- Algebraic Effects are "executed" only inside Adapters at the edge.
- every Adapter is an Interpreter of a particular DSL from the Domain Model.
- Interpreter are composed and vertically partitioned into distinct layers.
- several layers of composed interpreters is our our software architecture.
- implemented using the Tagless Final pattern.

## The Tagless Final pattern

- implement the pure Domain Model using the Tagless Final pattern.
- do not re-implement a VM within the Ocaml language VM and runtime. Reuse it.
- Ocaml syntactic forms are an implicit AST. Ocaml's functions are transitions.
- use the modules to define abstract interfaces (ADTs) of the Interpreters.
- parameterize these modules (we do not have type-classes or traits in Ocaml).
- implementation of such abstract interface produce an Interpreter of Effects.
- intended pure Algebraic Effects are just variant types (Initial Algebras).

## The causal chain

Design and review in this order:

> domain mental model -> ubiquitous language -> conceptual distinctions ->
> domain algebra -> type algebra -> public interface -> laws and examples ->
> executable specification -> implementation

The order matters. A data representation is an implementation decision; it must
not silently become the public model of the domain.

### Universal principles

1. **Name the domain before naming the code.** Collect the vocabulary, events,
   states, actors, units, and rules used by domain experts. The same word may
   mean different things in different bounded contexts; do not merge them
   because their runtime representations happen to coincide.
2. **Let data lead.** Model a finite choice with a sum, simultaneous facts with
   a product/record, and a transformation with a function. Nest these
   constructions as the domain requires. Prefer a named domain type to a raw
   primitive whenever the values have a distinct meaning, unit, or invariant.
3. **Use modules as abstraction barriers.** A module owns a coherent hidden
   decision, not every noun or every function. Its public interface states what
   clients may depend on; its implementation may be replaced while that contract
   remains true.
4. **Make alternatives and outcomes explicit.** Match variants exhaustively and
   name the meaningful cases. Use `option` when absence needs no explanation;
   use `('a, error) result` when the caller must distinguish a normal rejection
   or failure. A Boolean, a sentinel, or a free-form error string is not an
   adequate replacement for a domain outcome.
