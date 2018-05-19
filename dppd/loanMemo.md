The "loanMemo(clpr)" Benchmark
------------------------------

Part of the [DPPD
Library](https://github.com/leuschel/DPPD).

### General Description

This predicate calculates outstanding loan balances and has been written
in clp(R).

### The benchmark program

    map(_,[_],_).
    map(P,[H1,H2|Tail], Repayment) :-
            Call =.. [P,H1,H2, Repayment],
            call(Call), 
            map(P,[H2|Tail], Repayment).

    calcLoan(Amount,NewTotal, Interest, Repay) :-
            {NewTotal = Amount + (Amount * Interest) - Repay}.

### The partial deduction query

:- map(scheme1, L1, Repayment).\
The run-time query (10000 times)

:- map(scheme1, \[7000,A,B,C,D\], 400).

### Example solution

    map(scheme1,A,B) :- map__1(A,B).
    map(scheme2,A,B) :- map__2(A,B).
    map__1([B],C).
    map__1([D,E|F],G) :- 
      {E = ((201/200) * D ) - G },  map__1([E|F],G).
    map__2([B],C).
    map__2([D,E|F],G) :- 
      { E = ((101/100) * D) - G }, map__2([E|F],G).

------------------------------------------------------------------------

[Michael Leuschel](http://www.ecs.soton.ac.uk/%7Emal/michael.html)
