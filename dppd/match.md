The "match" Benchmark
---------------------

Part of the [DPPD Library](../dppd.html).

### General Description

This is the semi-naive string matcher from the Lam & Kusalik benchmarks.
The goal is to obtain a KMP matcher for the pattern `aab`. This
benchmark program uses the `\==` builtin. The run-time queries are less
sophisticated than the ones for [match.kmp](match.kmp.html).

### The benchmark program

    match(Pat,T) :- match1(Pat,T,Pat,T).

    match1([],Ts,P,T).
    match1([A|Ps],[B|Ts],P,[X|T]) :-
        A\==B,
        match1(P,T,P,T).
    match1([A|Ps],[A|Ts],P,T) :-
        match1(Ps,Ts,P,T).

### The partial deduction query

     :- match([a,a,b],String).

### The run-time queries

     :- match([a,a,b],[a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,a,a,b,w,x,y,z]).

### Example solution

The following KMP-matcher can be obtained by the [ECCE partial deduction
system](/~mal/systems/ecce.html) (to my knowledge this has first been
achieved by John Gallagher with his SP system).

    match__1([X1|X2]) :- match1__2(X1,X2).

    match1__2(X1,[X2|X3]) :- a \= X1, match1__2(X2,X3).
    match1__2(a,[X1|X2]) :- match1__3(X1,X2).

    match1__3(X1,X2) :- a \= X1, match1__2(X1,X2).
    match1__3(a,[X1|X2]) :- match1__4(X1,X2).

    match1__4(X1,X2) :- b \= X1, match1__3(X1,X2).
    match1__4(b,X1).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
