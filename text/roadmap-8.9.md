- [ ] Induction-recursion / induction-induction (Matthieu Sozeau)

  Simultaneous definition of a type and a function on that type / of a type and
  a family of types.

  [Branch](https://github.com/mattam82/coq/tree/IR)
  
  Status: this is really useful, with @cmangin we think we have good theoretical justification
  for adapting the guard checking to it, but the implementation in pretyping is a bit hackish.
  Need to evaluate what's needed to make it mergeable and set a deadline accordingly.

- [ ] Porting functional induction to Equations (Thierry Martinez, Matthieu Sozeau)

  Status: still at a design and evaluation stage but we have a good idea how it 
  should go. Will report at the next WG and maybe move to 8.9 at this time.

- [ ] Port plugins to `coq_makefile` (Enrico Tassi, Maxime Dénès)

- [ ] Small inversions (Thierry Martinez, Hugo Herbelin)

- [ ] Deriving binding (Matthieu Sozeau)

  This adds an official [Derive] command to Coq, on which plugins can plug their own
  derivers (implemented as they like, in ML, using tactics, ...)
  
  Status: This is quite trivial, I'll send an email to coqdev and potentially interested
  third-parties (e.g. QuickChick devs) about the API we need to agree on for that by the 
  end of december.
  
- [ ] Primitive projections API (Matthieu Sozeau)

  The idea is to do a pass of cleanup on the declaration of prim projs and their
  representation in the kernel, to see if we can move the compatibility-related
  hacks higher in the system.
  
  Status: no time to look at this now. Help wanted. 
  
- [ ] Significant indentation mode (Théo Zimmermann)

  Status: uncertain (lower priority than the point above and the one below).

- [ ] VM and native_compute as two backends for the same compiler (Maxime Dénès)
  - [ ] Unify the representation of values
    - [ ] Evar support for the VM
    - [ ] In VM, always create an accu, even if no arguments (like sorts)
    - [ ] Extend structured constant in nativelambda to those in VM
    - [ ] Inject structured constants in values without reallocating them
    - [ ] Merge Vconst and Vint into Vint of Uint63.t
    - [ ] Pass univ arguments separately in native_compute
  - [ ] Unify clambda and nativelambda
    - [ ] Clean up compilation of inductive with many constructors
    - [ ] Remove "prefixes" from nativelambda
    - [ ] Distinguish constant / nonconstant branches in nativelambda
    - [ ] Remove Lconstruct and use (N)Construct_I_tag in nativelambda
    - [ ] Keep Lazy / Force (?), ignore for VM + check what happens on toplevel match in native
    - [ ] In VM, either remove stacks or at least make kind_of_value return whd without atom_stk, but instead accu and then another function to build the stack from the accu
    - [ ] constructor vs pconstructor / inductive vs pinductive
    - [ ] review aliases vs pconstant
  - [ ] Unify vnorm and nativenorm
