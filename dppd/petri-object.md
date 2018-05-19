The "petri-object" Benchmark
----------------------------

Part of the [DPPD Library](../dppd.html).

### General Description

A reified version of the [petri-meta](petri-meta.html) benchmark. The
goal is to prove that for the Petri net at hand (and for *any* number of
processes) there is no trace that leads to an unsafe state with more
than 1 process in its critical section.

### The benchmark program

    unsafe(_,_,s(s(X)),_,_).
    unsafe(s(X),s(P),CS,Y,C) :- unsafe(X,P,s(CS),Y,C).
    unsafe(X,P,s(CS),Y,C) :- unsafe(X,s(P),CS,s(Y),C).
    unsafe(X,P,CS,s(Y),C) :- unsafe(s(X),P,CS,Y,s(C)).

### The partial deduction query

     :-  unsafe(X,s(0),0,0,0).

### Example solution

With the [ECCE partial deduction system](/~mal/systems/ecce.html) one
can obtain, together with the more specific program construction, a
result which is basically as follows:

    unsafe(X,s(0),0,0,0) :- fail.

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
