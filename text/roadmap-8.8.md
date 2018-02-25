# Estimated schedule

- Feature freeze on March 8th, 2018 (pull requests must be fully reviewed by this date, no exception will be made)
- 8.8+beta1 release on March 15th
- 8.8.0 release on April 16th (the 15th is a Sunday)

# OCaml version

# Roadmap

## New features

- [ ] Unified unifications (Matthieu Sozeau)

  Use a single unification algorithm for tactics and type checking (getting
  rid of unification.ml in favor of evarconv). Also removes the need for `Meta`s
  (an old construct for existential variables, subsumed by `Evar`s).

  [Branch](https://github.com/mattam82/coq/tree/unifall)
  
  Status: The code is here, infrastructure [PR I](https://github.com/coq/coq/pull/930) 
  is backwards-compatible and should be merged in 8.8. [PR II] on [apply] and the following
  work on porting [elim], [rewrite] etc introduce incompatibilities. The current plan
  is to make a kind of `Future` plugin (part of the Coq codebase or not) to get feedback
  on these, before we make a switch (probably a Coq 9 version will be waranted then).
  Will be looking at this with the help of @herbelin in december / january.

- [ ] Native integers and arrays (Maxime Dénès, Benjamin Grégoire)

  Primitive machine integers and (persistent) arrays for efficient computations.
  
  [Branch](https://github.com/maximedenes/coq/tree/vm-clambda)
  
  Status: More cleaning and pluging Coqprime and Bignums on the data structures.
 
- [ ] Deriving binding (Matthieu Sozeau)

  This adds an official [Derive] command to Coq, on which plugins can plug their own
  derivers (implemented as they like, in ML, using tactics, ...)
  
  Status: This is quite trivial, I'll send an email to coqdev and potentially interested
  third-parties (e.g. QuickChick devs) about the API we need to agree on for that by the 
  end of december.
  
- [X] `2: { ... }` (Théo Zimmermann)

  [PR](https://github.com/coq/coq/pull/6551)

- [ ] Significant indentation mode (Théo Zimmermann)

  Status: uncertain (lower priority than the point above and the one below).

- [ ] `Lemma :=` (Théo Zimmermann)

  Status: Work started

- [ ] Prolog cuts in `eauto` (Matthieu Sozeau, Cyril Cohen)

  Status: this is implemented as [PR 6285](https://github.com/coq/coq/pull/6285)

- [ ] Numeral notations (Pierre Letouzey, Daniel de Rauglaudre)

- [ ] Warning on generated names (Hugo Herbelin)

- [ ] Custom entries / tables for notations (Hugo Herbelin)

- [ ] Attributes (Vincent Laporte, Maxime Dénès)

  Status: implementation work started, cf Vincent's presentation at the WG
  
## Implementation cleanups

- [ ] SSReflect intro patterns (Enrico Tassi, Maxime Dénès)

  Status: Enrico started to work on it

- [ ] Porting `eauto` to `typeclasses eauto` code (Théo Zimmermann)

  Status: will start working on it again after the 8.7.1 release (Dec 15th).
  
  [PR](https://github.com/coq/coq/pull/721)

- [ ] Universe naming and context API

  Cleanup of global and local universe naming (put globals in nametab, local
  as bound variables in polymorphic universe contexts). Cleanup of the API for
  declaring mono/poly constants and inductives etc..
  
  Status: [PR 890](https://github.com/coq/coq/pull/890) implements proper global
  universe naming, @ppedrot has patches to fix bound (polymorphic) universe naming,
  PR incoming. The cumulative inductive PR already started cleaning the declaration
  API for constants and inductives, more cleanups and bugfixes related to this 
  are being done by @SkySimmer, @ppedrot and @mattam82 as well.

- [ ] Review mod\_subst (Pierre Letouzey)

- [ ] Separation of section and goal contexts (Hugo Herbelin)

- [ ] Primitive projections API (Matthieu Sozeau)

  The idea is to do a pass of cleanup on the declaration of prim projs and their
  representation in the kernel, to see if we can move the compatibility-related
  hacks higher in the system.
  
  Status: no time to look at this now. Help wanted. 

- [X] Remove state from futures (Emilio J. Gallego)

## Deprecation

## Tools

- [ ] Extraction improvements (Pierre Letouzey)

- [ ] Locations for `Proof using` error messages (Yves Bertot)

## Documentation

- [ ] Porting to Sphinx

Status: WIP, see https://docs.google.com/document/d/1Yo7dV4OI0AY9Di-lsEQ3UTmn5ygGLlhxjym7cTCMCWU/edit

- [ ] Improve SSReflect chapter (Assia Mahboubi)

## Libraries

- [ ] Stdlib split (Reals?) (Pierre Letouzey)

## Travis
