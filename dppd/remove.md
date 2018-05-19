The "remove" Benchmark
----------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

This is a rather sophisticated deforestation example, handed over to me
by Jesper Jorgensen.

### The benchmark program

    rr(X,Y) :- r(X,T), r(T,Y).

    r([],[]).
    r([X],[X]).
    r([X,X|T],[X|T1]) :- r(T,T1).
    r([X,Y|T],[X|T1]) :- X \== Y, r([Y|T],T1).

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
