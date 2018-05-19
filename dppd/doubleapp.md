The "doubleapp" Benchmark
-------------------------

Part of the [DPPD Library](../dppd.html).

### General Description

This is a naive implementation for a predicate that appends three lists,
written using two calls to the ordinary `append` predicate. This program
is inefficient because the intermediate variable `Int` is constructed by
the first call to `append` and then traversed again by the second call
to `append`.

### The benchmark program

    double_app(X,Y,Z,Res) :-
        app(X,Y,Int),
        app(Int,Z,Res).

    app([],L,L).
    app([H|X],Y,[H|Z]) :-
        app(X,Y,Z).

### The partial deduction query

     :- double_app(X,Y,Z,Res).

### The run-time queries

     :- double_app([],[a],[b,c],Res).
     :- double_app([a,b,c],[d,e,f],[g,h,i],Res).
     :- double_app([1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],
                [1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],
                [1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],Res).
     :- double_app([1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],
                [1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],
                [1,5,3,2,6,3,7,3,2,1,8,5,3,5,2,3],Res).

### Example solution

The following specialised program can be obtained by the [ECCE partial
deduction system](/~mal/systems/ecce.html). Note that the unnecessary
variable `Int` has been removed (i.e. deforestation has been performed).
(Also note that the two predicates `double_app__1` and `app_conj__2` are
identical and could be merged.)

    double_app__1([],X1,X2,X3) :-  app__3(X1,X2,X3).
    double_app__1([X1|X2],X3,X4,[X1|X5]) :- 
        app_conj__2(X2,X3,X4,X5).

    app_conj__2([],X1,X2,X3) :-  app__3(X1,X2,X3).
    app_conj__2([X1|X2],X3,X4,[X1|X5]) :- 
        app_conj__2(X2,X3,X4,X5).

    app__3([],X1,X1).
    app__3([X1|X2],X3,[X1|X4]) :- app__3(X2,X3,X4).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
