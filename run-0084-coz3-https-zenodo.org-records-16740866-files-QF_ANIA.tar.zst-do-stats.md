# .

* SAT 57
* UNSAT 16
* TIMEOUT 84
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ANIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_ANIA.tar.zst-do
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ANIA.tar.zst?download=1
Z3 commit message: qsat: decide quantifier-free goals so qe2 returns sat instead of unknown (fixes iss-7027/small-30) (#9970)

## Summary

Fixes the `iss-7027/small-30` snapshot regression (`Z3Prover/bench`
discussion #2705) **at its root**, instead of working around it by
retuning the LP heuristics.

- **Benchmark:** `inputs/issues/iss-7027/small-30.smt2` —
`(check-sat-using qe2)` over a single `(distinct ...)` of 33 mixed
Int/Real terms.
- The recorded oracle was `unknown`; current `master` produces
`timeout`.

## Root cause

`unknown`/`timeout` are both wrong here: the formula is a `distinct`
over 33 terms (free Int/Real constants plus the literals `0`/`1`), which
is **trivially `sat`** — there are infinitely many distinct reals.

The real bug is in the `qsat` tactic that backs `qe2`. Running
quantifier elimination on a **quantifier-free** formula has nothing to
eliminate, so `qsat` left an undecided residual goal and
`check-sat-using` reported `unknown`. This reproduces on any ground
formula with free variables, e.g.:

```
(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)   ; -> unknown (should be sat)
```

For `small-30` the QE alternation additionally drove `theory_lra`
integer branch-and-bound down a non-terminating path, surfacing as a
`timeout` under the capture budget (the symptom the `random_hammers`
schedule change happened to expose).

## Fix

Under `check-sat` semantics, top-level free variables are implicitly
existentially quantified. So when the `qsat` input has no quantifiers,
decide satisfiability directly (route through the existing `qsat_sat`
path) instead of producing a residual goal. `qe2`/`qe` now return
`sat`/`unsat` for ground formulas.

QE of genuinely-quantified formulas is unchanged: `apply qe2` on a
quantified goal produces the same projected formula as before (verified
identical to `master`). Only the degenerate quantifier-free case is
affected.

This supersedes the previous approach in this PR (reverting the
`lp.random_hammers` default). That default is left **unchanged**
(`true`), preserving #9958's aggregate QF_LIA benefit. `small-30` now
returns `sat` in ~0.01s regardless of the heuristic schedule, because
the QE machinery no longer runs on this ground instance.

Two changes:
- `src/qe/qsat.cpp`: short-circuit quantifier-free input to the
satisfiability decision path.
- `Z3Prover/bench` `inputs/issues/iss-7027/small-30.expected.out`:
oracle updated `unknown` -> `sat` (to be committed alongside this fix).

## Validation

```
$ z3 small-30.smt2
sat            # ~0.01s

$ echo '(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)' | z3 -in
sat
$ echo '(declare-fun a () Int)(assert (and (> a 0)(< a 0)))(check-sat-using qe2)' | z3 -in
unsat
```

Full unit-test suite (`test-z3 /a`) passes (92/92). Quantified `qe2`
round-trips (`apply qe2`) match `master` byte-for-byte.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_10.smt2 |    0.029s | 20.696MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_4.smt2 |    0.048s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_7.smt2 |    0.049s | 20.204MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_14.smt2 |    0.050s | 20.58MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_9.smt2 |    0.050s | 20.36MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/in-de42.c_AllErrorsAtOnce_Iteration7_0.smt2 |    0.050s | 20.548MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_2.smt2 |    0.052s | 20.396MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_7.smt2 |    0.052s | 20.68MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_5.smt2 |    0.053s | 21.424MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_3.smt2 |    0.054s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_13.smt2 |    0.055s | 20.448MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_5.smt2 |    0.055s | 20.672MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_1.smt2 |    0.057s | 20.308MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_8.smt2 |    0.058s | 20.888MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_3.smt2 |    0.059s | 20.656MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_0.smt2 |    0.060s | 20.416MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_2.smt2 |    0.060s | 20.86MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_1.smt2 |    0.060s | 20.844MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_5.smt2 |    0.064s | 20.328MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_4.smt2 |    0.064s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_6.smt2 |    0.065s | 20.86MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_9.smt2 |    0.065s | 20.588MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/ddlm2013.i_AllErrorsAtOnce_Iteration5_0.smt2 |    0.070s | 21.112MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_0.smt2 |    0.077s | 20.908MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/3.smt2 |    0.086s | 21.144MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_6.smt2 |    0.089s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_12.smt2 |    0.095s | 20.58MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_2.smt2 |    0.105s | 21.376MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lazy_false-unreach-call.i.smt2 |    0.123s | 22.076MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_dekker_true-unreach-call.i.smt2 |    0.126s | 23.268MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_9.smt2 |    0.128s | 22.036MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_peterson_true-unreach-call.i.smt2 |    0.134s | 22.528MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/4.smt2 |    0.136s | 22.164MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lamport_true-unreach-call.i.smt2 |    0.136s | 22.54MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/3.smt2 |    0.147s | 21.412MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/3.smt2 |    0.151s | 21.936MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_1.smt2 |    0.196s | 21.984MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_0.smt2 |    0.200s | 22.96MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/3.smt2 |    0.203s | 23.544MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_4.smt2 |    0.227s | 22.848MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_3.smt2 |    0.228s | 24.504MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/4.smt2 |    0.259s | 22.968MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_3.smt2 |    0.269s | 23.536MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/5.smt2 |    0.302s | 23.948MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_7.smt2 |    0.658s | 23.66MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/6.smt2 |    0.808s | 26.508MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/5.smt2 |    0.867s | 26.0MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/4.smt2 |    0.920s | 26.772MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_4.smt2 |    1.183s | 25.936MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_3.smt2 |    1.245s | 25.972MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-3.12-rc1.tar.xz-144_2a-drivers--media--usb--stk1160--stk1160.ko-entry_point_false-unreach-call.cil.out.c_TraceCheck_Iteration9_0.smt2 |    1.316s | 129.0MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_true-unreach-call.i.smt2 |    2.082s | 22.868MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_false-unreach-call.i.smt2 |    2.086s | 22.408MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_6.smt2 |    2.410s | 35.952MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/7.smt2 |    2.459s | 30.384MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/4.smt2 |    3.021s | 31.648MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-stable-1575714-1-150_1a-drivers--net--wireless--b43--b43.ko-entry_point_ldv-val-v0.8_false-unreach-call.cil.out.c_TraceCheck_Iteration4_0.smt2 |    3.060s | 292.0MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/7.smt2 |    3.935s | 35.052MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_8.smt2 |    4.038s | 27.1MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg40_true-unreach-call.i_AllErrorsAtOnce_Iteration17_TraceCheck_0.smt2 |    4.104s | 99.0MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/8.smt2 |    5.610s | 37.444MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/6.smt2 |    5.863s | 32.908MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.04_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration6_TraceCheck_0.smt2 |    6.406s | 46.676MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_10.smt2 |    7.301s | 42.288MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_13.smt2 |    7.740s | 42.328MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_7.smt2 |    8.041s | 42.432MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_4.smt2 |    8.486s | 42.436MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/5.smt2 |   12.025s | 47.272MiB| unsat | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/9.smt2 |   15.385s | 51.76MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rangesum40_false-unreach-call.i_AllErrorsAtOnce_Iteration19_TraceCheck_0.smt2 |   16.977s | 118.0MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_9.smt2 |   17.618s | 55.784MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_6.smt2 |   18.308s | 55.556MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_1.smt2 |   18.620s | 63.376MiB| sat | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-1.c_0.smt2 |   20.016s | 53.544MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/11.smt2 |   20.017s | 54.8MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/9.smt2 |   20.017s | 89.176MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_5.smt2 |   20.019s | 106.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_12.smt2 |   20.020s | 108.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_6.smt2 |   20.020s | 102.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/7.smt2 |   20.021s | 81.668MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_false-unreach-call.i.cil.c_8.smt2 |   20.021s | 74.2MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_false-unreach-call.i.cil.c_6.smt2 |   20.029s | 55.948MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/9.smt2 |   20.033s | 101.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.01_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration11_TraceCheck_0.smt2 |   20.033s | 43.82MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_14.smt2 |   20.034s | 59.888MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/11.smt2 |   20.035s | 64.524MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_11.smt2 |   20.036s | 68.844MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_12.smt2 |   20.037s | 53.812MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_5.smt2 |   20.037s | 112.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_5.smt2 |   20.039s | 60.332MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_2.smt2 |   20.039s | 121.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_2.smt2 |   20.039s | 113.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/8.smt2 |   20.040s | 74.86MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_5.smt2 |   20.041s | 101.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_8.smt2 |   20.043s | 66.672MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_11.smt2 |   20.043s | 59.704MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_10.smt2 |   20.043s | 109.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/11.smt2 |   20.044s | 113.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_3.smt2 |   20.044s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_1.smt2 |   20.045s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_true-unreach-call_true-valid-memsafety.i.cil.c_8.smt2 |   20.045s | 67.784MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/6.smt2 |   20.046s | 66.624MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/5.smt2 |   20.046s | 50.116MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_7.smt2 |   20.046s | 109.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_2.smt2 |   20.046s | 110.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_0.smt2 |   20.046s | 152.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_9.smt2 |   20.048s | 94.58MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_3.smt2 |   20.049s | 104.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_4.smt2 |   20.049s | 110.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_2.smt2 |   20.050s | 108.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_4.smt2 |   20.050s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_8.smt2 |   20.050s | 59.916MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/10.smt2 |   20.052s | 126.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_3.smt2 |   20.055s | 180.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_6.smt2 |   20.055s | 109.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.055s | 30.636MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_13.smt2 |   20.056s | 112.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_2.smt2 |   20.057s | 245.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_2.smt2 |   20.060s | 209.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/7.smt2 |   20.062s | 87.172MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_1.smt2 |   20.066s | 63.932MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_1.smt2 |   20.069s | 74.752MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/10.smt2 |   20.072s | 57.448MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-07.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.072s | 35.28MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum02-1.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.074s | 21.42MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_1.smt2 |   20.079s | 181.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/8.smt2 |   20.080s | 98.436MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_1.smt2 |   20.080s | 78.184MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_0.smt2 |   20.081s | 56.88MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_1.smt2 |   20.084s | 101.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_5.smt2 |   20.084s | 63.488MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.085s | 32.468MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/diskperf.i.cil-1.c_0.smt2 |   20.088s | 106.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/11.smt2 |   20.089s | 56.108MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/10.smt2 |   20.091s | 60.28MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/lcm2.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.094s | 28.612MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/8.smt2 |   20.095s | 65.076MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-2.c_0.smt2 |   20.095s | 45.672MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_3.smt2 |   20.096s | 948.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_4.smt2 |   20.098s | 76.1MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_4.smt2 |   20.098s | 64.9MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/10.smt2 |   20.099s | 66.048MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-12.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.099s | 77.292MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-hid-usbhid-usbmouse.ko_false-unreach-call.cil.out.i_AllErrorsAtOnce_Iteration21_TraceCheck_0.smt2 |   20.100s | 68.596MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_5.smt2 |   20.101s | 65.652MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_1.smt2 |   20.101s | 63.356MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_2.smt2 |   20.102s | 65.464MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/9.smt2 |   20.104s | 74.528MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_10.smt2 |   20.104s | 66.476MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_4.smt2 |   20.104s | 70.408MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/6.smt2 |   20.105s | 72.492MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_true-unreach-call.i.cil.c_6.smt2 |   20.107s | 91.064MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_1.smt2 |   20.107s | 147.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-14.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.108s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_3.smt2 |   20.108s | 74.888MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_11.smt2 |   20.109s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_1.smt2 |   20.111s | 674.0MiB| timeout | 0 |  |  |
