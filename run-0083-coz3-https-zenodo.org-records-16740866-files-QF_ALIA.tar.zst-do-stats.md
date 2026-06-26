# .

* SAT 52
* UNSAT 71
* TIMEOUT 53
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_ALIA.tar.zst-do
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1
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
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th1-6.smt2 |    0.027s | 19.628MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-5.smt2 |    0.029s | 20.624MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/read2.smt2                       |    0.051s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00005_001.cvc.smt2 |    0.053s | 20.36MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00002_001.cvc.smt2 |    0.054s | 20.408MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00015_001.cvc.smt2 |    0.055s | 20.4MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00002_001.cvc.smt2 |    0.057s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_cb19c7.smt2                |    0.059s | 20.744MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_ffa5fa.smt2                |    0.060s | 19.84MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00008_001.cvc.smt2 |    0.060s | 20.372MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00006_001.cvc.smt2 |    0.060s | 20.352MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th2-6.smt2 |    0.062s | 19.68MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00007_001.cvc.smt2 |    0.063s | 21.1MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00003_001.cvc.smt2 |    0.063s | 20.34MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-invalid-6.smt2 |    0.064s | 20.34MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00005_001.cvc.smt2 |    0.065s | 20.408MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00007_001.cvc.smt2 |    0.065s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00011_001.cvc.smt2 |    0.066s | 20.62MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th1-6.smt2 |    0.067s | 19.616MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_5b181b.smt2                |    0.068s | 20.04MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00003_001.cvc.smt2 |    0.069s | 20.324MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th2-6.smt2 |    0.070s | 19.568MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00004_001.cvc.smt2 |    0.070s | 20.312MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_7fd2c4.smt2                |    0.071s | 20.336MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00014_001.cvc.smt2 |    0.071s | 20.68MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00012_001.cvc.smt2 |    0.073s | 20.588MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00006_001.cvc.smt2 |    0.073s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_13f61c.smt2                |    0.074s | 20.896MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00014_001.cvc.smt2 |    0.074s | 20.636MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00010_001.cvc.smt2 |    0.074s | 20.356MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-10.smt2 |    0.075s | 21.984MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/pp-dmem2.smt2                    |    0.076s | 22.04MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00015_001.cvc.smt2 |    0.076s | 20.68MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_d421cb.smt2                |    0.077s | 20.68MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-5.smt2 |    0.078s | 20.796MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-5.smt2 |    0.078s | 20.932MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/pp-dmem-a.smt2                   |    0.078s | 22.18MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata.smt2                |    0.080s | 22.416MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata-a.smt2              |    0.081s | 22.156MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_408ff0.smt2                |    0.084s | 21.096MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00012_001.cvc.smt2 |    0.089s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00009_001.cvc.smt2 |    0.090s | 20.592MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00008_001.cvc.smt2 |    0.091s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00013_001.cvc.smt2 |    0.093s | 20.872MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00004_001.cvc.smt2 |    0.094s | 20.62MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_ed9849.smt2                |    0.095s | 21.12MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_46582a.smt2                |    0.098s | 22.316MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00010_001.cvc.smt2 |    0.099s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00001_001.cvc.smt2 |    0.100s | 20.128MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.5.smt2        |    0.101s | 21.172MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_22b1f2.smt2                |    0.102s | 22.176MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00011_001.cvc.smt2 |    0.102s | 20.408MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.6.smt2        |    0.102s | 21.636MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_174f4d.smt2                |    0.105s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00009_001.cvc.smt2 |    0.106s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-10.smt2 |    0.107s | 21.212MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.5.smt2             |    0.107s | 20.796MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00001_001.cvc.smt2 |    0.110s | 20.332MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_fdec13.smt2                |    0.112s | 20.872MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00013_001.cvc.smt2 |    0.112s | 20.36MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_3031c9.smt2                |    0.113s | 22.604MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-5.smt2 |    0.117s | 21.108MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.7.smt2        |    0.118s | 21.716MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-15.smt2 |    0.120s | 21.92MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-20.smt2 |    0.121s | 24.42MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-15.smt2 |    0.123s | 23.164MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_f5059f.smt2                |    0.128s | 22.176MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.6.smt2             |    0.130s | 20.876MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/piVC/piVC_509c40.smt2                |    0.139s | 22.208MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.8.smt2        |    0.145s | 22.152MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-10.smt2 |    0.179s | 22.684MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-10.smt2 |    0.184s | 22.66MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-20.smt2 |    0.186s | 22.924MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.9.smt2        |    0.223s | 22.616MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.7.smt2             |    0.274s | 21.428MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.10.smt2       |    0.276s | 23.22MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.11.smt2       |    0.279s | 24.356MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.13.smt2       |    0.439s | 25.0MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-15.smt2 |    0.456s | 25.012MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.12.smt2       |    0.460s | 23.92MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.8.smt2             |    0.485s | 21.876MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.14.smt2       |    0.551s | 25.324MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.15.smt2       |    0.625s | 26.18MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_2.smt2 |    0.747s | 31.528MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_4.smt2 |    0.748s | 31.624MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_3.smt2 |    0.846s | 31.552MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_1.smt2 |    0.859s | 31.628MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.15.smt2            |    0.876s | 26.028MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.16.smt2            |    0.927s | 26.432MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.9.smt2             |    0.958s | 22.736MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_7.smt2 |    1.019s | 34.936MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.17.smt2       |    1.052s | 27.872MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_6.smt2 |    1.057s | 34.876MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_3.smt2 |    1.090s | 34.84MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.18.smt2            |    1.135s | 28.22MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.18.smt2       |    1.474s | 30.792MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.20.smt2            |    1.698s | 30.088MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.10.smt2            |    1.716s | 23.22MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-20.smt2 |    1.733s | 29.892MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-15.smt2 |    1.739s | 26.572MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-10.smt2 |    2.015s | 23.48MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.11.smt2            |    2.059s | 23.68MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.16.smt2       |    2.159s | 28.736MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.19.smt2            |    2.345s | 28.716MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_6.smt2 |    2.583s | 46.884MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_7.smt2 |    2.729s | 46.88MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_5.smt2 |    3.544s | 60.272MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.17.smt2            |    3.647s | 28.976MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.22.smt2            |    4.051s | 32.18MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.21.smt2            |    4.733s | 33.552MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-1.i_0.smt2 |    4.899s | 45.672MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.12.smt2            |    5.741s | 25.54MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.13.smt2            |    6.573s | 25.588MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.20.smt2       |    8.556s | 36.864MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_1.smt2 |   10.078s | 66.136MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_2.smt2 |   10.417s | 66.096MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.14.smt2            |   10.679s | 27.028MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-20.smt2 |   10.768s | 34.032MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-15.smt2 |   13.056s | 27.34MiB| unsat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_7.smt2 |   13.144s | 351.0MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.21.smt2       |   16.603s | 42.468MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.19.smt2       |   17.380s | 40.82MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-011.c_AllErrorsAtOnce_Iteration2_0.smt2 |   19.114s | 26.076MiB| sat | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.23.smt2            |   20.013s | 36.036MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.24.smt2       |   20.014s | 39.912MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.26.smt2            |   20.014s | 37.416MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lamport.i_0.smt2 |   20.016s | 73.072MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_0.smt2 |   20.017s | 59.1MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-015.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.017s | 26.228MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.30.smt2       |   20.018s | 77.92MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-2.i_0.smt2 |   20.021s | 58.152MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_peterson.i_0.smt2 |   20.022s | 73.76MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_3.smt2 |   20.025s | 61.028MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_6.smt2 |   20.026s | 121.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_4.smt2 |   20.028s | 124.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.25.smt2       |   20.041s | 47.492MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-20.smt2 |   20.043s | 32.456MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/CostasArray-15.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.046s | 29.024MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-016.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.049s | 27.088MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-019.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.049s | 28.784MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.28.smt2       |   20.049s | 54.924MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.25.smt2            |   20.049s | 38.172MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.27.smt2       |   20.052s | 43.728MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.30.smt2            |   20.054s | 39.96MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_2.smt2 |   20.056s | 55.14MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_9.smt2 |   20.058s | 100.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_3.smt2 |   20.059s | 107.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_0.smt2 |   20.060s | 116.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-2.i_0.smt2 |   20.060s | 114.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_1.smt2 |   20.062s | 60.452MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-2.i_0.smt2 |   20.065s | 145.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_10.smt2 |   20.067s | 125.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_5.smt2 |   20.069s | 109.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.23.smt2       |   20.069s | 43.96MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.27.smt2            |   20.074s | 40.632MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_4.smt2 |   20.081s | 58.228MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_0.smt2 |   20.081s | 61.004MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_1.smt2 |   20.082s | 105.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-1.i_0.smt2 |   20.089s | 119.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_8.smt2 |   20.089s | 125.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.26.smt2       |   20.089s | 79.208MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.29.smt2       |   20.090s | 46.568MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_6.smt2 |   20.091s | 56.472MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.29.smt2            |   20.093s | 38.708MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_time_var_mutex.i_0.smt2 |   20.094s | 75.44MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_7.smt2 |   20.094s | 122.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-1.i_0.smt2 |   20.095s | 148.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_dekker.i_0.smt2 |   20.095s | 70.212MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.22.smt2       |   20.095s | 77.276MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.24.smt2            |   20.095s | 37.076MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_11.smt2 |   20.096s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.28.smt2            |   20.096s | 38.74MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_6.smt2 |   20.097s | 98.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-1.i_0.smt2 |   20.099s | 105.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-2.i_0.smt2 |   20.104s | 121.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_2.smt2 |   20.113s | 119.0MiB| timeout | 0 |  |  |
