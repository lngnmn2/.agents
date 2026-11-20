---
name: explicit-functional-ocaml
description:
  The rules and principles for writing idiomatic, textbook-grade functional
  OCaml. Algebraic domain models behind abstract interfaces, explicit errors,
  Tagless Final effects, law-based tests. Use whenever the task writes, reviews,
  refactors, or designs OCaml code (.ml, .mli, dune), even if the user never
  mentions style.
---

# Explicit Functional OCaml

Build programs whose structure is a faithful refinement of the problem domain.
Make domain values, alternatives, invariants, state transitions, outcomes,
dependencies, and effects visible in types and module interfaces. Keep stable
decisions public and representation choices private. The implementation then
changes without spreading change to its clients.

Readability and maintainability beat writeability. Our code lives for years. A
little extra effort at writing time is worth it whenever a rule improves
reading. A program is written once, modified ten times, and read a hundred
times.

The goals: readability, good API design, and above all idiom, uniformity, and
consistency. Whatever the choice, making it beforehand helps everyone.

The main goal: a one-to-one correspondence between the layers of the problem
domain and the layers of code. Never reach through to a lower layer's
representation. Name things in the vocabulary of the domain experts. Write a
pure mathematical model of the domain with algebraic types and finite state
machines (FSMs) with explicit transitions. The language then defines an
executable specification. This is all it takes.

Stay at the "just right" level of abstraction. Build declarative DSLs from a
small set of operators per type, exactly as mathematics captured Set and Group.

## The one rule

One Domain Concept -- One Module -- One Abstract Data Type -- One Algebraic Data
Type.

Everything else follows: extreme separation of concerns, loose coupling,
modifiability, testability. Apply it at every level: function, type, interface.

- A structure groups the declarations of one Domain Concept.
- A signature is its abstraction barrier. It hides the representation.
- A module is a namespace. No clashes, no unintended shadowing.
- The abstract type in the `.mli` has one algebraic representation in the `.ml`:
  a sum, a product, or a function.
- Name the module after the concept, in the experts' words.
- Split a module whenever it can be split. Ideally one file, one abstraction.

## The causal chain

Design and review in this order:

> domain mental model -> ubiquitous language -> conceptual distinctions ->
> domain algebra -> type algebra -> public interface -> laws and examples ->
> executable specification -> implementation

The order matters. A representation is an implementation decision. It must not
silently become the public model of the domain. Write the interfaces first, then
the law tests, before any code.

## Universal principles

1. **Name the domain before naming the code.** Collect the vocabulary, events,
   states, actors, units, and rules of the domain experts. One word may mean
   different things in different bounded contexts. Never merge them because
   their representations coincide.
2. **Let data lead.** A finite choice is a sum. Simultaneous facts are a
   product. A transformation is a function. A recursive structure is a recursive
   type. Nest them as the domain requires.
3. **Compute by the structure of the data.** Use pattern matching and structural
   recursion. The compiler checks the cases.
4. **Make alternatives and outcomes explicit.** Name each case. Use `option` for
   absence, `result` for failure. A boolean, a sentinel, or a string is not a
   domain outcome.
5. **Use modules as abstraction barriers.** A module owns one hidden decision,
   not every noun or function. Its interface is the contract. The implementation
   may change while the contract holds.
6. **Parameterize at the right level.** Functions abstract over values,
   parameterized types over types, functors over modules.
7. **Earn every abstraction.** It hides a representation, enforces an invariant,
   or parameterizes something real. "Future flexibility" is not a reason. Start
   with variants, records, functions, and signatures. Add generality
   (polymorphic variants, higher-rank polymorphism) only when it expresses a
   real domain requirement more clearly.

## Naming

- Use the ubiquitous language of the domain experts. Stay consistent with the
  domain and its literature.
- Keep names at the abstraction level of the experts' mental models.
- Name each module after its Domain Concept. Never `Service`, `Manager`,
  `Factory`, `Adapter`, `Port`, `Util`.
- Names in an interface are documentation: stable, descriptive, consistent.
- Same things look the same. Different things look different.
- Same meaning, same name for a function argument, in every function.
- Local identifiers are brief and reused from function to function. A list is
  `xs`, its element `x`, a list of lists `xss`.
- Name the main type `t` and the signature `S`: `Module.t`, `module type S`.

## Types

- Data types come first. They define the shape of the code.
- Encode constraints in types, not docstrings. The more, the better.
- Make illegal states unrepresentable.
- State what types cannot as an assertion. An assertion beats a comment: it is
  checked.
- Represent data only with algebraic types: variants, records, tuples.
- A sum specifies the possible states. A product groups values. The universal
  pattern is a sum of products. The sum may have one case.
- No naked `string` or `int` for a domain value. Give it a domain type. A type
  alias is not one: make the type abstract.
- Parse, don't validate. A smart constructor returns the valid value or an
  error.

## State machines

- Model a process as an FSM with explicit transitions.
- A state is a sum type. When the state must be known statically, index the type
  with a phantom type: `val submit : draft t -> submitted t`. An illegal
  transition then fails to type check.
- A transition is a GADT constructor indexed by its source and target states.
  The transitions form an Initial Algebra.
- Applying a transition is a pure function on that algebra.

## Modules and interfaces

- Subdivide every program into coherent modules. Think hard about modules and
  interfaces first, right from the start.
- Write a separate `.mli` for every module. Never place an interface with the
  code.
- The `.mli` declares an abstract type. It never leaks the representation.
- Export only smart constructors, observers, and functions that follow algebraic
  laws.
- Export `equal` and `pp` with every `t`, and `compare` when order is
  meaningful. Tests need them.
- Document every public item in the `.mli`: its purpose, its laws, its usage.
  Show usage in the exporting `.mli`, not in the implementation.
- Use a module only through its interface, according to the laws in its
  docstrings.
- Mirror the Standard Library and the OCaml reference manual: names, argument
  order, signatures.
- Qualify identifiers. Never `open` a module at the top level. Unqualified names
  are ambiguous and hide semantic errors.
- Alias a long path with `let module M = ... in`.
- Open locally only where qualification hurts readability: operators and
  let-syntax. Use `let open M in` or `M.( ... )`, in the smallest scope.

## Abstraction

- By specification: the interface says what a module does, never how. Client and
  implementer then work independently. The laws belong to the specification.
- By parameterization: a signature describes a required capability. A functor
  accepts any module satisfying it and produces a new structure with
  parameterized code.
- The output signature decides which type relationships clients observe. Export
  a type equality (`with type`) only when clients need it.
- Ask what each module requires. Name the requirement as a signature. Write the
  functor only when the requirement is real, never as indirection around a
  single use.
- Functors replace OOP inheritance and extension.

## Sums and pattern matching

- Sum types are the most fundamental: a closed set of possible states, inputs,
  outcomes.
- Define functions by clauses. Use `= function` everywhere. Keep functions
  short.
- Never be afraid of overusing pattern matching!
- Match exhaustively. Name each case. A wildcard hides the next constructor.
- Destructure with `let`, exhaustively. Make partial-match warnings errors in
  every build profile.
- No `if`. Decide by matching a variant, not a `bool`. Mandatory. Strictly
  enforced.
- `bool` survives only in Stdlib-shaped predicates: `equal`, `List.filter`.
- Every introduction meets an elimination (Gentzen). A constructor is eventually
  followed by a match. This keeps valid the implicit DAG of composed arrows,
  which defines the process the program encodes.

## Functions

- Order parameters for partial application on the first and pattern matching on
  the last, as `map` and `filter` do. Configuration first, subject last.
- Use labels only for `~f`, for an explicit context or state, or where a label
  names a Domain Concept and clarifies the behavior.
- Define operators per algebra. Mimic mathematical notation.
- Factor repeated code into functions (DRY).

## Errors

- Expected absence is `option`. Expected failure is `('a, error) result`.
- An error is a variant. One clause, one error. No strings, no sentinels.
- Handle every error explicitly, clause by clause.
- Raise only when the interface specification admits no meaningful value: a
  broken invariant, never an expected outcome.
- Stdlib partial functions raise. Use the `_opt` forms. Never `List.hd` or
  `Option.get`: match.
- Translate exceptions into `result` at one deliberate outer boundary: an
  Interpreter.
- Each module owns its `error` type. A layer translates the errors of the layer
  below.

## Composition

- Compose `option` and `result` with `let%bind` and `let%map` (Jane Street
  `ppx_let`). Never with nested `match`, `bind` chains, or `let*`.
- A monad module exports `return`, `>>=`, `>>|`, and the `Let_syntax` that
  `ppx_let` expects: an inner `Let_syntax` with `bind`, `map`, `both`.
- `let%bind` is `>>=`. `let%map` is `>>|`. Use the let forms in direct style,
  the operators point-free. Same module, same meaning.

## Purity

- Follow the tradition of pure functional programming.
- Referential transparency: an expression is replaceable by its value without
  changing the result of a computation.
- The OCaml compiler cannot verify it. Approximate it. The pure core calls no
  ambient effect (I/O, clock, randomness, environment) and depends on no I/O
  library. Effects reach it only through signatures.
- Isolate side effects in specialized, loosely coupled modules.
- Prefer immutable bindings and values. No mutable data and no imperative
  constructs (`ref`, `mutable`, arrays, `for`, `while`), in principle.
- Allow mutation only inside a small, owned, measured algorithm hidden behind
  the interface. It never mixes with domain decision logic.
- No classes, objects, or inheritance, in principle.
- OCaml 5 effects are invisible to the type system. Use handlers only inside an
  Interpreter at the edge.

## Algebras and Interpreters

- Everything in the Domain Model is mathematics: sums, products, functions.
- Define the algebras of Intents, FSM state transitions, and Algebraic Effects.
  Begin each as a pure Initial Algebra: a variant, or a GADT where the
  constructors refine the result type.
- GADTs live here and nowhere else: intents, transitions, and the
  domain-specific errors indexed by them.
- Produce pure descriptions of intended effects. The core never performs them.
- Effects execute only inside Interpreters at the edge. An Interpreter gives one
  algebra its meaning and lives in its own module.
- Stack Interpreters in layers. Each translates its algebra into the one below.
  Composed, layered Interpreters are the software architecture.
- Decode at the boundary. Wire formats stop at the Interpreter. Only domain
  types cross inward.
- Hexagonal architecture is implicit. Never name it in code.

### The Tagless Final pattern

- Implement the pure Domain Model in Tagless Final style.
- A module type is the abstract interface of an Interpreter. Parameterize the
  model by it. OCaml has no type classes or traits.
- Initial Algebras describe. Tagless Final Interpreters give meaning.
- OCaml's syntactic forms are the AST. Its functions are the transitions. Its
  runtime is the evaluator. Never build another VM: no variables, lambdas, or
  conditionals of your own.

## Tests

- Write tests with `qcheck` and `alcotest`, right from the start.
- Test the public interface only. Never the implementation.
- State each algebraic law in the `.mli`. Verify it with a `qcheck` property.
  Examples are `alcotest` cases.
- Write a law suite once, as a functor over the signature. Run it on every
  implementation (Liskov): each must substitute for the others.

## Elegance

OCaml is beautiful, simple, and graceful. The code you write must be elegant,
stylish, and tasteful. Elegance is not optional. It signals Just Right types and
the right algebraic design.

Reread, simplify, and clarify at every stage.
