The "upto.sum1" Benchmark
-------------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

This is a sophisticated deforestation example coming from the functional
programming community. It was handed over to me by Jesper Jorgensen, who
adapted it from Wadler's paper *Deforestation: Transforming programs to
eliminate intermediate trees* in TCS, 73:231-248, 1990. The program
calculates the sum of squares for 1 up to n.

### The benchmark program

    square_square(L,SSL) :-
        squares(L,SL),
        squares(SL,SSL).

    sumsquaresupto(N,S) :-
       upto(1,N,Ns),
       squares(Ns,SONs),
       sum(SONs,S).

    sumtrsquaretr(XT,S) :-
       squaretr(XT,SOXt),
       sumtr(SOXt,S).

    upto(M,N,[]) :- M>N.
    upto(M,N,[M|Ms]) :- 
        M =< N,
        M1 is M + 1,
        upto(M1,N,Ms).

    sum(Ns,S) :- sum1(Ns,0,S).

    sum1([],S,S).
    sum1([N|Ns],A,S) :-
        A1 is A + N,
        sum1(Ns,A1,S).

    square(N,SON) :- SON is N * N.

    squares([],[]).
    squares([N|Ns],[SON|SONs]) :-
        square(N,SON),
        squares(Ns,SONs).

    sumtr(leaf(X),X).
    sumtr(branch(Xt,Yt),S) :-
        sumtr(Xt,SX),
        sumtr(Yt,SY),
        S is SX + SY.

    squaretr(leaf(X),leaf(SOX)) :- 
        square(X,SOX).
    squaretr(branch(Xt,Yt),branch(SOXt,SOYt)) :-
        squaretr(Xt,SOXt),
        squaretr(Yt,SOYt).

### The partial deduction query

     :- sumsquaresupto(N,S).

### The run-time queries

     :- sumsquaresupto(5,X).
     :- sumsquaresupto(15,X).
     :- sumsquaresupto(25,X).
     :- sumsquaresupto(30,X).

### Example solution

    to be inserted

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
