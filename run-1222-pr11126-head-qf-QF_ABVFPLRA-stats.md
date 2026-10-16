# .

* SAT 59
* UNSAT 2
* TIMEOUT 13
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-16 00:23:13 UTC
Job description: PR11126 candidate QF_ABVFPLRA: paired comparison against baseline run 38481334983; same pinned master plus PR commits
Job tag: pr11126-head-qf-QF_ABVFPLRA
Runner: rise-runner-2
Z3 repo: levnach/z3
Z3 commit: bb027e1104bc379deaaf084e6f878ef4a213bf4a
Z3 branch: bb027e1104bc379deaaf084e6f878ef4a213bf4a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ABVFPLRA.tar.zst?download=1
Z3 commit message: perf: apply Z3 PR 11126 onto pinned master

Benchmark-only candidate based on master f05a5c6518028169dee82d8bb3108e71a80f3b5a. Applies PR commits 483ca99b8742930b4147b7445529c898ca159663, 611545acc6f4fd9c49fcb4b53e734f5a0cdada0e, and 129ad1e04ef520973327de75d205e6c81cc0490c without modifying the PR branch.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 6ba1bb05-6b4e-4cd9-866f-5b956b261736
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0874_true-unreach-call.c_4.smt2 |    0.021s | 19.688MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_45.smt2 |    0.021s | 19.868MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_2.smt2 |    0.022s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0685a_true-unreach-call.c_9.smt2 |    0.022s | 19.828MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_3.smt2 |    0.022s | 19.836MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_186.smt2 |    0.022s | 19.844MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_320.smt2 |    0.022s | 19.844MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_2.smt2 |    0.023s | 19.848MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_3.smt2 |    0.023s | 20.776MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_5.smt2 |    0.023s | 20.004MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0876_true-unreach-call.c_5.smt2 |    0.023s | 19.852MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_182.smt2 |    0.023s | 20.12MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_alt_true-unreach-call.c_39.smt2 |    0.023s | 19.988MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_332.smt2 |    0.023s | 20.336MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_0.smt2 |    0.023s | 19.872MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0660b_true-unreach-call.c_1.smt2 |    0.024s | 20.072MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_5.smt2 |    0.024s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0680a_true-unreach-call.c_1.smt2 |    0.024s | 20.068MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_324.smt2 |    0.024s | 20.084MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_73.smt2 |    0.024s | 20.836MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter_iir_true-unreach-call.c_34.smt2 |    0.025s | 19.64MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_72.smt2 |    0.025s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_alt_true-unreach-call.c_8.smt2 |    0.025s | 20.116MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_336.smt2 |    0.025s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_2.smt2 |    0.026s | 20.628MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_334.smt2 |    0.026s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_0.smt2 |    0.027s | 20.348MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_1.smt2 |    0.028s | 21.096MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_335.smt2 |    0.028s | 20.116MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/sin_interpolated_bigrange_loose_true-unreach-call.c_0.smt2 |    0.029s | 20.336MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_7.smt2 |    0.031s | 20.86MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/sin_interpolated_index_false-unreach-call.c_0.smt2 |    0.031s | 20.152MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_1.smt2 |    0.032s | 19.84MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0883_true-unreach-call.c_2.smt2 |    0.033s | 19.844MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_185.smt2 |    0.033s | 19.832MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_1.smt2 |    0.035s | 20.444MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_poly2_false-unreach-call.c_0.smt2 |    0.036s | 21.076MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0883_true-unreach-call.c_1.smt2 |    0.037s | 20.08MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_10.smt2 |    0.037s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0873a_true-unreach-call.c_2.smt2 |    0.038s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_8.smt2 |    0.038s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0870b_true-unreach-call.c_4.smt2 |    0.039s | 19.84MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_10.smt2 |    0.039s | 20.104MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_6.smt2 |    0.040s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_56.smt2 |    0.040s | 19.84MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_8.smt2 |    0.043s | 19.848MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_323.smt2 |    0.044s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter_iir_true-unreach-call.c_27.smt2 |    0.046s | 19.596MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_184.smt2 |    0.047s | 19.864MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0240b_true-unreach-call.c_0.smt2 |    0.049s | 25.08MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0662a_true-unreach-call.c_1.smt2 |    0.049s | 19.832MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_9.smt2 |    0.050s | 20.568MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0220b_true-unreach-call.c_0.smt2 |    0.065s | 25.044MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0620b_true-unreach-call.c_7.smt2 |    0.074s | 23.172MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c.p+cfa-reducer.c_AllErrorsAtOnce_Iteration4_TraceCheck_0.smt2 |    4.496s | 89.176MiB| unsat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation2_true-unreach-call.c.v+cfa-reducer.c_0.smt2 |    5.814s | 85.976MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |    6.021s | 90.956MiB| unsat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation_true-unreach-call.c.p+cfa-reducer.c_1.smt2 |    8.261s | 82.048MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation_true-unreach-call.c.p+cfa-reducer.c_0.smt2 |    8.495s | 82.212MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation2_true-unreach-call_true-termination.c_0.smt2 |   12.438s | 85.764MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_index_false-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   17.033s | 142.0MiB| sat | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_index_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.033s | 141.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_322.smt2 |   20.041s | 307.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_331.smt2 |   20.043s | 308.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_70.smt2 |   20.045s | 306.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_330.smt2 |   20.046s | 310.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_321.smt2 |   20.048s | 306.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter2_alt_true-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |   20.049s | 99.132MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_327.smt2 |   20.049s | 306.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_329.smt2 |   20.050s | 308.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter_iir_true-unreach-call.c_0.smt2 |   20.053s | 356.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_71.smt2 |   20.058s | 306.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_smallrange_true-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |   20.061s | 279.0MiB| timeout | 0 |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_poly2_false-unreach-call.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.067s | 247.0MiB| timeout | 0 |  |
