
SPEC = advisor applast contains.kmp contains.lam ctl ctlclp dbaccess depth doubleapp ex_depth flip grammar groundunify.complex groundunify.simple imperative-solve.power javabc lambdaint liftsolve.app liftsolve.db1 liftsolve.db2 liftsolve.lmkng loanMemo loanUnfold map.reduce map.rev match-append match match.kmp maxlength memo-solve missionaries model_elim multiply petri-meta petri-object picemul regexp.r1 regexp.r2 regexp.r3 relative remove remove2 rev_acc_type rev_acc_type.inffail rotateprune ssuply transpose upto.sum1 upto.sum2 vanilla-doubleapp

translate:
	for spec in $(SPEC); do \
		echo 'Translating: '; echo $$spec ; \
	    pandoc $$spec.html -o $$spec.md; \
		done
	echo "done"