The "multiply(clpr)" Benchmark
------------------------------

Part of the [DPPD
Library](https://github.com/leuschel/DPPD).

### General Description

This is a very simple multiple predicate written in Sicstus clp(r).

### The benchmark program

    multiply(_,Y,R) :- 
                  {Y =0, R = 0}.
    multiply(X,Y,R) :- 
                  {
                Y > 0, 
            Y1 = Y - 1,
                R = X + R1
              }, multiply(X,Y1,R1).

### The partial deduction query

:- multiply(X, 100, R).

### The run-time query (600 times)

:- multiply(1000,100, R).

### Example solution

The following specialised program can be obtained by using the Logen
partial deduction system with clp(R,Q) extensions.

    multiply__1(B,100,C) :- 
      {
        C = ((100) * B)
      }.
    multiply(A,100,C) :-
            multiply__1(A,100,C).

------------------------------------------------------------------------

[Michael Leuschel](http://www.ecs.soton.ac.uk/%7Emal/michael.html)
