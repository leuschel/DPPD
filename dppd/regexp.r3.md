The "regexp.r3" Benchmark
-------------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

A program testing whether a string matches a regular expression (using
difference lists). Much more naive (and smaller) than the program used
by Mogensen/Bondorf for Logimix ! The regular expression for this
benchmark is ((a+b)(a+b)(a+b)(a+b)(a+b)(a+b))\*. This benchmark contains
no built-in's nor negations.

### The benchmark program

    generate(empty,T,T).

    generate(char(X),[X|T],T).

    generate(or(X,Y),H,T) :- generate(X,H,T).
    generate(or(X,Y),H,T) :- generate(Y,H,T).

    generate(cat(X,Y),H,T) :- generate(X,H,T1), generate(Y,T1,T).

    generate(star(X),T,T).
    generate(star(X),H,T) :- generate(X,H,T1), generate(star(X),T1,T).

### The partial deduction query

     :- generate(star(cat(or(char(a),char(b)),cat(or(char(a),char(b)),
        cat(or(char(a),char(b)),cat(or(char(a),char(b)),
        cat(or(char(a),char(b)),or(char(a),char(b)))))))),S,[]).

### The run-time queries

     :- generate(star(cat(or(char(a),char(b)),cat(or(char(a),char(b)),
            cat(or(char(a),char(b)),cat(or(char(a),char(b)),
            cat(or(char(a),char(b)),or(char(a),char(b)))))))),
            [a,b,a,b,a,b, a,a,a,b,a,b],[]).
     :- generate(star(cat(or(char(a),char(b)),cat(or(char(a),char(b)),
            cat(or(char(a),char(b)),cat(or(char(a),char(b)),
            cat(or(char(a),char(b)),or(char(a),char(b)))))))),
            [a,X,a,b,a,b, a,a,a,Y,b,a],[]).
     :- generate(star(cat(or(char(a),char(b)),cat(or(char(a),char(b)),
            cat(or(char(a),char(b)),cat(or(char(a),char(b)),
            cat(or(char(a),char(b)),or(char(a),char(b)))))))),
            [a,b,a,b,a,a, a,b],[]).
     :- generate(star(cat(or(char(a),char(b)),cat(or(char(a),char(b)),
            cat(or(char(a),char(b)),cat(or(char(a),char(b)),
            cat(or(char(a),char(b)),or(char(a),char(b)))))))),
            [a,b,a,b,a,a, b,b,b,a,b,a, a,b,b,a,a,b],[]).

### Example solution

The following can be obtained by the [ECCE partial deduction
system](/~mal/systems/ecce.html). It runs considerably faster than the
original program (more than 3 times actually) and correspond to a
deterministic automaton.

     generate__1([]).
     generate__1(X1) :- generate_conj__2(X1).

     generate_conj__2([a|X1]) :- generate_conj__3(X1).
     generate_conj__2([b|X1]) :- generate_conj__3(X1).

     generate_conj__3([a|X1]) :- generate_conj__4(X1).
     generate_conj__3([b|X1]) :- generate_conj__4(X1).

     generate_conj__4([a|X1]) :- generate_conj__5(X1).
     generate_conj__4([b|X1]) :- generate_conj__5(X1).

     generate_conj__5([a|X1]) :- generate_conj__6(X1).
     generate_conj__5([b|X1]) :- generate_conj__6(X1).

     generate_conj__6([a|X1]) :- generate_conj__7(X1).
     generate_conj__6([b|X1]) :- generate_conj__7(X1).

     generate_conj__7([a|X1]) :- generate__1(X1).
     generate_conj__7([b|X1]) :- generate__1(X1).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
