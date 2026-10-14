# .

* SAT 54
* UNSAT 71
* TIMEOUT 51
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-14 05:21:12 UTC
Job description: PR11126 full QF baseline: pinned master f05a5c651802; paired smoke integrity gate passed | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: pr11126-master-qf-https-zenodo.org-records-QF_ALIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1
Z3 commit message: Add Tseitin/De Morgan QF_UF rewrite matchers (demorgan_and, implies_as_or, eq_not)

Adds op_and/op_or/op_implies interpreted Boolean operators and three new
soundness-proved rewrite matchers to Z3PatternMatchedProofChecker.fst,
closing the dominant QF_UF unmatched-leaf shapes found in the Round 2
revisit (De Morgan and-expansion and eq/not rearrangement from Z3's
Tseitin CNF normalization). Mirrored in extract_z3_proof.py.

Re-ran the 100-file QF_UF benchmark at 30s timeout: matched leaves rose
from 82/167 to 143/167 (49% -> 86%), unmatched leaves dropped from 85 to
24, with all 98 extracted proofs still passing end-to-end (no
regressions). Documented in QF_AX_QF_UF_BENCHMARK_REPORT.md's new
'Round 3' section.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_ALIA/piVC/piVC_7fd2c4.smt2                |    0.022s | 20.384MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00004_001.cvc.smt2 |    0.022s | 20.856MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00003_001.cvc.smt2 |    0.023s | 20.864MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00001_001.cvc.smt2 |    0.023s | 20.132MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00002_001.cvc.smt2 |    0.023s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_174f4d.smt2                |    0.024s | 20.832MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00007_001.cvc.smt2 |    0.024s | 20.364MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00013_001.cvc.smt2 |    0.024s | 20.364MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00014_001.cvc.smt2 |    0.025s | 21.12MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00004_001.cvc.smt2 |    0.025s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00011_001.cvc.smt2 |    0.025s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00005_001.cvc.smt2 |    0.025s | 20.616MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00008_001.cvc.smt2 |    0.025s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_cb19c7.smt2                |    0.026s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00009_001.cvc.smt2 |    0.026s | 21.116MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th1-6.smt2 |    0.027s | 20.04MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th1-6.smt2 |    0.028s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00014_001.cvc.smt2 |    0.028s | 20.868MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_d421cb.smt2                |    0.029s | 21.372MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00009_001.cvc.smt2 |    0.030s | 20.32MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00015_001.cvc.smt2 |    0.030s | 20.864MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th2-6.smt2 |    0.032s | 20.104MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00007_001.cvc.smt2 |    0.033s | 20.776MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_ffa5fa.smt2                |    0.037s | 20.092MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_ed9849.smt2                |    0.037s | 21.112MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_5b181b.smt2                |    0.037s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00012_001.cvc.smt2 |    0.037s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00002_001.cvc.smt2 |    0.037s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th2-6.smt2 |    0.038s | 20.092MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/read2.smt2                       |    0.038s | 20.396MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00005_001.cvc.smt2 |    0.038s | 20.864MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-invalid-6.smt2 |    0.040s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00015_001.cvc.smt2 |    0.041s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00012_001.cvc.smt2 |    0.043s | 20.872MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00001_001.cvc.smt2 |    0.043s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00010_001.cvc.smt2 |    0.044s | 20.88MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00006_001.cvc.smt2 |    0.044s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00003_001.cvc.smt2 |    0.044s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00008_001.cvc.smt2 |    0.044s | 21.296MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-5.smt2 |    0.047s | 20.868MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-5.smt2 |    0.047s | 21.38MiB| sat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_408ff0.smt2                |    0.047s | 21.384MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00010_001.cvc.smt2 |    0.047s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_fdec13.smt2                |    0.050s | 21.128MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00013_001.cvc.smt2 |    0.050s | 20.884MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-15.smt2 |    0.051s | 23.424MiB| sat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00011_001.cvc.smt2 |    0.051s | 20.616MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00006_001.cvc.smt2 |    0.052s | 20.624MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-10.smt2 |    0.053s | 21.128MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_13f61c.smt2                |    0.053s | 20.84MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_509c40.smt2                |    0.055s | 22.568MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.5.smt2        |    0.055s | 21.448MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-10.smt2 |    0.056s | 22.412MiB| sat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_3031c9.smt2                |    0.056s | 23.0MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-dmem2.smt2                    |    0.060s | 22.392MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-dmem-a.smt2                   |    0.060s | 22.652MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_22b1f2.smt2                |    0.061s | 22.816MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.5.smt2             |    0.062s | 21.348MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata.smt2                |    0.062s | 22.144MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-5.smt2 |    0.063s | 21.368MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-20.smt2 |    0.065s | 24.46MiB| sat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_46582a.smt2                |    0.065s | 22.548MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata-a.smt2              |    0.069s | 22.408MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.6.smt2        |    0.072s | 21.892MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-5.smt2 |    0.073s | 21.38MiB| sat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_f5059f.smt2                |    0.076s | 22.308MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.8.smt2        |    0.084s | 22.42MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.7.smt2        |    0.085s | 22.144MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-15.smt2 |    0.086s | 22.396MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-20.smt2 |    0.108s | 22.916MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.6.smt2             |    0.113s | 21.132MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-10.smt2 |    0.130s | 23.164MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.9.smt2        |    0.150s | 23.172MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.10.smt2       |    0.156s | 23.648MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-10.smt2 |    0.191s | 23.444MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.7.smt2             |    0.202s | 21.632MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.11.smt2       |    0.242s | 23.944MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.8.smt2             |    0.355s | 22.396MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.16.smt2       |    0.366s | 25.992MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.13.smt2       |    0.403s | 25.456MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.14.smt2       |    0.424s | 25.476MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.12.smt2       |    0.430s | 24.196MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-20.smt2 |    0.566s | 27.712MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.20.smt2            |    0.566s | 27.972MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_3.smt2 |    0.634s | 32.128MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_4.smt2 |    0.634s | 31.908MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.9.smt2             |    0.636s | 22.404MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-15.smt2 |    0.642s | 25.48MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_1.smt2 |    0.642s | 32.14MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_2.smt2 |    0.649s | 31.88MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.15.smt2       |    0.685s | 25.972MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.18.smt2       |    0.779s | 27.648MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_3.smt2 |    0.791s | 35.336MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_7.smt2 |    0.792s | 35.356MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_6.smt2 |    0.823s | 35.572MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-15.smt2 |    0.854s | 26.724MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.15.smt2            |    0.971s | 26.24MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.17.smt2       |    0.987s | 27.644MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.16.smt2            |    1.029s | 27.016MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-10.smt2 |    1.124s | 23.416MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.10.smt2            |    1.230s | 23.172MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.17.smt2            |    1.311s | 27.012MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.18.smt2            |    1.461s | 28.28MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.19.smt2            |    1.512s | 28.78MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.21.smt2            |    2.022s | 31.476MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.11.smt2            |    2.242s | 23.948MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_6.smt2 |    2.626s | 49.76MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_7.smt2 |    2.698s | 49.516MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_5.smt2 |    2.733s | 61.272MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.23.smt2       |    3.290s | 32.712MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.12.smt2            |    3.297s | 24.64MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-1.i_0.smt2 |    4.101s | 46.256MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.22.smt2            |    4.575s | 33.532MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.13.smt2            |    5.134s | 25.116MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.14.smt2            |    5.777s | 25.988MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.24.smt2            |    6.796s | 35.7MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_2.smt2 |    8.388s | 67.364MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_1.smt2 |    8.463s | 67.248MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.23.smt2            |    9.129s | 36.464MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-15.smt2 |    9.628s | 27.276MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.19.smt2       |   14.312s | 33.892MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.21.smt2       |   14.544s | 34.676MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-20.smt2 |   14.824s | 34.184MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_0.smt2 |   18.784s | 66.8MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.25.smt2       |   19.984s | 40.104MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_1.smt2 |   20.014s | 63.836MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_6.smt2 |   20.016s | 63.72MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_time_var_mutex.i_0.smt2 |   20.017s | 76.12MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_0.smt2 |   20.019s | 63.752MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_1.smt2 |   20.020s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-2.i_0.smt2 |   20.020s | 101.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-016.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.021s | 27.484MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/CostasArray-15.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.021s | 28.884MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.27.smt2            |   20.021s | 37.464MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-1.i_0.smt2 |   20.022s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.29.smt2       |   20.022s | 39.236MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_3.smt2 |   20.023s | 63.66MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.26.smt2            |   20.023s | 37.056MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-2.i_0.smt2 |   20.024s | 63.912MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-019.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.024s | 28.72MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-1.i_0.smt2 |   20.024s | 126.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_7.smt2 |   20.025s | 134.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.28.smt2            |   20.025s | 37.876MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.26.smt2       |   20.025s | 34.896MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_dekker.i_0.smt2 |   20.026s | 68.064MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.30.smt2            |   20.026s | 38.98MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.22.smt2       |   20.026s | 35.164MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-011.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.027s | 29.064MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_8.smt2 |   20.027s | 128.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.30.smt2       |   20.029s | 39.228MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.25.smt2            |   20.030s | 38.116MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.29.smt2            |   20.030s | 40.876MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-20.smt2 |   20.031s | 32.392MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_10.smt2 |   20.031s | 127.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_5.smt2 |   20.031s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.20.smt2       |   20.031s | 35.172MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_4.smt2 |   20.032s | 63.684MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-015.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.033s | 27.3MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-2.i_0.smt2 |   20.033s | 116.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_7.smt2 |   20.034s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-1.i_0.smt2 |   20.034s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_2.smt2 |   20.036s | 63.548MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_peterson.i_0.smt2 |   20.037s | 70.424MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_6.smt2 |   20.037s | 99.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_11.smt2 |   20.038s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_2.smt2 |   20.038s | 121.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_4.smt2 |   20.038s | 129.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_6.smt2 |   20.038s | 126.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lamport.i_0.smt2 |   20.038s | 72.008MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.24.smt2       |   20.038s | 35.716MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_3.smt2 |   20.039s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_0.smt2 |   20.039s | 123.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-2.i_0.smt2 |   20.040s | 97.752MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.28.smt2       |   20.040s | 37.736MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.27.smt2       |   20.040s | 37.604MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_9.smt2 |   20.048s | 104.0MiB| timeout | 0 |  |
