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
Experiment date and time: 2026-09-06 23:15:19 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_ALIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1
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
|non-incremental/QF_ALIA/cvc/read2.smt2                       |    0.025s | 20.136MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00004_001.cvc.smt2 |    0.025s | 20.42MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00008_001.cvc.smt2 |    0.025s | 20.448MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_5b181b.smt2                |    0.039s | 20.164MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00002_001.cvc.smt2 |    0.049s | 20.292MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_7fd2c4.smt2                |    0.051s | 19.956MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00001_001.cvc.smt2 |    0.052s | 20.16MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-20.smt2 |    0.052s | 24.124MiB| sat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata.smt2                |    0.054s | 22.156MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00004_001.cvc.smt2 |    0.054s | 20.428MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00003_001.cvc.smt2 |    0.055s | 20.148MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00011_001.cvc.smt2 |    0.055s | 20.868MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_174f4d.smt2                |    0.058s | 20.492MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00003_001.cvc.smt2 |    0.058s | 20.204MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00010_001.cvc.smt2 |    0.059s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00013_001.cvc.smt2 |    0.059s | 20.704MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00015_001.cvc.smt2 |    0.059s | 20.412MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th2-6.smt2 |    0.059s | 19.596MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_ed9849.smt2                |    0.060s | 20.788MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_cb19c7.smt2                |    0.060s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00001_001.cvc.smt2 |    0.060s | 20.432MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00009_001.cvc.smt2 |    0.060s | 20.412MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00005_001.cvc.smt2 |    0.060s | 20.252MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00002_001.cvc.smt2 |    0.060s | 20.468MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00012_001.cvc.smt2 |    0.060s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00010_001.cvc.smt2 |    0.061s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00007_001.cvc.smt2 |    0.061s | 20.4MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th1-6.smt2 |    0.061s | 20.1MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00009_001.cvc.smt2 |    0.062s | 20.428MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00013_001.cvc.smt2 |    0.062s | 20.5MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00008_001.cvc.smt2 |    0.062s | 20.592MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00014_001.cvc.smt2 |    0.062s | 20.484MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_d421cb.smt2                |    0.063s | 20.916MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00012_001.cvc.smt2 |    0.063s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th2-6.smt2 |    0.065s | 19.632MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_13f61c.smt2                |    0.066s | 20.872MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00014_001.cvc.smt2 |    0.067s | 20.74MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-5.smt2 |    0.069s | 20.844MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th1-6.smt2 |    0.070s | 19.6MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_408ff0.smt2                |    0.071s | 21.228MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-5.smt2 |    0.073s | 20.624MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-15.smt2 |    0.074s | 22.916MiB| sat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_ffa5fa.smt2                |    0.077s | 20.084MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00007_001.cvc.smt2 |    0.077s | 20.64MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.5.smt2             |    0.078s | 20.732MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-dmem2.smt2                    |    0.079s | 22.464MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-dmem-a.smt2                   |    0.080s | 22.232MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00015_001.cvc.smt2 |    0.081s | 20.652MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata-a.smt2              |    0.083s | 22.192MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-10.smt2 |    0.083s | 22.088MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-invalid-6.smt2 |    0.084s | 20.672MiB| sat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00006_001.cvc.smt2 |    0.086s | 20.628MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_22b1f2.smt2                |    0.088s | 22.376MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_46582a.smt2                |    0.090s | 22.408MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_f5059f.smt2                |    0.092s | 22.36MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.6.smt2        |    0.092s | 21.436MiB| sat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00005_001.cvc.smt2 |    0.097s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00006_001.cvc.smt2 |    0.097s | 20.372MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00011_001.cvc.smt2 |    0.097s | 20.376MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_3031c9.smt2                |    0.099s | 22.732MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_fdec13.smt2                |    0.104s | 20.88MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_509c40.smt2                |    0.104s | 22.348MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.5.smt2        |    0.111s | 21.28MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-10.smt2 |    0.112s | 21.208MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-5.smt2 |    0.120s | 21.256MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-15.smt2 |    0.121s | 22.184MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.7.smt2        |    0.124s | 21.9MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-5.smt2 |    0.125s | 20.904MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.8.smt2        |    0.131s | 22.392MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.6.smt2             |    0.160s | 21.188MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.9.smt2        |    0.193s | 22.92MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.7.smt2             |    0.199s | 21.284MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-20.smt2 |    0.207s | 22.672MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-10.smt2 |    0.207s | 22.896MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.10.smt2       |    0.234s | 22.852MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-10.smt2 |    0.237s | 22.968MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.11.smt2       |    0.307s | 23.548MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-15.smt2 |    0.318s | 24.116MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.12.smt2       |    0.330s | 23.752MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.13.smt2       |    0.507s | 24.788MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.8.smt2             |    0.509s | 21.892MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.14.smt2       |    0.524s | 25.008MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.18.smt2            |    0.610s | 27.104MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.15.smt2            |    0.666s | 25.316MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-15.smt2 |    0.795s | 26.04MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_2.smt2 |    0.796s | 31.796MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.9.smt2             |    0.812s | 22.436MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_4.smt2 |    0.814s | 31.872MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_1.smt2 |    0.840s | 31.928MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_3.smt2 |    0.884s | 31.884MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_6.smt2 |    1.055s | 35.304MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_3.smt2 |    1.078s | 35.304MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_7.smt2 |    1.093s | 35.18MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.17.smt2            |    1.243s | 27.208MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.15.smt2       |    1.247s | 26.94MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-10.smt2 |    1.483s | 23.2MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.10.smt2            |    1.769s | 23.2MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.19.smt2            |    1.811s | 28.46MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.16.smt2            |    1.875s | 26.788MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.11.smt2            |    2.245s | 23.552MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.12.smt2            |    3.038s | 24.388MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_6.smt2 |    3.461s | 49.3MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_5.smt2 |    3.978s | 61.38MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_7.smt2 |    4.052s | 49.348MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.16.smt2       |    4.133s | 28.5MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-011.c_AllErrorsAtOnce_Iteration2_0.smt2 |    4.509s | 23.864MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-1.i_0.smt2 |    4.729s | 46.22MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-20.smt2 |    4.867s | 31.624MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.13.smt2            |    5.253s | 25.328MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-20.smt2 |    5.278s | 31.852MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.20.smt2            |    5.307s | 31.808MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.17.smt2       |    5.318s | 30.116MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.14.smt2            |    6.147s | 25.52MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.20.smt2       |    6.815s | 32.804MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.19.smt2       |    7.793s | 31.652MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.24.smt2            |    8.217s | 36.208MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.21.smt2            |    8.266s | 33.836MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-15.smt2 |    8.282s | 26.88MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_1.smt2 |    9.559s | 67.38MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.22.smt2            |   10.177s | 35.756MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_2.smt2 |   10.371s | 67.252MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.23.smt2            |   12.204s | 35.972MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.18.smt2       |   12.266s | 32.2MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.25.smt2            |   17.391s | 39.888MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.26.smt2            |   18.167s | 40.0MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.28.smt2            |   20.013s | 42.368MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.21.smt2       |   20.013s | 34.864MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-2.i_0.smt2 |   20.014s | 60.788MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.28.smt2       |   20.019s | 38.448MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/CostasArray-15.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.025s | 29.624MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.26.smt2       |   20.026s | 37.24MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.30.smt2            |   20.027s | 39.848MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.29.smt2            |   20.029s | 39.556MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.27.smt2            |   20.039s | 38.08MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_11.smt2 |   20.043s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.30.smt2       |   20.044s | 40.236MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.23.smt2       |   20.044s | 37.008MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.27.smt2       |   20.044s | 36.316MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.25.smt2       |   20.045s | 37.432MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.24.smt2       |   20.046s | 37.188MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-019.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.046s | 28.4MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_time_var_mutex.i_0.smt2 |   20.047s | 74.396MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.22.smt2       |   20.049s | 35.188MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_2.smt2 |   20.052s | 59.132MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-2.i_0.smt2 |   20.052s | 115.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.29.smt2       |   20.053s | 34.6MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_4.smt2 |   20.053s | 128.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lamport.i_0.smt2 |   20.054s | 73.484MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-015.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.054s | 26.532MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-20.smt2 |   20.055s | 32.364MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_1.smt2 |   20.059s | 59.576MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_6.smt2 |   20.061s | 98.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_6.smt2 |   20.061s | 62.496MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_1.smt2 |   20.062s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_0.smt2 |   20.062s | 61.672MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_0.smt2 |   20.063s | 123.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-1.i_0.smt2 |   20.065s | 102.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-016.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.067s | 26.288MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_dekker.i_0.smt2 |   20.069s | 63.5MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-2.i_0.smt2 |   20.070s | 101.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_7.smt2 |   20.070s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-1.i_0.smt2 |   20.071s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_10.smt2 |   20.072s | 127.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_3.smt2 |   20.075s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_5.smt2 |   20.081s | 114.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_7.smt2 |   20.085s | 134.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_4.smt2 |   20.086s | 58.628MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_0.smt2 |   20.086s | 59.764MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_peterson.i_0.smt2 |   20.087s | 67.896MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_3.smt2 |   20.089s | 61.036MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_8.smt2 |   20.091s | 128.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_2.smt2 |   20.092s | 123.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-2.i_0.smt2 |   20.094s | 96.424MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_9.smt2 |   20.095s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-1.i_0.smt2 |   20.097s | 126.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_6.smt2 |   20.097s | 126.0MiB| timeout | 0 |  |
