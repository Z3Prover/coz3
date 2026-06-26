# .

* SAT 5
* UNSAT 12
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_AUFNIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_AUFNIA.tar.zst-
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_AUFNIA.tar.zst?download=1
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
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.02_false-unreach-call.i.cil.c_71.smt2 |    0.041s | 22.728MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.03_false-unreach-call.i.cil.c_71.smt2 |    0.043s | 22.912MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.01_false-unreach-call.i.cil.c_57.smt2 |    0.044s | 22.768MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.07_false-unreach-call.i.cil.c_0.smt2 |    0.045s | 22.892MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.10_true-unreach-call.i.cil.c_0.smt2 |    0.057s | 23.916MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.08_true-unreach-call.i.cil.c_1140.smt2 |    0.065s | 25.328MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.15_true-unreach-call.i.cil.c_1173.smt2 |    0.079s | 26.552MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.14_true-unreach-call.i.cil.c_959.smt2 |    0.082s | 26.832MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.16_true-unreach-call.i.cil.c_2036.smt2 |    0.089s | 27.652MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.12_true-unreach-call.i.cil.c_0.smt2 |    0.101s | 26.872MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.06_true-unreach-call.i.cil.c_2803.smt2 |    0.103s | 28.496MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.06_true-unreach-call.i.cil.c.smt2 |    0.113s | 30.032MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.01_false-unreach-call.i.cil.c_0.smt2 |    0.442s | 28.012MiB| sat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.04_false-unreach-call.i.cil.c_0.smt2 |    0.457s | 26.752MiB| sat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.02_false-unreach-call.i.cil.c_0.smt2 |    0.461s | 26.508MiB| sat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.11_false-unreach-call.i.cil.c_0.smt2 |    1.653s | 35.032MiB| sat | 0 |  |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.01_false-unreach-call.i.cil.c_0.smt2 |   14.559s | 51.776MiB| sat | 0 |  |  |
