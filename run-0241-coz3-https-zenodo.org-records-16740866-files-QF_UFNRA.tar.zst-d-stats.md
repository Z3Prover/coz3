# .

* SAT 31
* UNSAT 24
* TIMEOUT 3
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFNRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFNRA.tar.zst-d
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: ef66acc6b54144f10d6f64e9b557142ff438a31a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFNRA.tar.zst?download=1
Z3 commit message: change calculation of threads to use total threads indicated by parameter or processor count, subtract from worker threads based on backbone and core threads

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_UFNRA/20190906-CLEARSY/0004/00003.smt2    |    0.023s | 20.356MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631195.smt2              |    0.024s | 20.212MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631031.smt2              |    0.024s | 20.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/z3.641736.smt2                  |    0.024s | 20.192MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630785.smt2              |    0.025s | 20.36MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631113.smt2              |    0.026s | 20.356MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modSimpleTest.smt2 |    0.027s | 20.568MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630867.smt2              |    0.028s | 20.236MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.640350.smt2              |    0.029s | 19.468MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631277.smt2              |    0.033s | 20.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvInitial.smt2 |    0.033s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630949.smt2              |    0.035s | 20.264MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvVar1.smt2 |    0.045s | 21.068MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/l40f.smt2                       |    0.046s | 21.504MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/l40m.smt2                       |    0.048s | 21.392MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/z3.630166.smt2                  |    0.050s | 20.156MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvStep.smt2 |    0.055s | 20.924MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStepFinala.smt2 |    0.063s | 21.22MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStepFinal.smt2 |    0.073s | 20.98MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/m40.easy.smt2                   |    0.152s | 26.936MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/c40s.smt2                       |    0.196s | 28.66MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/c40f.smt2                       |    0.224s | 30.896MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40s50.smt2                      |    0.225s | 30.108MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/l40s.smt2                       |    0.343s | 36.868MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40m25.smt2                      |    0.403s | 33.1MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep2.smt2 |    0.409s | 21.396MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/c40m.smt2                       |    0.429s | 32.056MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40f25.smt2                      |    0.506s | 33.1MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40m50.smt2                      |    0.549s | 33.364MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40f10.smt2                      |    0.705s | 35.368MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep2a.smt2 |    0.721s | 21.508MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep4a.smt2 |    0.765s | 21.668MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40m99.smt2                      |    0.828s | 33.768MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40s25.smt2                      |    0.869s | 33.384MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40s10.smt2                      |    0.878s | 35.64MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40f50.smt2                      |    1.026s | 34.144MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/20u10.09.smt2                   |    1.130s | 30.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40s99.smt2                      |    1.133s | 34.648MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep1.smt2 |    1.146s | 21.836MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep3a.smt2 |    1.209s | 21.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40m10.smt2                      |    1.295s | 35.488MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40f99.smt2                      |    1.553s | 34.44MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep3.smt2 |    1.778s | 21.488MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep6.smt2 |    2.129s | 21.776MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep4.smt2 |    2.206s | 21.716MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep5.smt2 |    2.379s | 22.12MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/s40.smt2                        |    2.596s | 36.936MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep1a.smt2 |    5.341s | 21.712MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep6a.smt2 |    6.316s | 22.064MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/20revert.u.smt2                 |    7.020s | 33.408MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep5a.smt2 |    7.472s | 22.58MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40u20.19.smt2                   |    8.071s | 51.532MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/10u05.04.smt2                   |    9.081s | 25.776MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/m40.smt2                        |   10.410s | 51.532MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep7.smt2 |   14.647s | 23.304MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep7a.smt2 |   20.010s | 22.392MiB| timeout | 0 |  |  |
|non-incremental/QF_UFNRA/cas/30u15.14.smt2                   |   20.018s | 38.36MiB| timeout | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvFull.smt2 |   20.106s | 939.0MiB| timeout | 0 |  |  |
