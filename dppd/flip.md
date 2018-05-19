The "flip" Benchmark
--------------------

Part of the [DPPD Library](../dppd.html).

### General Description

A simple deforestation example from Wadler. The benchmark program flips
a tree structure twice (thus returning the original tree back).

### The benchmark program

    flipflip(XT,YT) :- flip(XT,TT), flip(TT,YT).

    flip(leaf(X),leaf(X)).
    flip(tree(XT,Info,YT),tree(FYT,Info,FXT)) :-
        flip(XT,FXT),
        flip(YT,FYT).

### The partial deduction query

     :- flipflip(T1,T2).

### The run-time queries

     :- flipflip( tree(leaf(s(0)),s(s(0)),leaf(s(s(0)))) , Res ).
     :- flipflip( tree(leaf(s(0)),s(s(0)),tree(leaf(s(s(0))),0,
                leaf(s(s(s(0)))))) , Res ).
     :- flipflip( tree(tree(leaf(s(0)),s(s(0)),leaf(s(s(0)))),s(s(0)),
            tree(leaf(s(s(0))),0,tree(leaf(s(s(s(s(0))))),s(s(s(s(0)))),
                leaf(s(s(s(s(s(0))))))))) , Res ).
     :- flipflip( tree(tree(leaf(s(0)),s(s(0)),tree(leaf(s(0)),s(s(0)),
            tree(leaf(s(s(0))),s(s(s(s(0)))),leaf(s(s(s(0))))))),s(s(0)),
            tree(leaf(s(s(0))),s(s(s(s(0)))),
            tree(leaf(s(s(s(s(0))))),s(s(s(s(0)))),
            tree(leaf(s(s(s(s(0))))),s(s(s(s(0)))),
            tree(leaf(s(s(s(s(0))))),0,leaf(s(s(s(s(0)))))))))) , Res ).

### Example solution

The following program can be obtained by the [ECCE partial deduction
system](/~mal/systems/ecce.html). It runs about 30 % faster than the
original.

     flipflip__1(X1,X2) :- 
         flip_conj__2(X1,X2).

     flip_conj__2(leaf(X1),leaf(X1)).
     flip_conj__2(tree(X1,X2,X3),tree(X4,X2,X5)) :- 
         flip_conj__2(X1,X4), 
         flip_conj__2(X3,X5).

Combined with a bottom-up propagation (for more details see the
technical report [CW
232](http://www.cs.kuleuven.ac.be/cwis/research/dtai/publications/abstracts.96.html#CW232.abstract)
) [ECCE](/cwis/research/dtai/prototypes/ecce.html) can also obtain the
following program which runs 45 % faster than the original:

    flipflip__1(X1,X1) :- 
        flip_conj__2(X1).

    flip_conj__2(leaf(X1)).
    flip_conj__2(tree(X1,X2,X3)) :- 
        flip_conj__2(X1), 
        flip_conj__2(X3).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
