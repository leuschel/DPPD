The "match.kmp" Benchmark
-------------------------

Part of the [DPPD Library](https://github.com/leuschel/DPPD).

### General Description

This is exactly like the Lam & Kusalik benchmark [match](match.html),
except that the run-time queries are more sophisticated than the ones
for [match](match.html).

### The run-time queries

     :- match([a,a,b], [a,a,a,a,c,d,a,a,a,e,f,g,h,a,a,b,d,f]).
     :- match([a,a,b], [a,b,a,b,a,a,a,a,c,a,a,a,a,a,a,a,a,b]).
     :- match([a,a,b], [a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,a,b]).
     :- match([a,a,b], [a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,a,a,b,w,x,y,z]).

------------------------------------------------------------------------

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)
