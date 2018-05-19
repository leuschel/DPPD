The "rotateprune" Benchmark
---------------------------

Part of the [DPPD Library](../dppd.html).

### General Description

A quite sophisticated deforestation example (originally by
Proietti/Pettorossi ?). This benchmark contains no built-in's nor
negations. This particular benchmark program is treated in more detail
e.g. in the technical report [CW
226](http://www.cs.kuleuven.ac.be/cwis/research/dtai/publications/abstracts.96.html#CW226.abstract).

### The benchmark program

    rp(T1,T2) :- rotate(T1,U), prune(U,T2).

    rotate(leaf(N),leaf(N)).
    rotate(tree(L,N,R),tree(RL,N,RR)) :- rotate(L,RL), rotate(R,RR).
    rotate(tree(L,N,R),tree(RR,N,RL)) :- rotate(L,RL), rotate(R,RR).


    prune(leaf(N),leaf(N)).
    prune(tree(L,0,R),leaf(0)).
    prune(tree(L,s(N),R),tree(PL,s(N),PR)) :- prune(L,PL), prune(R,PR).

### The partial deduction query

     :- rp(T1,T2).

### The run-time queries

     :- rp( tree(leaf(s(0)),s(s(0)),leaf(s(s(0)))), Res).
     :- rp( tree(leaf(s(0)),s(s(0)),tree(leaf(s(s(0))),0,leaf(s(s(s(0)))))), Res).
     :- rp( tree(tree(leaf(s(0)),s(s(0)),leaf(s(s(0)))),s(s(0)),
        tree(leaf(s(s(0))),0,tree(leaf(s(s(s(s(0))))),s(s(s(s(0)))),
        leaf(s(s(s(s(s(0))))))))), Res).
     :- rp( tree(tree(leaf(s(0)),s(s(0)),tree(leaf(s(0)),s(s(0)),
        tree(leaf(s(s(0))),s(s(s(s(0)))),leaf(s(s(s(0))))))),s(s(0)),
        tree(leaf(s(s(0))),s(s(s(s(0)))),
        tree(leaf(s(s(s(s(0))))),s(s(s(s(0)))),
        tree(leaf(s(s(s(s(0))))),s(s(s(s(0)))),
        tree(leaf(s(s(s(s(0))))),0,leaf(s(s(s(s(0)))))))))), Res).

### Example solution

The following can be obtained by the [ECCE partial deduction
system](/~mal/systems/ecce.html). It runs considerably faster than the
original program (more than 5 times actually).

     rp__1(X1,X2) :- rotate_conj__2(X1,X2).

     rotate_conj__2(leaf(X1),leaf(X1)).
     rotate_conj__2(tree(X1,0,X2),leaf(0)) :- 
        rotate__4(X1), 
        rotate__4(X2).
     rotate_conj__2(tree(X1,s(X2),X3),tree(X4,s(X2),X5)) :- 
        rotate_conj__2(X1,X4), 
        rotate_conj__2(X3,X5).
     rotate_conj__2(tree(X1,s(X2),X3),tree(X4,s(X2),X5)) :- 
        rotate_conj__2(X1,X5), 
        rotate_conj__2(X3,X4).

     rotate__4(leaf(X1)).
     rotate__4(tree(X1,X2,X3)) :- rotate__4(X1), rotate__4(X3).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
