# .

* SAT 58
* UNSAT 2
* TIMEOUT 14
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-14 05:19:07 UTC
Job description: PR11126 full QF baseline: pinned master f05a5c651802; paired smoke integrity gate passed | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ABVFPLRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: pr11126-master-qf-https-zenodo.org-records-QF_ABVFPLRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ABVFPLRA.tar.zst?download=1
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
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0873a_true-unreach-call.c_2.smt2 |    0.020s | 19.512MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_8.smt2 |    0.020s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter_iir_true-unreach-call.c_34.smt2 |    0.021s | 19.856MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_45.smt2 |    0.021s | 19.912MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_72.smt2 |    0.021s | 20.1MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_0.smt2 |    0.022s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_2.smt2 |    0.022s | 19.84MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_3.smt2 |    0.023s | 20.856MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0660b_true-unreach-call.c_1.smt2 |    0.023s | 19.724MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_5.smt2 |    0.023s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_324.smt2 |    0.023s | 20.324MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_335.smt2 |    0.023s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_334.smt2 |    0.023s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_6.smt2 |    0.024s | 20.424MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0876_true-unreach-call.c_5.smt2 |    0.024s | 19.592MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_336.smt2 |    0.024s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_9.smt2 |    0.025s | 20.528MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_1.smt2 |    0.025s | 20.676MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0685a_true-unreach-call.c_9.smt2 |    0.025s | 19.592MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_0.smt2 |    0.025s | 19.868MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_alt_true-unreach-call.c_8.smt2 |    0.025s | 19.836MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_2.smt2 |    0.026s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_182.smt2 |    0.026s | 20.12MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0874_true-unreach-call.c_4.smt2 |    0.029s | 19.848MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/sin_interpolated_bigrange_loose_true-unreach-call.c_0.smt2 |    0.029s | 20.1MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0883_true-unreach-call.c_2.smt2 |    0.030s | 19.832MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_1.smt2 |    0.031s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/sin_interpolated_index_false-unreach-call.c_0.smt2 |    0.031s | 20.464MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_5.smt2 |    0.033s | 21.088MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_2.smt2 |    0.035s | 20.34MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0883_true-unreach-call.c_1.smt2 |    0.036s | 20.144MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_10.smt2 |    0.036s | 20.144MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_8.smt2 |    0.037s | 20.584MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_185.smt2 |    0.037s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_7.smt2 |    0.038s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter_iir_true-unreach-call.c_27.smt2 |    0.038s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_poly2_false-unreach-call.c_0.smt2 |    0.039s | 21.12MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_323.smt2 |    0.039s | 19.884MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_56.smt2 |    0.040s | 20.088MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0870b_true-unreach-call.c_4.smt2 |    0.043s | 20.068MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0662a_true-unreach-call.c_1.smt2 |    0.043s | 19.592MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_alt_true-unreach-call.c_39.smt2 |    0.043s | 19.832MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_332.smt2 |    0.043s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_186.smt2 |    0.043s | 19.632MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_320.smt2 |    0.043s | 20.064MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_73.smt2 |    0.044s | 20.596MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0240b_true-unreach-call.c_0.smt2 |    0.046s | 24.9MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_184.smt2 |    0.046s | 19.596MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_3.smt2 |    0.049s | 19.62MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_1.smt2 |    0.050s | 20.824MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_10.smt2 |    0.050s | 20.212MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0680a_true-unreach-call.c_1.smt2 |    0.051s | 19.848MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0220b_true-unreach-call.c_0.smt2 |    0.065s | 25.064MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0620b_true-unreach-call.c_7.smt2 |    0.066s | 23.328MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c.p+cfa-reducer.c_AllErrorsAtOnce_Iteration4_TraceCheck_0.smt2 |    2.289s | 86.868MiB| unsat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation_true-unreach-call.c.p+cfa-reducer.c_0.smt2 |    5.047s | 81.288MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation2_true-unreach-call_true-termination.c_0.smt2 |    7.495s | 83.196MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation_true-unreach-call.c.p+cfa-reducer.c_1.smt2 |    7.818s | 81.44MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation2_true-unreach-call.c.v+cfa-reducer.c_0.smt2 |   10.161s | 88.24MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |   13.944s | 105.0MiB| unsat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_index_false-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.027s | 141.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_330.smt2 |   20.035s | 302.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_331.smt2 |   20.036s | 303.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter2_alt_true-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |   20.043s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter_iir_true-unreach-call.c_0.smt2 |   20.043s | 353.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_322.smt2 |   20.046s | 308.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_index_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.047s | 141.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_71.smt2 |   20.049s | 308.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_321.smt2 |   20.050s | 305.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_327.smt2 |   20.058s | 307.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_smallrange_true-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |   20.064s | 309.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_70.smt2 |   20.067s | 307.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_329.smt2 |   20.071s | 308.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_poly2_false-unreach-call.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.084s | 537.0MiB| timeout | 0 |  |
