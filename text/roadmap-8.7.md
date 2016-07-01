The purpose of this page is to summarize the different new features and
modifications that are going to be part of Coq 8.7.

# General Implementation

- [ ] Bytes/String PR by E. Arias. WIP, proposal is to split it in smaller
      chunks and do renamings.

      [EGJA] This is postponed until 4.02 is the default compiler.

- [ ] :question: [PR#185](https://github.com/coq/coq/pull/185)
   Remove unused printing infrastructure and duplication (E. J. Gallego)

  [EJGA] This is up to PMP/Enrico, I did this PR because the stuff is
  abandoned and it was indeed confusing people looking at it. It also
  saves 24K of bytecode and removes a duplicate code path.

# Kernel

- [ ] Optimizations in the (lazy) reduction machine, saving allocations

# Tactics

- [ ] :exclamation: :question: Fix semantics of pattern-matching in
   Ltac (non-linear patterns, difference between hyps and goal and
   hyps) (M. Sozeau)

  Idea: use a warning for using conversion instead of syntactic checks
   and output a warning about deprecation.
  No problem a priori.
  Decision: if we have time to evaluate before June 15th.
  Postponed to 8.7.

- [ ] :exclamation: Remove the double induction tactic which was deprecated.

- [ ] :exclamation: :question: [PR#140](https://github.com/coq/coq/pull/140): Iff as a proper
  connective (H. Herbelin)

  Problem of coercions. Compatibility issue..
  - Evaluate on contribs.
  Decision: wait on.

# Vernacular

- [ ] [PR#85](https://github.com/coq/coq/pull/85) Printing in cbv/cbn
 - Is it subsumed by other features?
 - Can it be made into a plugin?
 Decision:
   - change to implement only the hooks part by 8.6, as this can be
     generalized. Ask Thomas about the changes needed and update.
   - Ask for users to tell us and advertise it.

 Should provide a hook for a print function (and maybe more general, as it may
 be used for profiling).