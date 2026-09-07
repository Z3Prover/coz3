# .

* SAT 80
* UNSAT 32
* TIMEOUT 65
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-07 00:01:44 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_BVFPLRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_BVFPLRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_BVFPLRA.tar.zst?download=1
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
|non-incremental/QF_BVFPLRA/schanda/spark/zeros_consistent_3.smt2 |    0.025s | 19.628MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_while_true-unreach-call.i_AllErrorsAtOnce_Iteration3_TraceCheck_0.smt2 |    0.039s | 20.1MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Rump_double_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    0.041s | 19.884MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_2.smt2 |    0.044s | 19.612MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |    0.045s | 19.856MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_6.smt2 |    0.046s | 19.956MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_biNewton_pseudoconstant_true-unreach-call.c_2.smt2 |    0.049s | 29.892MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_0.smt2 |    0.052s | 20.46MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_5.smt2 |    0.052s | 20.06MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_4.smt2 |    0.058s | 19.596MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_while_false-unreach-call.i_AllErrorsAtOnce_Iteration3_TraceCheck_0.smt2 |    0.075s | 19.916MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_1.smt2 |    0.077s | 19.596MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_7.smt2 |    0.080s | 20.044MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |    0.081s | 19.596MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    0.081s | 19.84MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_5.smt2 |    0.230s | 31.904MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_3.smt2 |    0.234s | 31.656MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_0.smt2 |    0.249s | 31.892MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_true-unreach-call.i_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |    0.363s | 44.78MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float21_true-unreach-call.i_1.smt2 |    0.470s | 33.448MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_4.smt2 |    0.520s | 37.052MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float21_true-unreach-call.i_0.smt2 |    0.528s | 33.448MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_3.smt2 |    0.843s | 110.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_5.smt2 |    0.859s | 110.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_2.smt2 |    0.862s | 60.692MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_for_false-unreach-call.i_4.smt2 |    0.898s | 37.404MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_for_false-unreach-call.i_3.smt2 |    0.924s | 37.664MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_4.smt2 |    0.960s | 104.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_10.smt2 |    0.976s | 73.1MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c.p+cfa-reducer.c_1.smt2 |    1.050s | 119.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_2.smt2 |    1.080s | 110.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0240a_true-unreach-call.c_0.smt2 |    1.150s | 145.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_true-unreach-call.i_1.smt2 |    1.153s | 59.836MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_9.smt2 |    1.193s | 73.532MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_8.smt2 |    1.329s | 73.6MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_while_false-unreach-call.i_3.smt2 |    1.442s | 53.104MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0920b_true-unreach-call.c_3.smt2 |    1.444s | 84.9MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_for_true-unreach-call.i_4.smt2 |    1.503s | 38.232MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_while_true-unreach-call.i_2.smt2 |    1.534s | 53.144MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Float_div_bad_false-unreach-call.i_3.smt2 |    1.773s | 72.952MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter1_true-unreach-call.c.v+lhb-reducer.c_3.smt2 |    1.800s | 120.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_for_true-unreach-call.i_1.smt2 |    1.844s | 68.96MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_biNewton_pseudoconstant_true-unreach-call.c_4.smt2 |    1.867s | 300.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0661a_true-unreach-call.c_0.smt2 |    1.869s | 310.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_while_false-unreach-call.i_0.smt2 |    1.936s | 68.612MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_5_true-unreach-call_true-termination.i_0.smt2 |    2.134s | 128.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_2.smt2 |    2.296s | 311.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0684a_true-unreach-call.c_1.smt2 |    2.384s | 309.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0971_true-unreach-call.c_1.smt2 |    2.425s | 166.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter1_true-unreach-call.c.p+cfa-reducer.c_0.smt2 |    2.442s | 120.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float-div1_true-unreach-call.i_0.smt2 |    2.488s | 111.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c_0.smt2 |    2.490s | 150.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float-div1_true-unreach-call.i_1.smt2 |    2.544s | 111.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    2.685s | 28.816MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_0.smt2 |    2.702s | 36.632MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_2.smt2 |    2.704s | 311.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_1.smt2 |    2.745s | 137.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_4.smt2 |    2.780s | 36.576MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_9.smt2 |    2.850s | 37.188MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_2.smt2 |    2.877s | 164.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_bad_for_false-unreach-call.i_0.smt2 |    2.879s | 85.896MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_8.smt2 |    3.212s | 202.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_11.smt2 |    3.224s | 37.552MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_2.smt2 |    3.238s | 36.896MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_1_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    3.522s | 110.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_5.smt2 |    3.555s | 111.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_8.smt2 |    3.615s | 169.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/trunc_nondet_2_true-unreach-call.i_7.smt2 |    3.762s | 37.192MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0930_true-unreach-call.c_0.smt2 |    4.289s | 228.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c.p+cfa-reducer.c_0.smt2 |    4.291s | 151.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_8_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    5.018s | 200.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0960a_true-unreach-call.c_0.smt2 |    6.117s | 223.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_0.smt2 |    6.392s | 193.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_0.smt2 |    6.426s | 193.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter2_set_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    6.833s | 112.0MiB| unsat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_8_true-unreach-call_true-termination.i_0.smt2 |    7.000s | 426.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_7_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    7.017s | 208.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_2_false-unreach-call_true-termination.i_0.smt2 |    7.286s | 426.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_biNewton_pseudoconstant_true-unreach-call.c_1.smt2 |    7.309s | 423.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0686b_true-unreach-call.c_0.smt2 |    7.445s | 446.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_4_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    8.563s | 208.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_1.smt2 |    8.696s | 492.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_4.smt2 |    9.012s | 370.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0920b_true-unreach-call.c_1.smt2 |    9.514s | 234.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_6_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    9.598s | 208.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0930_true-unreach-call.c_1.smt2 |    9.823s | 230.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_1_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |    9.952s | 83.236MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_2.smt2 |    9.960s | 215.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0684a_true-unreach-call.c_0.smt2 |   10.456s | 601.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_2_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   10.520s | 107.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_3_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   11.084s | 108.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_7_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   12.208s | 389.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_3.smt2 |   12.985s | 729.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_2.smt2 |   13.090s | 513.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_8_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   13.135s | 395.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0832_true-unreach-call.c_3.smt2 |   13.299s | 729.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0520_true-unreach-call.c_6.smt2 |   13.401s | 799.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0530a_true-unreach-call.c_7.smt2 |   13.436s | 797.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_3.smt2 |   13.584s | 512.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_17.smt2 |   14.163s | 711.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Newton_pseudoconstant_true-unreach-call.c_4.smt2 |   14.472s | 286.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0931_true-unreach-call.c_0.smt2 |   14.555s | 235.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_422.smt2 |   15.096s | 1024.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_446.smt2 |   15.145s | 1024.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_315.smt2 |   15.260s | 1024.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0920b_true-unreach-call.c_0.smt2 |   16.299s | 240.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_3.smt2 |   16.524s | 1142.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_8.smt2 |   16.557s | 797.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0882_true-unreach-call.c_1.smt2 |   17.099s | 375.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0530a_true-unreach-call.c_3.smt2 |   17.409s | 795.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/Rump_double.c_0.smt2 |   19.384s | 2630.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_poly_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.035s | 108.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/filter2.c_0.smt2 |   20.038s | 284.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_8_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.040s | 84.832MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_5_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.042s | 84.628MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_2_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.042s | 85.328MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/filter2_reinit.c_0.smt2 |   20.042s | 284.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_414.smt2 |   20.045s | 1024.0MiB| sat | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_7_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.050s | 108.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_3_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.050s | 86.22MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_true-unreach-call.c_11.smt2 |   20.058s | 176.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_6_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.060s | 109.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_4_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.061s | 109.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_3_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.063s | 210.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Newton_pseudoconstant_true-unreach-call.c_188.smt2 |   20.064s | 291.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_4_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.066s | 85.428MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_7_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.067s | 84.996MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_6_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.069s | 396.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/square_6_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.069s | 84.88MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_8_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.071s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sine_5_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.073s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/cos_polynomial_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.074s | 643.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_5_false-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.078s | 204.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_2_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.078s | 399.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_2_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.079s | 203.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_0.smt2 |   20.080s | 322.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_1_1_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.081s | 203.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0883_true-unreach-call.c_1.smt2 |   20.084s | 378.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_8.smt2 |   20.084s | 275.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/digits_while_true-unreach-call.i_1.smt2 |   20.087s | 248.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_1_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.087s | 396.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_3_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.095s | 396.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490b_true-unreach-call.c_1.smt2 |   20.096s | 705.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/arctan_Pade_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.098s | 407.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_4_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.098s | 395.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Householder_constant_true-unreach-call.c_4.smt2 |   20.098s | 476.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490b.c_2.smt2 |   20.103s | 711.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_biNewton_pseudoconstant_true-unreach-call.c_4.smt2 |   20.104s | 469.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0970b_true-unreach-call.c_1.smt2 |   20.105s | 902.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_2.smt2 |   20.110s | 684.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/newton_2_5_true-unreach-call_true-termination.i_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.111s | 390.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_4.smt2 |   20.111s | 755.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Householder_constant_true-unreach-call.c_274.smt2 |   20.112s | 479.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_0.smt2 |   20.113s | 567.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter2_set_true-unreach-call_true-termination.c_0.smt2 |   20.116s | 379.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_set_true-unreach-call.c_11.smt2 |   20.122s | 363.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_interval_true-unreach-call.c_0.smt2 |   20.123s | 570.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/sqrt_Householder_interval_true-unreach-call.c_4.smt2 |   20.126s | 475.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_biNewton_pseudoconstant_true-unreach-call.c_0.smt2 |   20.128s | 632.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490a.c_2.smt2 |   20.128s | 711.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/sqrt_Householder_constant.c.p+cfa-reducer.c_2.smt2 |   20.131s | 624.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0920b_true-unreach-call.c_0.smt2 |   20.132s | 903.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_1.smt2 |   20.136s | 704.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0970a_true-unreach-call.c_0.smt2 |   20.142s | 893.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0250a.c_4.smt2 |   20.142s | 707.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0931_true-unreach-call.c_0.smt2 |   20.145s | 911.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0960b_true-unreach-call.c_1.smt2 |   20.148s | 903.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_430.smt2 |   20.151s | 1024.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_68.smt2 |   20.155s | 1024.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0530a_true-unreach-call.c_1.smt2 |   20.158s | 755.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_1.smt2 |   20.169s | 1110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_4.smt2 |   20.170s | 1110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_2.smt2 |   20.172s | 1110.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/Muller_Kahan_true-unreach-call.c_179.smt2 |   20.172s | 989.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20190429-UltimateAutomizerSvcomp2019/Muller_Kahan_true-unreach-call_true-termination.c_0.smt2 |   20.176s | 1117.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/20170501-Heizmann-UltimateAutomizer/water_pid_true-unreach-call.c_372.smt2 |   20.189s | 1077.0MiB| timeout | 0 |  |
|non-incremental/QF_BVFPLRA/2019-Gudemann/refPred1.smt2       |   20.225s | 1948.0MiB| timeout | 0 |  |
