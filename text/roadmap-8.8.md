# Schedule

# OCaml version

# Preliminary roadmap

## New features

- [ ] Unified unifications (Matthieu Sozeau)

  Use a single unification algorithm for tactics and type checking (getting
  rid of unification.ml in favor of evarconv). Also removes the need for `Meta`s
  (an old construct for existential variables, subsumed by `Evar`s).

  [Branch](https://github.com/mattam82/coq/tree/unifall)

- [ ] Native integers and arrays (Maxime Dénès, Benjamin Grégoire)

  Primitive machine integers and (persistent) arrays for efficient computations.
  
  [Branch](https://github.com/maximedenes/coq/tree/vm-clambda)
  
  Status: rebase/fixes in progress until December 4th, then more cleaning and pluging Coqprime and Bignums on it.

- [ ] Induction-recursion / induction-induction (Matthieu Sozeau)

  Simultaneous definition of a type and a function on that type / of a type and
  a family of types.

  [Branch](https://github.com/mattam82/coq/tree/IR)
  
- [ ] Deriving binding (Matthieu Sozeau)

- [ ] `2: { ... }` (Théo Zimmermann)

- [ ] Significant indentation mode (Théo Zimmermann)

- [ ] `Lemma :=` (Théo Zimmermann)

- [ ] Prolog cuts in `eauto` (Matthieu Sozeau, Cyril Cohen)

- [ ] Numeral notations (Pierre Letouzey, Daniel de Rauglaudre)

- [ ] Small inversions (Thierry Martinez, Hugo Herbelin)

- [ ] Warning on generated names (Hugo Herbelin)

- [ ] Custom entries / tables for notations (Hugo Herbelin)

- [ ] Attributes (Maxime Dénès, Vincent Laporte)

  Status: preliminary work ongoing (locality, vernac control), real work starting Nov 27th. Will update Arnaud's CEP.

## Implementation cleanups

- [ ] SSReflect intro patterns (Enrico Tassi, Maxime Dénès)

  Status: will start work on January 22nd

- [ ] Porting `eauto` to `typeclasses eauto` code (Théo Zimmermann)

- [ ] Universe naming and context API

- [ ] Review mod\_subst (Pierre Letouzey)

- [ ] Porting functional induction to Equations (Thierry Martinez, Matthieu Sozeau)

- [ ] Separation of section and goal contexts (Hugo Herbelin)

- [ ] Primitive projections API (Matthieu Sozeau)

- [ ] Remove state from futures (Emilio J. Gallego)

## Deprecation

## Tools

- [ ] Extraction improvements (Pierre Letouzey)

- [ ] Locations for `Proof using` error messages (Yves Bertot)

- [ ] Integration of the API for Elpi (Enrico Tassi)

  Status: will start work on January 15th

- [ ] Port plugins to `coq_makefile` (Enrico Tassi, Maxime Dénès)

  Status: will start work on January 15th

## Documentation

- [ ] Porting to Sphinx

- [ ] Improve SSReflect chapter (Assia Mahboubi)

## Libraries

- [ ] Stdlib split (Reals?) (Pierre Letouzey)

## Travis
