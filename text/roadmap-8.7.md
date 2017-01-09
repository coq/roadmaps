# Schedule

## Towards a 6-month cycle

During the Working Group on December 15, 2016, the Coq development team
decided that the release cycle would be 10-month long for Coq 8.7, and
6-month long for subsequent versions. To bootstrap this sorter release cycle,
only fully ready features will be merged to 8.7, postponing others to 8.8.

## 8.7 schedule

- Feature freeze: June 1st, 2017.
- First beta version expected around June 15th, 2017.
- Final version expected around October 15th, 2017.

# Changes already decided

## A more regular scheme for version numbers

Minor versions will now be numbered `X.Y.Z` instead of `X.YplZ`. The semantics
of minor versions (in terms of compatibility constraints and integration
policies) is left unchanged.

## OCaml version

Coq 8.7 will require OCaml version 4.02.1.

# Preliminary roadmap

This is a summary of the new features being worked on. No integration decision
has been taken yet.

## New features

- [ ] Ltac 2 (Pierre-Marie Pédrot)
A new version of the tactic language, with types and precise semantics.

[CEP](https://github.com/coq/ceps/blob/master/text/008-typed-ltac.md)
[Branch](https://github.com/ppedrot/coq/tree/ltac2)

- [ ] PPX (Pierre-Marie Pédrot)
A new implementation of extension macros (e.g. for plugins), not relying
on CamlpX.

[Branch](https://github.com/ppedrot/coq/tree/ppx-test)

- [ ] Unified unifications (Matthieu Sozeau)
Use a single unification algorithm for tactics and type checking (getting
rid of unification.ml in favor of evarconv). Also removes the need for `Meta`s
(an old construct for existential variables, subsumed by `Evar`s).

[Branch](https://github.com/mattam82/coq/tree/unifall)


- [ ] Induction-recursion / induction-induction (Matthieu Sozeau)
Simultaneous definition of a type and a function on that type / of a type and
a family of types.

[Branch](https://github.com/mattam82/coq/tree/IR)

- [ ] Deriving

- [ ] Tactic improvements (Hugo Herbelin)

- [ ] Attributes
A new way to declare modifiers for vernacular commands.

- [ ] Native integers and arrays (Maxime Dénès, Benjamin Grégoire)
Primitive machine integers and (persistent) arrays for efficient computations.

## Implementation cleanups

- [ ] Econstr (Pierre-Marie Pédrot)
Static typing ensuring evar insensitivy.

[CEP](https://github.com/coq/ceps/blob/master/text/010-econstr.md)
[Branch](https://github.com/ppedrot/coq/tree/econstr)

- [ ] Merge Function / Program Fixpoint / Equations (Matthieu Sozeau)

- [ ] Removing cooking from the kernel (Maxime Dénès, Enrico Tassi,
Hugo Herbelin)

- [ ] Reals library (Guillaume Melquiond)

- [ ] Printing infrastructure (Emilio J. Gallego)

- [ ] `coq_makefile` reimplementation (Enrico Tassi)
A new implementation of `coq_makefile` based on templates instead of `print`.

[Branch](https://github.com/gares/coq/tree/feature/coq_makefile2)

## Deprecation

- [ ] `-notop`
- [ ] `-compat` flags (we support 2 versions back)
- [ ] `Function`
If merged with `Program Fixpoint` and `Equations`, only one should remain
non deprecated.
- [ ] Review compatibility options

