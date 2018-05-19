The "loanUnfold(clpr)" Benchmark
--------------------------------

Part of the [DPPD
Library](https://github.com/leuschel/DPPD).

### General Description

This predicate calculates outstanding loan balances and has been written
in clp(R).

### The benchmark program

    loan(X, L1, Repay) :-
            {X>=7000},
            Term = [X|L1],
            map(scheme1, Term, Repay).

    loan(X, L1, Repay) :-
            {X >= 4000, X<7000},
            Term = [X|L1],
            map(scheme2, Term, Repay).

    loan(X, L1, Repay) :-
            {X >= 1000, X<4000},
            Term = [X|L1],
            map(scheme3, Term, Repay).

    loan(X, L1, Repay) :-
            {X >= 0, X<1000},
            Term = [X|L1],
            map(scheme4, Term, Repay).

    scheme1(Amount, New, Repayment) :-
            {Interest = 0.05},
            calcLoan(Amount, New, Interest,Repayment).

    scheme2(Amount, New, Repayment) :-
            {Interest = 0.1},
            calcLoan(Amount, New, Interest,Repayment).

    scheme3(Amount, New, Repayment) :-
            {Interest = 0.15},
            calcLoan(Amount, New, Interest,Repayment).

    scheme4(Amount, New, Repayment) :-
            {Interest = 0.2},
            calcLoan(Amount, New, Interest,Repayment).


    map(_,[_],_).
    map(P,[H1,H2|Tail], Repayment) :-
            Call =.. [P,H1,H2, Repayment],
            call(Call), 
            map(P,[H2|Tail], Repayment).

    calcLoan(Amount,NewTotal, Interest, Repay) :-
            {NewTotal = Amount + (Amount * Interest) - Repay}.

### The partial deduction query

:- {X &gt; 4000}, loan(X, \[B1,B2,B3,B4\], Repay).

### The run-time query (10000 times)

:- loan(7000,\[A,B,C,D\], 400).

### Example solution

    loan__1(B,C,D,E,F,G) :- 
      {
        C = (((21/20) * B) - G),
      D = (((441/400) * B) - ((41/20) * G)),
      E = (((9261/8000) * B) - ((1261/400) * G)),
      F = (((194481/160000) * B) - ((34481/8000) * G)),
      B >= (7000)
      }.
    loan__1(H,I,J,K,L,M) :- 
      {
        H < (7000),
      I = (((11/10) * H) - M),
      J = (((121/100) * H) - ((21/10) * M)),
      K = (((1331/1000) * H) - ((331/100) * M)),
      L = (((14641/10000) * H) - ((4641/1000) * M))
      }.
    loan(A,[B,C,D,E],F) :- 
      loan__0(A,B,C,D,E,F).
    loan__0(B,C,D,E,F,G) :- 
      loan__1(B,C,D,E,F,G).


------------------------------------------------------------------------

[Michael Leuschel](http://www.ecs.soton.ac.uk/%7Emal/michael.html)
