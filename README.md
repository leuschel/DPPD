DPPD (Dozens of Problems for Partial Deduction)

The DPPD (Dozens of Problems for Partial Deduction) Library of Benchmarks
-------------------------------------------------------------------------

Maintained by Michael Leuschel. Suggestions, comments or new benchmark ideas, are always welcome.

This library aims at being a standard suite of benchmarks for partial deduction. It started out by the observation that the only accepted benchmark suite in partial deduction (the so called Lam and Kusalik benchmarks) contains too few benchmarks, most of which are too simple and too small. So the idea came up to generate something like the TPTP (Thousands of Problems for Theorem Proving) library, but for partial deduction.

The library contains benchmarks consisting of declarative logic programs, together with descriptions on the particular specialisation that should be performed by a partial deducer. The library also contains run-time queries by which the specialised program should be compared to the original.

### Allowed Built-in's

All built-in's which can be given a declarative semantics (maybe under some restrictions of the selection rule) are allowed. For instance the following built-in's might occur in some of the benchmark programs:

*   =../2
*   call/1
*   =/2
*   is/2
*   =/2
*   nonvar/1 (supposed to be delayed until its argument is nonvar)
*   ground/1 (supposed to be delayed until its argument is nonvar)
*   \\=/2, \\==/2 (supposed to be delayed until sufficiently instantiated)

### The Benchmarks

The benchmarks marked with (LK) are the original Lam and Kusalik benchmarks. Benchmarks marked with (JJ) were brought to my attention or designed by Jesper Jorgensen. More details about the origins of the benchmarks can usually be found in their respective descriptions.

#### Pure LP benchmarks

*   [advisor](dppd/advisor.md)
*   [applast](dppd/applast.md)
*   [ctl](dppd/ctl.html)
*   [contains](dppd/contains.lam.html) (LK)
*   [contains.kmp](dppd/contains.kmp.html)
*   [depth](dppd/depth.html) (LK)
*   [doubleapp](dppd/doubleapp.html)
*   [ex_depth](dppd/ex_depth.html)
*   [flip](dppd/flip.html) (JJ)
*   [grammar](dppd/grammar.html) (LK)
*   [groundunify.complex](dppd/groundunify.complex.html)
*   [groundunify.simple](dppd/groundunify.simple.html)
*   [imperative.power](dppd/imperative-solve.power.html)
*   [liftsolve.app](dppd/liftsolve.app.html)
*   [liftsolve.db1](dppd/liftsolve.db1.html)
*   [liftsolve.db2](dppd/liftsolve.db2.html)
*   [liftsolve.lmkng](dppd/liftsolve.lmkng.html)
*   [map.reduce](dppd/map.reduce.html)
*   [map.rev](dppd/map.rev.html)
*   [match-append](dppd/match-append.html) (JJ)
*   [match](dppd/match.html) (LK)
*   [match.kmp](dppd/match.kmp.html)
*   [maxlength](dppd/maxlength.html)
*   [memo-solve](dppd/memo-solve.html)
*   [missionaries](dppd/missionaries.html)
*   [model-elim.app](dppd/model_elim.html)
*   [petri-meta](dppd/petri-meta.html)
*   [petri-object](dppd/petri-object.html)
*   [regexp.r1](dppd/regexp.r1.html)
*   [regexp.r2](dppd/regexp.r2.html)
*   [regexp.r3](dppd/regexp.r3.html)
*   [relative](dppd/relative.html) (LK)
*   [remove](dppd/remove.html) (JJ)
*   [remove2](dppd/remove2.html) (JJ)
*   [rev\_acc\_type](dppd/rev_acc_type.html)
*   [rev\_acc\_type.inffail](dppd/rev_acc_type.inffail.html)
*   [rotateprune](dppd/rotateprune.html)
*   [ssuply](dppd/ssuply.html) (LK)
*   [transpose](dppd/transpose.html) (LK)
*   [upto.sum1](dppd/upto.sum1.html) (JJ)
*   [upto.sum2](dppd/upto.sum2.html) (JJ)

#### CLP benchmarks

*   [CTL CLP](dppd/ctlclp.html)
*   [loanMemo](dppd/loanMemo.html)
*   [loanUnfold](dppd/loanUnfold.html)
*   [multiply](dppd/multiply.html)

#### Interpreters, added after 2011

*   [dbaccess](dppd/dbaccess.html)
*   [javabc](dppd/javabc.html)
*   [lambdaint](dppd/lambdaint.html)
*   [picemul](dppd/picemul.html)
*   [vanilla-doubleapp](dppd/vanilla-doubleapp.html)

### Acknowledgements

Direct or indirect contributors to the above list are: _Elvira Albert, Andre de Waal, John Gallagher, Micky Gomez-Zamalloa, Robert Glueck, Kim Henriksen, Thomas Horvath, Jesper Jorgensen, John Gallagher, A. Kusalik, J. Lam, Bern Martens, Alberto Pettorossi, Maurizio Proietti, German Puebla, Morten Heine Sorensen, Valentin Turchin and Phil Wadler_ .

* * *