The "petri-meta" Benchmark
--------------------------

Part of the [DPPD Library](../dppd.html).

### General Description

A metainterpreter for Petri nets with the net of the
[petri-object](petri-object.html) benchmark at the object level. The
goal is to prove that for the Petri net at hand (and for *any* number of
processes) there is no trace that leads to an unsafe state with more
than 1 process in its critical section.

### The benchmark program

    tr(Tr,X,St) :-
        trace(Tr,[X,s(0),0,0,0],St).
        
    trace([],State,State).
    trace([Action|As],InState,OutState) :-
        trans(Action,InState,S1),
        trace(As,S1,OutState).

    trans(enter_cs,[s(X),s(Sema),CritSec,Y,C],
               [X,Sema,s(CritSec),Y,C]).
    trans(exit_cs, [X,Sema,s(CritSec),Y,C],
               [X,s(Sema),CritSec,s(Y),C]).
    trans(restart, [X,Sema,CritSec,s(Y),ResetCtr],
               [s(X),s(Sema),CritSec,Y,s(ResetCtr)]).

### The partial deduction query

     :-  tr(Tr,X,[_,_,s(s(_)),_,_]).

### Example solution

For the more simple case X=0, it is possible to obtain a solution with
the [ECCE partial deduction system](../ecce.html) together with the more
specific program construction, which is basically as follows:

    tr(Tr,0,[_,_,s(s(_)),_,_]) :- fail.

For the general case, ECCE is not yet able to achieve this.

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
