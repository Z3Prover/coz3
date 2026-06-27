# .

* SAT 31
* UNSAT 24
* TIMEOUT 3
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFNRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFNRA.tar.zst-d
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFNRA.tar.zst?download=1
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
|non-incremental/QF_UFNRA/FFT/smtlib.640350.smt2              |    0.024s | 19.604MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631277.smt2              |    0.025s | 20.124MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630867.smt2              |    0.025s | 20.336MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modSimpleTest.smt2 |    0.026s | 20.68MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvInitial.smt2 |    0.031s | 20.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20190906-CLEARSY/0004/00003.smt2    |    0.039s | 20.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630949.smt2              |    0.041s | 20.08MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/z3.641736.smt2                  |    0.046s | 19.736MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631195.smt2              |    0.047s | 20.36MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.630785.smt2              |    0.047s | 20.132MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvStep.smt2 |    0.047s | 21.1MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631113.smt2              |    0.049s | 20.1MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/smtlib.631031.smt2              |    0.049s | 20.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/FFT/z3.630166.smt2                  |    0.050s | 19.912MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvVar1.smt2 |    0.058s | 20.948MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/l40m.smt2                       |    0.061s | 21.356MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStepFinala.smt2 |    0.062s | 21.172MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/l40f.smt2                       |    0.065s | 21.828MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStepFinal.smt2 |    0.071s | 21.004MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/m40.easy.smt2                   |    0.180s | 28.112MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/c40s.smt2                       |    0.201s | 29.54MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40s50.smt2                      |    0.233s | 30.628MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/c40f.smt2                       |    0.257s | 31.576MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep2.smt2 |    0.424s | 21.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/l40s.smt2                       |    0.451s | 41.04MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/c40m.smt2                       |    0.452s | 33.32MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40m25.smt2                      |    0.472s | 37.604MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40f25.smt2                      |    0.546s | 34.604MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40m50.smt2                      |    0.649s | 35.656MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep2a.smt2 |    0.737s | 21.376MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40f10.smt2                      |    0.765s | 38.212MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep4a.smt2 |    0.771s | 21.664MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40m99.smt2                      |    0.918s | 36.404MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40s10.smt2                      |    0.959s | 39.872MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40s25.smt2                      |    1.028s | 39.528MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40f50.smt2                      |    1.145s | 35.864MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep1.smt2 |    1.159s | 21.632MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/20u10.09.smt2                   |    1.191s | 31.42MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep3a.smt2 |    1.225s | 21.9MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40s99.smt2                      |    1.358s | 40.328MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40m10.smt2                      |    1.540s | 41.352MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40f99.smt2                      |    1.706s | 36.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep3.smt2 |    1.799s | 21.62MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep6.smt2 |    2.147s | 21.644MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep4.smt2 |    2.236s | 21.704MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep5.smt2 |    2.388s | 21.972MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/s40.smt2                        |    5.013s | 37.716MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep1a.smt2 |    5.340s | 21.64MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep6a.smt2 |    6.329s | 22.084MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep5a.smt2 |    7.483s | 22.7MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/20revert.u.smt2                 |    7.499s | 33.632MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/10u05.04.smt2                   |    9.059s | 25.852MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/40u20.19.smt2                   |    9.330s | 56.028MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/m40.smt2                        |   13.183s | 56.896MiB| sat | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep7.smt2 |   14.733s | 23.532MiB| unsat | 0 |  |  |
|non-incremental/QF_UFNRA/cas/30u15.14.smt2                   |   20.012s | 41.06MiB| timeout | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/sqrtStep7a.smt2 |   20.028s | 22.396MiB| timeout | 0 |  |  |
|non-incremental/QF_UFNRA/20230328-sqrtmodinv-hoenicke/modInvFull.smt2 |   20.121s | 939.0MiB| timeout | 0 |  |  |
