# .

* SAT 87
* UNSAT 33
* TIMEOUT 57
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-14 11:44:48 UTC
Job description: PR11126 full QF baseline: pinned master f05a5c651802; paired smoke integrity gate passed | Benchmark suite: https://zenodo.org/records/16740866/files/QF_BVFPLRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: pr11126-master-qf-https-zenodo.org-records-QF_BVFPLRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_BVFPLRA.tar.zst?download=1
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
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |    0.026s | 20.348MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_while_true-unreach-call.i_AllErrorsAtOnce_Iteration3_TraceCheck_0.smt2 |    0.026s | 20.34MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    0.030s | 20.1MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_while_false-unreach-call.i_AllErrorsAtOnce_Iteration3_TraceCheck_0.smt2 |    0.032s | 20.348MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_7.smt2 |    0.032s | 20.256MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_4.smt2 |    0.032s | 19.848MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Rump_double_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    0.032s | 20.348MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/schanda/spark/zeros_consistent_3.smt2 |    0.034s | 20.104MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |    0.036s | 20.344MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_0.smt2 |    0.037s | 20.176MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_5.smt2 |    0.037s | 19.84MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_2.smt2 |    0.039s | 20.092MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_6.smt2 |    0.043s | 20.104MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_1.smt2 |    0.044s | 19.844MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_biNewton_pseudoconstant_true-unreach-call.c_2.smt2 |    0.060s | 30.584MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_3.smt2 |    0.151s | 32.616MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_5.smt2 |    0.158s | 32.148MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_0.smt2 |    0.167s | 32.384MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_true-unreach-call.i_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |    0.197s | 45.252MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_4.smt2 |    0.265s | 37.136MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float21_true-unreach-call.i_1.smt2 |    0.370s | 33.692MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float21_true-unreach-call.i_0.smt2 |    0.382s | 33.692MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_4.smt2 |    0.406s | 104.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_for_false-unreach-call.i_4.smt2 |    0.416s | 37.652MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_3.smt2 |    0.489s | 111.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_2.smt2 |    0.510s | 111.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_1.smt2 |    0.513s | 59.788MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c.p+cfa-reducer.c_1.smt2 |    0.517s | 118.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_5.smt2 |    0.534s | 111.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_for_false-unreach-call.i_3.smt2 |    0.553s | 37.904MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_2.smt2 |    0.580s | 60.812MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_10.smt2 |    0.598s | 73.1MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_3.smt2 |    0.656s | 73.46MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0240a_true-unreach-call.c_0.smt2 |    0.678s | 145.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_while_false-unreach-call.i_3.smt2 |    0.711s | 53.38MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_while_true-unreach-call.i_2.smt2 |    0.729s | 53.128MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_9.smt2 |    0.752s | 73.724MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_8.smt2 |    0.766s | 73.716MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_for_true-unreach-call.i_4.smt2 |    0.845s | 38.672MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0920b_true-unreach-call.c_3.smt2 |    0.976s | 85.856MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_5_true-unreach-call_true-termination.i_0.smt2 |    1.027s | 128.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter1_true-unreach-call.c.p+cfa-reducer.c_0.smt2 |    1.052s | 120.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter1_true-unreach-call.c.v+lhb-reducer.c_3.smt2 |    1.074s | 121.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c_0.smt2 |    1.076s | 150.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_biNewton_pseudoconstant_true-unreach-call.c_4.smt2 |    1.139s | 301.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0661a_true-unreach-call.c_0.smt2 |    1.154s | 309.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0684a_true-unreach-call.c_1.smt2 |    1.166s | 309.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_for_true-unreach-call.i_1.smt2 |    1.195s | 69.228MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_5.smt2 |    1.199s | 112.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_while_false-unreach-call.i_0.smt2 |    1.204s | 68.984MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_2.smt2 |    1.206s | 309.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_2.smt2 |    1.340s | 312.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0971_true-unreach-call.c_1.smt2 |    1.379s | 167.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_for_false-unreach-call.i_0.smt2 |    1.391s | 86.372MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_2.smt2 |    1.418s | 165.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float-div1_true-unreach-call.i_0.smt2 |    1.437s | 111.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float-div1_true-unreach-call.i_1.smt2 |    1.440s | 111.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_1.smt2 |    1.517s | 137.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_1_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    1.567s | 111.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_8.smt2 |    1.785s | 202.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    1.888s | 28.86MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_8.smt2 |    2.026s | 170.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_0.smt2 |    2.060s | 37.956MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_4.smt2 |    2.071s | 36.104MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_2.smt2 |    2.074s | 37.904MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_11.smt2 |    2.205s | 37.876MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c.p+cfa-reducer.c_0.smt2 |    2.272s | 150.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0930_true-unreach-call.c_0.smt2 |    2.372s | 228.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_7.smt2 |    2.389s | 38.156MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_9.smt2 |    2.411s | 37.904MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_8_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    2.702s | 201.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0960a_true-unreach-call.c_0.smt2 |    3.215s | 223.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_7_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    3.387s | 208.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_0.smt2 |    3.952s | 193.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_0.smt2 |    3.960s | 193.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_1_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    4.149s | 83.188MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_3_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    4.315s | 108.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter2_set_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    4.373s | 113.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_6_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    4.478s | 209.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_2_false-unreach-call_true-termination.i_0.smt2 |    4.653s | 428.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_6_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    4.653s | 393.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_8_true-unreach-call_true-termination.i_0.smt2 |    4.685s | 427.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_2_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    4.709s | 107.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_4_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    4.776s | 208.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_biNewton_pseudoconstant_true-unreach-call.c_1.smt2 |    4.997s | 424.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0686b_true-unreach-call.c_0.smt2 |    5.434s | 445.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_4.smt2 |    5.526s | 370.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_1.smt2 |    5.742s | 492.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0930_true-unreach-call.c_1.smt2 |    5.943s | 230.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0920b_true-unreach-call.c_1.smt2 |    6.470s | 234.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_poly_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    6.732s | 102.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_2.smt2 |    6.737s | 217.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0684a_true-unreach-call.c_0.smt2 |    7.174s | 599.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_8_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    7.383s | 394.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_7_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    7.890s | 389.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_2.smt2 |    8.967s | 512.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Newton_pseudoconstant_true-unreach-call.c_4.smt2 |    9.033s | 286.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_3.smt2 |    9.130s | 515.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0520_true-unreach-call.c_6.smt2 |    9.174s | 798.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0832_true-unreach-call.c_3.smt2 |    9.393s | 729.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_3.smt2 |    9.490s | 729.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0530a_true-unreach-call.c_7.smt2 |    9.893s | 796.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_17.smt2 |   10.245s | 710.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0931_true-unreach-call.c_0.smt2 |   10.759s | 234.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_315.smt2 |   11.406s | 1023.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0920b_true-unreach-call.c_0.smt2 |   11.412s | 239.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_8.smt2 |   11.539s | 797.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_422.smt2 |   11.576s | 1023.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_446.smt2 |   11.731s | 1023.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0882_true-unreach-call.c_1.smt2 |   12.275s | 375.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_5_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   12.297s | 205.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_3.smt2 |   12.370s | 1142.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0530a_true-unreach-call.c_3.smt2 |   12.802s | 795.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_430.smt2 |   14.523s | 1024.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_68.smt2 |   14.595s | 1024.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_414.smt2 |   14.763s | 1024.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/Rump_double.c_0.smt2 |   15.063s | 2627.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_0.smt2 |   15.107s | 1135.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0530a_true-unreach-call.c_1.smt2 |   17.155s | 818.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_2.smt2 |   17.299s | 700.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_7_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.025s | 85.72MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_4_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.026s | 87.016MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_true-unreach-call.c_11.smt2 |   20.028s | 177.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_3_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.029s | 86.248MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_6_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.029s | 85.744MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_6_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.031s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_5_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.032s | 87.544MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_4_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.033s | 109.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/filter2.c_0.smt2 |   20.034s | 283.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/filter2_reinit.c_0.smt2 |   20.035s | 288.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_1_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.036s | 205.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_8_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.037s | 84.724MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Newton_pseudoconstant_true-unreach-call.c_188.smt2 |   20.037s | 297.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_2_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.038s | 205.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_8_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.039s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_5_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.039s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_while_true-unreach-call.i_1.smt2 |   20.040s | 251.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_2_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.041s | 85.232MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_7_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.044s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_1_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.048s | 397.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_3_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.050s | 211.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Householder_constant_true-unreach-call.c_4.smt2 |   20.053s | 476.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_5_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.054s | 391.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_8.smt2 |   20.054s | 275.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_0.smt2 |   20.056s | 355.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_4_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.057s | 397.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_set_true-unreach-call.c_11.smt2 |   20.058s | 362.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/arctan_Pade_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.059s | 408.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0883_true-unreach-call.c_1.smt2 |   20.062s | 385.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_0.smt2 |   20.066s | 570.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_2_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.066s | 402.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_0.smt2 |   20.066s | 622.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_3_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.069s | 400.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Householder_constant_true-unreach-call.c_274.smt2 |   20.074s | 524.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490a.c_2.smt2 |   20.075s | 776.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter2_set_true-unreach-call_true-termination.c_0.smt2 |   20.076s | 378.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490b.c_2.smt2 |   20.078s | 776.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/sqrt_Householder_constant.c.p+cfa-reducer.c_2.smt2 |   20.083s | 633.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_1.smt2 |   20.084s | 709.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Householder_interval_true-unreach-call.c_4.smt2 |   20.084s | 476.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490b_true-unreach-call.c_1.smt2 |   20.086s | 706.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0250a.c_4.smt2 |   20.086s | 779.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_biNewton_pseudoconstant_true-unreach-call.c_0.smt2 |   20.088s | 631.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_biNewton_pseudoconstant_true-unreach-call.c_4.smt2 |   20.088s | 470.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_4.smt2 |   20.093s | 755.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/water_pid_true-unreach-call.c_372.smt2 |   20.095s | 1077.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/cos_polynomial_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.096s | 647.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0960b_true-unreach-call.c_1.smt2 |   20.102s | 903.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_4.smt2 |   20.111s | 1110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0970a_true-unreach-call.c_0.smt2 |   20.118s | 894.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0970b_true-unreach-call.c_1.smt2 |   20.120s | 903.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0920b_true-unreach-call.c_0.smt2 |   20.121s | 904.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_179.smt2 |   20.122s | 990.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0931_true-unreach-call.c_0.smt2 |   20.127s | 911.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_1.smt2 |   20.150s | 1110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_2.smt2 |   20.156s | 1110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/2019-Gudemann/refPred1.smt2       |   20.182s | 1949.0MiB| timeout | 0 |  |
