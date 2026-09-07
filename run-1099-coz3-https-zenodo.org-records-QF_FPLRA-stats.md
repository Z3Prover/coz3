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
Experiment date and time: 2026-09-07 00:15:58 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_FPLRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1
Z3 commit message: Add witness_instantiation simplifier and fix TPTP preprocessing pipeline fixpoint (#10679)

## Summary

Adds a new opt-in TPTP preprocessing pass, ptp.witness_instantiation,
that synthesizes a fresh witness constant for any uninterpreted sort
with no ground term anywhere in the problem, and uses it to instantiate
orall-quantified formulas over that sort. This is sound (all sorts are
non-empty in classical logic) and lets search-time E-matching proceed on
axioms that would otherwise never fire for lack of a ground term to
match against (e.g. ANA068^1.p's FINITE_REAL_INTERVAL_1 axiom,
quantified over eal, with no ground eal term anywhere in the problem).

Also fixes a real bug in the TPTP preprocessing pipeline: it was only
flushed once before check_sat, so simplifiers that add new formulas
(lambda_reify_simplifier, leibniz_simplifier,
witness_instantiation_simplifier) never got a second pass to fully
simplify/rewrite those newly-added formulas. Adds an extra push()/pop(1)
flush right before check_sat, gated on any of these three passes being
enabled, guarded by a ry/catch so a latent lambda_reify_simplifier
sort-mismatch bug (which check_sat's own exception handling already
downgrades gracefully to GaveUp) can't escape as a hard crash.

This fixes ANA068^1.p and 3 of its 4 sibling problems with
ptp.reify_lambda_literals=true (previously all timed out).

## Testing

- Full 5,279-file TPTP higher-order regression confirms both
eify_lambda_literals and witness_instantiation have an inherent
net-negative trade-off at full corpus scale (gains on some problems,
timeouts on many others due to extra instantiation noise), consistent
with eify_lambda_literals's pre-existing regression profile
(historically measured at 53 gained / 591 lost even without this fix).
Both remain opt-in, default off; default behavior is unaffected.
- 99/99 unit tests pass (
inja test-z3 + ./test-z3.exe /a).
- Merged latest origin/master into this branch; rebuilt cleanly and
re-verified ANA068^1.p still solves.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_FPLRA/schanda/spark/zeros_consistent_1.smt2 |    0.025s | 19.672MiB| unsat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_1.smt2 |    0.049s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_0.smt2 |    1.920s | 158.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_4.smt2 |    2.540s | 203.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_3.smt2 |    2.584s | 180.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_6.smt2 |    4.319s | 182.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_3.smt2 |    5.055s | 416.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_2.smt2 |    5.059s | 398.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_1.smt2 |    5.093s | 398.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0250b_true-unreach-call.c_4.smt2 |    5.135s | 310.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_1.smt2 |    5.608s | 417.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_9.smt2 |    5.727s | 437.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_2.smt2 |    6.395s | 497.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/cos_polynomial_true-unreach-call_true-termination.c_0.smt2 |    7.071s | 525.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_5.smt2 |    7.083s | 526.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/cos_polynomial.c_0.smt2 |    7.325s | 533.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_0.smt2 |    8.596s | 410.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0520_true-unreach-call.c_0.smt2 |    8.658s | 211.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_12.smt2 |    9.431s | 410.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_2.smt2 |   10.082s | 495.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_3.smt2 |   10.291s | 762.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_10.smt2 |   10.369s | 463.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_9.smt2 |   10.841s | 408.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0874_true-unreach-call.c_3.smt2 |   11.231s | 236.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0870b.c_1.smt2 |   11.796s | 235.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Rump_double_true-unreach-call_true-termination.c_0.smt2 |   15.697s | 2635.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_4.smt2 |   16.866s | 1150.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0240a_true-unreach-call.c_6.smt2 |   17.482s | 410.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/cos_polynomial_true-unreach-call.c_9.smt2 |   18.696s | 476.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/filter2_reinit_true-unreach-call.c_7.smt2 |   19.696s | 184.0MiB| sat | 0 |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/image_filter_true-unreach-call.c_2.smt2 |   20.028s | 195.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0870b_true-unreach-call.c_3.smt2 |   20.054s | 211.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_6.smt2 |   20.071s | 534.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0873a_true-unreach-call.c_3.smt2 |   20.080s | 399.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_7.smt2 |   20.101s | 411.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_11.smt2 |   20.106s | 452.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_1.smt2 |   20.113s | 710.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330b_true-unreach-call.c_10.smt2 |   20.117s | 490.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_19.smt2 |   20.158s | 787.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_2.smt2 |   20.162s | 777.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_21.smt2 |   20.171s | 1110.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn3.smt2       |   20.189s | 1945.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490a.c_4.smt2 |   20.195s | 1697.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_3.smt2 |   20.196s | 1101.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/lnLaw.smt2            |   20.198s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0250a.c_1.smt2 |   20.199s | 1144.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_3.smt2 |   20.208s | 1688.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expLaw.smt2           |   20.221s | 1948.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp0.smt2       |   20.221s | 1940.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp1.smt2       |   20.222s | 1947.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn0.smt2       |   20.229s | 1940.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp2.smt2       |   20.234s | 1948.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit.smt2        |   20.235s | 1946.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp3.smt2       |   20.241s | 1955.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_3.smt2 |   20.242s | 1759.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn1.smt2       |   20.251s | 1944.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn2.smt2       |   20.254s | 1951.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit2.smt2       |   20.318s | 3701.0MiB| timeout | 0 |  |
|non-incremental/QF_FPLRA/2019-Gudemann/maxReward.smt2        |   20.390s | 3691.0MiB| timeout | 0 |  |
