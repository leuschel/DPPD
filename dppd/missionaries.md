The "missionaries" Benchmark
----------------------------

Part of the [DPPD Library](../dppd.html).

### General Description

A program for the missionaries and cannibals problem without using
built-ins, but with negation.

### The benchmark program

    go(Res) :- search(s(s(s(0))),s(s(s(0))),west,
        [state(s(s(s(0))),s(s(s(0))),west)],Res).

    search(0,0,Boat,Result,Result).
    search(WM,WC,east,History,Result) :-
        move_boat_west(WM,WC,NWM,NWC),
        safe(NWM,NWC),
        \+(loop(NWM,NWC,west,History)),
        search(NWM,NWC,west,[state(NWM,NWC,west)|History],Result).
    search(WM,WC,west,History,Result) :-
        move_boat_east(WM,WC,NWM,NWC),
        safe(NWM,NWC),
        \+(loop(NWM,NWC,east,History)),
        search(NWM,NWC,east,[state(NWM,NWC,east)|History],Result).

    safe(s(s(s(0))),WestCann) :- \+(gt(WestCann,s(s(s(0))) )).
    safe(0,WestCann) :- \+(gt(WestCann,s(s(s(0))) )).
    safe(W,W) :-  \+(ge(0,W)), \+(ge(W,s(s(s(0))) )).

    loop(WestMiss,WestCann,Boat,History) :-
        member(state(WestMiss,WestCann,Boat),History).

    move_boat_east(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        plus2(NewWestMiss,WestMiss), NewWestCann = WestCann.
    move_boat_east(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        plus1(NewWestMiss,WestMiss), NewWestCann = WestCann.
    move_boat_east(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        plus1(NewWestMiss,WestMiss),
        plus1(NewWestCann,WestCann).
    move_boat_east(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        NewWestMiss = WestMiss, plus1(NewWestCann,WestCann).
    move_boat_east(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        NewWestMiss = WestMiss, plus2(NewWestCann,WestCann).

    move_boat_west(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        plus2(WestMiss,NewWestMiss), NewWestCann = WestCann.
    move_boat_west(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        plus1(WestMiss,NewWestMiss), NewWestCann = WestCann.
    move_boat_west(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        plus1(WestMiss,NewWestMiss), plus1(WestCann,NewWestCann).
    move_boat_west(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        NewWestMiss = WestMiss, plus1(WestCann,NewWestCann).
    move_boat_west(WestMiss,WestCann,NewWestMiss,NewWestCann) :-
        NewWestMiss = WestMiss, plus2(WestCann,NewWestCann).

    member(X,[X|_]).
    member(X,[_|T]) :- member(X,T).


    gt(s(X),0).
    gt(s(X),s(Y)) :- gt(X,Y).

    ge(0,0).
    ge(s(X),0).
    ge(s(X),s(Y)) :- ge(X,Y).

    plus1(X,s(X)).
    plus2(X,s(s(X))).

### The partial deduction query

     :- search(X,Y,west,[state(X,Y,west)],Res).

### The run-time queries

     :- search(s(s(s(0))),s(s(s(0))),west,
        [state(s(s(s(0))),s(s(s(0))),west)],Res).

### Example solution

The best solution so far, using the [ECCE partial deduction
system](/~mal/systems/ecce.html) runs almost 2 times faster than the
original. Mixtus (0.3.3) and Paddy (Eclipse 3.5.1) did not terminate on
this example. The following solution is not optimal (but is a bit
smaller). Its relative execution time is 0.69.

    search__1(0,0,X1,[state(0,0,west)|X1]).
    search__1(X1,X2,X3,X4) :- 
        move_boat_east_conj__2(X1,X2,X5,X6), 
        not(loop__3(X5,X6,east,X1,X2,west,X3)), 
        search__4(X5,X6,X1,X2,X3,X4).

    move_boat_east_conj__2(s(s(X1)),X2,X1,X2) :- safe__11(X1,X2).
    move_boat_east_conj__2(s(X1),X2,X1,X2) :- safe__11(X1,X2).
    move_boat_east_conj__2(s(X1),s(X2),X1,X2) :- safe__11(X1,X2).
    move_boat_east_conj__2(X1,s(X2),X1,X2) :- safe__11(X1,X2).
    move_boat_east_conj__2(X1,s(s(X2)),X1,X2) :- safe__11(X1,X2).

    loop__3(X1,X2,X3,X4,X5,X6,[state(X1,X2,X3)|X7]).
    loop__3(X1,X2,X3,X4,X5,X6,[X7,X8|X9]) :- member__10(X1,X2,X3,X8,X9).

    search__4(0,0,X1,X2,X3,[state(0,0,east),state(X1,X2,west)|X3]).
    search__4(X1,X2,X3,X4,X5,X6) :- 
        move_boat_west_conj__5(X1,X2,X7,X8), 
        not(loop__3(X7,X8,west,X1,X2,east,[state(X3,X4,west)|X5])), 
        search__1(X7,X8,[state(X1,X2,east),state(X3,X4,west)|X5],X6).

    move_boat_west_conj__5(X1,X2,s(s(X1)),X2) :- safe__6(s(X1),X2).
    move_boat_west_conj__5(X1,X2,s(X1),X2) :- safe__6(X1,X2).
    move_boat_west_conj__5(X1,X2,s(X1),s(X2)) :- safe__6(X1,s(X2)).
    move_boat_west_conj__5(X1,X2,X1,s(X2)) :-  safe__7(X1,X2).
    move_boat_west_conj__5(X1,X2,X1,s(s(X2))) :- safe__7(X1,s(X2)).

    safe__6(s(s(0)),X1) :- not(gt__8(X1)).
    safe__6(X1,s(X1)) :- not(ge__9(s(X1))).

    safe__7(s(s(s(0))),X1) :- not(gt__8(s(X1))).
    safe__7(0,X1) :- not(gt__8(s(X1))).
    safe__7(s(X1),X1) :- not(ge__9(s(X1))).

    gt__8(s(s(s(s(X1))))).

    ge__9(s(s(s(0)))).
    ge__9(s(s(s(s(X1))))).

    member__10(X1,X2,X3,state(X1,X2,X3),X4).
    member__10(X1,X2,X3,X4,[X5|X6]) :- member__10(X1,X2,X3,X5,X6).

    safe__11(s(s(s(0))),X1) :- not(gt__8(X1)).
    safe__11(0,X1) :- not(gt__8(X1)).
    safe__11(X1,X1) :- not(ge__12(X1)), not(ge__9(X1)).

    ge__12(0).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
