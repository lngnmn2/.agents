---
name: idealistic-haskell-tradition
description: Just right way to write Haskell as mathematical formalism to express executable specifications as programs
---

- The hierarchy of abstractions of the problem Domain must have one-to-one structural correspondence to a hierarchy of Abstract Data Types and corresponding Modules.
- The "hierarchical, layered conceptual structure of complexity of the problem domain" has to have an one-to-one correspondence with the layered structure of the code, partitioned into orthogonal, loose-coupled modules, to achieve extreme separation of concerns and extreme modulariry and testability
- proper abstraction and modularity, just right abstract interfaces at the just right level of abstraction (not too low, consistent with the domain) is the key to proper software architecture
- GHC's hierarchy of typeclasses which implements arithmetic, equality, ordering, show, read, etc. is the golden standard of modularity and separation of concerns -- it is an optimum -- can be refined or simplified further -- perfectly matches the structural hyerarchy of pure mathematical abstractions
- one Domain abstraction -- one Abstract Data Type (Liskov) -- one distinct Module
- use naming/vocabulary of the problem Domain systematically and consistently -- how domain experts call/label/name "things" and describe "processes"
- write pure mathematics over structured values (of Algebraic Data Types), just like numbers
- every domain concept at an appropriate level of abstraction must have its own distinct Algebraic Data Type
- avoid monads in the pure mathematical Domain Model code, in principle.
- use monads ONLY for effectful actions/operations -- IO, randomness, non-determinism.

- Reader or Parser is a pure function, which returns a product (a tuple) of values: same input -- same output, not a monad.
- A State is not a Monad, it is a pure function that takes and returns a product (a tuple) of values: same input -- same output.
- refs are referentially transparent, explicitly-passed as parameters, closures (structured values). Use them as "variables".
- Writer is a monad, since it encapsulates a side-effect.
- strictly NO monadic combinators in the pure Domain Model code. Any use of a monadic combinator in the pure mathematical Domain model is the sign of a failure.
