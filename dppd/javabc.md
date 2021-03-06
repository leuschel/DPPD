The "javabc" Benchmark
----------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

javabc is a Java Bytecode interpreter from:

1.  M. Gomez-Zamalloa, E. Albert, G. Puebla, Improving the decompilation
    of Java bytecode to Prolog by partial evaluation, Electr. Notes
    Theor. Comput. Sci. 190 (1) (2007) 85-101.

### The benchmark program

Note: the program below also contains hints for the [BTA for Logen based
on
size-change-analysis](http://www.stups.uni-duesseldorf.de/w/Special:Publication/LeVi08_234).

        %:- module(small_jvm_int_I,[main/2],[assertions,regtypes]).

        % JVM interpreter intra-procedural and without heap.

        %%:- use_module(library(lists),[length/2,append/3]).  not required in SICS4

        % -----------------------------------------
        '$MEMO'(X) :- call(X).

        %:- use_module(library('jvm_in_ciao/interpreter/extended_lists'),[update/4]).

        update([_|Old],1,Value,[Value|Old]).
        update([X|Old],Index,Value,[X|New]):-
            Index>1,
            Index1 is Index - 1,
            update(Old,Index1,Value,New).

        % -----------------------------------------

        %%:- use_module(library(pe_types)).

        % Bytecode program
        %:- include('Counter_bc').
        localVar_length(2).

        bytecode(0,const(primitiveType(int),0),1).
        bytecode(1,istore(1),1).
        bytecode(2,iload(1),1).
        bytecode(3,iload(0),1).
        bytecode(4,if_icmp(geInt,9),3).  % jump to 13
        bytecode(7,iinc(1,1),3).
        bytecode(10,goto(-8),3).  % jump to 2
        bytecode(13,iload(1),1).
        bytecode(14,ireturn,1).


        %%:- include('ExpAlt_bc').


        % -----------------------------------------

        %:- entry main([X],Z).
        %:- entry main([X,Y],Z).

        % Main call. Initialize the state and calls execute/2.

        entry_driver(Nr,X,Top) :- Nr>0, entry(X,_), N1 is Nr-1, entry_driver(N1,X,Top).
        entry_driver(0,X,Top) :- entry(X,Top).

        entry(X,Top) :- main([X],Top).

        main(InputArgs,Top) :- 
            build_initial_state(InputArgs,PC,S0,V0),
            '$MEMO'(execute(PC,S0,V0,[Top|_],_)).

        %:- trust comp execute/2 : state * const + pe_type.
        %:- regtype state/1.
        state(PC,st(OS,LV)) :- f_sig(PC),dyn(OS),dyn(LV).

        % Main loop of the interpreter
        execute(PC,S1,V1,Sf,Vf) :-
            bytecode(PC,Inst,_),
            step(Inst,PC,S1,V1,NPC,S2,V2),
            '$MEMO'(execute(NPC,S2,V2,Sf,Vf)).
        execute(PC,[Top|_],_,[Top|_],_) :-
            bytecode(PC,ireturn,_).

        % Step definition. There would be one (or more) per bytecode instruction.
        step(const(_T,Z),PC,S,L,PCp,[Z|S],L) :-
                next(PC,PCp).

        step(istore(X),PC,[I|S],L,PCp,S,Lb) :- 
            next(PC,PCp),
            localVar_update(L,X,I,Lb).

        step(iload(X),PC,S,L,PCp,[I|S],L) :-
            next(PC,PCp),
            localVar_get(L,X,I).

        step(goto(O),PC,S,L,PCp,S,L) :-
            PCp is PC+O.

        step(ibinop(Op),PC,[I2,I1|S],L,PCp,[R|S],L) :-
            next(PC,PCp),
            semBinopInt(Op,I1,I2,R).

        step(iinc(X,Z),PC,S,L,PCp,S,Lb) :-
            next(PC,PCp),
            localVar_get(L,X,I),
            semBinopInt(addInt,I,Z,R),
            localVar_update(L,X,R,Lb).


        step(if_icmp(Cmp,O),PC,[I2,I1|S],L,PCp,S,L) :-
            semCompInt(Cmp,I1,I2),
            PCp is PC+O.
        step(if_icmp(Cmp,_O),PC,[I2,I1|S],L,PCp,S,L) :-
            next(PC,PCp),
            noSemCompInt(Cmp,I1,I2).

        %%%%%%%%%%%%%%%%%%%%%%%
        % Auxiliar predicates %
        %%%%%%%%%%%%%%%%%%%%%%%

        %:- trust comp next/2 + (eval,sideff(free)).
        next(PC,PCp) :-
            bytecode(PC,_,N),
            PCp is PC + N.

        %:- trust comp localVar_get/3 + (eval,sideff(free)).
        localVar_get(L,X,V) :-
            N is X+1,
            my_nth1(N,L,V).


        my_nth1(1, [H|_], H).
        my_nth1(N, [_|T], R) :-
            N > 1,
            N1 is N - 1,
            my_nth1(N1, T, R).

        %:- trust comp localVar_update/4 + (eval,sideff(free)).
        localVar_update(Old,X,V,New) :-
            N is X+1,
            update(Old,N,V,New).

        %:- trust comp build_initial_state/2  + (eval,sideff(free)).
        build_initial_state(Arguments,0,[],L) :-
            localVar_length(I),
            length(Arguments,ArgumentSize),
            S is I-ArgumentSize,
            length(RL,S),
            init(RL,0,S),
            append(Arguments,RL,L).

        init([],_,0).
        init([Value|L],Value,I):-
            I > 0,
            I1 is I-1,
            init(L,Value,I1).


        %%%%%%%%%%%%%%%%%%%%%%%%%
        % Arithmetic operations %
        %%%%%%%%%%%%%%%%%%%%%%%%%

        eqInt(I1,I1).
        neInt(I1,I2) :-
            I1 \= I2.
        ltInt(I1,I2) :-
            I1 < I2.
        leInt(I1,I2) :-
            I1 =< I2.
        geInt(I1,I2) :-
            I1 >= I2.
        gtInt(I1,I2) :-
            I1 > I2.

        semBinopInt(addInt,I1,I2,R) :-
            R is I1 + I2. 
        semBinopInt(subInt,I1,I2,R) :-
            R is I1 - I2. 
        semBinopInt(divInt,I1,I2,R) :-
            R is I1 // I2. 
        semBinopInt(mulInt,I1,I2,R) :-
            R is I1 * I2. 
        semBinopInt(remInt,I1,I2,R) :-
            R is I1 rem I2. 

        semCompInt(eqInt,I1,I2):-
            eqInt(I1,I2).
        semCompInt(neInt,I1,I2):-
            neInt(I1,I2).
        semCompInt(ltInt,I1,I2):-
            ltInt(I1,I2).
        semCompInt(leInt,I1,I2):-
            leInt(I1,I2).
        semCompInt(geInt,I1,I2):-
            geInt(I1,I2).
        semCompInt(gtInt,I1,I2):-
            gtInt(I1,I2).

        noSemCompInt(eqInt,I1,I2):-
            neInt(I1,I2).
        noSemCompInt(neInt,I1,I2):-
            eqInt(I1,I2).
        noSemCompInt(ltInt,I1,I2):-
            geInt(I1,I2).
        noSemCompInt(leInt,I1,I2):-
            gtInt(I1,I2).
        noSemCompInt(geInt,I1,I2):-
            ltInt(I1,I2).
        noSemCompInt(gtInt,I1,I2):-
            leInt(I1,I2).

### The partial deduction query

     :- entry_driver(Nr,X,Y).

### The run-time queries

     :- entry_driver(50000,5,R).

### Example solution

The following was obtained using
`ecce javabc/small_jvm_int_I.pl -pe "entry_driver(Nr,X,Y).""`:


        /* Specialised program generated by ECCE 2.0 */
        /* PD Goal: A */
        /* Parameters: Abs:l InstCheck:v Msv:s NgSlv:g Part:e Prun:n Sel:t Whstl:f Raf:yesFar:yes Dce:yes Poly:y Dpu:yes ParAbs:yes Msvp:no Rrc:yes */
        /* Transformation time: 400 ms */
        /* Unfolding time: 340 ms */
        /* Post-Processing time: 400 ms */

        /* Specialised Predicates: 
        entry_driver__1(A,B,C) :- entry_driver(A,B,C).
        init__2(A,B) :- init(A,0,B).
        step_conj__3(A,B,C) :- step(if_icmp(geInt,9),4,[A,0],[A,0|D1],B,[],[A,0|D1]), bytecode(B,C,E1).
        step__4(A,B,C,D,E,F,G) :- step(A,B,[],[C,0|D],E,F,G).
        execute__5(A,B,C,D) :- execute(A,B,C,[D|E1],F1).
        bytecode_conj__6(A,B,C,D) :- bytecode(A,E1,F1), step(E1,A,B,C,G1,H1,I1), '$MEMO'(execute(G1,H1,I1,[D|J1],K1)).
        step__7(A,B) :- step(if_icmp(geInt,9),4,[A,0|C1],[A,0|D1],B,C1,[A,0|D1]).
        step__8(A,B,C) :- step(if_icmp(geInt,9),4,[A,B|D1],[A|E1],C,D1,[A|E1]).
        step__9(A,B,C) :- step(if_icmp(geInt,9),4,[A,B|D1],E1,C,D1,E1).
        step__10(A,B,C) :- step(if_icmp(geInt,9),4,[A,B|D1],[A,B|E1],C,D1,[A,B|E1]).
        bytecode__11(A,B) :- bytecode(A,C1,B).
        my_nth1__12(A,B,C,D) :- my_nth1(A,[B|C],D).
        update__13(A,B,C,D,E,F) :- update([A|B],C,D,[E|F]).
        */

        entry_driver(A,B,C) :- 
            entry_driver__1(A,B,C).
        entry_driver__1(A,B,C) :- 
            A > 0, 
            length([B],D), 
            E is '-'(2,D), 
            length(F,E), 
            init__2(F,E), 
            append([B],F,[G,H|I]), 
            step_conj__3(G,J,K), 
            step__4(K,J,G,I,L,M,N), 
            execute__5(L,M,N,O), 
            P is '-'(A,1), 
            entry_driver__1(P,B,C).
        entry_driver__1(0,A,B) :- 
            length([A],C), 
            D is '-'(2,C), 
            length(E,D), 
            init__2(E,D), 
            append([A],E,[F,G|H]), 
            step_conj__3(F,I,J), 
            step__4(J,I,F,H,K,L,M), 
            execute__5(K,L,M,B).
        init__2([],0).
        init__2([0|A],B) :- 
            B > 0, 
            C is '-'(B,1), 
            init__2(A,C).
        step_conj__3(A,13,iload(1)) :- 
            0 >= A.
        step_conj__3(A,7,iinc(1,1)) :- 
            0 < A.
        step__4(const(A,B),C,D,E,F,[B],[D,0|E]) :- 
            bytecode__11(C,G), 
            F is '+'(C,G).
        step__4(iload(A),B,C,D,E,[F],[C,0|D]) :- 
            bytecode__11(B,G), 
            E is '+'(B,G), 
            H is '+'(A,1), 
            my_nth1__12(H,C,[0|D],F).
        step__4(goto(A),B,C,D,E,[],[C,0|D]) :- 
            E is '+'(B,A).
        step__4(iinc(A,B),C,D,E,F,[],[G|H]) :- 
            bytecode__11(C,I), 
            F is '+'(C,I), 
            J is '+'(A,1), 
            my_nth1__12(J,D,[0|E],K), 
            L is '+'(K,B), 
            M is '+'(A,1), 
            update__13(D,[0|E],M,L,G,H).
        execute__5(A,B,C,D) :- 
            bytecode_conj__6(A,B,C,D).
        execute__5(14,[A|B],C,A).
        bytecode_conj__6(0,A,[B,C|D],E) :- 
            step__7(B,F), 
            execute__5(F,A,[B,0|D],E).
        bytecode_conj__6(1,[A|B],[C,D|E],F) :- 
            step__10(C,A,G), 
            execute__5(G,B,[C,A|E],F).
        bytecode_conj__6(2,A,[B,C|D],E) :- 
            step__10(B,C,F), 
            execute__5(F,A,[B,C|D],E).
        bytecode_conj__6(3,[A|B],[C|D],E) :- 
            step__8(C,A,F), 
            execute__5(F,B,[C|D],E).
        bytecode_conj__6(4,[A,B|C],D,E) :- 
            step__9(A,B,F), 
            execute__5(F,C,D,E).
        bytecode_conj__6(7,A,[B,C|D],E) :- 
            F is '+'(C,1), 
            step__10(B,F,G), 
            execute__5(G,A,[B,F|D],E).
        bytecode_conj__6(10,A,[B,C|D],E) :- 
            step__10(B,C,F), 
            execute__5(F,A,[B,C|D],E).
        bytecode_conj__6(13,A,[B,C|D],C).
        step__7(A,13) :- 
            0 >= A.
        step__7(A,7) :- 
            0 < A.
        step__8(A,B,13) :- 
            B >= A.
        step__8(A,B,7) :- 
            B < A.
        step__9(A,B,13) :- 
            B >= A.
        step__9(A,B,7) :- 
            B < A.
        step__10(A,B,13) :- 
            B >= A.
        step__10(A,B,7) :- 
            B < A.
        bytecode__11(0,1).
        bytecode__11(1,1).
        bytecode__11(2,1).
        bytecode__11(3,1).
        bytecode__11(4,3).
        bytecode__11(7,3).
        bytecode__11(10,3).
        bytecode__11(13,1).
        bytecode__11(14,1).
        my_nth1__12(1,A,B,A).
        my_nth1__12(A,B,[C|D],E) :- 
            A > 1, 
            F is '-'(A,1), 
            my_nth1__12(F,C,D,E).
        update__13(A,B,1,C,C,B).
        update__13(A,[B|C],D,E,A,[F|G]) :- 
            D > 1, 
            H is '-'(D,1), 
            update__13(B,C,H,E,F,G).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
