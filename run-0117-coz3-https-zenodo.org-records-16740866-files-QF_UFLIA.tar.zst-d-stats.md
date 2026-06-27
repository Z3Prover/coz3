# .

* SAT 424
* UNSAT 187
* TIMEOUT 48
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFLIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFLIA.tar.zst-d
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFLIA.tar.zst?download=1
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
|non-incremental/QF_UFLIA/TwoSquares/smtlib.602059.smt2       |    0.021s | 19.9MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_13.smt2    |    0.022s | 19.776MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_19.smt2    |    0.022s | 19.724MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_04.smt2    |    0.023s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.604654.smt2       |    0.024s | 19.904MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_15.smt2    |    0.024s | 19.864MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_20.smt2    |    0.024s | 19.82MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.606691.smt2       |    0.025s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.639533.smt2       |    0.025s | 20.068MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_10.smt2    |    0.025s | 20.392MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_20.smt2    |    0.025s | 19.604MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_15.smt2    |    0.026s | 19.82MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.621344.smt2       |    0.027s | 20.384MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_19.smt2    |    0.027s | 20.54MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_03.smt2    |    0.028s | 20.672MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_07.smt2    |    0.028s | 20.904MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_08.smt2    |    0.028s | 19.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-19-1-4-2-1.smt2  |    0.028s | 20.388MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_08.smt2    |    0.031s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_11.smt2    |    0.032s | 20.752MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_05.smt2    |    0.032s | 20.624MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-08-3-4-3-5.smt2  |    0.032s | 20.816MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium14.smt2 |    0.032s | 20.748MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_11.smt2    |    0.034s | 19.64MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_17.smt2    |    0.034s | 20.776MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-20-5-1-5-1.smt2  |    0.034s | 20.504MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_12.smt2    |    0.035s | 19.784MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-18-3-4-5-4.smt2  |    0.035s | 21.164MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_15.smt2    |    0.036s | 20.8MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_14.smt2    |    0.036s | 19.84MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_08.smt2    |    0.037s | 21.564MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_07.smt2    |    0.037s | 19.82MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_16.smt2    |    0.037s | 19.66MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_31_51.smt2                 |    0.038s | 21.884MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_04.smt2    |    0.038s | 20.668MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_05.smt2    |    0.038s | 20.66MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_04.smt2    |    0.038s | 20.312MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-20-1-1-5-2.smt2  |    0.038s | 20.812MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-06-4-4-1-2.smt2  |    0.038s | 20.976MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_15.smt2    |    0.039s | 20.892MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-15-4-1-3-1.smt2  |    0.039s | 20.992MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-12-4-4-1-3.smt2  |    0.039s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_05.smt2    |    0.040s | 20.028MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_11.smt2    |    0.040s | 20.688MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-10-2-5-3-2.smt2  |    0.040s | 20.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_17.smt2    |    0.041s | 21.292MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_15.smt2    |    0.041s | 19.612MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_13.smt2    |    0.041s | 20.656MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-06-1-5-3-2.smt2  |    0.041s | 20.716MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-17-4-2-1-5.smt2  |    0.041s | 20.924MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_14.smt2    |    0.042s | 20.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_08.smt2    |    0.042s | 20.776MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_12.smt2    |    0.042s | 20.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_05.smt2    |    0.042s | 20.608MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_14.smt2    |    0.043s | 21.576MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_16.smt2    |    0.043s | 20.34MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_05.smt2    |    0.043s | 19.652MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-08-5-3-2-5.smt2  |    0.043s | 21.368MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-07-4-1-1-5.smt2  |    0.043s | 20.852MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-16-4-4-1-4.smt2  |    0.043s | 20.996MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_03.smt2    |    0.044s | 19.676MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_04.smt2    |    0.044s | 20.984MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_18.smt2    |    0.044s | 20.216MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_09.smt2    |    0.044s | 20.472MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_16.smt2    |    0.044s | 21.324MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_03.smt2    |    0.045s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_13.smt2    |    0.045s | 19.804MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-20-2-5-1-4.smt2  |    0.045s | 20.616MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-13-2-1-3-1.smt2  |    0.045s | 21.1MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-05-5-4-1-5.smt2  |    0.046s | 20.868MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.666105.smt2       |    0.047s | 20.352MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_08.smt2    |    0.047s | 20.416MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_06.smt2    |    0.047s | 20.672MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_17.smt2    |    0.047s | 20.328MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_10.smt2    |    0.048s | 19.704MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_18.smt2    |    0.048s | 20.404MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_20.smt2    |    0.048s | 20.396MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-18-1-1-4-1.smt2  |    0.048s | 20.912MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-09-3-2-3-2.smt2  |    0.049s | 20.872MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-12-2-5-3-4.smt2  |    0.049s | 21.12MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-05-2-1-2-3.smt2  |    0.049s | 21.16MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-07-1-2-1-1.smt2  |    0.049s | 21.12MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_11.smt2    |    0.051s | 20.344MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_14.smt2    |    0.051s | 20.676MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_17.smt2    |    0.051s | 22.12MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-16-2-4-3-5.smt2  |    0.051s | 20.672MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-20-3-5-3-5.smt2  |    0.051s | 21.1MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-06-2-2-5-1.smt2  |    0.051s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_7_7.smt2                   |    0.052s | 20.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_5_10.smt2                  |    0.052s | 20.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_15.smt2    |    0.052s | 22.132MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_16.smt2    |    0.052s | 19.844MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-14-4-3-1-1.smt2  |    0.052s | 20.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-07-4-5-1-2.smt2  |    0.052s | 20.14MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_14_14.smt2                 |    0.053s | 20.884MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_15.smt2    |    0.053s | 20.38MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-17-5-5-3-5.smt2  |    0.053s | 21.072MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-20-5-1-4-2.smt2  |    0.053s | 20.632MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_15_15.smt2                 |    0.054s | 20.912MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_20_20.smt2                 |    0.054s | 21.536MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_13.smt2    |    0.054s | 20.648MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-17-2-2-4-1.smt2  |    0.054s | 20.36MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-13-5-3-5-5.smt2  |    0.054s | 19.996MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-11-3-3-4-5.smt2  |    0.054s | 21.184MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium7.smt2 |    0.054s | 20.384MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_24_34.smt2                 |    0.055s | 21.38MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_39_39.smt2                 |    0.055s | 22.472MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_17.smt2    |    0.055s | 19.592MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_10.smt2    |    0.055s | 21.112MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_16.smt2    |    0.055s | 19.908MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-16-5-2-2-3.smt2  |    0.055s | 20.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-13-2-4-5-3.smt2  |    0.055s | 20.164MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_9_19.smt2                  |    0.056s | 20.848MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_03.smt2    |    0.056s | 20.156MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-09-5-1-2-5.smt2  |    0.056s | 20.152MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-06-4-4-5-5.smt2  |    0.056s | 21.052MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-10-1-4-2-5.smt2  |    0.056s | 20.196MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-08-4-4-4-2.smt2  |    0.056s | 20.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-05-1-5-4-2.smt2  |    0.056s | 20.496MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_16_16.smt2                 |    0.057s | 20.984MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.606727.smt2       |    0.057s | 19.84MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_07.smt2    |    0.057s | 20.268MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_13_13.smt2                 |    0.058s | 20.872MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-15-4-1-4-1.smt2  |    0.058s | 20.908MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-08-4-5-4-1.smt2  |    0.058s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_06.smt2    |    0.059s | 20.984MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_06.smt2    |    0.059s | 19.7MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_14.smt2    |    0.059s | 20.356MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-18-1-3-1-3.smt2  |    0.059s | 20.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-15-2-3-4-4.smt2  |    0.059s | 20.912MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-07-4-2-4-4.smt2  |    0.059s | 21.168MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-14-2-3-3-5.smt2  |    0.059s | 21.096MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-11-1-4-4-3.smt2  |    0.059s | 20.652MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_12.smt2    |    0.060s | 19.752MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_06.smt2    |    0.060s | 21.132MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-13-4-1-4-5.smt2  |    0.060s | 20.884MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-17-4-5-2-4.smt2  |    0.060s | 21.132MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-08-4-2-5-4.smt2  |    0.060s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-05-3-1-5-3.smt2  |    0.060s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-10-4-4-3-1.smt2  |    0.060s | 20.656MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-13-3-4-2-3.smt2  |    0.060s | 20.924MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-14-3-1-5-2.smt2  |    0.060s | 20.152MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-09-4-2-3-5.smt2  |    0.060s | 21.14MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-14-2-4-1-4.smt2  |    0.060s | 20.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_27_37.smt2                 |    0.061s | 21.784MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_6_6.smt2                   |    0.061s | 20.14MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_7_17.smt2                  |    0.061s | 20.148MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_19.smt2    |    0.061s | 22.944MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_06.smt2    |    0.061s | 20.424MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-14-1-1-1-1.smt2  |    0.061s | 20.592MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-12-5-2-1-5.smt2  |    0.061s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-10-1-3-5-1.smt2  |    0.061s | 20.976MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-16-3-5-2-4.smt2  |    0.061s | 20.736MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-07-1-3-3-2.smt2  |    0.061s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-05-5-2-1-5.smt2  |    0.061s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_13.smt2    |    0.062s | 21.328MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_11.smt2    |    0.062s | 21.768MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-13-5-4-1-2.smt2  |    0.062s | 21.124MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-17-5-4-3-1.smt2  |    0.062s | 21.012MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-12-5-4-4-2.smt2  |    0.062s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-14-2-4-3-1.smt2  |    0.062s | 21.024MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-08-5-1-1-4.smt2  |    0.062s | 20.844MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_33_43.smt2                 |    0.063s | 22.012MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_32_42.smt2                 |    0.063s | 21.86MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_09.smt2    |    0.063s | 21.056MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_03.smt2    |    0.063s | 20.316MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-06-1-1-2-4.smt2  |    0.063s | 20.952MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-08-2-2-4-5.smt2  |    0.063s | 20.844MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-19-3-1-4-1.smt2  |    0.063s | 20.904MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-18-2-2-2-5.smt2  |    0.063s | 21.252MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-05-1-4-4-5.smt2  |    0.063s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-06-4-2-1-1.smt2  |    0.063s | 20.98MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-14-4-5-4-2.smt2  |    0.063s | 20.864MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_12.smt2    |    0.064s | 21.092MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_15.smt2    |    0.064s | 20.772MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-15-3-2-1-5.smt2  |    0.064s | 21.152MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-08-1-4-4-5.smt2  |    0.064s | 21.104MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-20-1-1-2-2.smt2  |    0.064s | 21.184MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-09-3-5-5-3.smt2  |    0.064s | 20.144MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.602046.smt2       |    0.065s | 19.448MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.686126.smt2       |    0.065s | 19.648MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_03.smt2    |    0.065s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_09.smt2    |    0.065s | 20.872MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_12.smt2    |    0.065s | 19.684MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-20-3-2-4-5.smt2  |    0.065s | 20.672MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-11-4-2-2-1.smt2  |    0.065s | 20.868MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-10-5-1-5-2.smt2  |    0.065s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-10-5-2-5-3.smt2  |    0.065s | 20.864MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-11-4-4-1-3.smt2  |    0.065s | 21.132MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-16-3-4-1-5.smt2  |    0.065s | 20.648MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-16-1-5-4-3.smt2  |    0.065s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-08-5-1-5-3.smt2  |    0.065s | 20.588MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_18.smt2    |    0.066s | 23.084MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_19.smt2    |    0.066s | 21.62MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-05-2-5-4-2.smt2  |    0.066s | 21.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_7_12.smt2                  |    0.067s | 20.468MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_8_18.smt2                  |    0.067s | 20.612MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_35_45.smt2                 |    0.067s | 22.012MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_10.smt2    |    0.067s | 19.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_09.smt2    |    0.067s | 21.592MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_16.smt2    |    0.067s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_10.smt2    |    0.067s | 21.592MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-07-3-4-4-3.smt2  |    0.067s | 20.948MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-13-4-4-5-2.smt2  |    0.067s | 21.128MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-12-3-4-4-3.smt2  |    0.067s | 20.992MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-05-5-2-3-5.smt2  |    0.067s | 20.144MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_26_36.smt2                 |    0.068s | 21.64MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_16.smt2    |    0.068s | 21.656MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_20.smt2    |    0.068s | 20.992MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_09.smt2    |    0.068s | 19.616MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_08.smt2    |    0.068s | 19.86MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_19.smt2    |    0.068s | 22.384MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-20-5-1-5-4.smt2  |    0.068s | 21.168MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-16-4-4-4-1.smt2  |    0.068s | 21.264MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_12_12.smt2                 |    0.069s | 20.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.686091.smt2       |    0.069s | 19.924MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_09.smt2    |    0.069s | 20.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_13.smt2    |    0.069s | 22.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_15.smt2    |    0.069s | 22.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_14.smt2    |    0.069s | 22.304MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_18.smt2    |    0.069s | 22.26MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_14.smt2    |    0.069s | 19.88MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-08-5-3-1-5.smt2  |    0.069s | 21.428MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-09-1-5-4-4.smt2  |    0.069s | 21.444MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-18-1-3-4-5.smt2  |    0.069s | 20.812MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-19-3-3-3-4.smt2  |    0.069s | 20.996MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium6.smt2 |    0.069s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_20.smt2    |    0.070s | 21.608MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-08-5-2-5-3.smt2  |    0.070s | 20.82MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_10_15.smt2                 |    0.071s | 20.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_17_17.smt2                 |    0.071s | 21.08MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-09-1-5-4-2.smt2  |    0.071s | 21.56MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-10-4-3-1-3.smt2  |    0.071s | 20.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-19-1-4-1-2.smt2  |    0.071s | 20.964MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-19-2-2-2-5.smt2  |    0.071s | 21.432MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard9.smt2 |    0.071s | 20.824MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_27_27.smt2                 |    0.072s | 21.688MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_07.smt2    |    0.073s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-11-2-5-3-2.smt2  |    0.073s | 20.068MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-05-5-3-1-1.smt2  |    0.073s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_14_24.smt2                 |    0.074s | 21.08MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_32_52.smt2                 |    0.074s | 21.94MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_19_19.smt2                 |    0.074s | 21.144MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_9_14.smt2                  |    0.074s | 20.88MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_18.smt2    |    0.074s | 19.792MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_20.smt2    |    0.074s | 23.076MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-12-5-3-4-4.smt2  |    0.074s | 20.912MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-19-4-1-1-5.smt2  |    0.074s | 21.2MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-11-5-3-1-1.smt2  |    0.074s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_09.smt2    |    0.075s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-18-1-2-1-5.smt2  |    0.075s | 20.236MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-10-4-5-2-3.smt2  |    0.075s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-11-2-4-1-5.smt2  |    0.075s | 20.616MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-07-5-1-2-1.smt2  |    0.075s | 21.096MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_14.smt2    |    0.076s | 21.908MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_17.smt2    |    0.076s | 19.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-05-5-5-1-3.smt2  |    0.076s | 21.152MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_35_55.smt2                 |    0.077s | 22.252MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_8_13.smt2                  |    0.077s | 20.836MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_04.smt2    |    0.077s | 19.62MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-18-2-2-5-3.smt2  |    0.077s | 20.932MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-15-4-2-1-4.smt2  |    0.077s | 20.628MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-14-3-5-4-4.smt2  |    0.077s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-14-3-3-5-3.smt2  |    0.077s | 21.12MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-12-3-1-3-4.smt2  |    0.077s | 20.736MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-19-4-5-5-5.smt2  |    0.077s | 21.164MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-07-1-1-4-2.smt2  |    0.077s | 21.168MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-13-5-3-2-1.smt2  |    0.077s | 20.112MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_32_32.smt2                 |    0.078s | 21.704MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_5_5.smt2                   |    0.078s | 20.204MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.686056.smt2       |    0.078s | 19.828MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.666078.smt2       |    0.078s | 20.012MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-17-1-5-1-5.smt2  |    0.078s | 20.028MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-16-2-4-2-5.smt2  |    0.078s | 21.172MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-06-1-1-5-2.smt2  |    0.078s | 20.616MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_35_35.smt2                 |    0.079s | 22.252MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-10-5-5-5-4.smt2  |    0.079s | 21.132MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-08-3-2-1-3.smt2  |    0.079s | 20.804MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-11-1-1-1-1.smt2  |    0.079s | 20.628MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard7.smt2 |    0.079s | 20.524MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_30_30.smt2                 |    0.080s | 21.748MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_19.smt2    |    0.080s | 21.084MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_10.smt2    |    0.080s | 21.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium10.smt2 |    0.080s | 20.564MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium11.smt2 |    0.080s | 20.576MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_07.smt2    |    0.081s | 21.4MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-13-4-5-2-4.smt2  |    0.081s | 20.944MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_20.smt2    |    0.082s | 20.856MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-09-4-2-1-5.smt2  |    0.082s | 21.252MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-18-1-4-4-3.smt2  |    0.082s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_40_40.smt2                 |    0.083s | 22.632MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.675482.smt2       |    0.083s | 20.188MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-07-3-4-2-3.smt2  |    0.083s | 20.884MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_5_15.smt2                  |    0.084s | 20.148MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.675513.smt2       |    0.084s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard10.smt2 |    0.084s | 20.832MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard6.smt2 |    0.084s | 20.5MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_6_11.smt2                  |    0.085s | 20.4MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_6_16.smt2                  |    0.085s | 20.664MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_04.smt2    |    0.085s | 20.716MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_11.smt2    |    0.085s | 20.476MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-15-5-3-4-1.smt2  |    0.085s | 20.896MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-05-5-2-5-5.smt2  |    0.085s | 20.128MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_14_19.smt2                 |    0.086s | 20.864MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_10_10.smt2                 |    0.086s | 20.676MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_8_8.smt2                   |    0.086s | 20.6MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-19-5-3-2-5.smt2  |    0.086s | 20.896MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-10-3-4-2-2.smt2  |    0.086s | 20.796MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-06-3-5-4-4.smt2  |    0.086s | 20.832MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-16-4-1-1-5.smt2  |    0.086s | 21.44MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-07-1-5-2-3.smt2  |    0.086s | 20.936MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-07-3-2-3-1.smt2  |    0.086s | 20.512MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium5.smt2 |    0.086s | 20.384MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_9_9.smt2                   |    0.087s | 20.776MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_09.smt2    |    0.087s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-07-3-1-5-5.smt2  |    0.087s | 20.872MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-14-3-5-2-3.smt2  |    0.087s | 21.02MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard16.smt2 |    0.087s | 21.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_11_11.smt2                 |    0.088s | 20.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_03.smt2    |    0.088s | 19.588MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_13.smt2    |    0.088s | 21.024MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-18-5-5-3-2.smt2  |    0.088s | 20.816MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-15-1-1-3-3.smt2  |    0.088s | 20.884MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.688318.smt2       |    0.089s | 20.128MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_13.smt2    |    0.089s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_18.smt2    |    0.090s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-13-4-2-1-3.smt2  |    0.090s | 21.108MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-19-2-2-3-1.smt2  |    0.090s | 20.684MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_04.smt2    |    0.091s | 20.608MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_12.smt2    |    0.091s | 20.64MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_09.smt2    |    0.091s | 19.824MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-05-2-1-1-2.smt2  |    0.091s | 20.984MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-14-5-3-1-2.smt2  |    0.091s | 20.928MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_30_40.smt2                 |    0.092s | 21.928MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_29_29.smt2                 |    0.092s | 21.668MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_25_35.smt2                 |    0.092s | 22.06MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_07.smt2    |    0.092s | 21.48MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_06.smt2    |    0.092s | 20.152MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-17-2-1-4-1.smt2  |    0.092s | 20.788MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-17-5-2-4-4.smt2  |    0.092s | 21.132MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-20-5-1-4-4.smt2  |    0.092s | 21.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-11-5-2-2-2.smt2  |    0.092s | 21.14MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_18_18.smt2                 |    0.093s | 21.544MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_28_28.smt2                 |    0.093s | 21.432MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_38_48.smt2                 |    0.093s | 22.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-17-2-2-5-1.smt2  |    0.093s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_22_22.smt2                 |    0.094s | 21.408MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_23_23.smt2                 |    0.094s | 21.44MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_26_26.smt2                 |    0.094s | 21.684MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_08.smt2    |    0.094s | 20.892MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_09.smt2    |    0.094s | 21.412MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_17.smt2    |    0.094s | 19.844MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-08-4-1-1-1.smt2  |    0.094s | 21.128MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-11-4-3-2-2.smt2  |    0.094s | 20.924MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-13-4-4-2-1.smt2  |    0.094s | 21.1MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium17.smt2 |    0.094s | 20.98MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium8.smt2 |    0.094s | 20.34MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_14.smt2    |    0.095s | 20.9MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-18-4-2-3-1.smt2  |    0.095s | 21.028MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-16-4-4-5-1.smt2  |    0.095s | 20.824MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard12.smt2 |    0.095s | 20.932MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_17.smt2    |    0.096s | 21.776MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-11-2-1-5-2.smt2  |    0.096s | 21.036MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-12-4-2-4-5.smt2  |    0.096s | 21.26MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium9.smt2 |    0.096s | 20.504MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_12_22.smt2                 |    0.097s | 21.136MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_04.smt2    |    0.097s | 21.376MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_10.smt2    |    0.097s | 20.476MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-06-2-3-1-3.smt2  |    0.097s | 21.156MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-17-4-1-2-2.smt2  |    0.097s | 20.64MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard4.smt2 |    0.097s | 20.52MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_31_31.smt2                 |    0.098s | 21.788MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_14.smt2    |    0.098s | 21.284MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_18.smt2    |    0.098s | 22.068MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-20-5-5-1-5.smt2  |    0.098s | 20.672MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-19-3-4-2-4.smt2  |    0.098s | 20.808MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-12-4-1-1-4.smt2  |    0.098s | 20.624MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_11_16.smt2                 |    0.099s | 20.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_11.smt2    |    0.099s | 21.008MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_07.smt2    |    0.099s | 20.656MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_12.smt2    |    0.099s | 21.112MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_04.smt2    |    0.099s | 21.016MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_08.smt2    |    0.100s | 21.204MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-06-4-5-4-1.smt2  |    0.100s | 20.592MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-06-4-1-3-5.smt2  |    0.100s | 21.068MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-19-3-3-4-4.smt2  |    0.100s | 20.612MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.606709.smt2       |    0.101s | 20.056MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-12-3-4-4-1.smt2  |    0.101s | 21.012MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-14-5-5-5-1.smt2  |    0.101s | 21.432MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-05-12-1-4-2-1.smt2  |    0.101s | 20.936MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-16-4-4-1-4.smt2  |    0.101s | 21.2MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium16.smt2 |    0.101s | 20.888MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_37_57.smt2                 |    0.102s | 22.236MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.602033.smt2       |    0.102s | 19.664MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.666132.smt2       |    0.102s | 20.224MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.769286.smt2       |    0.102s | 19.948MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_20.smt2    |    0.102s | 21.348MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_13.smt2    |    0.102s | 21.696MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-19-4-5-1-1.smt2  |    0.102s | 21.084MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-12-5-4-2-5.smt2  |    0.102s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-10-5-3-2-1.smt2  |    0.102s | 21.116MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_39_49.smt2                 |    0.103s | 22.616MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.598294.smt2       |    0.103s | 19.964MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-06-3-4-5-1.smt2  |    0.103s | 20.724MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/TwoSquares/smtlib.675451.smt2       |    0.104s | 20.072MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_05.smt2    |    0.104s | 21.2MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-05-5-4-4-2.smt2  |    0.104s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-06-07-4-5-4-2.smt2  |    0.104s | 20.436MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-06-4-3-2-1.smt2  |    0.104s | 21.244MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium12.smt2 |    0.104s | 20.572MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_12.smt2    |    0.105s | 22.108MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium15.smt2 |    0.105s | 20.672MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_13_18.smt2                 |    0.106s | 21.228MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-16-2-2-1-4.smt2  |    0.106s | 21.14MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_18_28.smt2                 |    0.107s | 21.548MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-11-1-3-5-5.smt2  |    0.107s | 20.888MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard8.smt2 |    0.107s | 20.772MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_07.smt2    |    0.108s | 20.016MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-11-5-1-3-4.smt2  |    0.108s | 21.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium13.smt2 |    0.108s | 20.624MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_15.smt2    |    0.109s | 21.812MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_07.smt2    |    0.109s | 19.932MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_37_37.smt2                 |    0.111s | 23.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_06.smt2    |    0.111s | 19.512MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_11.smt2    |    0.111s | 19.872MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-09-3-5-5-3.smt2  |    0.111s | 20.948MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-16-3-1-4-1.smt2  |    0.111s | 21.156MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-16-19-3-4-4-1.smt2  |    0.111s | 20.972MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-18-3-3-4-1.smt2  |    0.111s | 21.024MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-17-2-1-1-4.smt2  |    0.111s | 21.32MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard11.smt2 |    0.111s | 20.888MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_10.smt2    |    0.112s | 21.8MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_06.smt2    |    0.112s | 20.412MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_17.smt2    |    0.112s | 22.832MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_10.smt2    |    0.112s | 19.596MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium20.smt2 |    0.112s | 20.976MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_6_QF_UFLIA.smt2 |    0.112s | 22.4MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_12.smt2    |    0.113s | 20.408MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_04.smt2    |    0.113s | 19.876MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_11.smt2    |    0.113s | 19.624MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-09-3-4-4-3.smt2  |    0.113s | 20.54MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_10_20.smt2                 |    0.114s | 20.804MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_05.smt2    |    0.114s | 20.236MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_04.smt2    |    0.114s | 19.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_03.smt2    |    0.114s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_19.smt2    |    0.114s | 19.772MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_19.smt2    |    0.114s | 20.148MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_05.smt2    |    0.114s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_06.smt2    |    0.115s | 19.592MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_08.smt2    |    0.115s | 20.688MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_20.smt2    |    0.115s | 22.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_08.smt2    |    0.115s | 19.912MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_04_20.smt2    |    0.115s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_03_05.smt2    |    0.115s | 19.632MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium19.smt2 |    0.115s | 20.98MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_40_60.smt2                 |    0.116s | 22.58MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_11_21.smt2                 |    0.116s | 20.956MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_13.smt2    |    0.116s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_uns_05_18.smt2    |    0.116s | 19.912MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_03_14.smt2    |    0.116s | 20.416MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_03.smt2    |    0.116s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_09.smt2    |    0.117s | 20.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard5.smt2 |    0.117s | 20.776MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_15_20.smt2                 |    0.118s | 21.28MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_16.smt2    |    0.118s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_03.smt2    |    0.118s | 20.828MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_19.smt2    |    0.118s | 20.664MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/medium/medium18.smt2 |    0.118s | 20.82MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_34_34.smt2                 |    0.119s | 22.428MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_19_29.smt2                 |    0.119s | 21.66MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_10.smt2    |    0.119s | 20.516MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_08.smt2    |    0.119s | 20.492MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-12-3-1-5-2.smt2  |    0.119s | 21.164MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-10-4-3-1-3.smt2  |    0.119s | 20.9MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_03.smt2    |    0.120s | 20.904MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_10.smt2    |    0.120s | 20.772MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_07.smt2    |    0.120s | 20.652MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_04_17.smt2    |    0.120s | 20.892MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_06.smt2    |    0.120s | 20.616MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_05_18.smt2    |    0.121s | 20.932MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_05.smt2    |    0.121s | 20.924MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-15-2-2-2-4.smt2  |    0.121s | 21.016MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-05-3-3-5-4.smt2  |    0.121s | 20.752MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_05.smt2    |    0.122s | 21.248MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-11-5-3-5-3.smt2  |    0.122s | 20.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_15.smt2    |    0.123s | 21.312MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_06.smt2    |    0.123s | 21.356MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-20-1-4-2-1.smt2  |    0.123s | 20.972MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-20-5-2-5-3.smt2  |    0.123s | 20.876MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_18.smt2    |    0.124s | 21.576MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_06_18.smt2    |    0.124s | 21.176MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_11.smt2    |    0.125s | 21.4MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-07-3-4-1-2.smt2  |    0.125s | 20.9MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_19.smt2    |    0.126s | 22.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_07.smt2    |    0.126s | 21.54MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_13.smt2    |    0.127s | 22.292MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_16.smt2    |    0.127s | 21.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_07_19.smt2    |    0.127s | 21.684MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_12.smt2    |    0.127s | 21.364MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_12.smt2    |    0.128s | 21.576MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_17.smt2    |    0.129s | 21.968MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-15-2-2-4-2.smt2  |    0.129s | 21.264MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard13.smt2 |    0.129s | 21.276MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-17-5-4-5-4.smt2  |    0.130s | 21.416MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-18-4-1-3-5.smt2  |    0.130s | 21.604MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_22_32.smt2                 |    0.131s | 21.844MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_11.smt2    |    0.131s | 21.824MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_08_20.smt2    |    0.131s | 22.048MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_09_16.smt2    |    0.133s | 22.1MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-13-5-5-4-5.smt2  |    0.134s | 20.812MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-17-5-2-2-5.smt2  |    0.134s | 20.904MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard14.smt2 |    0.135s | 20.884MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_12_17.smt2                 |    0.137s | 21.368MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Hash/hash_sat_10_16.smt2    |    0.138s | 22.648MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-06-4-1-5-3.smt2  |    0.140s | 20.412MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_16_26.smt2                 |    0.142s | 21.412MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-15-4-4-5-5.smt2  |    0.142s | 20.792MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-18-08-4-5-2-5.smt2  |    0.142s | 20.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-13-2-5-2-3.smt2  |    0.145s | 21.204MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard17.smt2 |    0.145s | 21.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-13-2-3-1-3.smt2  |    0.151s | 20.808MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/63058_aa742630eef64f949de269382c1f9035_25_UFLIA.smt2 |    0.151s | 24.46MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_17_27.smt2                 |    0.153s | 21.54MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard19.smt2 |    0.156s | 21.34MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-15-4-4-1-5.smt2  |    0.157s | 20.528MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-09-1-4-4-1.smt2  |    0.158s | 20.688MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_23_33.smt2                 |    0.159s | 22.056MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-09-5-5-2-4.smt2  |    0.159s | 20.692MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_34_QF_UFLIA.smt2 |    0.159s | 26.288MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-10-3-5-2-5.smt2  |    0.160s | 21.092MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-13-20-2-2-4-4.smt2  |    0.161s | 20.98MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-10-14-2-1-4-4.smt2  |    0.162s | 20.664MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-11-09-1-2-1-3.smt2  |    0.162s | 20.86MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-15-12-2-2-3-4.smt2  |    0.163s | 21.032MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-09-4-3-5-5.smt2  |    0.164s | 20.94MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-07-1-4-1-3.smt2  |    0.164s | 21.068MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard15.smt2 |    0.164s | 21.096MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_20_30.smt2                 |    0.165s | 21.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-07-10-2-4-5-3.smt2  |    0.166s | 20.568MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-05-2-4-4-5.smt2  |    0.166s | 20.708MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-08-09-4-5-5-4.smt2  |    0.166s | 20.6MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-08-1-1-5-3.smt2  |    0.167s | 20.652MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-12-18-1-3-1-2.smt2  |    0.167s | 21.156MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-14-09-3-3-4-4.smt2  |    0.168s | 21.008MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-17-06-5-5-4-1.smt2  |    0.169s | 20.968MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-09-15-5-3-2-3.smt2  |    0.169s | 20.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-20-14-2-3-3-3.smt2  |    0.173s | 21.372MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_21_31.smt2                 |    0.174s | 21.864MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/Wisa/xs-19-15-3-1-3-4.smt2  |    0.174s | 21.26MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_13_23.smt2                 |    0.179s | 21.612MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_24_24.smt2                 |    0.180s | 22.172MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_28_48.smt2                 |    0.180s | 22.396MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_28_38.smt2                 |    0.182s | 22.14MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_29_39.smt2                 |    0.182s | 22.68MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard18.smt2 |    0.182s | 21.32MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/mathsat/EufLaArithmetic/hard/hard20.smt2 |    0.183s | 21.376MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_25_25.smt2                 |    0.184s | 22.016MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_21_21.smt2                 |    0.209s | 21.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_7_QF_UFLIA.smt2 |    0.219s | 26.476MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_40_QF_UFLIA.smt2 |    0.239s | 25.384MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_38_58.smt2                 |    0.250s | 23.64MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_34_54.smt2                 |    0.250s | 22.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_33_33.smt2                 |    0.260s | 22.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_37_47.smt2                 |    0.268s | 23.104MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_31_41.smt2                 |    0.286s | 22.912MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_38_38.smt2                 |    0.305s | 23.472MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_8_QF_UFLIA.smt2 |    0.329s | 26.84MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_36_56.smt2                 |    0.357s | 23.364MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_40_50.smt2                 |    0.368s | 23.84MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_36_36.smt2                 |    0.378s | 23.676MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_15_25.smt2                 |    0.403s | 21.576MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_34_44.smt2                 |    0.431s | 23.472MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_16_36.smt2                 |    0.435s | 21.552MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_36_46.smt2                 |    0.438s | 23.42MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_39_59.smt2                 |    0.487s | 23.988MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_33_53.smt2                 |    0.519s | 23.064MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_17_37.smt2                 |    0.525s | 21.648MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_58_QF_UFLIA.smt2 |    0.556s | 31.264MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_27_47.smt2                 |    0.583s | 22.644MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_29_49.smt2                 |    0.592s | 22.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_35_QF_UFLIA.smt2 |    0.593s | 36.94MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_43_QF_UFLIA.smt2 |    0.601s | 35.672MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_30_50.smt2                 |    0.880s | 22.684MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_18_38.smt2                 |    1.356s | 22.084MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_19_39.smt2                 |    1.698s | 23.148MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_42_QF_UFLIA.smt2 |    1.760s | 43.34MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_26_46.smt2                 |    1.886s | 22.788MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_20_40.smt2                 |    2.378s | 24.5MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_60_QF_UFLIA.smt2 |    2.514s | 57.832MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_24_44.smt2                 |    2.848s | 23.38MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_21_41.smt2                 |    3.135s | 25.28MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/72771_f9d228efc97cf1458e38_64_QF_UFLIA.smt2 |    3.147s | 42.452MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_63_QF_UFLIA.smt2 |    3.197s | 36.452MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_21_QF_UFLIA.smt2 |    5.222s | 86.372MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_70_QF_UFLIA.smt2 |    5.239s | 74.984MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_18_QF_UFLIA.smt2 |    6.152s | 92.348MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_22_42.smt2                 |    6.362s | 26.12MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_14_QF_UFLIA.smt2 |    6.726s | 100.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_62_QF_UFLIA.smt2 |    6.761s | 45.32MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_41_QF_UFLIA.smt2 |    7.218s | 55.988MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_17_QF_UFLIA.smt2 |    7.429s | 99.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_72_QF_UFLIA.smt2 |    9.710s | 541.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_23_43.smt2                 |   10.467s | 29.196MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFLIA.smt2 |   10.965s | 120.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_45_QF_UFLIA.smt2 |   12.134s | 82.244MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFLIA.smt2 |   15.127s | 224.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_59_QF_UFLIA.smt2 |   15.380s | 106.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_22_QF_UFLIA.smt2 |   15.923s | 178.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_48_QF_UFLIA.smt2 |   16.191s | 51.468MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFLIA.smt2 |   17.329s | 174.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_46_QF_UFLIA.smt2 |   18.358s | 106.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_71_QF_UFLIA.smt2 |   20.024s | 165.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_5_QF_UFLIA.smt2 |   20.037s | 118.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_39_QF_UFLIA.smt2 |   20.041s | 123.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/wisas/xs_25_45.smt2                 |   20.043s | 34.14MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_36_QF_UFLIA.smt2 |   20.044s | 340.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_24_QF_UFLIA.smt2 |   20.044s | 193.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_15_QF_UFLIA.smt2 |   20.045s | 371.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_57_QF_UFLIA.smt2 |   20.046s | 34.056MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFLIA.smt2 |   20.047s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFLIA.smt2 |   20.050s | 115.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_31_QF_UFLIA.smt2 |   20.056s | 150.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_55_QF_UFLIA.smt2 |   20.057s | 27.796MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/93493_5990a6bf5f2740164f77_50_QF_UFLIA.smt2 |   20.058s | 196.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_19_QF_UFLIA.smt2 |   20.059s | 92.428MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_61_QF_UFLIA.smt2 |   20.061s | 68.996MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_23_QF_UFLIA.smt2 |   20.064s | 177.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_74_QF_UFLIA.smt2 |   20.066s | 591.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_44_QF_UFLIA.smt2 |   20.068s | 123.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_47_QF_UFLIA.smt2 |   20.073s | 50.572MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/63058_55d6bef5390186355f11_26_QF_UFLIA.smt2 |   20.074s | 182.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_0_UFLIA.smt2 |   20.077s | 137.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFLIA.smt2 |   20.078s | 151.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFLIA.smt2 |   20.081s | 112.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFLIA.smt2 |   20.082s | 227.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_56_QF_UFLIA.smt2 |   20.084s | 144.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_20_QF_UFLIA.smt2 |   20.086s | 402.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/93493_798593962ee29ad45ac8_52_QF_UFLIA.smt2 |   20.087s | 189.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_38_QF_UFLIA.smt2 |   20.090s | 372.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_1_UFLIA.smt2 |   20.093s | 235.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/93493_4ea6163ed03941199c785278ccc42812_49_QF_UFLIA.smt2 |   20.093s | 180.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_28_QF_UFLIA.smt2 |   20.094s | 440.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_33_QF_UFLIA.smt2 |   20.094s | 304.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_30_QF_UFLIA.smt2 |   20.094s | 289.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_3_UFLIA.smt2 |   20.095s | 375.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_2_UFLIA.smt2 |   20.095s | 176.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_76_QF_UFLIA.smt2 |   20.096s | 479.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_4_UFLIA.smt2 |   20.098s | 589.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFLIA.smt2 |   20.101s | 585.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_32_QF_UFLIA.smt2 |   20.106s | 282.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_29_QF_UFLIA.smt2 |   20.113s | 294.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFLIA.smt2 |   20.116s | 473.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_27_QF_UFLIA.smt2 |   20.121s | 445.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_73_QF_UFLIA.smt2 |   20.127s | 592.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFLIA.smt2 |   20.127s | 531.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFLIA.smt2 |   20.135s | 927.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_75_QF_UFLIA.smt2 |   20.138s | 578.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFLIA.smt2 |   20.185s | 1185.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFLIA/20230314-Jaroslav-Bendik-Certora/38347_525a1ca0331f2bcbf520_54_QF_UFLIA.smt2 |   20.198s | 1253.0MiB| timeout | 0 |  |  |
