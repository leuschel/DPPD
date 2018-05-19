The "match-append" Benchmark
----------------------------

Part of the [DPPD Library](../dppd.html).

### General Description

A very naive string matcher, written with 2 appends. Same runtime
queries than match.kmp. This benchmark contains no built-in's nor
negations.

### The benchmark program

    match(P,S) :- append(S1,_,S),append(_,P,S1).

    append([],L,L).
    append([H|L1],L2,[H|L3]) :- append(L1,L2,L3).

### The partial deduction query

     :- match([a,a,b],String).

### The run-time queries

     :- match([a,a,b], [a,a,a,a,c,d,a,a,a,e,f,g,h,a,a,b,d,f]).
     :- match([a,a,b], [a,b,a,b,a,a,a,a,c,a,a,a,a,a,a,a,a,b]).
     :- match([a,a,b], [a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,b]).
     :- match([a,a,b], [a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,a,a,b,w,x,y,z]).

### Example solution

The following can be obtained by the [ECCE partial deduction
system](/~mal/systems/ecce.html). Although it runs considerably faster
than the original program (33 times actually) it is still not a KMP
style matcher.

     match__1([X1,X2,X3|X4]) :- append__append(X4,X1,X2,X3).

     append__append(X1,a,a,b).
     append__append([X1|X2],X3,X4,X5) :- append__append(X2,X4,X5,X1).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
