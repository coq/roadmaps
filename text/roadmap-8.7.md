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

This is a summary of the work in progress. No integration decision
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

- [ ] Induction-recursion / induction-induction (Matthieu Sozeau)

  Simultaneous definition of a type and a function on that type / of a type and
  a family of types.

  [Branch](https://github.com/mattam82/coq/tree/IR)

- [ ] Deriving

- [ ] Tactic improvements (Hugo Herbelin)

- [ ] Attributes

  A new way to declare modifiers for vernacular commands.

- [ ] Flambda support (Emilio J. Gallego)

  + [Bug 5270: Add FLambda support](https://coq.inria.fr/bugs/show_bug.cgi?id=5270)

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

  Ready to go.
  + [CEP](https://github.com/coq/ceps/blob/master/text/009-unified-pretty-printing.md)
  + [PR Updates to the Pretty Printing Infrastucture](https://github.com/coq/coq/pull/390)
  + [Bug 4669: printing of dependent evars in the goal does not respect Set Printing Width](https://coq.inria.fr/bugs/show_bug.cgi?id=4669)

- [ ] Safe String patch (Emilio J. Gallego and others)

  Ready to go, needs a choice wrt to `CEmitcodes` but that should take a 5 minutes discussion.

  + [PR](https://github.com/coq/coq/pull/134)
  + [Bug 4278: Add support for building with -safe-string](https://coq.inria.fr/bugs/show_bug.cgi?id=4278)

- [ ] STM (Emilio J. Gallego)

  + [PR: Port Toplevel to the STM API](https://github.com/coq/coq/pull/441)
  + [PR: META update](https://github.com/coq/coq/pull/436)
  + [Merged PR](https://github.com/coq/coq/pull/403)
  + Pending: STM state / co-tailrecursive cleanup.
  + [Bug 4972: Feedbacks can refer to state id not yet seen](https://coq.inria.fr/bugs/show_bug.cgi?id=4972)
  + [Bug 4789: Fix duplicated error handling paths](https://coq.inria.fr/bugs/show_bug.cgi?id=4789)
  + [Bug 5053: Tactic parser broken for "Load" under CoqIDE](https://coq.inria.fr/bugs/show_bug.cgi?id=5053)
  + [Bug 5091: Avoid adhoc parsing in the STM](https://coq.inria.fr/bugs/show_bug.cgi?id=5091)
  + [Bug 5101: XML protocol, invalid state id behavior](https://coq.inria.fr/bugs/show_bug.cgi?id=5101)
  + [Bug 5118: Stm / XML protocol returns error on "hidden" virtual state if in a nested command and resilient error mode](https://coq.inria.fr/bugs/show_bug.cgi?id=5118)

## Deprecation

- [ ] `-notop`
- [ ] `-compat` flags (we support 2 versions back)
- [ ] `Function`

  If merged with `Program Fixpoint` and `Equations`, only one should remain
  non deprecated.

- [ ] Review compatibility options

## Tools

- [ ] `coq_makefile` reimplementation (Enrico Tassi)

  A new implementation of `coq_makefile` based on templates instead of `print`.

  [Branch](https://github.com/gares/coq/tree/feature/coq_makefile2)

- [ ] External CoqIDE (Emilio J. Gallego)

  + [POC](https://github.com/ejgallego/coqide-exp)

- [ ] External Checker (Emilio J. Gallego)

  + [POC](https://github.com/ejgallego/coq-checker)

## Libraries

- [ ] Improve Coq Bundling of commonly used libs (Emilio J. Gallego)

  + [Bug 5028: Improvements to namespace management of ML modules](https://coq.inria.fr/bugs/show_bug.cgi?id=5028)
  + [Bug 5257: Deprecate non qualified imports for 8.7](https://coq.inria.fr/bugs/show_bug.cgi?id=5257)

## Travis

- [ ] Travis CI
  + [PR add fiat-parsers](https://github.com/coq/coq/pull/447)
  + [PR Ltac Plugin compat fixes](https://github.com/coq/coq/pull/452)
  + [PR Backport to 8.6](https://github.com/coq/coq/pull/453)
  + Needs policy.
  + Needs a better makefile and overlay system.
