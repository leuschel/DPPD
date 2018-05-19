The "regexp.r1" Benchmark
-------------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

A program testing whether a string matches a regular expression (using
difference lists). Much more naive (and smaller) than the program used
by Mogensen/Bondorf for Logimix ! The regular expression for this
benchmark is (a+b)\*aab. This benchmark contains no built-in's nor
negations.

### The benchmark program

    generate(empty,T,T).

    generate(char(X),[X|T],T).

    generate(or(X,Y),H,T) :- generate(X,H,T).
    generate(or(X,Y),H,T) :- generate(Y,H,T).

    generate(cat(X,Y),H,T) :- generate(X,H,T1), generate(Y,T1,T).

    generate(star(X),T,T).
    generate(star(X),H,T) :- generate(X,H,T1), generate(star(X),T1,T).

### The partial deduction query

     :- generate(cat(star(or(char(a),char(b))), cat(char(a),cat(char(a),char(b)))), S, []).

### The run-time queries

     :- generate(cat(star(or(char(a),char(b))), cat(char(a),cat(char(a),char(b)))),
            [a,a,a,a,a,a,b,b,a,a,a,b],[]).
     :- generate(cat(star(or(char(a),char(b))), cat(char(a),cat(char(a),char(b)))),
            [a,a,a,a,a,a,b,b,a,b],[]).
     :- generate(cat(star(or(char(a),char(b))), cat(char(a),cat(char(a),char(b)))),
            [a,b,a,b,a,b,a,b,a,b,a],[]).
     :- generate(cat(star(or(char(a),char(b))), cat(char(a),cat(char(a),char(b)))),
            [X,Y,Z,V],[]).

### Example solution

The following can be obtained by the [ECCE partial deduction
system](/~mal/systems/ecce.html). Although it runs considerably faster
than the original program (5 times actually) it does not correspond to a
deterministic automaton yet.

     generate__1(X1) :- generate__2(X1).
     generate__2([a,a,b]).
     generate__2([a|X1]) :- generate__2(X1).
     generate__2([b|X1]) :- generate__2(X1).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
