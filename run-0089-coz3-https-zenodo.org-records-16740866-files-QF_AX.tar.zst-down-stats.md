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
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_AX.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_AX.tar.zst-down
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_AX.tar.zst?download=1
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
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00002_001.cvc.smt2 |    0.021s | 19.628MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_003.cvc.smt2 |    0.023s | 20.1MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_009.cvc.smt2 |    0.023s | 20.28MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00004_007.cvc.smt2 |    0.023s | 20.6MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_004.cvc.smt2 |    0.024s | 20.108MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_002.cvc.smt2 |    0.024s | 19.884MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00010_003.cvc.smt2 |    0.024s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00020_003.cvc.smt2 |    0.024s | 20.16MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_nf_ai_00001_001.cvc.smt2 |    0.024s | 20.156MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00006_001.cvc.smt2 |    0.024s | 20.04MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00002_008.cvc.smt2 |    0.025s | 19.876MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_009.cvc.smt2 |    0.025s | 20.2MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_003.cvc.smt2 |    0.026s | 20.444MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00020_007.cvc.smt2 |    0.026s | 20.156MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00020_005.cvc.smt2 |    0.026s | 20.38MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00010_001.cvc.smt2 |    0.026s | 20.32MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_nf_ai_00006_001.cvc.smt2 |    0.026s | 20.4MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00010_006.cvc.smt2 |    0.027s | 20.392MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_009.cvc.smt2 |    0.027s | 20.112MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00006_001.cvc.smt2 |    0.028s | 20.156MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00030_009.cvc.smt2 |    0.028s | 20.636MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00010_004.cvc.smt2 |    0.028s | 20.236MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00007_001.cvc.smt2 |    0.028s | 20.592MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00020_004.cvc.smt2 |    0.029s | 20.42MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00030_003.cvc.smt2 |    0.029s | 20.612MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00009_001.cvc.smt2 |    0.030s | 20.628MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_sf_ai_00007_001.cvc.smt2 |    0.030s | 20.048MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00007_001.cvc.smt2 |    0.030s | 20.248MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00006_009.cvc.smt2 |    0.031s | 20.348MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00040_005.cvc.smt2 |    0.031s | 20.884MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_005.cvc.smt2 |    0.032s | 21.168MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00003_005.cvc.smt2 |    0.033s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00007_005.cvc.smt2 |    0.033s | 20.112MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00006_004.cvc.smt2 |    0.033s | 20.44MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00030_008.cvc.smt2 |    0.033s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00010_005.cvc.smt2 |    0.033s | 20.004MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00005_002.cvc.smt2 |    0.034s | 20.18MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_003.cvc.smt2 |    0.034s | 20.88MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_001.cvc.smt2 |    0.035s | 20.408MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_007.cvc.smt2 |    0.035s | 19.86MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_003.cvc.smt2 |    0.035s | 21.304MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_008.cvc.smt2 |    0.036s | 20.44MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00004_002.cvc.smt2 |    0.037s | 20.144MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_002.cvc.smt2 |    0.037s | 20.36MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00004_004.cvc.smt2 |    0.037s | 20.04MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00010_004.cvc.smt2 |    0.037s | 20.46MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_002.cvc.smt2 |    0.037s | 20.132MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_001.cvc.smt2 |    0.037s | 21.364MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_001.cvc.smt2 |    0.038s | 20.116MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_008.cvc.smt2 |    0.039s | 20.044MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_nf_ai_00002_001.cvc.smt2 |    0.039s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00004_002.cvc.smt2 |    0.040s | 20.156MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00003_001.cvc.smt2 |    0.040s | 19.888MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_005.cvc.smt2 |    0.040s | 20.092MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00002_001.cvc.smt2 |    0.040s | 19.952MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00007_007.cvc.smt2 |    0.041s | 20.116MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00006_006.cvc.smt2 |    0.041s | 20.152MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_001.cvc.smt2 |    0.041s | 21.616MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00002_001.cvc.smt2 |    0.041s | 20.148MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_009.cvc.smt2 |    0.042s | 20.6MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_002.cvc.smt2 |    0.042s | 20.608MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_sf_ai_00004_001.cvc.smt2 |    0.042s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00060_008.cvc.smt2 |    0.043s | 22.38MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_sf_ai_00001_001.cvc.smt2 |    0.043s | 20.384MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00003_005.cvc.smt2 |    0.044s | 20.004MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00007_009.cvc.smt2 |    0.044s | 20.356MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/cvc/read5.smt2                         |    0.044s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_007.cvc.smt2 |    0.044s | 20.164MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_nf_ai_00004_001.cvc.smt2 |    0.044s | 20.36MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00004_001.cvc.smt2 |    0.044s | 20.152MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00009_001.cvc.smt2 |    0.044s | 20.164MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_nf_ai_00003_001.cvc.smt2 |    0.044s | 19.96MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00006_003.cvc.smt2 |    0.045s | 20.12MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_008.cvc.smt2 |    0.045s | 19.868MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00002_009.cvc.smt2 |    0.045s | 20.016MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00050_004.cvc.smt2 |    0.045s | 21.8MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00050_003.cvc.smt2 |    0.045s | 21.616MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00010_001.cvc.smt2 |    0.045s | 20.26MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_006.cvc.smt2 |    0.046s | 20.124MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00005_003.cvc.smt2 |    0.046s | 19.896MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_003.cvc.smt2 |    0.046s | 20.668MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00002_005.cvc.smt2 |    0.046s | 19.888MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_001.cvc.smt2 |    0.046s | 20.36MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_009.cvc.smt2 |    0.046s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00010_002.cvc.smt2 |    0.046s | 20.128MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00030_006.cvc.smt2 |    0.046s | 20.632MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00008_001.cvc.smt2 |    0.046s | 20.388MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00006_001.cvc.smt2 |    0.046s | 20.264MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00007_006.cvc.smt2 |    0.047s | 20.144MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_008.cvc.smt2 |    0.047s | 20.136MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_005.cvc.smt2 |    0.047s | 20.48MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_001.cvc.smt2 |    0.047s | 20.536MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_005.cvc.smt2 |    0.047s | 20.868MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00005_001.cvc.smt2 |    0.047s | 20.412MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_009.cvc.smt2 |    0.048s | 20.152MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_003.cvc.smt2 |    0.048s | 20.416MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t2_np_nf_ai_00007_001.cvc.smt2 |    0.048s | 20.252MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_sf_ai_00005_001.cvc.smt2 |    0.048s | 20.108MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00003_005.cvc.smt2 |    0.049s | 20.0MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00002_005.cvc.smt2 |    0.049s | 19.86MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_008.cvc.smt2 |    0.049s | 20.104MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00005_001.cvc.smt2 |    0.049s | 20.044MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_nf_ai_00008_001.cvc.smt2 |    0.049s | 20.384MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00007_005.cvc.smt2 |    0.050s | 20.1MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00007_009.cvc.smt2 |    0.050s | 20.092MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00002_008.cvc.smt2 |    0.050s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00009_005.cvc.smt2 |    0.050s | 20.224MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00003_008.cvc.smt2 |    0.050s | 20.352MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00005_001.cvc.smt2 |    0.050s | 19.928MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00030_002.cvc.smt2 |    0.050s | 20.812MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_006.cvc.smt2 |    0.050s | 20.608MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_sf_ai_00009_001.cvc.smt2 |    0.050s | 20.4MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_001.cvc.smt2 |    0.051s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_002.cvc.smt2 |    0.051s | 20.4MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_006.cvc.smt2 |    0.051s | 20.632MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_009.cvc.smt2 |    0.051s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_007.cvc.smt2 |    0.051s | 20.232MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_008.cvc.smt2 |    0.051s | 20.224MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00004_005.cvc.smt2 |    0.051s | 20.12MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00010_006.cvc.smt2 |    0.051s | 20.608MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00040_001.cvc.smt2 |    0.051s | 20.868MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_009.cvc.smt2 |    0.052s | 20.396MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00040_001.cvc.smt2 |    0.052s | 21.208MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00003_001.cvc.smt2 |    0.052s | 20.108MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_sf_ai_00005_001.cvc.smt2 |    0.052s | 20.132MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00005_009.cvc.smt2 |    0.053s | 20.012MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_009.cvc.smt2 |    0.053s | 19.94MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_004.cvc.smt2 |    0.053s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_006.cvc.smt2 |    0.053s | 20.5MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00006_001.cvc.smt2 |    0.053s | 20.324MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00007_004.cvc.smt2 |    0.054s | 20.176MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00006_008.cvc.smt2 |    0.054s | 20.34MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_004.cvc.smt2 |    0.054s | 20.332MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00007_007.cvc.smt2 |    0.054s | 20.152MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00004_009.cvc.smt2 |    0.054s | 20.116MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00007_009.cvc.smt2 |    0.054s | 20.26MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00005_006.cvc.smt2 |    0.054s | 19.96MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00050_006.cvc.smt2 |    0.054s | 21.64MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_003.cvc.smt2 |    0.055s | 20.072MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_004.cvc.smt2 |    0.055s | 20.336MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_002.cvc.smt2 |    0.055s | 21.648MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00030_002.cvc.smt2 |    0.055s | 20.8MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00040_003.cvc.smt2 |    0.055s | 21.172MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t1_np_nf_ai_00002_001.cvc.smt2 |    0.055s | 20.096MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_sf_ai_00003_001.cvc.smt2 |    0.055s | 20.296MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00003_004.cvc.smt2 |    0.056s | 20.092MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00007_001.cvc.smt2 |    0.056s | 20.216MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_004.cvc.smt2 |    0.056s | 20.148MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00004_001.cvc.smt2 |    0.056s | 19.916MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00040_006.cvc.smt2 |    0.056s | 20.852MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_sf_ai_00010_001.cvc.smt2 |    0.056s | 20.632MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00002_006.cvc.smt2 |    0.057s | 20.084MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00002_002.cvc.smt2 |    0.057s | 19.924MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00006_001.cvc.smt2 |    0.057s | 20.152MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00002_006.cvc.smt2 |    0.057s | 19.892MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_002.cvc.smt2 |    0.057s | 20.192MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_009.cvc.smt2 |    0.057s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00030_008.cvc.smt2 |    0.057s | 20.508MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00010_009.cvc.smt2 |    0.057s | 20.06MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_004.cvc.smt2 |    0.058s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_009.cvc.smt2 |    0.058s | 20.268MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00003_006.cvc.smt2 |    0.058s | 20.048MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00004_009.cvc.smt2 |    0.058s | 20.148MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_007.cvc.smt2 |    0.058s | 20.16MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_001.cvc.smt2 |    0.058s | 20.312MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_007.cvc.smt2 |    0.059s | 20.276MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00004_003.cvc.smt2 |    0.059s | 20.016MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00005_007.cvc.smt2 |    0.059s | 20.104MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00007_006.cvc.smt2 |    0.059s | 20.644MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00008_002.cvc.smt2 |    0.059s | 20.608MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00004_007.cvc.smt2 |    0.059s | 19.92MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00005_003.cvc.smt2 |    0.059s | 20.016MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_003.cvc.smt2 |    0.059s | 19.404MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00030_002.cvc.smt2 |    0.059s | 20.628MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00010_001.cvc.smt2 |    0.059s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_002.cvc.smt2 |    0.059s | 22.444MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t1_np_nf_ai_00009_001.cvc.smt2 |    0.059s | 20.148MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00004_008.cvc.smt2 |    0.060s | 20.16MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00004_001.cvc.smt2 |    0.060s | 20.196MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00005_001.cvc.smt2 |    0.060s | 19.656MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00003_002.cvc.smt2 |    0.060s | 20.328MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_005.cvc.smt2 |    0.060s | 20.116MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_001.cvc.smt2 |    0.060s | 19.904MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_009.cvc.smt2 |    0.060s | 20.344MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00006_005.cvc.smt2 |    0.060s | 20.492MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00003_003.cvc.smt2 |    0.060s | 19.656MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_001.cvc.smt2 |    0.060s | 20.232MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_007.cvc.smt2 |    0.060s | 20.108MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00020_006.cvc.smt2 |    0.060s | 20.38MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00010_006.cvc.smt2 |    0.060s | 20.384MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_005.cvc.smt2 |    0.061s | 19.992MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00006_003.cvc.smt2 |    0.061s | 20.168MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_008.cvc.smt2 |    0.061s | 20.324MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00004_004.cvc.smt2 |    0.061s | 19.924MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_001.cvc.smt2 |    0.061s | 20.896MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_009.cvc.smt2 |    0.061s | 19.9MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_003.cvc.smt2 |    0.061s | 19.9MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_007.cvc.smt2 |    0.061s | 20.056MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_003.cvc.smt2 |    0.061s | 20.5MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00007_007.cvc.smt2 |    0.061s | 20.22MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00004_006.cvc.smt2 |    0.061s | 20.012MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00020_005.cvc.smt2 |    0.061s | 20.584MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00020_007.cvc.smt2 |    0.061s | 20.232MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00030_004.cvc.smt2 |    0.061s | 20.392MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00030_007.cvc.smt2 |    0.061s | 20.42MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_002.cvc.smt2 |    0.062s | 20.164MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_008.cvc.smt2 |    0.062s | 20.304MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_001.cvc.smt2 |    0.062s | 20.084MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00010_005.cvc.smt2 |    0.062s | 20.716MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_002.cvc.smt2 |    0.062s | 20.156MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00006_004.cvc.smt2 |    0.062s | 19.912MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_007.cvc.smt2 |    0.062s | 20.188MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_007.cvc.smt2 |    0.062s | 20.616MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00020_009.cvc.smt2 |    0.062s | 20.28MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00008_001.cvc.smt2 |    0.062s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_001.cvc.smt2 |    0.063s | 20.328MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00006_009.cvc.smt2 |    0.063s | 20.388MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_008.cvc.smt2 |    0.063s | 20.328MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00007_007.cvc.smt2 |    0.063s | 20.148MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00040_006.cvc.smt2 |    0.063s | 21.3MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_sf_ai_00008_001.cvc.smt2 |    0.063s | 20.416MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_001.cvc.smt2 |    0.064s | 20.356MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_001.cvc.smt2 |    0.064s | 20.772MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00009_002.cvc.smt2 |    0.064s | 20.34MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00002_002.cvc.smt2 |    0.064s | 20.056MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00005_007.cvc.smt2 |    0.064s | 20.092MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00008_005.cvc.smt2 |    0.064s | 20.192MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00010_003.cvc.smt2 |    0.064s | 20.14MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00060_004.cvc.smt2 |    0.064s | 21.676MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00040_007.cvc.smt2 |    0.064s | 20.92MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00020_004.cvc.smt2 |    0.064s | 20.14MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00004_004.cvc.smt2 |    0.065s | 20.084MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_006.cvc.smt2 |    0.065s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00005_007.cvc.smt2 |    0.065s | 20.42MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00020_005.cvc.smt2 |    0.065s | 20.088MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_002.cvc.smt2 |    0.066s | 19.984MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00008_001.cvc.smt2 |    0.066s | 20.668MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00007_005.cvc.smt2 |    0.066s | 20.164MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00040_002.cvc.smt2 |    0.066s | 20.9MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00020_002.cvc.smt2 |    0.066s | 20.42MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00009_003.cvc.smt2 |    0.067s | 20.672MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_008.cvc.smt2 |    0.067s | 20.432MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00009_008.cvc.smt2 |    0.067s | 20.424MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_002.cvc.smt2 |    0.067s | 20.624MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00010_009.cvc.smt2 |    0.067s | 20.14MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_004.cvc.smt2 |    0.067s | 20.136MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00004_005.cvc.smt2 |    0.068s | 19.944MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00009_005.cvc.smt2 |    0.068s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00004_006.cvc.smt2 |    0.068s | 20.168MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_009.cvc.smt2 |    0.068s | 20.912MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00002_005.cvc.smt2 |    0.068s | 19.948MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00007_003.cvc.smt2 |    0.068s | 20.288MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00010_005.cvc.smt2 |    0.068s | 20.612MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_003.cvc.smt2 |    0.068s | 19.596MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00003_006.cvc.smt2 |    0.068s | 19.928MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00030_001.cvc.smt2 |    0.068s | 20.56MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_006.cvc.smt2 |    0.068s | 21.076MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00030_009.cvc.smt2 |    0.068s | 20.56MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_008.cvc.smt2 |    0.068s | 20.416MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_t3_np_nf_ai_00010_001.cvc.smt2 |    0.068s | 20.488MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_007.cvc.smt2 |    0.069s | 19.816MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_003.cvc.smt2 |    0.069s | 20.432MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_003.cvc.smt2 |    0.069s | 20.136MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00010_006.cvc.smt2 |    0.069s | 20.18MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00009_006.cvc.smt2 |    0.069s | 20.472MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_006.cvc.smt2 |    0.069s | 20.272MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00008_005.cvc.smt2 |    0.069s | 20.624MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00030_005.cvc.smt2 |    0.069s | 20.724MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00020_003.cvc.smt2 |    0.069s | 20.44MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00040_003.cvc.smt2 |    0.069s | 20.948MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00060_003.cvc.smt2 |    0.070s | 22.356MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_004.cvc.smt2 |    0.070s | 21.152MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_002.cvc.smt2 |    0.070s | 20.64MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_sf_ai_00003_001.cvc.smt2 |    0.070s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00004_001.cvc.smt2 |    0.071s | 19.956MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00007_001.cvc.smt2 |    0.071s | 20.164MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00007_002.cvc.smt2 |    0.071s | 20.296MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00003_008.cvc.smt2 |    0.071s | 20.0MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_002.cvc.smt2 |    0.071s | 21.208MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_008.cvc.smt2 |    0.071s | 21.636MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00020_008.cvc.smt2 |    0.071s | 20.492MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00004_008.cvc.smt2 |    0.072s | 20.336MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_003.cvc.smt2 |    0.072s | 20.088MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_006.cvc.smt2 |    0.072s | 19.896MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00040_006.cvc.smt2 |    0.072s | 21.38MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00060_002.cvc.smt2 |    0.072s | 21.884MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00010_008.cvc.smt2 |    0.072s | 20.332MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_004.cvc.smt2 |    0.073s | 20.012MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_002.cvc.smt2 |    0.073s | 20.16MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_002.cvc.smt2 |    0.073s | 20.164MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00008_004.cvc.smt2 |    0.073s | 20.176MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00008_007.cvc.smt2 |    0.073s | 20.348MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_004.cvc.smt2 |    0.073s | 20.02MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00002_006.cvc.smt2 |    0.073s | 19.812MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_005.cvc.smt2 |    0.073s | 20.356MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00040_009.cvc.smt2 |    0.073s | 20.84MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00050_009.cvc.smt2 |    0.073s | 21.168MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00008_002.cvc.smt2 |    0.074s | 20.348MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00002_003.cvc.smt2 |    0.074s | 19.924MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00003_008.cvc.smt2 |    0.074s | 19.892MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00030_006.cvc.smt2 |    0.074s | 20.632MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00050_009.cvc.smt2 |    0.074s | 21.128MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00004_003.cvc.smt2 |    0.075s | 19.896MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00007_008.cvc.smt2 |    0.075s | 20.372MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00050_004.cvc.smt2 |    0.075s | 21.832MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00020_001.cvc.smt2 |    0.075s | 20.952MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00020_006.cvc.smt2 |    0.075s | 20.112MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00006_006.cvc.smt2 |    0.076s | 19.992MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00003_001.cvc.smt2 |    0.076s | 20.108MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00050_008.cvc.smt2 |    0.076s | 21.688MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00020_002.cvc.smt2 |    0.076s | 20.564MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00050_009.cvc.smt2 |    0.076s | 21.64MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00003_005.cvc.smt2 |    0.077s | 20.064MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00007_001.cvc.smt2 |    0.077s | 20.292MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00005_008.cvc.smt2 |    0.077s | 20.02MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00008_001.cvc.smt2 |    0.077s | 20.664MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00040_009.cvc.smt2 |    0.077s | 21.156MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00060_001.cvc.smt2 |    0.077s | 21.6MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_008.cvc.smt2 |    0.077s | 22.4MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00010_003.cvc.smt2 |    0.077s | 19.956MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_004.cvc.smt2 |    0.078s | 20.644MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00050_001.cvc.smt2 |    0.078s | 21.296MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00050_005.cvc.smt2 |    0.078s | 21.768MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00060_009.cvc.smt2 |    0.078s | 22.824MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_002.cvc.smt2 |    0.078s | 21.656MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_004.cvc.smt2 |    0.078s | 20.584MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00002_009.cvc.smt2 |    0.079s | 19.964MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00002_002.cvc.smt2 |    0.079s | 19.908MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00003_007.cvc.smt2 |    0.079s | 20.048MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00010_005.cvc.smt2 |    0.079s | 20.156MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00040_007.cvc.smt2 |    0.079s | 20.976MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00060_007.cvc.smt2 |    0.079s | 22.216MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00060_009.cvc.smt2 |    0.079s | 21.812MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00010_004.cvc.smt2 |    0.079s | 20.024MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_003.cvc.smt2 |    0.080s | 20.18MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00003_004.cvc.smt2 |    0.080s | 19.916MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00009_009.cvc.smt2 |    0.080s | 20.796MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00010_004.cvc.smt2 |    0.080s | 20.396MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_002.cvc.smt2 |    0.080s | 20.172MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_005.cvc.smt2 |    0.080s | 21.648MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00030_007.cvc.smt2 |    0.080s | 20.848MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00020_004.cvc.smt2 |    0.080s | 20.836MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00009_003.cvc.smt2 |    0.081s | 20.616MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00040_007.cvc.smt2 |    0.081s | 21.652MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00030_008.cvc.smt2 |    0.081s | 20.62MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_005.cvc.smt2 |    0.081s | 20.64MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storeinv/storeinv_invalid_t3_np_nf_ai_00004_001.cvc.smt2 |    0.081s | 19.912MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00002_001.cvc.smt2 |    0.082s | 19.864MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00030_009.cvc.smt2 |    0.082s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00050_001.cvc.smt2 |    0.082s | 21.636MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_004.cvc.smt2 |    0.083s | 20.1MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00009_007.cvc.smt2 |    0.083s | 20.42MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00006_005.cvc.smt2 |    0.083s | 20.132MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00008_003.cvc.smt2 |    0.083s | 19.62MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00005_006.cvc.smt2 |    0.083s | 20.192MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_006.cvc.smt2 |    0.083s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00008_007.cvc.smt2 |    0.083s | 20.12MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00020_009.cvc.smt2 |    0.083s | 20.56MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00050_004.cvc.smt2 |    0.083s | 21.148MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_001.cvc.smt2 |    0.083s | 20.348MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00005_009.cvc.smt2 |    0.084s | 20.104MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00002_002.cvc.smt2 |    0.084s | 19.992MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00009_001.cvc.smt2 |    0.084s | 20.52MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_006.cvc.smt2 |    0.084s | 20.124MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00030_001.cvc.smt2 |    0.084s | 20.88MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_001.cvc.smt2 |    0.084s | 20.936MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00020_007.cvc.smt2 |    0.084s | 20.404MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_003.cvc.smt2 |    0.084s | 22.468MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_007.cvc.smt2 |    0.084s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00050_005.cvc.smt2 |    0.084s | 21.156MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00003_004.cvc.smt2 |    0.085s | 19.956MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_004.cvc.smt2 |    0.085s | 20.248MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_008.cvc.smt2 |    0.085s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00020_006.cvc.smt2 |    0.085s | 20.608MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00060_005.cvc.smt2 |    0.085s | 22.428MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00060_005.cvc.smt2 |    0.085s | 22.412MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00010_002.cvc.smt2 |    0.085s | 20.296MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00009_002.cvc.smt2 |    0.086s | 20.456MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00007_005.cvc.smt2 |    0.086s | 20.16MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00010_005.cvc.smt2 |    0.086s | 20.452MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_008.cvc.smt2 |    0.086s | 20.116MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_004.cvc.smt2 |    0.086s | 21.104MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00020_002.cvc.smt2 |    0.086s | 20.144MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_009.cvc.smt2 |    0.086s | 21.124MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00002_005.cvc.smt2 |    0.087s | 20.12MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_005.cvc.smt2 |    0.087s | 20.396MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_008.cvc.smt2 |    0.087s | 19.84MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00050_003.cvc.smt2 |    0.087s | 21.632MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00050_004.cvc.smt2 |    0.087s | 21.14MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00010_003.cvc.smt2 |    0.087s | 20.032MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_001.cvc.smt2 |    0.087s | 21.12MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00030_007.cvc.smt2 |    0.087s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00007_004.cvc.smt2 |    0.088s | 20.12MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00007_008.cvc.smt2 |    0.088s | 20.152MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_006.cvc.smt2 |    0.088s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00008_008.cvc.smt2 |    0.088s | 20.54MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_002.cvc.smt2 |    0.088s | 20.42MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_004.cvc.smt2 |    0.088s | 21.268MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00050_002.cvc.smt2 |    0.088s | 21.364MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_007.cvc.smt2 |    0.089s | 20.436MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00007_006.cvc.smt2 |    0.089s | 20.292MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00006_002.cvc.smt2 |    0.089s | 20.156MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_008.cvc.smt2 |    0.089s | 21.356MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00006_002.cvc.smt2 |    0.090s | 20.42MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00008_001.cvc.smt2 |    0.090s | 20.4MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_008.cvc.smt2 |    0.090s | 21.676MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00010_008.cvc.smt2 |    0.090s | 20.124MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00006_007.cvc.smt2 |    0.091s | 20.108MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00003_006.cvc.smt2 |    0.091s | 19.876MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00006_002.cvc.smt2 |    0.091s | 20.228MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00060_001.cvc.smt2 |    0.091s | 21.716MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00005_002.cvc.smt2 |    0.092s | 20.036MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_005.cvc.smt2 |    0.092s | 20.204MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_006.cvc.smt2 |    0.092s | 20.296MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_008.cvc.smt2 |    0.092s | 20.864MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00005_004.cvc.smt2 |    0.093s | 19.988MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00005_001.cvc.smt2 |    0.093s | 19.664MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_007.cvc.smt2 |    0.093s | 20.448MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00009_008.cvc.smt2 |    0.093s | 20.508MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00009_007.cvc.smt2 |    0.093s | 20.36MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00003_002.cvc.smt2 |    0.093s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00005_004.cvc.smt2 |    0.093s | 20.084MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00050_005.cvc.smt2 |    0.093s | 21.176MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00050_006.cvc.smt2 |    0.093s | 21.18MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00030_004.cvc.smt2 |    0.093s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00004_006.cvc.smt2 |    0.094s | 20.048MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00002_006.cvc.smt2 |    0.094s | 19.824MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00009_004.cvc.smt2 |    0.094s | 20.684MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00060_006.cvc.smt2 |    0.094s | 22.404MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00010_009.cvc.smt2 |    0.094s | 20.108MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00010_009.cvc.smt2 |    0.095s | 20.6MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00006_002.cvc.smt2 |    0.095s | 20.144MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_002.cvc.smt2 |    0.095s | 20.284MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00007_003.cvc.smt2 |    0.095s | 20.3MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00040_008.cvc.smt2 |    0.095s | 21.052MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00040_003.cvc.smt2 |    0.095s | 21.356MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00008_008.cvc.smt2 |    0.096s | 20.612MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_009.cvc.smt2 |    0.096s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00050_009.cvc.smt2 |    0.096s | 21.928MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00060_005.cvc.smt2 |    0.096s | 21.732MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00010_008.cvc.smt2 |    0.097s | 20.324MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00009_005.cvc.smt2 |    0.097s | 20.612MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00040_004.cvc.smt2 |    0.097s | 20.928MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_005.cvc.smt2 |    0.097s | 21.244MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00006_007.cvc.smt2 |    0.098s | 20.496MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00005_005.cvc.smt2 |    0.098s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00020_008.cvc.smt2 |    0.098s | 20.372MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00020_003.cvc.smt2 |    0.098s | 20.5MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_006.cvc.smt2 |    0.099s | 20.444MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_001.cvc.smt2 |    0.099s | 22.356MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00020_001.cvc.smt2 |    0.099s | 20.496MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00050_003.cvc.smt2 |    0.099s | 21.356MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_004.cvc.smt2 |    0.100s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_002.cvc.smt2 |    0.100s | 21.596MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00050_007.cvc.smt2 |    0.100s | 21.888MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_009.cvc.smt2 |    0.100s | 20.952MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_004.cvc.smt2 |    0.101s | 22.34MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00050_007.cvc.smt2 |    0.102s | 21.592MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00010_001.cvc.smt2 |    0.102s | 20.1MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00007_002.cvc.smt2 |    0.103s | 20.336MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00040_005.cvc.smt2 |    0.103s | 21.02MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00020_001.cvc.smt2 |    0.103s | 20.384MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00010_008.cvc.smt2 |    0.103s | 20.38MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00040_007.cvc.smt2 |    0.103s | 21.368MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00007_008.cvc.smt2 |    0.104s | 20.192MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00030_003.cvc.smt2 |    0.104s | 20.78MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00040_004.cvc.smt2 |    0.104s | 21.212MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00050_002.cvc.smt2 |    0.104s | 22.112MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_005.cvc.smt2 |    0.105s | 20.108MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00010_005.cvc.smt2 |    0.106s | 20.696MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00060_009.cvc.smt2 |    0.107s | 22.708MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00010_008.cvc.smt2 |    0.109s | 20.604MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_007.cvc.smt2 |    0.110s | 21.636MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00004_006.cvc.smt2 |    0.111s | 20.108MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00008_006.cvc.smt2 |    0.112s | 20.636MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_005.cvc.smt2 |    0.114s | 20.296MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00060_007.cvc.smt2 |    0.114s | 22.58MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00060_004.cvc.smt2 |    0.114s | 21.736MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00050_007.cvc.smt2 |    0.115s | 21.192MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00004_001.cvc.smt2 |    0.118s | 19.884MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00009_003.cvc.smt2 |    0.118s | 20.552MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_nf_ai_00060_006.cvc.smt2 |    0.118s | 21.72MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00010_004.cvc.smt2 |    0.119s | 20.608MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00060_003.cvc.smt2 |    0.119s | 21.844MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00008_008.cvc.smt2 |    0.121s | 20.452MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_nf_ai_00010_006.cvc.smt2 |    0.121s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00010_002.cvc.smt2 |    0.122s | 20.52MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00003_006.cvc.smt2 |    0.122s | 20.312MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00003_001.cvc.smt2 |    0.123s | 19.904MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_sf_ai_00005_004.cvc.smt2 |    0.123s | 20.116MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00010_007.cvc.smt2 |    0.123s | 20.856MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00010_007.cvc.smt2 |    0.123s | 20.68MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00010_003.cvc.smt2 |    0.124s | 20.86MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00002_009.cvc.smt2 |    0.124s | 20.344MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_008.cvc.smt2 |    0.124s | 20.528MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00010_005.cvc.smt2 |    0.124s | 20.092MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00010_006.cvc.smt2 |    0.126s | 20.536MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00008_007.cvc.smt2 |    0.126s | 20.268MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00008_009.cvc.smt2 |    0.126s | 20.592MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00005_007.cvc.smt2 |    0.126s | 20.112MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00020_008.cvc.smt2 |    0.126s | 20.732MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00020_003.cvc.smt2 |    0.126s | 20.76MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00030_005.cvc.smt2 |    0.126s | 20.612MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00020_009.cvc.smt2 |    0.126s | 20.52MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_sf_ai_00020_006.cvc.smt2 |    0.126s | 20.48MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00020_002.cvc.smt2 |    0.126s | 20.58MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00007_003.cvc.smt2 |    0.127s | 20.152MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00030_004.cvc.smt2 |    0.127s | 20.66MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00040_003.cvc.smt2 |    0.127s | 21.136MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00030_005.cvc.smt2 |    0.127s | 20.656MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00030_007.cvc.smt2 |    0.127s | 21.128MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00008_008.cvc.smt2 |    0.128s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00030_008.cvc.smt2 |    0.128s | 20.76MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_001.cvc.smt2 |    0.129s | 21.18MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_sf_ai_00006_005.cvc.smt2 |    0.130s | 20.24MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t1_np_nf_ai_00008_005.cvc.smt2 |    0.130s | 20.272MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_nf_ai_00030_006.cvc.smt2 |    0.130s | 20.672MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00040_002.cvc.smt2 |    0.130s | 20.848MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00003_007.cvc.smt2 |    0.131s | 19.908MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_invalid_t3_np_nf_ai_00009_007.cvc.smt2 |    0.131s | 20.244MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t2_np_nf_ai_00040_008.cvc.smt2 |    0.131s | 21.108MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00050_006.cvc.smt2 |    0.133s | 21.372MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_nf_ai_00050_008.cvc.smt2 |    0.135s | 21.176MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00050_006.cvc.smt2 |    0.138s | 21.724MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00060_008.cvc.smt2 |    0.138s | 21.752MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t1_np_sf_ai_00060_005.cvc.smt2 |    0.139s | 21.712MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00010_008.cvc.smt2 |    0.140s | 20.716MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t3_np_sf_ai_00060_008.cvc.smt2 |    0.140s | 21.9MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_sf_ai_00060_009.cvc.smt2 |    0.141s | 21.728MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_t2_np_nf_ai_00060_003.cvc.smt2 |    0.142s | 21.86MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_001.cvc.smt2 |    0.144s | 20.444MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t1_np_sf_ai_00060_001.cvc.smt2 |    0.144s | 22.904MiB| sat | 0 |  |  |
|non-incremental/QF_AX/storecomm/storecomm_invalid_t3_np_sf_ai_00060_004.cvc.smt2 |    0.144s | 22.448MiB| sat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00009_009.cvc.smt2 |    0.157s | 20.764MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00009_006.cvc.smt2 |    0.159s | 20.908MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00008_009.cvc.smt2 |    0.159s | 20.532MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00009_003.cvc.smt2 |    0.162s | 20.696MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00009_006.cvc.smt2 |    0.179s | 21.124MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_sf_ai_00010_004.cvc.smt2 |    0.180s | 20.836MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00009_004.cvc.smt2 |    0.191s | 21.444MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00010_003.cvc.smt2 |    0.222s | 20.964MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00009_004.cvc.smt2 |    0.228s | 21.472MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00010_001.cvc.smt2 |    0.239s | 21.476MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_sf_ai_00010_003.cvc.smt2 |    0.249s | 21.328MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00010_007.cvc.smt2 |    0.258s | 21.332MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_nf_ai_00010_007.cvc.smt2 |    0.284s | 21.112MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00010_001.cvc.smt2 |    0.412s | 21.716MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t1_np_nf_ai_00010_009.cvc.smt2 |    0.433s | 22.18MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00010_009.cvc.smt2 |    0.536s | 22.036MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t3_np_sf_ai_00010_002.cvc.smt2 |    1.681s | 23.256MiB| unsat | 0 |  |  |
|non-incremental/QF_AX/swap/swap_t2_np_nf_ai_00010_002.cvc.smt2 |    2.345s | 23.536MiB| unsat | 0 |  |  |
