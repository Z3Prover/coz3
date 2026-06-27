# .

* SAT 3
* UNSAT 2
* TIMEOUT 198
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFDT.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFDT.tar.zst-do
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDT.tar.zst?download=1
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
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/afp/coinductive_list/x2015_09_10_17_01_00_473_1716450.smt_in.smt2 |    0.023s | 19.776MiB| sat | 0 |  |  |
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/distro/stream_processor/x2015_09_10_16_45_47_401_987519.smt_in.smt2 |    0.045s | 19.904MiB| sat | 0 |  |  |
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/distro/stream_processor/x2015_09_10_16_45_54_875_1000989.smt_in.smt2 |    0.047s | 20.304MiB| sat | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e10.smt2     |   19.102s | 34.056MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e04.smt2     |   19.330s | 38.12MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k22.smt2     |   20.014s | 29.252MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e70.smt2     |   20.016s | 35.632MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k42.smt2     |   20.016s | 40.392MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e74.smt2     |   20.017s | 37.612MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e51.smt2     |   20.018s | 34.216MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e46.smt2     |   20.019s | 35.276MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k15.smt2     |   20.020s | 34.264MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e89.smt2     |   20.020s | 37.184MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k17.smt2     |   20.021s | 45.084MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k14.smt2     |   20.021s | 29.96MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k32.smt2     |   20.022s | 31.632MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e53.smt2     |   20.023s | 34.556MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e65.smt2     |   20.023s | 38.164MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e08.smt2     |   20.024s | 30.312MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e00.smt2     |   20.024s | 32.272MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e25.smt2     |   20.025s | 33.904MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k30.smt2     |   20.026s | 30.608MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k80.smt2     |   20.027s | 62.988MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k29.smt2     |   20.029s | 38.78MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e38.smt2     |   20.029s | 31.408MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e48.smt2     |   20.030s | 31.168MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e99.smt2     |   20.031s | 38.208MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e37.smt2     |   20.031s | 38.164MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e62.smt2     |   20.032s | 35.948MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k27.smt2     |   20.033s | 38.632MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e59.smt2     |   20.033s | 39.436MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k51.smt2     |   20.036s | 35.244MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e91.smt2     |   20.038s | 34.784MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e02.smt2     |   20.038s | 30.672MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k79.smt2     |   20.038s | 33.876MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e33.smt2     |   20.038s | 35.316MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e30.smt2     |   20.039s | 29.332MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e93.smt2     |   20.040s | 30.584MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k56.smt2     |   20.040s | 34.364MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e07.smt2     |   20.041s | 35.068MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k31.smt2     |   20.041s | 30.88MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k86.smt2     |   20.041s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e15.smt2     |   20.042s | 31.224MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k84.smt2     |   20.042s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e64.smt2     |   20.042s | 37.296MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k94.smt2     |   20.042s | 35.62MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k21.smt2     |   20.042s | 37.684MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k06.smt2     |   20.042s | 45.264MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e94.smt2     |   20.043s | 32.948MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e98.smt2     |   20.043s | 33.76MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k90.smt2     |   20.043s | 34.336MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k19.smt2     |   20.043s | 31.34MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e31.smt2     |   20.043s | 32.304MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e68.smt2     |   20.044s | 32.352MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e66.smt2     |   20.044s | 40.276MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k65.smt2     |   20.044s | 37.612MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k91.smt2     |   20.044s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k62.smt2     |   20.044s | 39.104MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k66.smt2     |   20.044s | 29.296MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e86.smt2     |   20.044s | 35.58MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e19.smt2     |   20.045s | 31.98MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k26.smt2     |   20.045s | 34.984MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e63.smt2     |   20.046s | 38.052MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k50.smt2     |   20.046s | 38.912MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k77.smt2     |   20.047s | 37.552MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e42.smt2     |   20.047s | 31.112MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k75.smt2     |   20.047s | 37.448MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e45.smt2     |   20.047s | 37.056MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e32.smt2     |   20.047s | 35.712MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e77.smt2     |   20.047s | 29.652MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k67.smt2     |   20.047s | 32.16MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e13.smt2     |   20.047s | 31.548MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k49.smt2     |   20.047s | 33.768MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e01.smt2     |   20.048s | 32.552MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e40.smt2     |   20.048s | 34.38MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k68.smt2     |   20.048s | 37.392MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k92.smt2     |   20.048s | 72.728MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e14.smt2     |   20.048s | 33.76MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e05.smt2     |   20.048s | 35.584MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k07.smt2     |   20.048s | 45.68MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k59.smt2     |   20.048s | 31.408MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e24.smt2     |   20.048s | 33.596MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e17.smt2     |   20.049s | 38.772MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k61.smt2     |   20.049s | 37.836MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k23.smt2     |   20.049s | 33.164MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e44.smt2     |   20.049s | 31.812MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e52.smt2     |   20.049s | 35.476MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e82.smt2     |   20.049s | 36.932MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e81.smt2     |   20.050s | 29.556MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e90.smt2     |   20.050s | 38.804MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k47.smt2     |   20.050s | 78.048MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k76.smt2     |   20.050s | 37.748MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k18.smt2     |   20.051s | 31.144MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e18.smt2     |   20.051s | 31.948MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e28.smt2     |   20.051s | 34.864MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k04.smt2     |   20.051s | 44.192MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e27.smt2     |   20.052s | 38.012MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k35.smt2     |   20.052s | 42.148MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e21.smt2     |   20.052s | 34.86MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e22.smt2     |   20.053s | 32.46MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e36.smt2     |   20.053s | 30.832MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k33.smt2     |   20.053s | 38.7MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e20.smt2     |   20.054s | 34.608MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e92.smt2     |   20.054s | 34.552MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k00.smt2     |   20.054s | 37.012MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k16.smt2     |   20.054s | 32.84MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e83.smt2     |   20.054s | 30.388MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e06.smt2     |   20.054s | 31.64MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e39.smt2     |   20.054s | 32.82MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k13.smt2     |   20.054s | 29.82MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k24.smt2     |   20.054s | 31.332MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e61.smt2     |   20.055s | 34.932MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e09.smt2     |   20.055s | 36.256MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e26.smt2     |   20.055s | 32.696MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k73.smt2     |   20.056s | 30.808MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e76.smt2     |   20.056s | 35.02MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e67.smt2     |   20.056s | 32.54MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k64.smt2     |   20.056s | 33.296MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k69.smt2     |   20.056s | 53.808MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k36.smt2     |   20.056s | 32.944MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k28.smt2     |   20.056s | 31.812MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e12.smt2     |   20.056s | 32.164MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k40.smt2     |   20.057s | 35.076MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e35.smt2     |   20.057s | 37.584MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e60.smt2     |   20.057s | 35.32MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k11.smt2     |   20.057s | 40.128MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k52.smt2     |   20.058s | 37.328MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k03.smt2     |   20.059s | 39.168MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k01.smt2     |   20.060s | 35.052MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e71.smt2     |   20.060s | 34.148MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k25.smt2     |   20.060s | 33.828MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e55.smt2     |   20.061s | 31.792MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k05.smt2     |   20.061s | 44.012MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k43.smt2     |   20.062s | 72.484MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e50.smt2     |   20.062s | 31.696MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k71.smt2     |   20.063s | 86.296MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k70.smt2     |   20.063s | 37.492MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k58.smt2     |   20.063s | 32.848MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e16.smt2     |   20.063s | 34.696MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k37.smt2     |   20.063s | 70.26MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k46.smt2     |   20.064s | 76.336MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k88.smt2     |   20.065s | 118.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k41.smt2     |   20.065s | 34.724MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e34.smt2     |   20.066s | 32.628MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e57.smt2     |   20.066s | 36.096MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k08.smt2     |   20.066s | 45.7MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e43.smt2     |   20.067s | 32.344MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e88.smt2     |   20.068s | 30.884MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e79.smt2     |   20.068s | 40.108MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k74.smt2     |   20.068s | 35.244MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e11.smt2     |   20.069s | 35.056MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k34.smt2     |   20.069s | 34.768MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e75.smt2     |   20.069s | 29.516MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k10.smt2     |   20.070s | 49.356MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e80.smt2     |   20.070s | 37.392MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e41.smt2     |   20.070s | 32.352MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e23.smt2     |   20.071s | 33.096MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k85.smt2     |   20.071s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e85.smt2     |   20.071s | 38.124MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e73.smt2     |   20.071s | 38.54MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e03.smt2     |   20.072s | 34.244MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k02.smt2     |   20.073s | 34.564MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e56.smt2     |   20.073s | 33.144MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k54.smt2     |   20.073s | 186.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k89.smt2     |   20.078s | 99.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k12.smt2     |   20.078s | 37.84MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k87.smt2     |   20.078s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k57.smt2     |   20.078s | 87.408MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e97.smt2     |   20.079s | 183.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k63.smt2     |   20.079s | 87.82MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k48.smt2     |   20.080s | 76.572MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e47.smt2     |   20.081s | 32.68MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e29.smt2     |   20.081s | 33.568MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e54.smt2     |   20.081s | 34.064MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k53.smt2     |   20.082s | 172.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k09.smt2     |   20.082s | 37.808MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k96.smt2     |   20.084s | 180.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k78.smt2     |   20.088s | 144.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k81.smt2     |   20.088s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k55.smt2     |   20.088s | 188.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k98.smt2     |   20.088s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e78.smt2     |   20.089s | 33.728MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k38.smt2     |   20.089s | 35.432MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e58.smt2     |   20.089s | 33.848MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k97.smt2     |   20.091s | 180.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k39.smt2     |   20.091s | 72.564MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e84.smt2     |   20.092s | 32.576MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k95.smt2     |   20.093s | 101.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k60.smt2     |   20.094s | 34.668MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k45.smt2     |   20.096s | 186.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k20.smt2     |   20.096s | 31.736MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e72.smt2     |   20.096s | 38.548MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k72.smt2     |   20.097s | 97.688MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e69.smt2     |   20.098s | 40.044MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e49.smt2     |   20.099s | 32.94MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k99.smt2     |   20.101s | 180.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e87.smt2     |   20.102s | 36.316MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e95.smt2     |   20.103s | 39.676MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k82.smt2     |   20.104s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k93.smt2     |   20.106s | 179.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k83.smt2     |   20.107s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k44.smt2     |   20.109s | 184.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e96.smt2     |   20.117s | 177.0MiB| timeout | 0 |  |  |
