# .

* SAT 28
* UNSAT 2
* TIMEOUT 29
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-13 22:28:42 UTC
Job description: PR11126 smoke baseline: pinned master f05a5c651802; job-local Ninja setup | Benchmark suite: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1 | Source list: benchmarks-smoke.txt
Job tag: pr11126-master-smoke2-https-zenodo.org-records-QF_FPLRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1
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
|non-incremental/QF_FPLRA/schanda/spark/zeros_consistent_1.smt2 |    0.023s | 20.096MiB| unsat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_1.smt2 |    0.045s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_0.smt2 |    1.638s | 158.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_4.smt2 |    1.941s | 203.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_3.smt2 |    2.261s | 180.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_6.smt2 |    3.780s | 182.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_3.smt2 |    4.522s | 415.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0250b_true-unreach-call.c_4.smt2 |    4.654s | 307.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_2.smt2 |    4.673s | 399.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_1.smt2 |    4.701s | 398.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_1.smt2 |    4.981s | 419.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_9.smt2 |    5.198s | 437.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_2.smt2 |    5.960s | 494.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_5.smt2 |    6.284s | 530.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/cos_polynomial_true-unreach-call_true-termination.c_0.smt2 |    6.387s | 525.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/cos_polynomial.c_0.smt2 |    6.489s | 533.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_0.smt2 |    7.698s | 410.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0520_true-unreach-call.c_0.smt2 |    7.843s | 211.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_3.smt2 |    8.412s | 762.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_12.smt2 |    8.463s | 410.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_2.smt2 |    9.239s | 495.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_10.smt2 |    9.401s | 463.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_9.smt2 |    9.786s | 408.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0874_true-unreach-call.c_3.smt2 |   10.538s | 236.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0870b.c_1.smt2 |   12.016s | 235.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_4.smt2 |   14.423s | 1148.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Rump_double_true-unreach-call_true-termination.c_0.smt2 |   14.734s | 2640.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0240a_true-unreach-call.c_6.smt2 |   15.776s | 410.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/cos_polynomial_true-unreach-call.c_9.smt2 |   17.980s | 476.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/filter2_reinit_true-unreach-call.c_7.smt2 |   18.494s | 182.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/image_filter_true-unreach-call.c_2.smt2 |   20.028s | 195.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0873a_true-unreach-call.c_3.smt2 |   20.045s | 399.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0870b_true-unreach-call.c_3.smt2 |   20.050s | 235.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330b_true-unreach-call.c_10.smt2 |   20.063s | 490.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_7.smt2 |   20.067s | 411.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_6.smt2 |   20.068s | 534.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_11.smt2 |   20.072s | 452.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_2.smt2 |   20.074s | 776.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_1.smt2 |   20.092s | 710.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_19.smt2 |   20.092s | 808.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_21.smt2 |   20.122s | 1111.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn3.smt2       |   20.149s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0250a.c_1.smt2 |   20.149s | 1144.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn1.smt2       |   20.151s | 1945.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp1.smt2       |   20.151s | 1947.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp2.smt2       |   20.151s | 1948.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp3.smt2       |   20.152s | 1956.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp0.smt2       |   20.155s | 1940.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490a.c_4.smt2 |   20.155s | 1697.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_3.smt2 |   20.157s | 1099.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_3.smt2 |   20.160s | 1688.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn0.smt2       |   20.163s | 1940.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/lnLaw.smt2            |   20.168s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn2.smt2       |   20.172s | 1951.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expLaw.smt2           |   20.180s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_3.smt2 |   20.182s | 1757.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit.smt2        |   20.266s | 2443.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit2.smt2       |   20.273s | 3701.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/maxReward.smt2        |   20.313s | 3690.0MiB| timeout | 0 |  |
