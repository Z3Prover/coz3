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
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_RDL.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_RDL.tar.zst-dow
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: ef66acc6b54144f10d6f64e9b557142ff438a31a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_RDL.tar.zst?download=1
Z3 commit message: change calculation of threads to use total threads indicated by parameter or processor count, subtract from worker threads based on backbone and core threads

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_RDL/sal/fischer9-mutex-1.smt2             |    0.031s | 20.448MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-3.smt2             |    0.036s | 21.508MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-04.smt2 |    0.040s | 21.128MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking06.smt2 |    0.044s | 21.404MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-05.smt2 |    0.049s | 21.504MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-1.smt2 |    0.050s | 20.164MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-4.smt2             |    0.050s | 21.836MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-08.smt2 |    0.057s | 22.044MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking03.smt2 |    0.059s | 20.924MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-03.smt2 |    0.061s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_800.smt2              |    0.063s | 23.64MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-2.smt2 |    0.065s | 21.332MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-3.smt2             |    0.070s | 21.092MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-1.smt2             |    0.075s | 20.28MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/check/bignum_rdl1.smt2                |    0.077s | 19.904MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking10.smt2 |    0.078s | 22.068MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking07.smt2 |    0.079s | 21.424MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/check/bignum_rdl2.smt2                |    0.081s | 19.884MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_330.smt2             |    0.083s | 23.192MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking16.smt2 |    0.083s | 23.02MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking08.smt2 |    0.083s | 21.652MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-6.smt2             |    0.084s | 23.044MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-matrix1x1.pddl.smt2 |    0.085s | 20.264MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-5.smt2             |    0.086s | 22.492MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1000.smt2             |    0.088s | 23.512MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking01.smt2 |    0.088s | 20.392MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-07.smt2 |    0.088s | 21.98MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking02.smt2 |    0.089s | 20.848MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking18.smt2 |    0.089s | 23.344MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-3.smt2             |    0.089s | 20.62MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-2.smt2 |    0.093s | 21.092MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-06.smt2 |    0.094s | 21.576MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking04.smt2 |    0.095s | 21.108MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-6.smt2             |    0.096s | 20.904MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_250.smt2             |    0.098s | 22.224MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-10.smt2 |    0.100s | 22.308MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking05.smt2 |    0.101s | 21.128MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking09.smt2 |    0.103s | 21.872MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking21.smt2 |    0.103s | 23.9MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking22.smt2 |    0.105s | 23.916MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking11.smt2 |    0.106s | 22.184MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_850.smt2             |    0.110s | 23.948MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking13.smt2 |    0.110s | 22.4MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-1.smt2             |    0.110s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-09.smt2 |    0.111s | 22.524MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-2.smt2             |    0.113s | 20.32MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking14.smt2 |    0.116s | 22.756MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-7.smt2             |    0.116s | 22.336MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking15.smt2 |    0.117s | 22.964MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-5.smt2             |    0.120s | 20.904MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking12.smt2 |    0.122s | 22.264MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking17.smt2 |    0.122s | 23.256MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-2.smt2             |    0.125s | 20.736MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-7.smt2             |    0.139s | 21.728MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_800.smt2             |    0.143s | 23.992MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking19.smt2 |    0.143s | 23.508MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-3.smt2 |    0.145s | 23.084MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-9.smt2             |    0.145s | 21.744MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking20.smt2 |    0.150s | 23.592MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-5.smt2             |    0.150s | 21.712MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-10.smt2            |    0.152s | 22.052MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_800.smt2             |    0.158s | 23.72MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_700.smt2             |    0.159s | 22.436MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-4.smt2             |    0.160s | 21.224MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-4.smt2             |    0.162s | 21.468MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-8.smt2             |    0.163s | 21.332MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-6.smt2             |    0.174s | 22.036MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_700.smt2             |    0.178s | 23.472MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-2.smt2             |    0.178s | 20.88MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_1100.smt2             |    0.195s | 24.308MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-20.smt2 |    0.207s | 24.356MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-7.smt2             |    0.234s | 23.8MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1400.smt2             |    0.244s | 24.172MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_1000.smt2            |    0.245s | 24.832MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_700.smt2             |    0.248s | 23.788MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-8.smt2             |    0.260s | 22.752MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_800.smt2             |    0.268s | 23.684MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-11.smt2            |    0.309s | 22.288MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_900.smt2             |    0.326s | 24.168MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-5.smt2              |    0.339s | 34.724MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_550.smt2             |    0.345s | 24.84MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_1000.smt2            |    0.360s | 24.568MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-16.smt2            |    0.371s | 23.476MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-12.smt2            |    0.378s | 22.536MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-9.smt2             |    0.388s | 23.532MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_900.smt2              |    0.422s | 24.288MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-8.smt2             |    0.429s | 24.272MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-13.smt2            |    0.459s | 22.728MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_1100.smt2            |    0.468s | 24.976MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_800.smt2             |    0.527s | 24.212MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_1200.smt2            |    0.554s | 25.228MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-30.smt2 |    0.576s | 27.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_1000.smt2             |    0.603s | 24.48MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_850.smt2             |    0.638s | 24.464MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_950.smt2             |    0.653s | 24.716MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_900.smt2             |    0.662s | 24.768MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-14.smt2            |    0.681s | 23.208MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-5.base.cvc.smt2    |    0.729s | 50.112MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-6.base.cvc.smt2    |    0.734s | 56.08MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-40.smt2 |    0.749s | 28.192MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-15.smt2            |    0.752s | 23.504MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_830.smt2             |    0.788s | 24.492MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1300.smt2             |    0.792s | 24.896MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-3.smt2 |    0.836s | 24.688MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-7.base.cvc.smt2    |    0.850s | 60.312MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_900.smt2             |    0.913s | 25.224MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_1200.smt2            |    0.922s | 25.728MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-17.smt2            |    0.924s | 24.184MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_943.smt2              |    0.935s | 24.78MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-11.smt2            |    0.969s | 25.388MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-19.smt2            |    1.028s | 24.62MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-9.smt2             |    1.035s | 25.436MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-10.smt2            |    1.038s | 24.372MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-5.induction.cvc.smt2 |    1.050s | 66.824MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-12.smt2            |    1.182s | 24.948MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-18.smt2            |    1.248s | 24.46MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_1100.smt2            |    1.285s | 25.484MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-20.smt2            |    1.473s | 25.468MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_1100.smt2            |    1.484s | 25.484MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_430.smt2             |    1.485s | 25.576MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_1200.smt2            |    1.491s | 25.972MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-6.induction.cvc.smt2 |    1.568s | 84.228MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-9.base.cvc.smt2    |    1.583s | 87.928MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_750.smt2               |    1.591s | 65.572MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_1000.smt2            |    1.635s | 26.016MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_1000.smt2            |    1.635s | 25.68MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_900.smt2             |    1.650s | 24.972MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_750.smt2               |    1.923s | 65.876MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-4.smt2 |    1.938s | 27.936MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-7.induction.cvc.smt2 |    2.016s | 87.948MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-13.smt2            |    2.159s | 25.912MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-9.induction.cvc.smt2 |    2.235s | 124.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-10.smt2             |    2.236s | 60.388MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-8.base.cvc.smt2    |    2.317s | 84.02MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_750.smt2               |    2.392s | 66.48MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_1200.smt2            |    2.409s | 26.212MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_500.smt2              |    2.757s | 49.62MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-8.induction.cvc.smt2 |    2.927s | 107.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-11.smt2            |    3.055s | 27.448MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-10.induction.cvc.smt2 |    3.082s | 127.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_1100.smt2            |    3.140s | 26.3MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_900.smt2             |    3.152s | 25.868MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-10.smt2            |    3.212s | 27.884MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-11.induction.cvc.smt2 |    3.274s | 152.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_934.smt2             |    3.321s | 26.292MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-60.smt2 |    3.424s | 49.02MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_888.smt2             |    3.540s | 26.252MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1200.smt2             |    3.624s | 25.784MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-14.smt2            |    3.642s | 27.396MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-12.induction.cvc.smt2 |    3.806s | 154.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-13.induction.cvc.smt2 |    3.903s | 184.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-10.base.cvc.smt2   |    3.908s | 98.348MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-16.smt2            |    4.497s | 27.42MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_1100.smt2            |    4.614s | 26.868MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_1000.smt2            |    4.668s | 26.252MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-16.induction.cvc.smt2 |    4.896s | 239.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-14.induction.cvc.smt2 |    4.925s | 184.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-80.smt2 |    5.353s | 45.092MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_900.smt2             |    5.481s | 26.84MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-15.smt2            |    5.531s | 27.956MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_397.smt2             |    5.792s | 26.508MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-11.base.cvc.smt2   |    5.846s | 104.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-15.induction.cvc.smt2 |    6.091s | 224.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_1005.smt2            |    6.177s | 26.996MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-12.smt2            |    6.317s | 29.108MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_944.smt2             |    6.376s | 26.776MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-70.smt2 |    6.679s | 43.76MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_950.smt2             |    6.748s | 26.74MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_930.smt2             |    7.021s | 27.224MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-18.smt2            |    7.452s | 30.044MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-17.smt2            |    7.808s | 29.544MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_1100.smt2            |    8.050s | 27.68MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-17.induction.cvc.smt2 |    8.198s | 250.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_888.smt2             |    9.285s | 27.652MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-18.induction.cvc.smt2 |    9.434s | 275.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-19.smt2            |   10.244s | 30.916MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-13.base.cvc.smt2   |   10.494s | 117.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-14.smt2            |   10.917s | 30.98MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-12.base.cvc.smt2   |   10.991s | 112.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-90.smt2 |   11.220s | 47.68MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-19.induction.cvc.smt2 |   11.379s | 299.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1234.smt2             |   11.477s | 28.488MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_887.smt2             |   12.472s | 27.952MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-15.smt2             |   13.980s | 98.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-13.smt2            |   14.194s | 32.384MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-14.base.cvc.smt2   |   15.408s | 125.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-20.induction.cvc.smt2 |   15.564s | 371.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-20.smt2            |   17.079s | 32.308MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_1059.smt2            |   20.011s | 29.356MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-19.smt2            |   20.012s | 36.02MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-5.smt2 |   20.017s | 46.664MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_1000.smt2              |   20.037s | 92.816MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_1000.smt2            |   20.041s | 28.988MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_2900.smt2            |   20.043s | 204.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_862.smt2               |   20.047s | 84.684MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_860.smt2               |   20.049s | 86.416MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-15.smt2            |   20.049s | 34.368MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_600.smt2              |   20.053s | 66.528MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-5.smt2 |   20.053s | 80.272MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_800.smt2              |   20.054s | 75.852MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-4.smt2 |   20.054s | 46.288MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-20.smt2            |   20.054s | 37.22MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_969.smt2               |   20.055s | 90.088MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_910.smt2               |   20.056s | 88.604MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_2900.smt2            |   20.058s | 203.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_890.smt2               |   20.059s | 89.056MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_827.smt2               |   20.059s | 81.276MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-18.smt2            |   20.059s | 36.536MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_1010.smt2            |   20.061s | 29.388MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_691.smt2              |   20.061s | 73.932MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-20.smt2             |   20.062s | 125.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_950.smt2               |   20.063s | 89.336MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-16.base.cvc.smt2   |   20.064s | 177.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_2990.smt2            |   20.065s | 219.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-6.smt2 |   20.068s | 56.196MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_1005.smt2            |   20.070s | 28.9MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_2885.smt2            |   20.072s | 207.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_667.smt2              |   20.073s | 71.328MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-18.base.cvc.smt2   |   20.073s | 194.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_670.smt2              |   20.074s | 72.592MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_3004.smt2            |   20.075s | 206.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_894.smt2               |   20.076s | 86.756MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_950.smt2               |   20.077s | 89.988MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-7.smt2 |   20.077s | 89.316MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-matrix2x2.pddl.smt2 |   20.077s | 129.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-20.base.cvc.smt2   |   20.077s | 225.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-6.smt2 |   20.081s | 143.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-17.smt2            |   20.083s | 36.056MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-16.smt2            |   20.086s | 35.604MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_850.smt2               |   20.089s | 77.872MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-19.base.cvc.smt2   |   20.089s | 227.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-15.base.cvc.smt2   |   20.092s | 140.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv13_3104.smt2            |   20.105s | 208.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_2992.smt2            |   20.108s | 204.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv13_3000.smt2            |   20.111s | 216.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_919.smt2               |   20.137s | 86.68MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_700.smt2              |   20.139s | 73.124MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_828.smt2               |   20.141s | 81.316MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_850.smt2               |   20.147s | 82.384MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_950.smt2               |   20.149s | 91.692MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_2895.smt2            |   20.149s | 203.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_2800.smt2            |   20.151s | 205.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_1000.smt2            |   20.153s | 29.548MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv13_3150.smt2            |   20.153s | 217.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_950.smt2               |   20.157s | 90.512MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_2972.smt2            |   20.157s | 210.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-17.base.cvc.smt2   |   20.161s | 191.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_3000.smt2            |   20.163s | 206.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_2905.smt2            |   20.164s | 210.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_3050.smt2            |   20.166s | 222.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv13_3200.smt2            |   20.173s | 218.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_887.smt2               |   20.184s | 85.432MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_3050.smt2            |   20.191s | 205.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_2988.smt2            |   20.194s | 208.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_2983.smt2            |   20.200s | 201.0MiB| timeout | 0 |  |  |
