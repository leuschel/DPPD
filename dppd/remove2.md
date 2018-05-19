The "remove2" Benchmark
-----------------------

Part of the [DPPD Library](../dppd.html).

### General Description

This is an even more sophisticated deforestation example, handed over to
me by Jesper Jorgensen and adapted from Turchin's paper *The concept of
a supercompiler* in TOPLAS, 8(3):292-325, 1986.

### The benchmark program

    rr(X,Y) :- f(X,T), f(T,Y).

    f([],[]).
    f([A|T],Y) :- h(A,T,Y).

    h(A,[],[A]).
    h(A,[B|S],Y) :- g(A,B,[B|S],S,Y).

    g(A,A,_,S,[A|Y]) :- f(S,Y).
    g(A,B,T,_,[A|Y]) :- A \== B,f(T,Y).

### The partial deduction query

     :- rr(X,Y).

### The run-time queries

     :- rr([a,a,b,b,a,a,a,a,c,d,a,b,a,a,d,d],Y).
     :- rr([a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z],Y).
     :- rr([a,b,b,b,c,d,e,f,g,g,h,i,j,k,l,m,m,m,m,m,m,m,m,
            n,o,p,q,r,s,t,u,v,v,v,v,w,x,y,z,z,z],Y).

### Example solution

    to be inserted

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
