The "maxlength" Benchmark
-------------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

A simple benchmark testing whether a partial deducer can perform
tupling. The benchmark program traverses a list twice to get its length
as well as the maximum value of the list. By tupling these traversals
are merged into a single traversal. The benchmark program uses built-ins
but no negations. This particular benchmark program is treated in more
detail in the technical report [CW
225](http://www.cs.kuleuven.ac.be/cwis/research/dtai/publications/abstracts.96.html#CW225.abstract).

### The benchmark program

    max_length(Ls,M,Len) :- max(Ls,M), my_length(Ls,Len).

    my_length([],0).
    my_length([X|T],L) :-
        my_length(T,LT),
        L is LT + 1.

    max(X,M) :- max1(X,0,M).

    max1([],M,M).
    max1([H|T],N,M) :-
        '=<'(H,N),
        max1(T,N,M).
    max1([H|T],N,M) :-
        '>'(H,N),
        max1(T,H,M).

### The partial deduction query

     :- max_length(L,Max,Len).

### The run-time queries

     :- max_length([1,2,3,4,3,2,1],Max,Len).
     :- max_length([],Max,Len).
     :- max_length([1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],Max,Len).
     :- max_length([1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],Max,5).
     :- max_length([1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],3,Len).

### Example solution

With the [ECCE partial deduction system](/~mal/systems/ecce.html) one
can obtain the following tupled program (which runs 20 % faster on
Sicstus Prolog, but unfortunately 10 % slower on Prolog by BIM - the
problem is probably due to caching behaviour of the Sparc processor):

     max_length__1(X1,X2,X3) :- max1_conj__2(X1,0,X2,X3).

     max1_conj__2([],X1,X1,0).
     max1_conj__2([X1|X2],X3,X4,X5) :- 
         X1 =< X3, 
         max1_conj__2(X2,X3,X4,X6), 
         X5 is '+'(X6,1).
     max1_conj__2([X1|X2],X3,X4,X5) :- 
         X1 > X3, 
         max1_conj__2(X2,X1,X4,X6), 
         X5 is '+'(X6,1).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
