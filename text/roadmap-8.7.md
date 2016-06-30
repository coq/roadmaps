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
