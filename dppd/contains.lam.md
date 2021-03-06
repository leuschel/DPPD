The "contains" Benchmark
------------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

This is the contains example from the Lam & Kusalik benchmarks. It is a
string matcher which tries to be somewhat clever, but is highly
non-deterministic and in the end not very efficient at all. This
benchmark program uses the `\==` builtin.

### The benchmark program

    contains( _pat, _str ) :- con( _str, ([],_pat) ).

    con( _, (_,[]) ).
    con( [_t|_rem_str], _pat_info ) :-
        new( _t, _pat_info, _new_pat_info ),
        con( _rem_str, _new_pat_info ).

    new( _t, (_prefix,[_t|_rem_postfix]), (_new_prefix,_rem_postfix) ) :-
        append( _prefix, [_t], _new_prefix ).
    new( _t, (_prefix,[_dift|_rem_postfix]), (_new_prefix,_new_postfix) ) :-
        _t \== _dift,
        append( _prefix, [_t], _temp ),
        append( _new_prefix, _rest, _prefix ),
        append( _, _new_prefix, _temp ),
        append( _rest, [_dift|_rem_postfix], _new_postfix ).

    append( [], _L, _L ).
    append( [_X|_Xs], _Ys, [_X|_Zs] ) :- append( _Xs, _Ys, _Zs ).

### The partial deduction query

     :- contains([a,a,b],X).

### The run-time queries

     :- contains([a,a,b],[a,b,c,d,e,f,g,h,a,a,b,
              i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z]).

### Example solution

The following specialised program can be obtained by the [ECCE partial
deduction system](/~mal/systems/ecce.html). It runs more than 10 times
faster than the original.

    contains__1([X1|X2]) :-  con__2(X1,X2).

    con__2(a,[X1|X2]) :- con__3(X1,X2).
    con__2(X1,[X2|X3]) :- X1 \= a,con__2(X2,X3).

    con__3(a,[X1|X2]) :- con__4(X1,X2).
    con__3(X1,[X2|X3]) :- X1 \= a,con__2(X2,X3).

    con__4(b,X1).
    con__4(X1,[X2|X3]) :- X1 \= b,con__2(X2,X3).
    con__4(a,[X1|X2]) :- con__3(X1,X2).
    con__4(a,[X1|X2]) :- con__4(X1,X2).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
