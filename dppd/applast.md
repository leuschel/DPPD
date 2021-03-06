DPPD: applast

The "applast" Benchmark
-----------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

This is a benchmark by Michael Leuschel which contains no built-in's nor negations. Solving this benchmarks requires conjunctive partial deductions as well as a bottom-up success propagation. It is a simple example which illustrates the difficulties that can arise when specialising the ground representation. More details can be found in the technical reports [CW 210](http://www.cs.kuleuven.ac.be/cwis/research/dtai/publications/abstracts.95.html#CW210.abstract) and [CW 232](http://www.cs.kuleuven.ac.be/cwis/research/dtai/publications/abstracts.96.html#CW232.abstract).

### The benchmark program

applast(L,X,Last) :- append(L,\[X\],LX),last(Last,LX).

last(X,\[X\]).
last(X,\[H|T\]) :- last(X,T).

append(\[\],L,L).
append(\[H|L1\],L2,\[H|L3\]) :- append(L1,L2,L3).

### The partial deduction query

 :\- applast(L,X,Last).

### The run-time queries

 :\- applast(\[a,b,c,d\],L,e).
 :\- applast(\[a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,a,a,b,w,x,y\],z,L).
 :\- applast(\[a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,a,a,b,w,x,y\],z,q).

### Example solution

Combined with a bottom-up propagation (for more details see the technical report [CW 232](http://www.cs.kuleuven.ac.be/cwis/research/dtai/publications/abstracts.96.html#CW232.abstract) ) the [ECCE partial deduction system](https://github.com/leuschel/ecce) can obtain the following program:

 applast__1(L,a) :- al(L).

 al(\[\]).
 al(\[H|T\]) :- al(T).

* * *

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)