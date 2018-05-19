The "relative" Benchmark
------------------------

Part of the [DPPD Library](../dppd.html).

### General Description

A Lam & Kusalik benchmark which can be fully unfolded. It uses neither
negations nor built-ins.

### The benchmark program

    relative(X,Y) :- ancestor(Z,X), ancestor(Z,Y).
     
    ancestor(X,Y) :- parent(X,Y).
    ancestor(X,Y) :- parent(X,Z),ancestor(Z,Y).
     
    parent(X,Y) :- father(X,Y).
    parent(X,Y) :- mother(X,Y).
     
    father(jap,carol).
    father(jap,jonas).
    father(jonas,maria).
    mother(carol,paulina).
    mother(carol,albertina).
    mother(albertina,peter).
    mother(maria,mary).
    mother(maria,jose).
    mother(mary,anna).
    mother(mary,john).

### The partial deduction query

     :- relative(john,X).

### The run-time queries

     :- relative(john,peter).

### Example solution

This benchmark can be fully unfolded. With the [ECCE partial deduction
system](/~mal/systems/ecce.html) one can obtain the following program,
which runs more than 300 times faster than the original:

    relative__1(anna).
    relative__1(john).
    relative__1(carol).
    relative__1(jonas).
    relative__1(paulina).
    relative__1(albertina).
    relative__1(peter).
    relative__1(maria).
    relative__1(mary).
    relative__1(jose).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
