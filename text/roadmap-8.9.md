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
