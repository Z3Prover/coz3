# .

* SAT 27
* UNSAT 2
* TIMEOUT 30
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-28 00:44:01 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1 | Source list: benchmarks-smoke.txt
Job tag: coz3-https-zenodo.org-records-QF_FPLRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 148e396f0d6b5d8c1e98429f53653ba5f25ad35b
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1
Z3 commit message: Restore array extensionality after value-root disequality shortcut (#10936)

Quantified array queries regressed after `is_diseq` began treating
distinct value roots as disequal. `already_diseq` then skipped
extensionality witnesses needed for E-matching and MBQI progress.

- **Scoped disequality check**
  - Keep the value-root shortcut as the default `is_diseq` behavior.
- Add `is_diseq_no_value_check` for callers that require
congruence/atom-based disequality only.

- **Array extensionality**
- Use the no-value-check variant exclusively in
`theory_array_base::already_diseq`.
- Prevent distinct select-value roots from suppressing an extensionality
witness.

```cpp
if (ctx.is_diseq_no_value_check(parent, other)) {
    return true;
}
```

- **Regression coverage**
- Add an SMT-context test asserting that a disequal array pair with
selects rooted at distinct values still emits an array extensionality
axiom.

<!-- START COPILOT CODING AGENT SUFFIX -->

- Fixes #10934

---------

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: NikolajBjorner <3085284+NikolajBjorner@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_1.smt2 |    0.035s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_FPLRA/schanda/spark/zeros_consistent_1.smt2 |    0.046s | 19.904MiB| unsat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_0.smt2 |    1.976s | 158.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_4.smt2 |    2.569s | 203.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_3.smt2 |    2.834s | 180.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_6.smt2 |    4.309s | 182.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_1.smt2 |    5.082s | 398.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_3.smt2 |    5.261s | 416.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_2.smt2 |    5.338s | 398.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0250b_true-unreach-call.c_4.smt2 |    5.464s | 306.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_1.smt2 |    5.538s | 417.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_9.smt2 |    5.913s | 434.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_2.smt2 |    6.700s | 494.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/cos_polynomial_true-unreach-call_true-termination.c_0.smt2 |    7.257s | 525.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/cos_polynomial.c_0.smt2 |    7.283s | 537.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_5.smt2 |    7.309s | 526.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0520_true-unreach-call.c_0.smt2 |    8.703s | 211.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_0.smt2 |    8.708s | 408.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_3.smt2 |    9.302s | 762.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_12.smt2 |    9.702s | 410.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_2.smt2 |   10.156s | 494.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_10.smt2 |   10.210s | 463.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_9.smt2 |   10.897s | 408.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0874_true-unreach-call.c_3.smt2 |   11.256s | 236.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0870b.c_1.smt2 |   12.345s | 235.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_4.smt2 |   15.410s | 1152.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Rump_double_true-unreach-call_true-termination.c_0.smt2 |   15.729s | 2634.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0240a_true-unreach-call.c_6.smt2 |   17.620s | 407.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/cos_polynomial_true-unreach-call.c_9.smt2 |   18.668s | 477.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0870b_true-unreach-call.c_3.smt2 |   20.059s | 211.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/filter2_reinit_true-unreach-call.c_7.smt2 |   20.069s | 183.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/image_filter_true-unreach-call.c_2.smt2 |   20.076s | 197.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_11.smt2 |   20.091s | 452.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_6.smt2 |   20.114s | 534.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_7.smt2 |   20.118s | 411.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0873a_true-unreach-call.c_3.smt2 |   20.120s | 399.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330b_true-unreach-call.c_10.smt2 |   20.130s | 490.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_1.smt2 |   20.161s | 710.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_19.smt2 |   20.171s | 787.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_2.smt2 |   20.172s | 775.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_21.smt2 |   20.183s | 1110.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0250a.c_1.smt2 |   20.212s | 1145.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn0.smt2       |   20.236s | 1940.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_3.smt2 |   20.243s | 1102.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn3.smt2       |   20.244s | 1945.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp2.smt2       |   20.250s | 1948.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp3.smt2       |   20.254s | 1955.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_3.smt2 |   20.257s | 1689.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit.smt2        |   20.257s | 1945.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp0.smt2       |   20.281s | 1940.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn1.smt2       |   20.288s | 1945.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expLaw.smt2           |   20.288s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_3.smt2 |   20.290s | 1756.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn2.smt2       |   20.294s | 1951.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp1.smt2       |   20.297s | 1947.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490a.c_4.smt2 |   20.301s | 1696.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/lnLaw.smt2            |   20.303s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/maxReward.smt2        |   20.405s | 3690.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit2.smt2       |   20.409s | 3701.0MiB| timeout | 0 |  |
