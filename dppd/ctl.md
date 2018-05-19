The "ctl" Benchmark
-------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

This is a CTL model checker written for XSB-Prolog to be specialised for
an infinite state Petri net and a safety property (however, the runtime
query is so that it will also terminate under ordinary Prolog).

### The benchmark program

    /* A Model Checker for CTL fomulas */
    /* written for XSB-Prolog */
    /* by Michael Leuschel, Thierry Massart */


    sat(_E,true).
    sat(_E,false) :- fail.

    sat(E,p(P)) :- prop(E,P). /* elementary proposition */

    sat(E,and(F,G)) :- sat(E,F), sat(E,G).

    sat(E,or(F,_G)) :- sat(E,F).
    sat(E,or(_F,G)) :- sat(E,G).

    sat(E,not(F)) :- not(sat(E,F)).

    sat(E,en(F)) :- /* exists next */
        trans(_Act,E,E2),sat(E2,F).

    sat(E,an(F)) :- /* always next */
        not(sat(E,en(not(F)))).

    sat(E,eu(F,G)) :- /* exists until */
        sat_eu(E,F,G).

    sat(E,au(F,G)) :- /* always until */
      sat(E,not(eu(not(G),and(not(F),not(G))))),
      sat_noteg(E,not(G)).

    sat(E,ef(F)) :- /* exists future */
     sat(E,eu(true,F)).

    sat(E,af(F)) :- /* always future */
      sat_noteg(E,not(F)).

    sat(E,eg(F)) :- /* exists global */
      not(sat_noteg(E,F)). /* we want gfp -> negate lfp of negation */

    sat(E,ag(F)) :- /* always global */
      sat(E,not(ef(not(F)))).
      

    /* :- table sat_eu/3.*/
    /* tabulation to compute least-fixed point using XSB */
      
    sat_eu(E,_F,G) :- /* exists until */
        sat(E,G).
    sat_eu(E,F,G) :- /* exists until */
       sat(E,F), trans(_Act,E,E2), sat_eu(E2,F,G).


    /* :- table sat_noteg/2.*/
    /* tabulation to compute least-fixed point using XSB */

    sat_noteg(E,F) :-
       sat(E,not(F)).
    sat_noteg(E,F) :-
       not(  (trans(_Act,E,E2), not(sat_noteg(E2, F) ))  ).
       /* loop via double negation --> monotonic --> ok */
       
       /*
       sat(E,an(noteg(F)))  -->
       
       not(sat(E,en(not(noteg(F)))))  -->
       
       not(  trans(_Act,E,E2),sat(E2, not(noteg(F)) )  )  -->
       
       not(  trans(_Act,E,E2), not(sat(E2, noteg(F)) )  ) -->
       
       not(  trans(_Act,E,E2), not(sat_noteg(E2, F) )  )
       */


    trans(enter_cs,[s(X),s(Sema),CritSec,Y,C],
               [X,Sema,s(CritSec),Y,C]).
    trans(exit_cs, [X,Sema,s(CritSec),Y,C],
               [X,s(Sema),CritSec,s(Y),C]).
    trans(restart, [X,Sema,CritSec,s(Y),ResetCtr],
               [s(X),Sema,CritSec,Y,s(ResetCtr)]).

    prop([X,Sema,s(s(CritSec)),Y,C],unsafe).
    prop([0,Sema,0,0,C],deadlock).
    prop([X,0,0,0,C],deadlock).

### The partial deduction query

     :- sat(A,ef(p(unsafe))).

### The run-time query (30 times)

     :- sat([s(s(0)),0,0,s(s(s(s(0)))),0],ef(p(unsafe))).

### Example solution

The following specialised program can be obtained by the [LOGEN partial
deduction system](/~mal/systems/logen.html). This runs amost 6 times
faster than the original.

    sat_eu__1([B,C,s(s(D)),E,F]).
    sat_eu__1([s(G),s(H),I,J,K]) :- 
      sat_eu__1([G,H,s(I),J,K]).
    sat_eu__1([L,M,s(N),O,P]) :- 
      sat_eu__1([L,s(M),N,s(O),P]).
    sat_eu__1([Q,R,S,s(T),U]) :- 
      sat_eu__1([s(Q),R,S,T,s(U)]).
    sat__0(B) :- 
      sat_eu__1(B).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
