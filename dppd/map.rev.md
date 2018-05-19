The "map.rev" Benchmark
-----------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

Specialising the higher-order map/3 (using call and =..) for the reverse
program. The benchmark program uses built-ins but no negations. The
benchmark illustrates that partial deduction can be used to make
declarative higher-order programming in Prolog/LP efficient.

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

     :- map(rev,L,R).

### The run-time queries

     :- map(rev,[[a,b],[c,d,e]],[L1,L2]).
     :- map(rev,[[],[a,b],[c,d,e],[f,g,h,i]],Res).
     :- map(rev,[[],[a,b],[c,d,e],[],[f,g,h],[i,j],[k,l],[m],[n]],Res).

### Example solution

With the [ECCE partial deduction system](/~mal/systems/ecce.html) one
can obtain the following program (which runs almost 10 times faster than
the original):

     map__1([],[]).
     map__1([X1|X2],[X3|X4]) :- 
          rev__2(X1,X3), 
          map__1(X2,X4).

     rev__2([],[]).
     rev__2([X1|X2],X3) :- 
          rev__3(X2,X1,[],X3).

     rev__3([],X1,X2,[X1|X2]).
     rev__3([X1|X2],X3,X4,X5) :- 
          rev__3(X2,X1,[X3|X4],X5).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
