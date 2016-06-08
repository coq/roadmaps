The purpose of this page is to summarize the different new features and
modifications that are going to be part of Coq 8.6.

? indicates points which were not discussed/undecided/whose description
is incomplete, or which required more discussion/in-depth explanations 
(at the time of the last WG).

The other items were agreed to be integrated in the next release at the last WG.

! indicates changes introducing incompatibilities w.r.t. 8.5 (not taking into
account plugin interfaces) 

# General Implementation

- [x] ! We settled on requiring ocaml >= 4.01.0 for 8.6.
  There is a known issue with 4.01.0 and camlp4 where
  Coq compilation fails, this case is now detected early
  in the configure script.

   [EJGA] Note that this will likely break Debian packages, as they ship a patched Ocaml 4.01.0 thus configure will fail.
 
- [x] Switch to using ocamlfind for finding compilers (new dependency)
  
- [ ] Module name conflicts with the ocaml compiler.
 While waiting for an upstream solution, we decided
 to rename files in our codebase whose names are conflicting 
 with the compiler-libs library of the ocaml compiler.

- [ ] Bytes/String PR by E. Arias. WIP, proposal is to split it in smaller
  chunks and do renamings.

   [EGJA] A few minor parts should be able to go into 8.6 IMHO. Personally, I would just postpone the rest until 4.02 is the default compiler to avoid shipping more compatibility burden.

- [ ] M. Kosik implemented the move to Context.Rel/Named for manipulating
 named and de Bruijn contexts using an algebraic datatype distinguishing
 declarations and definitions and modules with a similar interface. This
 will break plugins but the move to the new representation is really
 straightforward.  An advantage of the new representation is that users
 are less likely to forget that there are potentially local definitions
 and not only declarations in these contexts.

- [ ] ! PR [[https://github.com/coq/coq/pull/143|#143]] now [[https://github.com/coq/coq/pull/179|#179]]: Feedback/pp cleanup (E. J. Gallego)
   [EJGA] I should be able to get this in shape for 8.6.
   [EJGA] Done
   
   8.5: compatibility issue, breaks contribs. Documented in CHANGES.

 Discussion about instructions on how to fix the contribs and policy for
 PRs breaking contribs.
 Decision: merged.

- [ ] PR [[https://github.com/coq/coq/pull/158|#158]]: Fixing the
   "beautifier" and checking the parsing-printing reversibility
   (H. Herbelin)

 Comments: merging needed and testing on the contribs for
 minor changes in the .v files.

 Decision: ok, with configure option to activating/deactivating.

- [ ] PR [[https://github.com/coq/coq/pull/86|#86]]: simplify sort_fields
   (G. Sherer)
 Decision: cleanup without parsing rule and merge.

- [ ] ! PR [[[[https://github.com/coq/coq/pull/117|#117]]: iota split into iota0+phi+psi and ML API cleanup for
  reduction functions (H. Herbelin).

  Decision: Set iota to be match + fix + cofix at user level, preserving
  compatibility.
  No incompatibility at the ML level. Maxime and Hugo to work on the merge.

- [ ] New warning system.
 Decision: ok.

- [ ] Flag deprecated commands: Add Setoid/Morphism/...?
 Decision: deprecate.

- [ ] Windows SDK built from sources using Michael's script (Enrico).

- [ ] Error resilient mode for STM (Enrico) [[https://github.com/coq/coq/pull/173|#173]]
 Decision: ok. Difference between coqc and coqide.

- [x] Compartimentalize IDE-API specific serialization in IDE (PR#180,
 EJGA).

- [ ] Use -pack for plugins
- [ ] [[https://github.com/coq/coq/pull/185|#185]] Remove unused printing infrastructure and duplication.
   [EJGA] This is up to PMP/Enrico, I did this PR because the stuff is abandoned and it was indeed confusing people looking at it. It also saves 24K of bytecode and removes a duplicate code path.

# Kernel

- [x] New universe cycle detection algorithm by J.H. Jourdan.
 Much faster on typical graphs, implements a state-of-the-art incremental cycle
 detection algorithm by Bender, M. A., Fineman, J. T., Gilbert, S., &
 Tarjan, R. E.
      
- [x] Now accepting unit props in mutual definitions (B. Barras, 045b695)
 Any change due to this? kernel/checker

- [ ] Optimizations in the (lazy) reduction machine, saving allocations

# Elaboration, Gallina

- [ ] Ltac implementation refactoring, "Ltac as a plugin" (P.M. Pédrot).
 Uniform handling of generic arguments.

  Incompatibilities:
  ! at the syntax level, when using constr:, ltac:
	grammar entries in Ltac code, parentheses become mandatory
	(e.g.: constr:((x, y)) for the pair of terms x y).
	ipattern_list:([] []). Uniformity vs "non-uglyness".
       All about parsing. It will break. 
       Using constr:((x, y)). 

  Decision: ask users about grepping before we can include this,
  so we can evaluate the incompatibility better and in this case
  specialize constr:().

- [ ] At the level of ML: camlp4 quotations of ltac are no longer
  supported (<:ltac < auto with *>>)
  
 Some tactics and vernac entries were moved to 
 TACTIC EXTEND/VERNAC EXTEND thanks to the refactoring.

 - [ ] ML: API cleanup/changes for maps of existential variables and
  universes. pretyping/evd.ml was moved to engine/evd.ml and a new
  programming interface engine/sigma.ml is provided to statically ensure
  the state is used monotonously (using GADTs). namegen, termops,
  logic_monad, proofview_monad are also in engine.

- [ ] ? Evar naming:

  * Unnamed evars generated identifiers are not stable and shouldn't be 
  used to refer to evars (MS: can they? HH: in 8.5pl1, only the evars named using ?[x] or ?[?x] can be referred to).
  
  HH: Whether generated identifiers should be unparsable or not is a question which is worth a discussion. My observation is that both approaches have their supporters.

  * There are two kinds of names printed the same way? the generated ones
  and the user-specified ones? (HH: Yes, and this is natural I think.)
  
  * We have another occurrence of the "declare at first use" phenomenon.
  ?[e] declares and evar and uses it.
  {{{Check ?[n]+?n}}} differs from {{{Check ?n+?[n]}}}.
  Should we use ?[n] several times? (HH's note: it fails in 8.5) HH: There is a convenient situation where one would like to use ?[n] several times: {{{match t with Constructor x => u | _ => ?[n] end}}}.

  Proposition by PMP to use tactics in terms [let t := ltac:(evar sometype) in u] ? Ongoing CEP by HH on sharing evars.

  Note: {{{instantiate(n:=t)}}} is working in 8.5pl1 for user-named evars but not for coq-generated ones.

  * Bug in context compatibility checking.

  Many proposals:
  ltac:(let x := evar "e" T in exact (x + x))
  replace (?A + ?B) with B + A.

  No decision really, timeout

  ## Unification:

  - [ ] Unification of Let-In bodies without unifying their types (in
	evarconv heuristic of first-order unifications) (9cc95f5)
   Decision: unification, use cumulativity state leq.

  - [ ] Keyed Unification:

  ! The strategy is now to do a first pass without conversion and
  a pass with full conversion of arguments if this fails, when
  selecting subterms. Keyed Unification is still restricted to
  [unify_to_subterm], used by the standard rewrite only (M. Sozeau).

  Decision: ok.
  Follow the compat flag.  

  ## Typeclasses:

  - [ ] Option to add eta-unification during resolution.
  - [ ] Option to do resolution following the dependency order of subgoals
  in resolution (previously, and by default, the most dependent ones
  are tried first, respecting the semantics of the previous proof engine).
  - [ ] Option to switch to an iterative deepening search strategy.
  Should be renamed bfs.
  
  - [ ] New implementation of typeclasses eauto based on new proof engine,
  could replace eauto as well: full backtracking, Hint Cut supported,
  iterative deepening, limited search, ... (M. Sozeau) 
  [[https://github.com/mattam82/coq/commits/bteauto|branch]]. 
  To be turned into a PR, compatibility checks to do first.
  Decision: ok. Compatibility with eauto?
  
- [ ] PR [[https://github.com/coq/coq/pull/142|#142]] Patterns in abstractions (D. de Rauglaudre)

# Vernacular

- [ ] ! Forbiding "Require" inside modules and module types (Import is
   fine) Users complain. Do we deprecate or not? We should say
   something.

   Decision: message with workaround (move outside module).
   Still an incompatibility to 8.4.

- [ ] Print Assumptions now prints axioms through inductive definitions (M. Lasson)

- [ ] Deprecate non-qualified Requires ? Emitting a warning may be fine.

- [ ] PR [[https://github.com/coq/coq/pull/78|#78]] Assume
   Positive/Guarded/... Syntax issue on attributes, naming.

  Decision: provide the API. No decision on the syntax.
  Let it be accessible to plugins only.


- [ ] PR [[https://github.com/coq/coq/pull/85|#85]] Printing in cbv/cbn
 - Is it subsumed by other features?
 - Can it be made into a plugin?
 Decision:
   - change to implement only the hooks part by 8.6, as this can be
     generalized. Ask Thomas about the changes needed and update.
   - Ask for users to tell us and advertise it.

- [ ] Search has an option to print only the list of names found (C.
  Pit-Claudel). Maybe a generalized API is in order (PR by G. Malecha)?

- [ ] New flag "Shrink Abstract" that minimizes proofs generated by the abstract
  tactical w.r.t. variables appearing in the body of the proof. Also
  improved Shrink Obligations.

  Compatibility questions. Set it on and deprecate the flag (abstract is
  not specifying its proof term).
 
- [ ] Universe binders @{} are now accepted by Program and Instance commands.
  Locations of universe binders can be taken into account in error messages.

- [ ] Support for (@foo) args in patterns, when @foo has no arguments (H. Herbelin).

- [ ] @, abbreviations and notations are now interpreted in patterns like in terms (H. Herbelin).
    
- [ ] PR [[https://github.com/coq/coq/pull/156|#156]] Coq-level numeral printers, now a CEP: [[CEP/Numeral Notation]] (D. de Rauglaudre)
 Discussion:
   P. Letouzey helped improve it.
   should be revised.
   For BigZ, BigN old parsers will still work.
   Ltac for real numbers, not so clear it's the right way.
   This just adds a purely .v solution to have printers.
 Decision:
   ok. Advertise it too, new feature, not entirely complete.

- [x] PR [[https://github.com/coq/coq/pull/64|#64]]: Add a Print Ltacs
   vernacular (C. Pit-Claudel)

- [ ] PR [[https://github.com/coq/coq/pull/113|#113]]: Add the "not a
    keyword" modifier to notations (J.P. Delaix?)
  Discussion: camlp4/camlp5 discrepancy. Fix not needed anymore.
  Decision: closed. 


- [ ] PR [[https://github.com/coq/coq/pull/114|#114]]: Set Debug Foo vs Set Foo Debug (H. Herbelin)

- [ ] PR [[https://github.com/coq/coq/pull/162|#162]]: Search Interface Revisions (G. Malecha)
  Decision: Moving pattern_of_string/dirpath_of_string_list elsewhere.
  Merge.  

# Tactics

- [ ] ! double induction, which is deprecated (but not warned as such),
  was improved by H. Herbelin, introducing an incompatibility (it succeeds
  more often). Compatibility flag?
  induction m, n = induction m ; destruct m
  double induction n m = elim n ; elim m.
  Decision: remove the double induction tactic.

- [ ] ! invariants on (a, b, ...), intropattern for generalized cartesian products
  Stop autocompleting with ? (H. Herbelin)
  Used to: warning about more names, or less.
  Now: exact number of names.
  Decision: ok, relying on the compat_version ().

  The error message could be improved?

- [x] ! "Set Regular Subst Tactic" on by default, subst has a more
   canonical strategy and can succeed more often.
 
 - [ ] ! congruence now uses build_selector from Equality (H. Herbelin)
 Decision: ok, incompatibility on discriminate on dependent types.

 - [ ] Clearing on the fly
 To be applied to destruct/applied.
 Design decisions on default, currently compatible.
 Discussion: postpone the design on configurability,
 the default for e.g. apply is not changeable even if more natural.
 the code is already merged using > for clearing explicitely.

 - [ ] ! Clear not failing when an hypothesis didn't exist in Ltac mode,
 now it does (only in strict mode?).
 Decision: ok.

 - [ ] ! contradiction (H. Herbelin)
 Adds incompatibility: more success. ~ True and ~ (x = x), part of [easy].
 Decision: ok. Compatibility issue.

 - [ ] refine and conv_pbs (E. Tassi, M. Dénès)
 Discussion: refine can be unsafe in the sense of not checking which
 unification problems are solved.
 Problem of API and sealing. 
 - Fix refine, document API
 Decision: fix ok.

- Set Printing Unification Problems by M. Sozeau.

- [ ] PR [[https://github.com/coq/coq/pull/74|#74]]: Range selector
   (C. Mangin)
 Decision: take 1st part, document it in refman (associativity).
 Do not allow ?[x].

- [ ] ! PR [[https://github.com/coq/coq/pull/100|#100]]: fresh accepts more
 things (P. Courtieu) fresh will succeed more often=incompatibilies.
 Are these incompatibilites difficult to fix? Nobody should rely on it,
 (is_constr, is_var, ...)
 Decision: ok with review by Pierre-Marie.

- [ ] ! PR [[https://github.com/coq/coq/pull/140|#140]]: Iff as a proper
  connective (H. Herbelin)

  Problem of coercions. Compatibility issue..
  - Evaluate on contribs.
  Decision: wait on.

- [ ] PR [[https://github.com/coq/coq/pull/146|#146]]: Ssreflect pattern
   matching facilities (E. Tassi)
  Discussion: some documentation issues
  Decision: ok. Documentation of the ltac and advertisement.


- [ ] PR [[https://github.com/coq/coq/pull/150|#150]]: LtacProf (trunk) (J. Gross, P. Steckler)
  Decision: in 8.6. No incompatibility, accept that its incomplete on
  backtracking, as a debug feature.

- [ ] ?! injection as ([intropattern]): changed? Compatibility is not
  guaranteed here (H. Herbelin).
  What changed? Postponed to June 1st.

- [ ] ? PR [[https://github.com/coq/coq/pull/164|#164]]: A few tactics for
   8.6 (H. Herbelin)
   Discussion:
   H: it is stepwise additions, so next version will add something else.
   G: it should be the default if it's the right way.

   - Injection introduces many incompatibilities, we'll need a
   legacy tactic to replace it. Idea to use a module for deprecated tactics.
   
   - intros until 1. (1st). opposition to 1st

   - 

- [ ] ! Properly handle Hint Extern with conclusions of the form
   _ -> _" in typeclass resolution (M. Sozeau)
   This breaks compatibility, these Hint Externs were not
   found before as the pattern was matched on the conclusion of the
   goal, removing arrows.

  Decision: put a warning at least on Hint Externs, maybe fix it.

- [ ] Fix semantics of pattern-matching in Ltac (non-linear patterns, difference between hyps and goal and hyps)
   (M. Sozeau)
  Idea: use a warning for using conversion instead of syntactic checks
   and output a warning about deprecation.
  No problem a priori.
  Decision: if we have time to evaluate before June 15th.

- [ ] ! Remove some atomic tactics from the AST. This breaks "intro" in strict mode (i.e. Ltac := ...) by forcing its argument to have been defined beforehand, e.g. "intro x" is rejected and should be turned into "let x := fresh in intro x" (or similar).

- [ ] ! Force "let rec" Ltac to be a thunk, e.g. "let rec x := idtac" is forbidden and should be turned into "let rec x _ := idtac".

# Standard Library

- [ ] ! Changes in QArith/Qcanon,Qcabs by P. Letouzey with minor incompatibilities
   Decision: Lemma additions, minor incompatibilities
   
- [ ] Introduction of MMaps

- [x] ! nil, cons, None and Some have their type argument set maximally implicit.
 Incompatibility should be clearly mentionned.
 
- [ ] function_scope is now declared in Notations and bound to Funclass.
- [ ] Scopes can be bound to classes again. (J. Gross)

- [ ] Definition of eta in VectorSpec.v
- [ ] ListSet lemmas in Stdlib (S. Hinderer)
- [ ] PR [[https://github.com/coq/coq/pull/135|#135]]: Export Nat in NPeano.v (J. Gross) 

# CoqIDE

- [x] Modernization of the preferences (P.M. Pédrot).
- [ ] PR [[https://github.com/coq/coq/pull/67|#67]]: Add a Show Proof query
 to CoqIDE.
 Decision: ok. Merging help needed from PM.

# Tools
- [ ] PR [[https://github.com/coq/coq/pull/166|#166]]:
 Add -o option to coqc to choose the .vo file directory (Enrico)
 -o cmxs issue ? options to ML compiler ?
 -L issue ?
 Decision: ok, think about the makefile.
