# .

* SAT 53
* UNSAT 71
* TIMEOUT 52
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_ALIA.tar.zst-do
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: ef66acc6b54144f10d6f64e9b557142ff438a31a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1
Z3 commit message: change calculation of threads to use total threads indicated by parameter or processor count, subtract from worker threads based on backbone and core threads

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th1-6.smt2 |    0.025s | 19.848MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/read2.smt2                       |    0.027s | 20.348MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00013_001.cvc.smt2 |    0.027s | 20.388MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00010_001.cvc.smt2 |    0.029s | 20.608MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_5b181b.smt2                |    0.031s | 20.46MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00007_001.cvc.smt2 |    0.033s | 20.84MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00001_001.cvc.smt2 |    0.033s | 20.224MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th2-6.smt2 |    0.037s | 19.848MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_174f4d.smt2                |    0.037s | 20.352MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00006_001.cvc.smt2 |    0.037s | 20.852MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00008_001.cvc.smt2 |    0.043s | 20.36MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00006_001.cvc.smt2 |    0.043s | 20.656MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00013_001.cvc.smt2 |    0.043s | 20.848MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_ed9849.smt2                |    0.044s | 20.888MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00014_001.cvc.smt2 |    0.045s | 20.544MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00009_001.cvc.smt2 |    0.047s | 20.372MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-5.smt2 |    0.048s | 20.952MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_fdec13.smt2                |    0.048s | 20.876MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_cb19c7.smt2                |    0.050s | 20.572MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00004_001.cvc.smt2 |    0.051s | 20.208MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00009_001.cvc.smt2 |    0.052s | 20.664MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_7fd2c4.smt2                |    0.053s | 20.396MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata.smt2                |    0.056s | 22.156MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00011_001.cvc.smt2 |    0.057s | 20.608MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th1-6.smt2 |    0.058s | 19.636MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_13f61c.smt2                |    0.058s | 21.1MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_ffa5fa.smt2                |    0.058s | 19.596MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00011_001.cvc.smt2 |    0.058s | 20.596MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00003_001.cvc.smt2 |    0.058s | 20.372MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-5.smt2 |    0.059s | 20.912MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00012_001.cvc.smt2 |    0.059s | 20.62MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00015_001.cvc.smt2 |    0.059s | 20.532MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th2-6.smt2 |    0.060s | 19.78MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-invalid-6.smt2 |    0.062s | 20.264MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00002_001.cvc.smt2 |    0.062s | 20.376MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00005_001.cvc.smt2 |    0.063s | 20.616MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00002_001.cvc.smt2 |    0.063s | 20.12MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00001_001.cvc.smt2 |    0.063s | 20.124MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00008_001.cvc.smt2 |    0.064s | 20.356MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00010_001.cvc.smt2 |    0.064s | 20.62MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00003_001.cvc.smt2 |    0.065s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00007_001.cvc.smt2 |    0.065s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00014_001.cvc.smt2 |    0.069s | 20.636MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_d421cb.smt2                |    0.070s | 20.876MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-5.smt2 |    0.071s | 20.92MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_408ff0.smt2                |    0.071s | 21.216MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00015_001.cvc.smt2 |    0.073s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_22b1f2.smt2                |    0.075s | 22.06MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/pp-dmem2.smt2                    |    0.076s | 22.24MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.7.smt2        |    0.079s | 21.82MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/pp-dmem-a.smt2                   |    0.080s | 22.032MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata-a.smt2              |    0.080s | 22.108MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_46582a.smt2                |    0.080s | 22.136MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-10.smt2 |    0.082s | 21.676MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.5.smt2        |    0.082s | 21.068MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-15.smt2 |    0.083s | 23.136MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-10.smt2 |    0.084s | 21.464MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_509c40.smt2                |    0.086s | 22.284MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00012_001.cvc.smt2 |    0.086s | 20.648MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-5.smt2 |    0.089s | 21.004MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00004_001.cvc.smt2 |    0.091s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00005_001.cvc.smt2 |    0.092s | 20.356MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.6.smt2        |    0.092s | 21.624MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_3031c9.smt2                |    0.095s | 22.748MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-20.smt2 |    0.097s | 23.848MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_f5059f.smt2                |    0.104s | 22.308MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.5.smt2             |    0.112s | 20.872MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-15.smt2 |    0.118s | 21.968MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.8.smt2        |    0.123s | 22.156MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.6.smt2             |    0.124s | 20.872MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-10.smt2 |    0.160s | 22.672MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-10.smt2 |    0.161s | 22.664MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-20.smt2 |    0.166s | 22.892MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.9.smt2        |    0.171s | 22.584MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.7.smt2             |    0.219s | 21.204MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.10.smt2       |    0.259s | 23.108MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.11.smt2       |    0.320s | 23.736MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-15.smt2 |    0.395s | 24.972MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.13.smt2       |    0.407s | 24.72MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.12.smt2       |    0.437s | 23.692MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.8.smt2             |    0.440s | 21.94MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.14.smt2       |    0.551s | 25.04MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.15.smt2       |    0.597s | 25.724MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.15.smt2            |    0.707s | 25.652MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_4.smt2 |    0.790s | 31.364MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_1.smt2 |    0.799s | 31.376MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.16.smt2            |    0.808s | 25.844MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_3.smt2 |    0.813s | 31.416MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_2.smt2 |    0.822s | 31.4MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.9.smt2             |    0.833s | 22.34MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_3.smt2 |    0.980s | 34.568MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_7.smt2 |    0.989s | 34.676MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_6.smt2 |    1.016s | 34.56MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.18.smt2            |    1.044s | 27.592MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.17.smt2       |    1.044s | 27.14MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.18.smt2       |    1.307s | 28.292MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.20.smt2            |    1.381s | 29.16MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.10.smt2            |    1.455s | 23.068MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-20.smt2 |    1.474s | 29.196MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-15.smt2 |    1.496s | 26.256MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.11.smt2            |    1.526s | 23.188MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-10.smt2 |    1.605s | 23.308MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.19.smt2            |    1.728s | 28.032MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.16.smt2       |    1.942s | 27.348MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_6.smt2 |    2.596s | 46.404MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.17.smt2            |    2.681s | 28.224MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_7.smt2 |    2.749s | 46.348MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.22.smt2            |    3.128s | 31.492MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.21.smt2            |    3.449s | 31.876MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_5.smt2 |    3.770s | 60.004MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.12.smt2            |    4.249s | 25.156MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.13.smt2            |    4.723s | 25.168MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-1.i_0.smt2 |    4.979s | 45.752MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.20.smt2       |    7.166s | 33.384MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.14.smt2            |    7.714s | 26.576MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-20.smt2 |    8.323s | 32.656MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-15.smt2 |    8.892s | 26.388MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_2.smt2 |    9.342s | 66.036MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_1.smt2 |   10.037s | 66.168MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_7.smt2 |   13.100s | 351.0MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.21.smt2       |   13.994s | 35.736MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.19.smt2       |   14.402s | 33.032MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-011.c_AllErrorsAtOnce_Iteration2_0.smt2 |   16.678s | 26.016MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.23.smt2            |   19.193s | 37.636MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.22.smt2       |   20.013s | 31.82MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.23.smt2       |   20.023s | 36.272MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.26.smt2       |   20.025s | 37.08MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.25.smt2            |   20.026s | 37.56MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.28.smt2            |   20.026s | 38.388MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.29.smt2       |   20.030s | 37.952MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.26.smt2            |   20.032s | 39.74MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.28.smt2       |   20.035s | 37.772MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.24.smt2            |   20.036s | 37.8MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.25.smt2       |   20.037s | 37.156MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.29.smt2            |   20.041s | 40.624MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.30.smt2       |   20.042s | 37.464MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.24.smt2       |   20.042s | 35.804MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_4.smt2 |   20.043s | 124.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.30.smt2            |   20.045s | 39.096MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_3.smt2 |   20.048s | 58.78MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-015.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.051s | 26.368MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.27.smt2       |   20.051s | 37.292MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.27.smt2            |   20.051s | 39.036MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_time_var_mutex.i_0.smt2 |   20.052s | 72.808MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-019.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.052s | 28.748MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_dekker.i_0.smt2 |   20.052s | 69.672MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/CostasArray-15.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.053s | 29.496MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-016.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.053s | 27.484MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_5.smt2 |   20.054s | 109.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_0.smt2 |   20.056s | 60.684MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_1.smt2 |   20.056s | 105.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-2.i_0.smt2 |   20.057s | 98.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_10.smt2 |   20.057s | 125.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_4.smt2 |   20.059s | 57.688MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-2.i_0.smt2 |   20.060s | 115.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-1.i_0.smt2 |   20.060s | 102.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_6.smt2 |   20.060s | 58.036MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_0.smt2 |   20.062s | 57.288MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_2.smt2 |   20.063s | 60.744MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-2.i_0.smt2 |   20.065s | 59.708MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-2.i_0.smt2 |   20.065s | 95.328MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_2.smt2 |   20.066s | 119.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_11.smt2 |   20.066s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_peterson.i_0.smt2 |   20.068s | 72.732MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lamport.i_0.smt2 |   20.068s | 71.852MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-1.i_0.smt2 |   20.069s | 124.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_3.smt2 |   20.072s | 107.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_1.smt2 |   20.076s | 57.412MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-20.smt2 |   20.080s | 32.336MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_9.smt2 |   20.085s | 100.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_7.smt2 |   20.086s | 121.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_6.smt2 |   20.089s | 98.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_6.smt2 |   20.090s | 121.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_8.smt2 |   20.090s | 125.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-1.i_0.smt2 |   20.091s | 101.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_0.smt2 |   20.091s | 116.0MiB| timeout | 0 |  |  |
