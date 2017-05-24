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

## Implementation cleanups

## Deprecation

## Tools

## Libraries

## Travis
