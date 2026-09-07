# .

* SAT 26
* UNSAT 21
* TIMEOUT 8
* UNKNOWN 3

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-07 02:31:03 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFNRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_UFNRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFNRA.tar.zst?download=1
Z3 commit message: Add witness_instantiation simplifier and fix TPTP preprocessing pipeline fixpoint (#10679)

## Summary

Adds a new opt-in TPTP preprocessing pass, ptp.witness_instantiation,
that synthesizes a fresh witness constant for any uninterpreted sort
with no ground term anywhere in the problem, and uses it to instantiate
orall-quantified formulas over that sort. This is sound (all sorts are
non-empty in classical logic) and lets search-time E-matching proceed on
axioms that would otherwise never fire for lack of a ground term to
match against (e.g. ANA068^1.p's FINITE_REAL_INTERVAL_1 axiom,
quantified over eal, with no ground eal term anywhere in the problem).

Also fixes a real bug in the TPTP preprocessing pipeline: it was only
flushed once before check_sat, so simplifiers that add new formulas
(lambda_reify_simplifier, leibniz_simplifier,
witness_instantiation_simplifier) never got a second pass to fully
simplify/rewrite those newly-added formulas. Adds an extra push()/pop(1)
flush right before check_sat, gated on any of these three passes being
enabled, guarded by a ry/catch so a latent lambda_reify_simplifier
sort-mismatch bug (which check_sat's own exception handling already
downgrades gracefully to GaveUp) can't escape as a hard crash.

This fixes ANA068^1.p and 3 of its 4 sibling problems with
ptp.reify_lambda_literals=true (previously all timed out).

## Testing

- Full 5,279-file TPTP higher-order regression confirms both
eify_lambda_literals and witness_instantiation have an inherent
net-negative trade-off at full corpus scale (gains on some problems,
timeouts on many others due to extra instantiation noise), consistent
with eify_lambda_literals's pre-existing regression profile
(historically measured at 53 gained / 591 lost even without this fix).
Both remain opt-in, default off; default behavior is unaffected.
- 99/99 unit tests pass (
inja test-z3 + ./test-z3.exe /a).
- Merged latest origin/master into this branch; rebuilt cleanly and
re-verified ANA068^1.p still solves.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_UFNRA/FFT/smtlib.631113.smt2              |    0.023s | 20.156MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631195.smt2              |    0.023s | 20.156MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/FFT/z3.641736.smt2                  |    0.040s | 19.624MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/FFT/smtlib.640350.smt2              |    0.041s | 19.62MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631277.smt2              |    0.041s | 20.172MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630949.smt2              |    0.042s | 19.9MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20190906-CLEARSY/0004/00003.smt2    |    0.043s | 20.676MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630785.smt2              |    0.045s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modSimpleTest.smt2 |    0.045s | 20.644MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvInitial.smt2 |    0.048s | 20.932MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631031.smt2              |    0.050s | 20.356MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630867.smt2              |    0.050s | 19.904MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/FFT/z3.630166.smt2                  |    0.050s | 20.188MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/l40m.smt2                       |    0.050s | 21.256MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvVar1.smt2 |    0.060s | 20.88MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvStep.smt2 |    0.061s | 21.136MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/l40f.smt2                       |    0.068s | 21.244MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStepFinal.smt2 |    0.069s | 21.12MiB| unknown | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStepFinala.smt2 |    0.085s | 20.92MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/m40.easy.smt2                   |    0.164s | 27.34MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/c40s.smt2                       |    0.185s | 29.068MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40m50.smt2                      |    0.209s | 30.188MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/c40m.smt2                       |    0.228s | 31.368MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40f50.smt2                      |    0.244s | 32.504MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/s40.smt2                        |    0.275s | 30.74MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/l40s.smt2                       |    0.284s | 32.712MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/c40f.smt2                       |    0.318s | 30.936MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40s50.smt2                      |    0.336s | 32.596MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40s99.smt2                      |    0.409s | 33.056MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40m25.smt2                      |    0.426s | 33.008MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40f10.smt2                      |    0.515s | 34.852MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40m99.smt2                      |    0.598s | 32.884MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40m10.smt2                      |    0.620s | 33.536MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40s10.smt2                      |    0.815s | 38.816MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/40f25.smt2                      |    0.823s | 33.776MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep2a.smt2 |    0.851s | 21.632MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep3a.smt2 |    0.867s | 21.548MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep4a.smt2 |    0.881s | 21.612MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep3.smt2 |    1.146s | 21.588MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep1.smt2 |    1.161s | 21.556MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep2.smt2 |    1.178s | 21.552MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/cas/40f99.smt2                      |    1.358s | 34.32MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep4.smt2 |    1.656s | 21.768MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/cas/40s25.smt2                      |    1.769s | 39.304MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/cas/10u05.04.smt2                   |    1.904s | 25.392MiB| sat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep1a.smt2 |    2.080s | 22.332MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep5.smt2 |    2.444s | 21.836MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep6.smt2 |    3.510s | 21.784MiB| unknown | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep5a.smt2 |    3.512s | 21.756MiB| unsat | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep7.smt2 |   13.466s | 22.18MiB| unknown | 0 |  |
|non-incremental/QF_UFNRA/cas/40u20.19.smt2                   |   20.018s | 41.484MiB| timeout | 0 |  |
|non-incremental/QF_UFNRA/cas/20u10.09.smt2                   |   20.021s | 32.608MiB| timeout | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep6a.smt2 |   20.025s | 22.464MiB| timeout | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep7a.smt2 |   20.031s | 23.524MiB| timeout | 0 |  |
|non-incremental/QF_UFNRA/cas/20revert.u.smt2                 |   20.032s | 33.696MiB| timeout | 0 |  |
|non-incremental/QF_UFNRA/cas/m40.smt2                        |   20.032s | 43.332MiB| timeout | 0 |  |
|non-incremental/QF_UFNRA/cas/30u15.14.smt2                   |   20.035s | 35.144MiB| timeout | 0 |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvFull.smt2 |   20.120s | 937.0MiB| timeout | 0 |  |
