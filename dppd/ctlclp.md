The "ctl(clpr)" Benchmark
-------------------------

Part of the [DPPD
Library](https://github.com/leuschel/DPPD).

### General Description

This is based upton a CTL model checker written for XSB-Prolog from the
DPPD library. It has been rewritten to use clp(r).

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

    /* classical Prolog version:

    trans(enter_cs,[X,Sema,CritSec,Y,C],
               [X1,Sema1,Cs1,Y,C]) :-
             X>0, X1 is X-1,
             Sema>0, Sema1 is Sema-1,
             CritSec>=0,
             Cs1 is CritSec+1,
             Y>=0, C>=0.
    trans(exit_cs, [X,Sema,CritSec,Y,C],
                   [X,Sema1,CritSec1,Y1,C]) :-
             CritSec>0, CritSec1 is CritSec-1,
             Sema>=0,Sema1 is Sema + 1,
             Y>=0,Y1 is Y+1,
             C>=0.
    trans(restart, [X,Sema,CritSec,Y,ResetCtr],
                   [X1,Sema,CritSec,Y1,ResetCtr1]) :-
               Y>0, Y1 is Y-1,
               X>= 0, X1 is X+1,
               Sema >= 0, CritSec >= 0,
               ResetCtr>=0, ResetCtr<10, ResetCtr1 is ResetCtr + 1.
               
    prop([_X,_Sema,CritSec,_Y,_C],unsafe) :- CritSec>1.
    prop([0,_Sema,0,0,_C],deadlock).
    prop([_X,0,0,0,_C],deadlock).

    */
               
               
    trans(enter_cs,[X,Sema,CritSec,Y,C],
               [X1,Sema1,Cs1,Y,C]) :-
             {X>0}, {X1 = X-1},
             {Sema>0}, {Sema1 = Sema-1},
             {CritSec>=0},
             {Cs1 = CritSec+1},
             {Y>=0}, {C>=0}.
    trans(exit_cs, [X,Sema,CritSec,Y,C],
                   [X,Sema1,CritSec1,Y1,C]) :-
             {CritSec>0, CritSec1 = CritSec-1,
             Sema>=0,Sema1 = Sema + 1,
             Y>=0,Y1 = Y+1,
             C>=0}.
    trans(restart, [X,Sema,CritSec,Y,ResetCtr],
                   [X1,Sema,CritSec,Y1,ResetCtr1]) :-
               {Y>0, Y1 = Y-1,
               X>= 0, X1 = X+1,
               Sema >= 0, CritSec >= 0,
               ResetCtr>=0, ResetCtr<10, ResetCtr1 = ResetCtr + 1}.
               /* print(restart([X,Sema,CritSec,Y,ResetCtr])),nl. */

    prop([_X,_Sema,CritSec,_Y,_C],unsafe) :- {CritSec>1}.
    prop([0,_Sema,0,0,_C],deadlock).
    prop([_X,0,0,0,_C],deadlock).



    mc(X) :- sat([X,1,0,0,0],ef(p(unsafe))).
    mc(_).

    mc2(X) :- sat([X,1,0,0,0],ef(p(deadlock))).

    mc3(A,E,B,C,D) :-
        sat([A,E,B,C,D],ef(p(deadlock))).
    mc3(A,B,C,D,E).    

### The partial deduction query

:- mc3(A,B,C,D,E).

### The run-time query

:- mc3(2,1,0,0,0).

### Example solution

This program was specialised using Logen with the clpr extension. It
runs 35% faster than the original program.


    :- use_module(library(clpr)).
    :- consult('../PrintBench.pl').

    sat_eu__4(B,C,D,E,F) :- 
      {
        G = ((-1) + B),
      H = ((-1) + C),
      I = ((1) + D),
      B > (0),
      D >= (0)
      },
      sat_eu__2(G,H,I,E,F).
    sat_eu__4(J,K,L,M,N) :- 
      {
        O = ((-1) + L),
      P = ((1) + K),
      Q = ((1) + M),
      L > (0)
      },
      sat_eu__4(J,P,O,Q,N).
    sat_eu__4(R,S,T,U,V) :- 
      {
        V < (10),
      W = ((-1) + U),
      X = ((1) + R),
      Y = ((1) + V),
      T >= (0),
      R >= (0)
      },
      sat_eu__1(X,S,T,W,Y).
    sat_eu__3(B,C,D,E,F) :- 
      {
        G = ((-1) + B),
      H = ((-1) + C),
      I = ((1) + D),
      B > (0)
      },
      sat_eu__2(G,H,I,E,F).
    sat_eu__3(J,K,L,M,N) :- 
      {
        O = ((-1) + L),
      P = ((1) + K),
      Q = ((1) + M),
      L > (0)
      },
      sat_eu__4(J,P,O,Q,N).
    sat_eu__3(R,S,T,U,V) :- 
      {
        V < (10),
      W = ((-1) + U),
      X = ((1) + R),
      Y = ((1) + V),
      R >= (0)
      },
      sat_eu__1(X,S,T,W,Y).
    sat_eu__5(B,C,D,E,F) :- 
      {
        G = ((-1) + B),
      H = ((-1) + C),
      I = ((1) + D),
      C > (0),
      E >= (0)
      },
      sat_eu__2(G,H,I,E,F).
    sat_eu__5(J,K,L,M,N) :- 
      {
        O = ((-1) + L),
      P = ((1) + K),
      Q = ((1) + M),
      M >= (0)
      },
      sat_eu__1(J,P,O,Q,N).
    sat_eu__5(R,S,T,U,V) :- 
      {
        V < (10),
      W = ((-1) + U),
      X = ((1) + R),
      Y = ((1) + V),
      U > (0)
      },
      sat_eu__5(X,S,T,W,Y).
    sat_eu__2(B,C,D,E,F) :- 
      {
        G = ((-1) + B),
      H = ((-1) + C),
      I = ((1) + D),
      C > (0),
      B > (0)
      },
      sat_eu__2(G,H,I,E,F).
    sat_eu__2(J,K,L,M,N) :- 
      {
        O = ((-1) + L),
      P = ((1) + K),
      Q = ((1) + M),
      K >= (0)
      },
      sat_eu__3(J,P,O,Q,N).
    sat_eu__2(R,S,T,U,V) :- 
      {
        V < (10),
      W = ((-1) + U),
      X = ((1) + R),
      Y = ((1) + V),
      U > (0),
      S >= (0),
      R >= (0)
      },
      sat_eu__5(X,S,T,W,Y).
    sat_eu__6(B,C,D,E,F) :- 
      {
        G = ((-1) + B),
      H = ((-1) + C),
      I = ((1) + D),
      D >= (0)
      },
      sat_eu__2(G,H,I,E,F).
    sat_eu__6(J,K,L,M,N) :- 
      {
        O = ((-1) + L),
      P = ((1) + K),
      Q = ((1) + M),
      L > (0)
      },
      sat_eu__4(J,P,O,Q,N).
    sat_eu__6(R,S,T,U,V) :- 
      {
        V < (10),
      W = ((-1) + U),
      X = ((1) + R),
      Y = ((1) + V),
      T >= (0)
      },
      sat_eu__1(X,S,T,W,Y).
    sat_eu__7(B,0,0,0,C).
    sat_eu__7(D,E,F,G,H) :- 
      {
        I = ((-1) + D),
      J = ((-1) + E),
      K = ((1) + F),
      E > (0),
      G >= (0)
      },
      sat_eu__2(I,J,K,G,H).
    sat_eu__7(L,M,N,O,P) :- 
      {
        Q = ((-1) + N),
      R = ((1) + M),
      S = ((1) + O),
      N > (0),
      O >= (0)
      },
      sat_eu__6(L,R,Q,S,P).
    sat_eu__7(T,U,V,W,X) :- 
      {
        X < (10),
      Y = ((-1) + W),
      Z = ((1) + T),
      A1 = ((1) + X),
      W > (0)
      },
      sat_eu__7(Z,U,V,Y,A1).
    sat_eu__1(B,0,0,0,C).
    sat_eu__1(D,E,F,G,H) :- 
      {
        I = ((-1) + D),
      J = ((-1) + E),
      K = ((1) + F),
      E > (0)
      },
      sat_eu__2(I,J,K,G,H).
    sat_eu__1(L,M,N,O,P) :- 
      {
        Q = ((-1) + N),
      R = ((1) + M),
      S = ((1) + O),
      N > (0)
      },
      sat_eu__6(L,R,Q,S,P).
    sat_eu__1(T,U,V,W,X) :- 
      {
        X < (10),
      Y = ((-1) + W),
      Z = ((1) + T),
      A1 = ((1) + X),
      W > (0)
      },
      sat_eu__7(Z,U,V,Y,A1).
    mc3(A,B,C,D,E) :- 
      mc3__0(A,B,C,D,E).
    mc3__0(B,C,D,E,F) :- 
      sat_eu__1(B,C,D,E,F).

    mc3__0(B,F,C,D,E). %added for timing only

------------------------------------------------------------------------

[Michael Leuschel](http://www.ecs.soton.ac.uk/%7Emal/michael.html)
