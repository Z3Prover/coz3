# .

* SAT 26
* UNSAT 2
* TIMEOUT 31
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-13 22:52:20 UTC
Job description: PR11126 smoke candidate: all three PR commits applied to pinned master f05a5c651802 | Benchmark suite: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1 | Source list: benchmarks-smoke.txt
Job tag: pr11126-head-smoke-https-zenodo.org-records-QF_FPLRA
Runner: rise-runner-2
Z3 repo: levnach/z3
Z3 commit: bb027e1104bc379deaaf084e6f878ef4a213bf4a
Z3 branch: bb027e1104bc379deaaf084e6f878ef4a213bf4a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1
Z3 commit message: perf: apply Z3 PR 11126 onto pinned master

Benchmark-only candidate based on master f05a5c6518028169dee82d8bb3108e71a80f3b5a. Applies PR commits 483ca99b8742930b4147b7445529c898ca159663, 611545acc6f4fd9c49fcb4b53e734f5a0cdada0e, and 129ad1e04ef520973327de75d205e6c81cc0490c without modifying the PR branch.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 6ba1bb05-6b4e-4cd9-866f-5b956b261736
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_FPLRA/schanda/spark/zeros_consistent_1.smt2 |    0.025s | 19.82MiB| unsat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_1.smt2 |    0.049s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_3.smt2 |    1.093s | 298.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_1.smt2 |    1.121s | 297.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_2.smt2 |    1.153s | 297.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_1.smt2 |    1.198s | 308.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_4.smt2 |    1.595s | 201.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_0.smt2 |    1.641s | 157.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0240a_true-unreach-call.c_6.smt2 |    3.672s | 381.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_12.smt2 |    4.566s | 409.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_5.smt2 |    5.449s | 511.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0250b_true-unreach-call.c_4.smt2 |    5.450s | 298.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0870b.c_1.smt2 |    5.486s | 229.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_11.smt2 |    5.853s | 439.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_9.smt2 |    5.941s | 439.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_9.smt2 |    6.463s | 410.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_3.smt2 |    7.500s | 182.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0873a_true-unreach-call.c_3.smt2 |    8.002s | 359.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_1.smt2 |    8.415s | 762.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0870b_true-unreach-call.c_3.smt2 |    8.820s | 229.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_3.smt2 |    8.847s | 792.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_2.smt2 |    9.299s | 497.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_6.smt2 |    9.622s | 197.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0874_true-unreach-call.c_3.smt2 |   11.862s | 244.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_7.smt2 |   14.298s | 400.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Rump_double_true-unreach-call_true-termination.c_0.smt2 |   15.329s | 2760.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/filter2_reinit_true-unreach-call.c_7.smt2 |   17.676s | 178.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_10.smt2 |   18.477s | 482.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/image_filter_true-unreach-call.c_2.smt2 |   20.035s | 197.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0520_true-unreach-call.c_0.smt2 |   20.040s | 201.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_0.smt2 |   20.064s | 419.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/cos_polynomial_true-unreach-call.c_9.smt2 |   20.065s | 481.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/cos_polynomial.c_0.smt2 |   20.071s | 540.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_6.smt2 |   20.073s | 532.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330b_true-unreach-call.c_10.smt2 |   20.077s | 537.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_2.smt2 |   20.079s | 502.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/cos_polynomial_true-unreach-call_true-termination.c_0.smt2 |   20.083s | 495.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_2.smt2 |   20.083s | 775.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_19.smt2 |   20.102s | 745.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0250a.c_1.smt2 |   20.110s | 1103.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_4.smt2 |   20.121s | 1103.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_3.smt2 |   20.131s | 1105.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_21.smt2 |   20.137s | 1104.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expLaw.smt2           |   20.149s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp3.smt2       |   20.159s | 1971.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp2.smt2       |   20.159s | 1931.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn3.smt2       |   20.161s | 1941.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn2.smt2       |   20.166s | 1943.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490a.c_4.smt2 |   20.166s | 1688.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp0.smt2       |   20.169s | 1954.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn1.smt2       |   20.169s | 1941.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn0.smt2       |   20.169s | 1941.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp1.smt2       |   20.171s | 1971.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_3.smt2 |   20.172s | 1690.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/lnLaw.smt2            |   20.177s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_3.smt2 |   20.182s | 1700.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit.smt2        |   20.211s | 2264.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit2.smt2       |   20.239s | 3702.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/maxReward.smt2        |   20.270s | 3691.0MiB| timeout | 0 |  |
