DPPD (Dozens of Problems for Partial Deduction)

The DPPD (Dozens of Problems for Partial Deduction) Library of Benchmarks
-------------------------------------------------------------------------

Maintained by Michael Leuschel. Suggestions, comments or new benchmark ideas, are always welcome.

This library aims at being a standard suite of benchmarks for partial deduction. It started out by the observation that the only accepted benchmark suite in partial deduction (the so called Lam and Kusalik benchmarks) contains too few benchmarks, most of which are too simple and too small. So the idea came up to generate something like the TPTP (Thousands of Problems for Theorem Proving) library, but for partial deduction.

The library contains benchmarks consisting of declarative logic programs, together with descriptions on the particular specialisation that should be performed by a partial deducer.
The library also contains run-time queries by which the specialised program should be compared to the original.

See also the [ECCE](https://github.com/leuschel/ecce) partial deduction system.

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
*   [ctl](dppd/ctl.md)
*   [contains](dppd/contains.lam.md) (LK)
*   [contains.kmp](dppd/contains.kmp.md)
*   [depth](dppd/depth.md) (LK)
*   [doubleapp](dppd/doubleapp.md)
*   [ex_depth](dppd/ex_depth.md)
*   [flip](dppd/flip.md) (JJ)
*   [grammar](dppd/grammar.md) (LK)
*   [groundunify.complex](dppd/groundunify.complex.md)
*   [groundunify.simple](dppd/groundunify.simple.md)
*   [imperative.power](dppd/imperative-solve.power.md)
*   [liftsolve.app](dppd/liftsolve.app.md)
*   [liftsolve.db1](dppd/liftsolve.db1.md)
*   [liftsolve.db2](dppd/liftsolve.db2.md)
*   [liftsolve.lmkng](dppd/liftsolve.lmkng.md)
*   [map.reduce](dppd/map.reduce.md)
*   [map.rev](dppd/map.rev.md)
*   [match-append](dppd/match-append.md) (JJ)
*   [match](dppd/match.md) (LK)
*   [match.kmp](dppd/match.kmp.md)
*   [maxlength](dppd/maxlength.md)
*   [memo-solve](dppd/memo-solve.md)
*   [missionaries](dppd/missionaries.md)
*   [model-elim.app](dppd/model_elim.md)
*   [petri-meta](dppd/petri-meta.md)
*   [petri-object](dppd/petri-object.md)
*   [regexp.r1](dppd/regexp.r1.md)
*   [regexp.r2](dppd/regexp.r2.md)
*   [regexp.r3](dppd/regexp.r3.md)
*   [relative](dppd/relative.md) (LK)
*   [remove](dppd/remove.md) (JJ)
*   [remove2](dppd/remove2.md) (JJ)
*   [rev\_acc\_type](dppd/rev_acc_type.md)
*   [rev\_acc\_type.inffail](dppd/rev_acc_type.inffail.md)
*   [rotateprune](dppd/rotateprune.md)
*   [ssuply](dppd/ssuply.md) (LK)
*   [transpose](dppd/transpose.md) (LK)
*   [upto.sum1](dppd/upto.sum1.md) (JJ)
*   [upto.sum2](dppd/upto.sum2.md) (JJ)

#### CLP benchmarks

*   [CTL CLP](dppd/ctlclp.md)
*   [loanMemo](dppd/loanMemo.md)
*   [loanUnfold](dppd/loanUnfold.md)
*   [multiply](dppd/multiply.md)

#### Interpreters, added after 2011

*   [dbaccess](dppd/dbaccess.md)
*   [javabc](dppd/javabc.md)
*   [lambdaint](dppd/lambdaint.md)
*   [picemul](dppd/picemul.md)
*   [vanilla-doubleapp](dppd/vanilla-doubleapp.md)

### Acknowledgements

Direct or indirect contributors to the above list are: _Elvira Albert, Andre de Waal, John Gallagher, Micky Gomez-Zamalloa, Robert Glueck, Kim Henriksen, Thomas Horvath, Jesper Jorgensen, John Gallagher, A. Kusalik, J. Lam, Bern Martens, Alberto Pettorossi, Maurizio Proietti, German Puebla, Morten Heine Sorensen, Valentin Turchin and Phil Wadler_ .

* * *