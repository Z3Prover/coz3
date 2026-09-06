# .

* SAT 272
* UNSAT 279
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-06 23:27:35 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_AX.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_AX
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_AX.tar.zst?download=1
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
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_003.cvc.smt2 |    0.020s | 19.516MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00005_009.cvc.smt2 |    0.022s | 20.104MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00003_005.cvc.smt2 |    0.023s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_001.cvc.smt2 |    0.023s | 19.9MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00004_002.cvc.smt2 |    0.023s | 20.392MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00002_006.cvc.smt2 |    0.023s | 20.056MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_nf_ai_00001_001.cvc.smt2 |    0.023s | 19.804MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_002.cvc.smt2 |    0.024s | 20.144MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00009_005.cvc.smt2 |    0.024s | 20.424MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00020_008.cvc.smt2 |    0.024s | 20.212MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00010_009.cvc.smt2 |    0.024s | 20.14MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00020_005.cvc.smt2 |    0.025s | 20.604MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00010_003.cvc.smt2 |    0.026s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00003_002.cvc.smt2 |    0.026s | 20.056MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00010_001.cvc.smt2 |    0.026s | 20.916MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00030_001.cvc.smt2 |    0.027s | 20.624MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_007.cvc.smt2 |    0.027s | 20.204MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00008_005.cvc.smt2 |    0.028s | 20.08MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_009.cvc.smt2 |    0.028s | 20.32MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00030_006.cvc.smt2 |    0.028s | 20.756MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00020_006.cvc.smt2 |    0.028s | 20.264MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00020_001.cvc.smt2 |    0.028s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00020_002.cvc.smt2 |    0.029s | 20.564MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_008.cvc.smt2 |    0.029s | 20.18MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00030_002.cvc.smt2 |    0.030s | 20.676MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00020_003.cvc.smt2 |    0.030s | 20.668MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_008.cvc.smt2 |    0.031s | 20.08MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00030_003.cvc.smt2 |    0.031s | 20.74MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_007.cvc.smt2 |    0.032s | 19.916MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00009_006.cvc.smt2 |    0.032s | 20.588MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00007_007.cvc.smt2 |    0.032s | 20.224MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_009.cvc.smt2 |    0.032s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00040_003.cvc.smt2 |    0.032s | 20.888MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00009_001.cvc.smt2 |    0.032s | 20.412MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00040_006.cvc.smt2 |    0.033s | 21.408MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_003.cvc.smt2 |    0.033s | 21.628MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_002.cvc.smt2 |    0.033s | 21.232MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_004.cvc.smt2 |    0.036s | 20.46MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00050_003.cvc.smt2 |    0.037s | 21.816MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00050_008.cvc.smt2 |    0.038s | 21.3MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_005.cvc.smt2 |    0.038s | 21.236MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_007.cvc.smt2 |    0.039s | 20.204MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00009_005.cvc.smt2 |    0.041s | 20.196MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00060_008.cvc.smt2 |    0.041s | 21.732MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00060_005.cvc.smt2 |    0.041s | 21.776MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00020_003.cvc.smt2 |    0.042s | 20.232MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00060_005.cvc.smt2 |    0.043s | 22.448MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00060_001.cvc.smt2 |    0.043s | 21.82MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_008.cvc.smt2 |    0.044s | 20.188MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_004.cvc.smt2 |    0.044s | 20.328MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00050_007.cvc.smt2 |    0.045s | 21.676MiB| sat | 0 |  |
|non-incremental/QF_AX/cvc/read5.smt2                         |    0.046s | 20.14MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00010_004.cvc.smt2 |    0.046s | 20.336MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00050_005.cvc.smt2 |    0.046s | 21.648MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00010_006.cvc.smt2 |    0.046s | 20.124MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_009.cvc.smt2 |    0.047s | 19.916MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_009.cvc.smt2 |    0.047s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_004.cvc.smt2 |    0.048s | 20.072MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00010_009.cvc.smt2 |    0.048s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_001.cvc.smt2 |    0.048s | 20.112MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_007.cvc.smt2 |    0.049s | 20.256MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00005_005.cvc.smt2 |    0.049s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_002.cvc.smt2 |    0.049s | 20.276MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_nf_ai_00004_001.cvc.smt2 |    0.049s | 20.112MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_006.cvc.smt2 |    0.050s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_006.cvc.smt2 |    0.050s | 20.008MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_004.cvc.smt2 |    0.050s | 20.664MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00010_003.cvc.smt2 |    0.050s | 20.404MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_sf_ai_00001_001.cvc.smt2 |    0.050s | 19.888MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00010_003.cvc.smt2 |    0.051s | 20.2MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00020_003.cvc.smt2 |    0.051s | 20.584MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_001.cvc.smt2 |    0.051s | 21.192MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00003_006.cvc.smt2 |    0.052s | 20.264MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00005_009.cvc.smt2 |    0.052s | 20.12MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_007.cvc.smt2 |    0.052s | 19.956MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00003_002.cvc.smt2 |    0.052s | 20.132MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_nf_ai_00006_001.cvc.smt2 |    0.052s | 20.296MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_003.cvc.smt2 |    0.053s | 20.34MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_nf_ai_00007_001.cvc.smt2 |    0.053s | 20.388MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_008.cvc.smt2 |    0.054s | 21.1MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00010_004.cvc.smt2 |    0.054s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_002.cvc.smt2 |    0.054s | 20.836MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_sf_ai_00003_001.cvc.smt2 |    0.054s | 20.244MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00008_009.cvc.smt2 |    0.055s | 20.544MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00007_001.cvc.smt2 |    0.055s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00002_009.cvc.smt2 |    0.055s | 20.02MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00020_005.cvc.smt2 |    0.055s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00020_004.cvc.smt2 |    0.055s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_008.cvc.smt2 |    0.056s | 19.912MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00004_006.cvc.smt2 |    0.056s | 20.248MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00002_002.cvc.smt2 |    0.056s | 20.08MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00008_001.cvc.smt2 |    0.056s | 20.424MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00020_008.cvc.smt2 |    0.056s | 20.432MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_003.cvc.smt2 |    0.056s | 21.076MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00010_004.cvc.smt2 |    0.056s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00050_005.cvc.smt2 |    0.056s | 21.128MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00008_001.cvc.smt2 |    0.056s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_004.cvc.smt2 |    0.057s | 20.064MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00005_006.cvc.smt2 |    0.057s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_006.cvc.smt2 |    0.057s | 19.96MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00007_008.cvc.smt2 |    0.057s | 20.104MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00030_003.cvc.smt2 |    0.057s | 20.704MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00030_001.cvc.smt2 |    0.057s | 20.9MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_007.cvc.smt2 |    0.057s | 21.628MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_002.cvc.smt2 |    0.057s | 21.368MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00020_003.cvc.smt2 |    0.057s | 20.652MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00020_009.cvc.smt2 |    0.057s | 20.66MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00007_001.cvc.smt2 |    0.057s | 20.112MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_nf_ai_00004_001.cvc.smt2 |    0.057s | 20.604MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00005_007.cvc.smt2 |    0.058s | 20.316MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00006_004.cvc.smt2 |    0.058s | 20.152MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_003.cvc.smt2 |    0.058s | 19.888MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_003.cvc.smt2 |    0.058s | 19.92MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_001.cvc.smt2 |    0.058s | 20.148MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00030_002.cvc.smt2 |    0.058s | 20.664MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_nf_ai_00002_001.cvc.smt2 |    0.058s | 19.988MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00005_007.cvc.smt2 |    0.059s | 20.512MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_002.cvc.smt2 |    0.059s | 20.576MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_008.cvc.smt2 |    0.059s | 20.48MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00009_003.cvc.smt2 |    0.059s | 20.668MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00002_005.cvc.smt2 |    0.059s | 20.104MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00003_004.cvc.smt2 |    0.059s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00002_006.cvc.smt2 |    0.059s | 19.912MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00030_007.cvc.smt2 |    0.059s | 20.652MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_001.cvc.smt2 |    0.059s | 20.744MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00006_001.cvc.smt2 |    0.060s | 20.296MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_006.cvc.smt2 |    0.060s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_002.cvc.smt2 |    0.060s | 20.588MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00006_002.cvc.smt2 |    0.060s | 20.16MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_004.cvc.smt2 |    0.060s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00004_007.cvc.smt2 |    0.060s | 20.06MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00002_005.cvc.smt2 |    0.060s | 19.932MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00006_002.cvc.smt2 |    0.060s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_002.cvc.smt2 |    0.060s | 20.148MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_007.cvc.smt2 |    0.060s | 20.128MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00020_007.cvc.smt2 |    0.060s | 20.844MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00030_004.cvc.smt2 |    0.060s | 20.424MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_004.cvc.smt2 |    0.060s | 21.124MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00050_009.cvc.smt2 |    0.060s | 21.356MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00020_002.cvc.smt2 |    0.060s | 20.456MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_006.cvc.smt2 |    0.060s | 20.064MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00060_002.cvc.smt2 |    0.060s | 21.728MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00040_004.cvc.smt2 |    0.060s | 21.516MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00030_007.cvc.smt2 |    0.060s | 20.84MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_003.cvc.smt2 |    0.061s | 20.48MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_002.cvc.smt2 |    0.061s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_004.cvc.smt2 |    0.061s | 20.016MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00003_006.cvc.smt2 |    0.061s | 20.004MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00010_009.cvc.smt2 |    0.061s | 20.376MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_009.cvc.smt2 |    0.061s | 20.116MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00060_006.cvc.smt2 |    0.061s | 22.412MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00020_006.cvc.smt2 |    0.061s | 20.112MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00010_005.cvc.smt2 |    0.061s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00020_007.cvc.smt2 |    0.061s | 20.168MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00010_008.cvc.smt2 |    0.061s | 20.152MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_006.cvc.smt2 |    0.061s | 20.588MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00002_001.cvc.smt2 |    0.061s | 19.884MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_sf_ai_00007_001.cvc.smt2 |    0.061s | 20.148MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_sf_ai_00004_001.cvc.smt2 |    0.061s | 20.292MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00009_007.cvc.smt2 |    0.062s | 20.272MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_001.cvc.smt2 |    0.062s | 19.596MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_007.cvc.smt2 |    0.062s | 20.44MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00003_001.cvc.smt2 |    0.062s | 20.124MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_006.cvc.smt2 |    0.062s | 20.204MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00006_003.cvc.smt2 |    0.062s | 20.168MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00003_008.cvc.smt2 |    0.062s | 19.852MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00007_009.cvc.smt2 |    0.062s | 20.596MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00005_003.cvc.smt2 |    0.062s | 20.06MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00020_005.cvc.smt2 |    0.062s | 20.628MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00020_006.cvc.smt2 |    0.062s | 20.624MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00040_003.cvc.smt2 |    0.062s | 20.924MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_003.cvc.smt2 |    0.062s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_002.cvc.smt2 |    0.062s | 21.216MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00008_003.cvc.smt2 |    0.063s | 19.596MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00002_009.cvc.smt2 |    0.063s | 20.288MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_007.cvc.smt2 |    0.063s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_002.cvc.smt2 |    0.063s | 19.908MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_006.cvc.smt2 |    0.063s | 20.2MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_006.cvc.smt2 |    0.063s | 20.668MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_003.cvc.smt2 |    0.063s | 20.04MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00003_005.cvc.smt2 |    0.063s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_009.cvc.smt2 |    0.063s | 21.124MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00020_007.cvc.smt2 |    0.063s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00020_004.cvc.smt2 |    0.063s | 20.616MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00050_006.cvc.smt2 |    0.063s | 21.988MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00010_008.cvc.smt2 |    0.063s | 20.048MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00009_001.cvc.smt2 |    0.063s | 20.592MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00008_008.cvc.smt2 |    0.064s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00010_009.cvc.smt2 |    0.064s | 20.416MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00004_009.cvc.smt2 |    0.064s | 20.1MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00002_008.cvc.smt2 |    0.064s | 20.348MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00002_003.cvc.smt2 |    0.064s | 19.864MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00009_008.cvc.smt2 |    0.064s | 20.712MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_008.cvc.smt2 |    0.064s | 20.116MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00007_009.cvc.smt2 |    0.064s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_008.cvc.smt2 |    0.064s | 20.364MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_005.cvc.smt2 |    0.064s | 21.148MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00040_002.cvc.smt2 |    0.064s | 20.848MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_008.cvc.smt2 |    0.064s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_008.cvc.smt2 |    0.064s | 21.232MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_007.cvc.smt2 |    0.064s | 21.816MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00030_008.cvc.smt2 |    0.064s | 20.9MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_sf_ai_00005_001.cvc.smt2 |    0.064s | 20.236MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00006_001.cvc.smt2 |    0.064s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00007_003.cvc.smt2 |    0.065s | 20.116MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00004_008.cvc.smt2 |    0.065s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00008_002.cvc.smt2 |    0.065s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00002_005.cvc.smt2 |    0.065s | 19.944MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_009.cvc.smt2 |    0.065s | 20.428MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_004.cvc.smt2 |    0.065s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00007_008.cvc.smt2 |    0.065s | 20.66MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_008.cvc.smt2 |    0.065s | 20.444MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00050_006.cvc.smt2 |    0.065s | 21.256MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_006.cvc.smt2 |    0.065s | 20.648MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00050_004.cvc.smt2 |    0.065s | 21.784MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_005.cvc.smt2 |    0.065s | 20.472MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00003_001.cvc.smt2 |    0.065s | 20.1MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00005_001.cvc.smt2 |    0.065s | 20.148MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00007_004.cvc.smt2 |    0.066s | 20.16MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00002_006.cvc.smt2 |    0.066s | 19.952MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_002.cvc.smt2 |    0.066s | 19.864MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00004_001.cvc.smt2 |    0.066s | 19.844MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_008.cvc.smt2 |    0.066s | 20.912MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_001.cvc.smt2 |    0.066s | 22.416MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_004.cvc.smt2 |    0.066s | 21.256MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00030_008.cvc.smt2 |    0.066s | 20.856MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00030_006.cvc.smt2 |    0.066s | 20.64MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_004.cvc.smt2 |    0.066s | 20.116MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_009.cvc.smt2 |    0.066s | 20.824MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00030_008.cvc.smt2 |    0.066s | 20.516MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00060_001.cvc.smt2 |    0.066s | 22.54MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00002_002.cvc.smt2 |    0.067s | 20.252MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_003.cvc.smt2 |    0.067s | 19.628MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00007_007.cvc.smt2 |    0.067s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_005.cvc.smt2 |    0.067s | 20.744MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00007_001.cvc.smt2 |    0.067s | 20.32MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_009.cvc.smt2 |    0.067s | 20.736MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_006.cvc.smt2 |    0.067s | 20.524MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00060_008.cvc.smt2 |    0.067s | 21.7MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00040_009.cvc.smt2 |    0.067s | 21.316MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_008.cvc.smt2 |    0.067s | 20.88MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00030_009.cvc.smt2 |    0.067s | 20.62MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00003_007.cvc.smt2 |    0.068s | 20.084MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00006_003.cvc.smt2 |    0.068s | 20.112MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00005_006.cvc.smt2 |    0.068s | 19.888MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00008_004.cvc.smt2 |    0.068s | 20.312MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_008.cvc.smt2 |    0.068s | 20.204MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00030_005.cvc.smt2 |    0.068s | 20.584MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00030_004.cvc.smt2 |    0.068s | 20.908MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00008_001.cvc.smt2 |    0.068s | 20.364MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00007_004.cvc.smt2 |    0.069s | 20.14MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00009_008.cvc.smt2 |    0.069s | 20.404MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_007.cvc.smt2 |    0.069s | 20.832MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_009.cvc.smt2 |    0.069s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00004_004.cvc.smt2 |    0.069s | 20.16MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00007_006.cvc.smt2 |    0.069s | 20.352MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00040_002.cvc.smt2 |    0.069s | 20.824MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00030_004.cvc.smt2 |    0.069s | 20.46MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00060_005.cvc.smt2 |    0.069s | 21.716MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00040_006.cvc.smt2 |    0.069s | 20.892MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00030_007.cvc.smt2 |    0.069s | 20.756MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00060_004.cvc.smt2 |    0.069s | 21.68MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_001.cvc.smt2 |    0.069s | 21.836MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_001.cvc.smt2 |    0.069s | 20.136MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_001.cvc.smt2 |    0.069s | 21.4MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00004_001.cvc.smt2 |    0.070s | 20.284MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00007_002.cvc.smt2 |    0.070s | 20.38MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00008_008.cvc.smt2 |    0.070s | 20.396MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00003_006.cvc.smt2 |    0.070s | 20.08MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_004.cvc.smt2 |    0.070s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00002_008.cvc.smt2 |    0.070s | 19.88MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00002_001.cvc.smt2 |    0.070s | 19.636MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_005.cvc.smt2 |    0.070s | 20.28MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00020_009.cvc.smt2 |    0.070s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_001.cvc.smt2 |    0.070s | 20.908MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00050_003.cvc.smt2 |    0.070s | 21.132MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_nf_ai_00008_001.cvc.smt2 |    0.070s | 20.18MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00004_001.cvc.smt2 |    0.070s | 20.188MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_007.cvc.smt2 |    0.071s | 20.384MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00002_002.cvc.smt2 |    0.071s | 19.924MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00004_004.cvc.smt2 |    0.071s | 20.148MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_001.cvc.smt2 |    0.071s | 20.096MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00008_005.cvc.smt2 |    0.071s | 20.176MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00004_008.cvc.smt2 |    0.071s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00009_003.cvc.smt2 |    0.071s | 20.816MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_003.cvc.smt2 |    0.071s | 20.788MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00040_006.cvc.smt2 |    0.071s | 20.892MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_009.cvc.smt2 |    0.071s | 21.104MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00040_005.cvc.smt2 |    0.071s | 20.844MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_005.cvc.smt2 |    0.071s | 20.908MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00040_009.cvc.smt2 |    0.071s | 21.148MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00050_002.cvc.smt2 |    0.071s | 21.332MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_006.cvc.smt2 |    0.071s | 21.124MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00010_001.cvc.smt2 |    0.071s | 20.352MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00009_001.cvc.smt2 |    0.071s | 20.644MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00002_001.cvc.smt2 |    0.071s | 19.988MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00004_004.cvc.smt2 |    0.072s | 20.1MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00005_007.cvc.smt2 |    0.072s | 20.34MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_001.cvc.smt2 |    0.072s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00004_005.cvc.smt2 |    0.072s | 20.12MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_005.cvc.smt2 |    0.072s | 20.168MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00004_006.cvc.smt2 |    0.072s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_006.cvc.smt2 |    0.072s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00006_007.cvc.smt2 |    0.072s | 20.396MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_005.cvc.smt2 |    0.072s | 20.588MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_008.cvc.smt2 |    0.072s | 21.336MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00040_007.cvc.smt2 |    0.072s | 20.912MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_008.cvc.smt2 |    0.073s | 20.044MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_003.cvc.smt2 |    0.073s | 20.164MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00002_005.cvc.smt2 |    0.073s | 20.092MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_002.cvc.smt2 |    0.073s | 20.388MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00004_007.cvc.smt2 |    0.073s | 20.112MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00006_009.cvc.smt2 |    0.073s | 20.156MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_009.cvc.smt2 |    0.073s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_003.cvc.smt2 |    0.073s | 19.888MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_003.cvc.smt2 |    0.073s | 20.404MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00002_006.cvc.smt2 |    0.073s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_007.cvc.smt2 |    0.073s | 20.212MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00030_002.cvc.smt2 |    0.073s | 20.688MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00050_001.cvc.smt2 |    0.073s | 21.716MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_sf_ai_00003_001.cvc.smt2 |    0.073s | 20.112MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_005.cvc.smt2 |    0.074s | 20.168MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_001.cvc.smt2 |    0.074s | 20.172MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00005_002.cvc.smt2 |    0.074s | 20.216MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_007.cvc.smt2 |    0.074s | 20.136MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00003_004.cvc.smt2 |    0.074s | 20.092MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_004.cvc.smt2 |    0.074s | 20.38MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00006_009.cvc.smt2 |    0.074s | 20.16MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00003_001.cvc.smt2 |    0.074s | 20.116MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00040_003.cvc.smt2 |    0.074s | 21.128MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00050_009.cvc.smt2 |    0.074s | 21.264MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_005.cvc.smt2 |    0.074s | 21.904MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00050_005.cvc.smt2 |    0.074s | 21.208MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_nf_ai_00003_001.cvc.smt2 |    0.074s | 19.872MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00006_001.cvc.smt2 |    0.074s | 20.132MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_nf_ai_00002_001.cvc.smt2 |    0.074s | 19.888MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00005_008.cvc.smt2 |    0.075s | 19.908MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_002.cvc.smt2 |    0.075s | 20.148MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_001.cvc.smt2 |    0.075s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00006_001.cvc.smt2 |    0.075s | 20.14MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_009.cvc.smt2 |    0.075s | 20.112MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_002.cvc.smt2 |    0.075s | 19.908MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00007_003.cvc.smt2 |    0.075s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00004_003.cvc.smt2 |    0.075s | 19.912MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_008.cvc.smt2 |    0.075s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_005.cvc.smt2 |    0.075s | 20.076MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00003_004.cvc.smt2 |    0.075s | 19.852MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00003_008.cvc.smt2 |    0.075s | 20.076MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00050_007.cvc.smt2 |    0.075s | 21.304MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_007.cvc.smt2 |    0.075s | 20.512MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00020_002.cvc.smt2 |    0.075s | 20.364MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00030_007.cvc.smt2 |    0.075s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00060_001.cvc.smt2 |    0.075s | 21.8MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00050_009.cvc.smt2 |    0.075s | 21.636MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00010_002.cvc.smt2 |    0.075s | 20.572MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00009_001.cvc.smt2 |    0.075s | 20.152MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00005_001.cvc.smt2 |    0.075s | 20.132MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00006_006.cvc.smt2 |    0.076s | 20.408MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_006.cvc.smt2 |    0.076s | 20.18MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_004.cvc.smt2 |    0.076s | 20.256MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_006.cvc.smt2 |    0.076s | 20.416MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00006_008.cvc.smt2 |    0.076s | 20.848MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_008.cvc.smt2 |    0.076s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_004.cvc.smt2 |    0.076s | 20.708MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00004_005.cvc.smt2 |    0.076s | 19.98MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00010_005.cvc.smt2 |    0.076s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00020_001.cvc.smt2 |    0.076s | 20.156MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00010_005.cvc.smt2 |    0.076s | 20.156MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00005_001.cvc.smt2 |    0.076s | 20.196MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_009.cvc.smt2 |    0.077s | 20.172MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_008.cvc.smt2 |    0.077s | 20.384MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00007_005.cvc.smt2 |    0.077s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00004_001.cvc.smt2 |    0.077s | 20.02MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00010_006.cvc.smt2 |    0.077s | 20.112MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00002_001.cvc.smt2 |    0.077s | 19.548MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_008.cvc.smt2 |    0.077s | 20.268MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_002.cvc.smt2 |    0.077s | 20.392MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00050_009.cvc.smt2 |    0.077s | 21.696MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_008.cvc.smt2 |    0.077s | 22.408MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00020_004.cvc.smt2 |    0.077s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00040_007.cvc.smt2 |    0.077s | 21.268MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00006_001.cvc.smt2 |    0.077s | 20.148MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00010_001.cvc.smt2 |    0.077s | 20.4MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_008.cvc.smt2 |    0.078s | 20.116MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00003_005.cvc.smt2 |    0.078s | 19.908MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_008.cvc.smt2 |    0.078s | 20.484MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00005_003.cvc.smt2 |    0.078s | 20.528MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00008_001.cvc.smt2 |    0.078s | 20.304MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_001.cvc.smt2 |    0.078s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00006_005.cvc.smt2 |    0.078s | 20.356MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00005_001.cvc.smt2 |    0.078s | 19.744MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00003_006.cvc.smt2 |    0.078s | 20.336MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_004.cvc.smt2 |    0.078s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00003_005.cvc.smt2 |    0.078s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00002_002.cvc.smt2 |    0.078s | 19.912MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00060_007.cvc.smt2 |    0.078s | 22.676MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_002.cvc.smt2 |    0.078s | 21.676MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_005.cvc.smt2 |    0.079s | 20.268MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00008_002.cvc.smt2 |    0.079s | 20.172MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00007_009.cvc.smt2 |    0.079s | 20.348MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_004.cvc.smt2 |    0.079s | 20.344MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00002_009.cvc.smt2 |    0.079s | 19.836MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_002.cvc.smt2 |    0.079s | 20.372MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00004_009.cvc.smt2 |    0.079s | 20.392MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00005_007.cvc.smt2 |    0.079s | 20.272MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00007_007.cvc.smt2 |    0.079s | 20.12MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00003_007.cvc.smt2 |    0.079s | 19.884MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00008_007.cvc.smt2 |    0.079s | 20.136MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_001.cvc.smt2 |    0.079s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00060_003.cvc.smt2 |    0.079s | 21.948MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00020_009.cvc.smt2 |    0.079s | 20.3MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00007_001.cvc.smt2 |    0.079s | 20.168MiB| unsat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_sf_ai_00008_001.cvc.smt2 |    0.079s | 20.424MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00005_001.cvc.smt2 |    0.080s | 20.116MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00004_002.cvc.smt2 |    0.080s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00007_007.cvc.smt2 |    0.080s | 20.576MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_009.cvc.smt2 |    0.080s | 20.628MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00040_001.cvc.smt2 |    0.080s | 21.264MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00060_009.cvc.smt2 |    0.080s | 21.676MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_004.cvc.smt2 |    0.080s | 22.36MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00010_001.cvc.smt2 |    0.080s | 20.164MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00010_005.cvc.smt2 |    0.081s | 20.892MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00007_006.cvc.smt2 |    0.081s | 20.552MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_002.cvc.smt2 |    0.081s | 22.456MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_002.cvc.smt2 |    0.081s | 20.176MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_001.cvc.smt2 |    0.081s | 20.316MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00007_006.cvc.smt2 |    0.082s | 20.428MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_009.cvc.smt2 |    0.082s | 20.112MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_005.cvc.smt2 |    0.082s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00010_002.cvc.smt2 |    0.082s | 20.156MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_009.cvc.smt2 |    0.082s | 22.412MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00060_008.cvc.smt2 |    0.082s | 22.396MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_001.cvc.smt2 |    0.083s | 20.128MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00006_002.cvc.smt2 |    0.083s | 20.328MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_001.cvc.smt2 |    0.083s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00009_007.cvc.smt2 |    0.083s | 20.48MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00040_007.cvc.smt2 |    0.083s | 21.124MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00050_001.cvc.smt2 |    0.083s | 21.136MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_004.cvc.smt2 |    0.083s | 21.128MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00010_004.cvc.smt2 |    0.084s | 20.46MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00040_003.cvc.smt2 |    0.084s | 21.128MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00040_001.cvc.smt2 |    0.084s | 21.028MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_001.cvc.smt2 |    0.085s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00008_001.cvc.smt2 |    0.085s | 20.384MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00050_006.cvc.smt2 |    0.085s | 21.268MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00030_006.cvc.smt2 |    0.085s | 20.732MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00009_001.cvc.smt2 |    0.086s | 20.624MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_007.cvc.smt2 |    0.086s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00010_007.cvc.smt2 |    0.086s | 20.34MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00020_008.cvc.smt2 |    0.086s | 20.204MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00010_003.cvc.smt2 |    0.086s | 20.148MiB| sat | 0 |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_sf_ai_00010_001.cvc.smt2 |    0.086s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00009_005.cvc.smt2 |    0.087s | 20.352MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_005.cvc.smt2 |    0.087s | 21.16MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_001.cvc.smt2 |    0.088s | 19.896MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00010_002.cvc.smt2 |    0.088s | 20.756MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00050_003.cvc.smt2 |    0.088s | 21.8MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00010_006.cvc.smt2 |    0.088s | 20.112MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_009.cvc.smt2 |    0.089s | 20.452MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00040_004.cvc.smt2 |    0.089s | 20.932MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_001.cvc.smt2 |    0.090s | 21.32MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00060_009.cvc.smt2 |    0.090s | 22.648MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00060_004.cvc.smt2 |    0.090s | 22.036MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00060_009.cvc.smt2 |    0.091s | 21.784MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00008_009.cvc.smt2 |    0.092s | 21.044MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00010_006.cvc.smt2 |    0.093s | 20.196MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00010_003.cvc.smt2 |    0.093s | 20.424MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_003.cvc.smt2 |    0.093s | 22.428MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00030_009.cvc.smt2 |    0.093s | 20.864MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_008.cvc.smt2 |    0.093s | 21.652MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00050_004.cvc.smt2 |    0.094s | 21.676MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_002.cvc.smt2 |    0.095s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_001.cvc.smt2 |    0.096s | 20.692MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00008_008.cvc.smt2 |    0.097s | 20.64MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00050_008.cvc.smt2 |    0.097s | 21.556MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00030_005.cvc.smt2 |    0.097s | 20.844MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00006_006.cvc.smt2 |    0.098s | 20.124MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00050_007.cvc.smt2 |    0.098s | 21.296MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00060_003.cvc.smt2 |    0.098s | 22.44MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00040_008.cvc.smt2 |    0.098s | 21.008MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00020_001.cvc.smt2 |    0.099s | 20.196MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00020_002.cvc.smt2 |    0.101s | 20.588MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_001.cvc.smt2 |    0.102s | 20.48MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_003.cvc.smt2 |    0.102s | 19.596MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00006_005.cvc.smt2 |    0.102s | 20.6MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00050_004.cvc.smt2 |    0.102s | 21.16MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00060_003.cvc.smt2 |    0.102s | 21.736MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_006.cvc.smt2 |    0.103s | 20.112MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00050_002.cvc.smt2 |    0.104s | 21.684MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00008_006.cvc.smt2 |    0.105s | 20.62MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00005_004.cvc.smt2 |    0.105s | 20.024MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00007_005.cvc.smt2 |    0.105s | 20.104MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_006.cvc.smt2 |    0.105s | 19.984MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00030_005.cvc.smt2 |    0.105s | 20.776MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_005.cvc.smt2 |    0.106s | 20.132MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_004.cvc.smt2 |    0.106s | 19.816MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00006_004.cvc.smt2 |    0.106s | 20.408MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00010_005.cvc.smt2 |    0.106s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00005_004.cvc.smt2 |    0.107s | 20.052MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_007.cvc.smt2 |    0.107s | 20.168MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00003_008.cvc.smt2 |    0.107s | 20.08MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00009_003.cvc.smt2 |    0.107s | 20.832MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_003.cvc.smt2 |    0.108s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_005.cvc.smt2 |    0.108s | 20.128MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00007_005.cvc.smt2 |    0.108s | 20.12MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00004_003.cvc.smt2 |    0.108s | 20.148MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_005.cvc.smt2 |    0.109s | 20.128MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_002.cvc.smt2 |    0.109s | 20.164MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00010_005.cvc.smt2 |    0.109s | 20.628MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_005.cvc.smt2 |    0.110s | 20.348MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_004.cvc.smt2 |    0.110s | 20.348MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_009.cvc.smt2 |    0.110s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00060_006.cvc.smt2 |    0.110s | 21.784MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00010_007.cvc.smt2 |    0.111s | 20.924MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00008_007.cvc.smt2 |    0.111s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00060_007.cvc.smt2 |    0.111s | 21.696MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_006.cvc.smt2 |    0.111s | 21.632MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00007_001.cvc.smt2 |    0.112s | 20.412MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00007_008.cvc.smt2 |    0.113s | 20.228MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_002.cvc.smt2 |    0.113s | 20.5MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_008.cvc.smt2 |    0.113s | 20.58MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00060_005.cvc.smt2 |    0.114s | 22.396MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00010_008.cvc.smt2 |    0.115s | 20.756MiB| unsat | 0 |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00060_004.cvc.smt2 |    0.117s | 22.536MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00009_003.cvc.smt2 |    0.123s | 20.548MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00009_002.cvc.smt2 |    0.123s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_007.cvc.smt2 |    0.126s | 20.348MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00009_002.cvc.smt2 |    0.126s | 20.448MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00009_009.cvc.smt2 |    0.127s | 20.616MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00009_009.cvc.smt2 |    0.129s | 20.664MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_003.cvc.smt2 |    0.134s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00007_002.cvc.smt2 |    0.137s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_006.cvc.smt2 |    0.138s | 20.6MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00010_004.cvc.smt2 |    0.138s | 20.872MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00010_005.cvc.smt2 |    0.139s | 20.64MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00010_008.cvc.smt2 |    0.141s | 20.68MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_006.cvc.smt2 |    0.146s | 20.92MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00010_004.cvc.smt2 |    0.153s | 20.68MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00010_003.cvc.smt2 |    0.196s | 21.128MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00009_006.cvc.smt2 |    0.200s | 21.124MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00009_004.cvc.smt2 |    0.230s | 21.54MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00009_004.cvc.smt2 |    0.260s | 21.408MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00010_007.cvc.smt2 |    0.262s | 21.224MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00010_001.cvc.smt2 |    0.267s | 21.44MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00010_007.cvc.smt2 |    0.272s | 21.424MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00010_003.cvc.smt2 |    0.282s | 21.144MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00010_009.cvc.smt2 |    0.439s | 22.044MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00010_001.cvc.smt2 |    0.474s | 21.64MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00010_009.cvc.smt2 |    0.511s | 21.728MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00010_002.cvc.smt2 |    1.778s | 23.148MiB| unsat | 0 |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00010_002.cvc.smt2 |    2.046s | 23.532MiB| unsat | 0 |  |
