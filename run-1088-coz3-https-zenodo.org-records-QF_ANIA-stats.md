# .

* SAT 57
* UNSAT 17
* TIMEOUT 83
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-06 23:17:24 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ANIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_ANIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ANIA.tar.zst?download=1
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
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_7.smt2 |    0.025s | 20.672MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_12.smt2 |    0.025s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_13.smt2 |    0.028s | 20.156MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_3.smt2 |    0.032s | 20.788MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_7.smt2 |    0.033s | 20.592MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_4.smt2 |    0.034s | 20.388MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_5.smt2 |    0.038s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_3.smt2 |    0.040s | 20.436MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_9.smt2 |    0.041s | 20.212MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_8.smt2 |    0.042s | 20.876MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_0.smt2 |    0.043s | 20.82MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/in-de42.c_AllErrorsAtOnce_Iteration7_0.smt2 |    0.047s | 20.864MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/3.smt2 |    0.047s | 21.132MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_1.smt2 |    0.048s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_10.smt2 |    0.049s | 20.284MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_2.smt2 |    0.050s | 20.296MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_0.smt2 |    0.050s | 20.304MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_4.smt2 |    0.054s | 20.756MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_9.smt2 |    0.055s | 21.048MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_1.smt2 |    0.056s | 20.844MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_6.smt2 |    0.056s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_14.smt2 |    0.058s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_5.smt2 |    0.067s | 20.376MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_2.smt2 |    0.067s | 20.884MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_6.smt2 |    0.067s | 20.676MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lazy_false-unreach-call.i.smt2 |    0.070s | 22.076MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/3.smt2 |    0.075s | 21.248MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_2.smt2 |    0.082s | 21.288MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/3.smt2 |    0.084s | 21.336MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_1.smt2 |    0.088s | 21.796MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lamport_true-unreach-call.i.smt2 |    0.088s | 22.7MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_peterson_true-unreach-call.i.smt2 |    0.088s | 22.784MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_dekker_true-unreach-call.i.smt2 |    0.094s | 23.34MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/4.smt2 |    0.096s | 22.084MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_5.smt2 |    0.166s | 21.704MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_9.smt2 |    0.168s | 22.412MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_3.smt2 |    0.180s | 23.688MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/4.smt2 |    0.184s | 22.748MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/3.smt2 |    0.187s | 22.916MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/5.smt2 |    0.196s | 23.432MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_3.smt2 |    0.247s | 23.592MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_0.smt2 |    0.293s | 23.624MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_4.smt2 |    0.312s | 23.176MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/5.smt2 |    0.456s | 24.456MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/lcm2.c_AllErrorsAtOnce_Iteration3_0.smt2 |    0.464s | 22.456MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/6.smt2 |    0.518s | 25.368MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_7.smt2 |    0.562s | 23.428MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/4.smt2 |    0.804s | 27.016MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/ddlm2013.i_AllErrorsAtOnce_Iteration5_0.smt2 |    0.936s | 21.948MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/7.smt2 |    1.227s | 29.328MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_3.smt2 |    1.276s | 25.168MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_4.smt2 |    1.317s | 25.04MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/9.smt2 |    1.908s | 32.448MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_6.smt2 |    1.949s | 34.696MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_8.smt2 |    2.081s | 26.52MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_false-unreach-call.i.smt2 |    2.086s | 22.58MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_true-unreach-call.i.smt2 |    2.186s | 22.832MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/7.smt2 |    2.532s | 30.748MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-stable-1575714-1-150_1a-drivers--net--wireless--b43--b43.ko-entry_point_ldv-val-v0.8_false-unreach-call.cil.out.c_TraceCheck_Iteration4_0.smt2 |    3.080s | 293.0MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/4.smt2 |    4.221s | 32.512MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_12.smt2 |    5.727s | 39.676MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_9.smt2 |    5.742s | 39.756MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/6.smt2 |    6.156s | 36.084MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.04_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration6_TraceCheck_0.smt2 |    6.753s | 47.548MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/8.smt2 |    6.760s | 36.108MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_6.smt2 |    6.805s | 39.836MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_7.smt2 |    7.963s | 42.56MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_10.smt2 |    8.219s | 42.62MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_4.smt2 |    8.287s | 42.504MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_13.smt2 |    8.739s | 42.56MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/5.smt2 |    8.796s | 42.284MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-3.12-rc1.tar.xz-144_2a-drivers--media--usb--stk1160--stk1160.ko-entry_point_false-unreach-call.cil.out.c_TraceCheck_Iteration9_0.smt2 |   15.632s | 672.0MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg40_true-unreach-call.i_AllErrorsAtOnce_Iteration17_TraceCheck_0.smt2 |   18.428s | 98.048MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_1.smt2 |   19.330s | 51.54MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_false-unreach-call.i.cil.c_8.smt2 |   20.015s | 72.968MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/10.smt2 |   20.016s | 58.512MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-1.c_0.smt2 |   20.016s | 51.432MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_8.smt2 |   20.017s | 67.108MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum02-1.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.017s | 21.272MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_0.smt2 |   20.017s | 55.988MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/8.smt2 |   20.018s | 97.096MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/10.smt2 |   20.018s | 50.884MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_1.smt2 |   20.018s | 61.66MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_4.smt2 |   20.019s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_7.smt2 |   20.019s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_12.smt2 |   20.020s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_6.smt2 |   20.021s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_13.smt2 |   20.022s | 114.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_14.smt2 |   20.022s | 60.128MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/10.smt2 |   20.024s | 98.272MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_0.smt2 |   20.025s | 91.276MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.025s | 31.176MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_false-unreach-call.i.cil.c_6.smt2 |   20.026s | 49.268MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_3.smt2 |   20.026s | 180.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/7.smt2 |   20.026s | 77.312MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_10.smt2 |   20.027s | 63.912MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-07.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.027s | 34.084MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_1.smt2 |   20.027s | 73.212MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_9.smt2 |   20.028s | 94.736MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_true-unreach-call_true-valid-memsafety.i.cil.c_8.smt2 |   20.029s | 66.124MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.01_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration11_TraceCheck_0.smt2 |   20.029s | 41.976MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/8.smt2 |   20.029s | 55.756MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_5.smt2 |   20.030s | 114.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_5.smt2 |   20.030s | 102.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/11.smt2 |   20.030s | 53.716MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_4.smt2 |   20.031s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_true-unreach-call.i.cil.c_6.smt2 |   20.031s | 58.328MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.031s | 33.528MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/11.smt2 |   20.032s | 72.848MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_3.smt2 |   20.032s | 76.508MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_2.smt2 |   20.032s | 55.512MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_11.smt2 |   20.033s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/11.smt2 |   20.033s | 120.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_3.smt2 |   20.034s | 106.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_8.smt2 |   20.034s | 59.64MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_5.smt2 |   20.034s | 80.908MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_4.smt2 |   20.034s | 76.452MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_10.smt2 |   20.035s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/9.smt2 |   20.035s | 81.776MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_2.smt2 |   20.036s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/diskperf.i.cil-1.c_0.smt2 |   20.036s | 107.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_2.smt2 |   20.037s | 124.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_2.smt2 |   20.038s | 115.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_5.smt2 |   20.038s | 108.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_1.smt2 |   20.041s | 79.94MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_1.smt2 |   20.042s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_11.smt2 |   20.044s | 60.884MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/6.smt2 |   20.045s | 64.424MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-hid-usbhid-usbmouse.ko_false-unreach-call.cil.out.i_AllErrorsAtOnce_Iteration21_TraceCheck_0.smt2 |   20.047s | 62.804MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/11.smt2 |   20.047s | 53.236MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/5.smt2 |   20.047s | 57.252MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/9.smt2 |   20.047s | 64.468MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-2.c_0.smt2 |   20.047s | 46.064MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_1.smt2 |   20.047s | 76.52MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_5.smt2 |   20.048s | 59.132MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_2.smt2 |   20.048s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_6.smt2 |   20.048s | 113.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rangesum40_false-unreach-call.i_AllErrorsAtOnce_Iteration19_TraceCheck_0.smt2 |   20.048s | 98.976MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_2.smt2 |   20.048s | 208.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/8.smt2 |   20.048s | 71.988MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-14.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.048s | 113.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_5.smt2 |   20.048s | 64.776MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_4.smt2 |   20.048s | 65.64MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_3.smt2 |   20.049s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_11.smt2 |   20.050s | 68.528MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/7.smt2 |   20.050s | 81.2MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/6.smt2 |   20.050s | 67.692MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_4.smt2 |   20.050s | 68.668MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-12.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.051s | 80.204MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_1.smt2 |   20.052s | 102.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_2.smt2 |   20.052s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/9.smt2 |   20.054s | 109.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/10.smt2 |   20.055s | 154.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_1.smt2 |   20.056s | 146.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_3.smt2 |   20.060s | 471.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_1.smt2 |   20.063s | 197.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_1.smt2 |   20.070s | 684.0MiB| timeout | 0 |  |
