# .

* SAT 93
* UNSAT 96
* TIMEOUT 66
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_RDL.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_RDL.tar.zst-dow
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_RDL.tar.zst?download=1
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
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-matrix1x1.pddl.smt2 |    0.030s | 20.352MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-2.smt2             |    0.032s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-1.smt2             |    0.033s | 20.62MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking02.smt2 |    0.034s | 20.772MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-2.smt2             |    0.037s | 20.896MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-5.smt2             |    0.044s | 20.8MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-3.smt2             |    0.046s | 21.492MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-3.smt2             |    0.049s | 20.8MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-6.smt2             |    0.059s | 20.912MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/check/bignum_rdl2.smt2                |    0.073s | 20.132MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_850.smt2             |    0.077s | 23.868MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/check/bignum_rdl1.smt2                |    0.079s | 19.924MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking04.smt2 |    0.079s | 21.032MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking10.smt2 |    0.082s | 22.248MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-2.smt2 |    0.085s | 21.288MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-4.smt2             |    0.085s | 20.764MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-1.smt2             |    0.089s | 20.38MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-8.smt2             |    0.090s | 21.272MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-10.smt2 |    0.092s | 22.288MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-2.smt2             |    0.094s | 20.648MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1000.smt2             |    0.098s | 23.56MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_250.smt2             |    0.098s | 22.144MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-3.smt2             |    0.100s | 21.048MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-4.smt2             |    0.105s | 21.52MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-1.smt2 |    0.106s | 20.388MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_330.smt2             |    0.107s | 22.892MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking09.smt2 |    0.109s | 21.952MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-6.smt2             |    0.109s | 22.996MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-04.smt2 |    0.113s | 21.32MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-09.smt2 |    0.114s | 22.4MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking03.smt2 |    0.115s | 21.036MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_800.smt2             |    0.116s | 23.668MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-03.smt2 |    0.119s | 21.64MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-2.smt2 |    0.120s | 21.16MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking01.smt2 |    0.121s | 20.392MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-5.smt2             |    0.122s | 21.76MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking06.smt2 |    0.126s | 21.184MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking05.smt2 |    0.127s | 21.364MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-06.smt2 |    0.127s | 21.42MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_700.smt2             |    0.129s | 22.516MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking07.smt2 |    0.130s | 21.628MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-05.smt2 |    0.132s | 21.528MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-7.smt2             |    0.133s | 21.212MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-6.smt2             |    0.133s | 21.98MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-1.smt2             |    0.136s | 20.112MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-5.smt2             |    0.136s | 22.432MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-07.smt2 |    0.137s | 22.012MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_700.smt2             |    0.138s | 23.488MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking08.smt2 |    0.139s | 21.732MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_700.smt2             |    0.142s | 23.736MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking13.smt2 |    0.144s | 22.52MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking15.smt2 |    0.145s | 22.908MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking17.smt2 |    0.145s | 23.104MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-08.smt2 |    0.145s | 22.048MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking11.smt2 |    0.146s | 22.344MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking12.smt2 |    0.147s | 22.264MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-3.smt2 |    0.147s | 23.088MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking16.smt2 |    0.151s | 23.004MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_800.smt2             |    0.153s | 23.976MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-4.smt2             |    0.153s | 21.992MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1400.smt2             |    0.154s | 24.22MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking14.smt2 |    0.160s | 22.784MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-8.smt2             |    0.160s | 22.72MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking18.smt2 |    0.171s | 23.452MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_800.smt2             |    0.179s | 23.776MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_1100.smt2             |    0.179s | 24.464MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_800.smt2              |    0.180s | 23.44MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking20.smt2 |    0.183s | 23.588MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking19.smt2 |    0.183s | 23.564MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking22.smt2 |    0.187s | 24.344MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking21.smt2 |    0.190s | 24.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-9.smt2             |    0.217s | 21.704MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_550.smt2             |    0.221s | 24.78MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-7.smt2             |    0.224s | 22.18MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-20.smt2 |    0.244s | 24.376MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-10.smt2            |    0.252s | 21.96MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-12.smt2            |    0.253s | 22.496MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_1000.smt2            |    0.256s | 24.72MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-13.smt2            |    0.288s | 22.788MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_1100.smt2            |    0.328s | 24.972MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-11.smt2            |    0.329s | 22.272MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_1000.smt2            |    0.347s | 24.62MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_900.smt2             |    0.364s | 24.172MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_800.smt2             |    0.391s | 24.364MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-7.smt2             |    0.392s | 23.576MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_1200.smt2            |    0.416s | 25.068MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_900.smt2              |    0.469s | 24.224MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-5.smt2              |    0.487s | 34.684MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-8.smt2             |    0.526s | 24.484MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_1000.smt2             |    0.572s | 24.716MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-9.smt2             |    0.574s | 23.692MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_950.smt2             |    0.575s | 24.88MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_830.smt2             |    0.581s | 24.32MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-5.base.cvc.smt2    |    0.621s | 50.324MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-14.smt2            |    0.625s | 23.204MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-30.smt2 |    0.627s | 26.932MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_850.smt2             |    0.657s | 24.28MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1300.smt2             |    0.662s | 25.008MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-16.smt2            |    0.685s | 23.572MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-6.base.cvc.smt2    |    0.724s | 56.044MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-15.smt2            |    0.776s | 23.456MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-3.smt2 |    0.835s | 24.78MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_1200.smt2            |    0.838s | 25.644MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-40.smt2 |    0.865s | 28.372MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-18.smt2            |    0.899s | 24.472MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz6_943.smt2              |    0.917s | 24.588MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-5.induction.cvc.smt2 |    0.923s | 66.832MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_900.smt2             |    0.937s | 24.732MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_900.smt2             |    0.938s | 25.116MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-11.smt2            |    0.988s | 25.48MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_1100.smt2            |    1.014s | 25.524MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-17.smt2            |    1.030s | 24.22MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-7.base.cvc.smt2    |    1.046s | 60.264MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-10.smt2            |    1.142s | 24.412MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_1100.smt2            |    1.172s | 25.632MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-19.smt2            |    1.286s | 24.86MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_1000.smt2            |    1.358s | 25.688MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_430.smt2             |    1.451s | 25.5MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-20.smt2            |    1.489s | 25.412MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_1200.smt2            |    1.499s | 26.192MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_1000.smt2            |    1.512s | 26.108MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-12.smt2            |    1.513s | 24.996MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_750.smt2               |    1.542s | 66.32MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-6.induction.cvc.smt2 |    1.580s | 84.296MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-13.smt2            |    1.585s | 26.108MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-9.smt2             |    1.664s | 25.448MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_900.smt2             |    1.741s | 24.932MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_750.smt2               |    1.779s | 65.744MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-7.induction.cvc.smt2 |    1.833s | 88.168MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_1200.smt2            |    1.834s | 26.028MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-9.base.cvc.smt2    |    1.935s | 87.684MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-4.smt2 |    1.953s | 28.136MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-10.smt2             |    2.082s | 60.248MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-9.induction.cvc.smt2 |    2.256s | 124.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-8.base.cvc.smt2    |    2.288s | 83.812MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_750.smt2               |    2.395s | 65.728MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-8.induction.cvc.smt2 |    2.729s | 107.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-10.smt2            |    2.804s | 27.832MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_1100.smt2            |    2.816s | 26.38MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-10.induction.cvc.smt2 |    2.924s | 127.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-11.smt2            |    2.982s | 27.544MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_500.smt2              |    3.047s | 49.62MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_900.smt2             |    3.098s | 25.736MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1200.smt2             |    3.396s | 25.704MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-11.induction.cvc.smt2 |    3.484s | 152.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb02_888.smt2             |    3.493s | 26.336MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb09_934.smt2             |    3.504s | 26.328MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-10.base.cvc.smt2   |    3.512s | 98.452MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-14.smt2            |    3.674s | 27.464MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-60.smt2 |    3.989s | 48.896MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-12.induction.cvc.smt2 |    4.309s | 154.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-14.induction.cvc.smt2 |    4.508s | 184.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-13.induction.cvc.smt2 |    4.537s | 184.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_1100.smt2            |    4.539s | 27.2MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_1000.smt2            |    4.635s | 26.22MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-16.smt2            |    4.675s | 27.444MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb07_397.smt2             |    5.173s | 26.464MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-80.smt2 |    5.363s | 45.104MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-16.induction.cvc.smt2 |    5.384s | 238.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_900.smt2             |    5.926s | 26.912MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb04_1005.smt2            |    6.049s | 27.004MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-15.smt2            |    6.060s | 28.104MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb10_944.smt2             |    6.222s | 27.048MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-15.induction.cvc.smt2 |    6.295s | 224.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-12.smt2            |    6.377s | 29.04MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-18.smt2            |    6.396s | 29.76MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-11.base.cvc.smt2   |    6.497s | 104.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-70.smt2 |    6.592s | 43.956MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_950.smt2             |    6.736s | 26.7MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_930.smt2             |    6.908s | 27.152MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-17.smt2            |    7.654s | 29.54MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-17.induction.cvc.smt2 |    7.673s | 250.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_1100.smt2            |    7.848s | 27.584MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb08_888.smt2             |    8.985s | 27.792MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-18.induction.cvc.smt2 |    9.590s | 275.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-19.smt2            |   10.106s | 30.74MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-19.induction.cvc.smt2 |   10.352s | 300.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-13.base.cvc.smt2   |   10.466s | 117.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-14.smt2            |   11.179s | 31.164MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz5_1234.smt2             |   11.535s | 28.296MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb05_887.smt2             |   11.544s | 27.952MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-12.base.cvc.smt2   |   12.172s | 112.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-90.smt2 |   12.353s | 47.86MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-13.smt2            |   13.656s | 32.412MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-20.induction.cvc.smt2 |   13.773s | 371.0MiB| sat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-15.smt2             |   14.275s | 98.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-14.base.cvc.smt2   |   15.712s | 125.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-20.smt2            |   17.243s | 32.18MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-15.base.cvc.smt2   |   19.306s | 140.0MiB| unsat | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_894.smt2               |   20.017s | 85.712MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-18.smt2            |   20.022s | 35.944MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_2885.smt2            |   20.027s | 207.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-19.base.cvc.smt2   |   20.030s | 228.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-16.smt2            |   20.032s | 35.728MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_3004.smt2            |   20.033s | 206.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb03_1005.smt2            |   20.037s | 29.12MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_800.smt2              |   20.043s | 75.576MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_667.smt2              |   20.046s | 72.152MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-16.base.cvc.smt2   |   20.046s | 177.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_3050.smt2            |   20.047s | 203.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_828.smt2               |   20.047s | 81.272MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_950.smt2               |   20.048s | 90.056MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_1000.smt2            |   20.050s | 29.012MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-20.smt2            |   20.052s | 36.932MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_919.smt2               |   20.053s | 86.44MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_910.smt2               |   20.054s | 88.28MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_700.smt2              |   20.055s | 73.144MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_2895.smt2            |   20.056s | 202.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_862.smt2               |   20.057s | 84.4MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_2990.smt2            |   20.057s | 220.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_850.smt2               |   20.057s | 78.308MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_950.smt2               |   20.058s | 90.332MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_691.smt2              |   20.060s | 73.428MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_969.smt2               |   20.061s | 90.192MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv13_3150.smt2            |   20.061s | 217.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_827.smt2               |   20.062s | 81.164MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_670.smt2              |   20.062s | 72.5MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn2_890.smt2               |   20.063s | 89.136MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_1010.smt2            |   20.063s | 29.58MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-4.smt2 |   20.063s | 46.128MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn3_860.smt2               |   20.064s | 86.58MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_950.smt2               |   20.065s | 89.004MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_2983.smt2            |   20.066s | 200.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv13_3200.smt2            |   20.066s | 216.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_2972.smt2            |   20.066s | 210.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_2905.smt2            |   20.067s | 210.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-5.smt2 |   20.068s | 46.636MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_850.smt2               |   20.069s | 82.352MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-17.smt2            |   20.069s | 36.26MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-17.base.cvc.smt2   |   20.071s | 192.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_2900.smt2            |   20.075s | 207.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv13_3104.smt2            |   20.076s | 207.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-20.base.cvc.smt2   |   20.077s | 224.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_2992.smt2            |   20.078s | 207.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-18.base.cvc.smt2   |   20.078s | 192.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-7.smt2 |   20.082s | 88.604MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_950.smt2               |   20.084s | 90.432MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-20.smt2             |   20.087s | 125.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-19.smt2            |   20.090s | 35.78MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv13_3000.smt2            |   20.091s | 216.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb06_1000.smt2            |   20.092s | 29.664MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/orb01_1059.smt2            |   20.093s | 29.52MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-15.smt2            |   20.094s | 34.428MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-6.smt2 |   20.097s | 56.076MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/abz7_600.smt2              |   20.098s | 66.52MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn1_887.smt2               |   20.099s | 85.444MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-5.smt2 |   20.099s | 80.056MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_2800.smt2            |   20.100s | 206.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/yn4_1000.smt2              |   20.101s | 93.192MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-6.smt2 |   20.103s | 143.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-matrix2x2.pddl.smt2 |   20.103s | 129.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_2900.smt2            |   20.110s | 206.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv14_3000.smt2            |   20.111s | 208.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv11_2988.smt2            |   20.111s | 223.0MiB| timeout | 0 |  |  |
|non-incremental/QF_RDL/scheduling/swv12_3050.smt2            |   20.113s | 221.0MiB| timeout | 0 |  |  |
