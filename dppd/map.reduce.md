The "map.reduce" Benchmark
--------------------------

Part of the [DPPD Library](../dppd.html).

### General Description

Specialising the higher-order map/3 (using call and =..) for the
higher-order reduce/4 in turn applied to add/3. The benchmark program
uses built-ins but no negations. The benchmark illustrates that partial
deduction can be used to make declarative higher-order programming in
Prolog/LP efficient.

### The benchmark program

    map(P,[],[]).
    map(P,[H|T],[PH|PT]) :-
        Call =.. [P,H,PH],
        call(Call),
        map(P,T,PT).

    reduce(Func,Base,[],Base).
    reduce(Func,Base,[H|T],Res) :-
        reduce(Func,Base,T,TRes),
        Call =.. [Func,H,TRes,Res],
        call(Call).

    q(a,b).
    q(b,c).
    q(c,d).
    q(d,e).

    reduce_add(List,Res) :-
        reduce(add,0,List,Res).
    add(X,Y,Z) :-
        Z is X + Y.

    rev(L,R) :-
        rev(L,[],R).

    rev([],L,L).
    rev([H|T],A,R) :-
        rev(T,[H|A],R).

### The partial deduction query

     :- map(reduce_add,L,R).

### The run-time queries

     :- map(reduce_add,[[1,2],[1,2,3]],[L1,L2]).
     :- map(reduce_add,[[],[1,2],[5,6,7],[8,9,10]],Res).
     :- map(reduce_add,[[],[1,2],[5,6,7],[],[8,9,10],[11,12],
                    [13,14],[15],[16]],Res).

### Example solution

With the [ECCE partial deduction system](/~mal/systems/ecce.html) one
can obtain the following program (which runs more than 12 times faster
than the original):

     map__1([],[]).
     map__1([X1|X2],[X3|X4]) :- 
         reduce__3(X1,X3), 
         map__1(X2,X4).

     reduce__3([],0).
     reduce__3([X1|X2],X3) :- 
         reduce__3(X2,X4), 
         X3 is '+'(X1,X4).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
