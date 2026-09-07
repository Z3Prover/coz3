# .

* SAT 93
* UNSAT 95
* TIMEOUT 67
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-07 01:03:44 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_RDL.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_RDL
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_RDL.tar.zst?download=1
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
|non-incremental/QF_RDL/sal/fischer3-mutex-2.smt2             |    0.033s | 20.444MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-2.smt2 |    0.039s | 21.568MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking04.smt2 |    0.043s | 21.08MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-6.smt2             |    0.049s | 21.152MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking10.smt2 |    0.053s | 22.176MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-6.smt2             |    0.055s | 21.976MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking03.smt2 |    0.062s | 20.688MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-matrix1x1.pddl.smt2 |    0.065s | 20.24MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-06.smt2 |    0.067s | 21.708MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking01.smt2 |    0.070s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_700.smt2             |    0.072s | 22.54MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-1.smt2 |    0.076s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-2.smt2 |    0.076s | 21.416MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-04.smt2 |    0.079s | 21.484MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking06.smt2 |    0.084s | 21.456MiB| sat | 0 |  |
|non-incremental/QF_RDL/check/bignum_rdl1.smt2                |    0.085s | 20.128MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1000.smt2             |    0.085s | 23.428MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking02.smt2 |    0.085s | 20.832MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-6.smt2             |    0.087s | 22.98MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-3.smt2             |    0.093s | 20.752MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking08.smt2 |    0.095s | 21.684MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-10.smt2 |    0.095s | 22.6MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking07.smt2 |    0.101s | 21.816MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking15.smt2 |    0.101s | 22.848MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking05.smt2 |    0.102s | 21.356MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-03.smt2 |    0.102s | 21.14MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-4.smt2             |    0.103s | 21.86MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-09.smt2 |    0.104s | 22.284MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-8.smt2             |    0.104s | 21.444MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_700.smt2             |    0.107s | 23.44MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-08.smt2 |    0.108s | 21.932MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-07.smt2 |    0.110s | 21.932MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking17.smt2 |    0.112s | 23.22MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking13.smt2 |    0.114s | 22.528MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking09.smt2 |    0.114s | 21.864MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_330.smt2             |    0.118s | 23.092MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_850.smt2             |    0.119s | 24.2MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_800.smt2              |    0.121s | 23.496MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking11.smt2 |    0.126s | 22.36MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_250.smt2             |    0.127s | 22.416MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_700.smt2             |    0.130s | 23.804MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking12.smt2 |    0.131s | 22.436MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking18.smt2 |    0.133s | 23.28MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking14.smt2 |    0.134s | 22.772MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-05.smt2 |    0.134s | 21.4MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-5.smt2             |    0.136s | 21.172MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-7.smt2             |    0.137s | 21.18MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-1.smt2             |    0.137s | 20.44MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking16.smt2 |    0.142s | 23.116MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-1.smt2             |    0.143s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-2.smt2             |    0.144s | 20.704MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_800.smt2             |    0.146s | 23.976MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-2.smt2             |    0.151s | 20.896MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-3.smt2             |    0.153s | 20.876MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_800.smt2             |    0.154s | 23.88MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-3.smt2 |    0.154s | 23.236MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking22.smt2 |    0.155s | 24.068MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking20.smt2 |    0.158s | 23.612MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-9.smt2             |    0.163s | 21.812MiB| unsat | 0 |  |
|non-incremental/QF_RDL/check/bignum_rdl2.smt2                |    0.164s | 19.852MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking19.smt2 |    0.166s | 23.46MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking21.smt2 |    0.167s | 23.928MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-3.smt2             |    0.167s | 21.408MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-4.smt2             |    0.170s | 20.728MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-1.smt2             |    0.174s | 20.24MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_550.smt2             |    0.186s | 24.892MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_1100.smt2             |    0.187s | 24.424MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1400.smt2             |    0.193s | 24.164MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-4.smt2             |    0.193s | 21.28MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-5.smt2             |    0.198s | 21.524MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-5.smt2             |    0.214s | 22.46MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_800.smt2             |    0.215s | 23.956MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_1000.smt2            |    0.222s | 24.724MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-7.smt2             |    0.245s | 22.224MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-10.smt2            |    0.253s | 21.988MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-20.smt2 |    0.258s | 24.404MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-11.smt2            |    0.296s | 22.24MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_1000.smt2            |    0.297s | 24.568MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-8.smt2             |    0.305s | 24.216MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-8.smt2             |    0.309s | 22.992MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_1100.smt2            |    0.340s | 25.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_900.smt2             |    0.340s | 24.368MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_1000.smt2             |    0.346s | 24.696MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-14.smt2            |    0.398s | 22.948MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-5.smt2              |    0.411s | 34.62MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-12.smt2            |    0.431s | 22.504MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_800.smt2             |    0.447s | 24.196MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_1200.smt2            |    0.452s | 25.16MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_850.smt2             |    0.456s | 24.248MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-7.smt2             |    0.459s | 23.544MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_830.smt2             |    0.525s | 24.424MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_950.smt2             |    0.541s | 24.768MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-30.smt2 |    0.554s | 26.832MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-13.smt2            |    0.558s | 22.736MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-9.smt2             |    0.599s | 23.64MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-15.smt2            |    0.649s | 23.336MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_900.smt2              |    0.681s | 24.424MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_900.smt2             |    0.705s | 24.728MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1300.smt2             |    0.707s | 25.008MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-17.smt2            |    0.723s | 24.128MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-5.base.cvc.smt2    |    0.738s | 50.256MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-3.smt2 |    0.772s | 24.724MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_900.smt2             |    0.775s | 25.128MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-40.smt2 |    0.850s | 28.272MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_943.smt2              |    0.869s | 24.588MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_1200.smt2            |    0.903s | 25.776MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_1100.smt2            |    0.916s | 25.472MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-6.base.cvc.smt2    |    0.950s | 56.292MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-11.smt2            |    0.973s | 25.476MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-7.base.cvc.smt2    |    1.035s | 60.232MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-16.smt2            |    1.036s | 23.788MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-19.smt2            |    1.081s | 24.648MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-10.smt2            |    1.124s | 24.46MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_1100.smt2            |    1.244s | 25.436MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-18.smt2            |    1.246s | 24.552MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-5.induction.cvc.smt2 |    1.252s | 66.808MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-9.smt2             |    1.260s | 25.532MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-20.smt2            |    1.278s | 25.288MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_1000.smt2            |    1.388s | 25.664MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_430.smt2             |    1.407s | 25.568MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_1200.smt2            |    1.462s | 26.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_1200.smt2            |    1.557s | 26.18MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_1000.smt2            |    1.578s | 26.092MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-10.smt2             |    1.652s | 59.336MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_900.smt2             |    1.686s | 24.976MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-12.smt2            |    1.690s | 25.104MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_750.smt2               |    1.716s | 66.536MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-9.base.cvc.smt2    |    1.730s | 87.748MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-13.smt2            |    1.778s | 26.008MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-7.induction.cvc.smt2 |    1.802s | 88.088MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-4.smt2 |    1.998s | 28.06MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-6.induction.cvc.smt2 |    2.120s | 84.128MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-9.induction.cvc.smt2 |    2.181s | 124.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_750.smt2               |    2.187s | 65.52MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-8.base.cvc.smt2    |    2.495s | 83.968MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-8.induction.cvc.smt2 |    2.546s | 107.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_750.smt2               |    2.567s | 66.056MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_500.smt2              |    2.665s | 49.596MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_1100.smt2            |    2.669s | 26.372MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-10.smt2            |    2.733s | 27.632MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-10.induction.cvc.smt2 |    2.903s | 127.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_900.smt2             |    3.052s | 25.88MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-11.smt2            |    3.079s | 27.556MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_888.smt2             |    3.296s | 26.184MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-60.smt2 |    3.311s | 48.964MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-11.induction.cvc.smt2 |    3.332s | 152.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1200.smt2             |    3.545s | 25.888MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-14.smt2            |    3.644s | 27.444MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-12.induction.cvc.smt2 |    3.674s | 154.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-13.induction.cvc.smt2 |    3.850s | 184.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_934.smt2             |    3.913s | 26.288MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-16.smt2            |    4.153s | 27.568MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-10.base.cvc.smt2   |    4.158s | 98.104MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_1100.smt2            |    4.633s | 26.996MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-14.induction.cvc.smt2 |    4.641s | 185.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_1000.smt2            |    4.911s | 26.556MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_397.smt2             |    5.239s | 26.368MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-16.induction.cvc.smt2 |    5.384s | 239.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-80.smt2 |    5.457s | 45.136MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_900.smt2             |    5.617s | 26.888MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-15.induction.cvc.smt2 |    5.680s | 223.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-11.base.cvc.smt2   |    5.956s | 104.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-12.smt2            |    5.956s | 28.848MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-15.smt2            |    5.974s | 28.66MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_1005.smt2            |    6.118s | 26.976MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_944.smt2             |    6.145s | 26.836MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-18.smt2            |    6.376s | 29.82MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_950.smt2             |    6.729s | 26.76MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_930.smt2             |    6.892s | 27.024MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-17.smt2            |    7.107s | 29.464MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-70.smt2 |    7.357s | 43.88MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-17.induction.cvc.smt2 |    7.700s | 251.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_1100.smt2            |    8.126s | 27.584MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_888.smt2             |    8.930s | 27.788MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-13.base.cvc.smt2   |    9.694s | 118.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-18.induction.cvc.smt2 |    9.924s | 276.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-19.smt2            |   10.558s | 30.86MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-19.induction.cvc.smt2 |   10.921s | 300.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-14.smt2            |   10.959s | 31.44MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-12.base.cvc.smt2   |   11.377s | 112.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-90.smt2 |   11.453s | 47.72MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1234.smt2             |   11.511s | 28.34MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_887.smt2             |   11.736s | 28.26MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-15.smt2             |   13.019s | 99.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-20.smt2            |   13.567s | 31.392MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-20.induction.cvc.smt2 |   13.992s | 370.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-13.smt2            |   14.259s | 32.448MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-14.base.cvc.smt2   |   16.942s | 125.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_1000.smt2            |   20.011s | 29.54MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_827.smt2               |   20.016s | 80.828MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_1000.smt2              |   20.016s | 93.548MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_850.smt2               |   20.020s | 82.564MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_969.smt2               |   20.021s | 90.128MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-5.smt2 |   20.024s | 80.164MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_2885.smt2            |   20.030s | 202.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-19.base.cvc.smt2   |   20.032s | 228.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_1005.smt2            |   20.032s | 28.964MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_2990.smt2            |   20.032s | 221.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_1059.smt2            |   20.032s | 29.328MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_800.smt2              |   20.036s | 75.68MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_3050.smt2            |   20.036s | 224.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_887.smt2               |   20.038s | 85.308MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_862.smt2               |   20.041s | 84.808MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-18.smt2            |   20.041s | 35.932MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_1010.smt2            |   20.042s | 29.516MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_670.smt2              |   20.043s | 72.052MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_1000.smt2            |   20.045s | 29.064MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_950.smt2               |   20.045s | 90.46MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_2895.smt2            |   20.049s | 203.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_2972.smt2            |   20.049s | 213.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_667.smt2              |   20.050s | 72.1MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_3000.smt2            |   20.050s | 205.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_2900.smt2            |   20.051s | 209.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_3004.smt2            |   20.051s | 206.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv13_3000.smt2            |   20.052s | 216.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_2800.smt2            |   20.052s | 205.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-4.smt2 |   20.053s | 46.192MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_950.smt2               |   20.056s | 91.668MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_919.smt2               |   20.057s | 86.68MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_910.smt2               |   20.058s | 88.584MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_2905.smt2            |   20.059s | 214.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_600.smt2              |   20.059s | 66.444MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_950.smt2               |   20.060s | 90.624MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_850.smt2               |   20.060s | 77.912MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-5.smt2 |   20.061s | 46.784MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_860.smt2               |   20.064s | 86.468MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-matrix2x2.pddl.smt2 |   20.065s | 129.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-6.smt2 |   20.069s | 56.312MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv13_3150.smt2            |   20.072s | 214.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_3050.smt2            |   20.072s | 203.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_2988.smt2            |   20.073s | 207.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_890.smt2               |   20.073s | 89.032MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-6.smt2 |   20.073s | 142.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_828.smt2               |   20.074s | 81.188MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_2983.smt2            |   20.076s | 201.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_950.smt2               |   20.076s | 89.428MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv13_3200.smt2            |   20.081s | 216.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-20.smt2             |   20.082s | 130.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-7.smt2 |   20.083s | 88.5MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-16.base.cvc.smt2   |   20.086s | 176.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-20.base.cvc.smt2   |   20.094s | 224.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_700.smt2              |   20.101s | 73.244MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-20.smt2            |   20.110s | 37.24MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_2992.smt2            |   20.116s | 207.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-16.smt2            |   20.130s | 36.616MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_2900.smt2            |   20.136s | 205.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-15.smt2            |   20.138s | 34.284MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-17.smt2            |   20.146s | 36.232MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-19.smt2            |   20.146s | 37.068MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-18.base.cvc.smt2   |   20.148s | 194.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-15.base.cvc.smt2   |   20.149s | 140.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_691.smt2              |   20.149s | 73.892MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_894.smt2               |   20.151s | 86.812MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv13_3104.smt2            |   20.154s | 208.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-17.base.cvc.smt2   |   20.160s | 192.0MiB| timeout | 0 |  |
