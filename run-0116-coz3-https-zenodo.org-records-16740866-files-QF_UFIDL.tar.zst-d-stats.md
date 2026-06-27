# .

* SAT 189
* UNSAT 312
* TIMEOUT 127
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFIDL.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFIDL.tar.zst-d
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFIDL.tar.zst?download=1
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
|non-incremental/QF_UFIDL/uclid/elf.rf6.smt2                  |    0.033s | 20.612MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/32s.smt2                      |    0.036s | 20.92MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.883173.smt2       |    0.036s | 19.568MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.877456.smt2       |    0.038s | 19.5MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.600189.smt2       |    0.039s | 20.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.693723.smt2       |    0.040s | 19.68MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.693706.smt2       |    0.040s | 19.88MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.600173.smt2       |    0.041s | 20.076MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO0.smt2            |    0.042s | 20.024MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.693740.smt2       |    0.042s | 19.496MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8n_s.smt2                    |    0.043s | 20.56MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6_s.smt2                     |    0.043s | 20.424MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.883203.smt2       |    0.043s | 19.648MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc0.smt2 |    0.044s | 20.396MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7_s.smt2                     |    0.044s | 20.372MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10_s.smt2                    |    0.044s | 20.34MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded0.smt2 |    0.045s | 20.572MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10b_s.smt2                   |    0.045s | 20.468MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.877439.smt2       |    0.045s | 19.696MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/set_union0.smt2                 |    0.046s | 20.312MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9n_s.smt2                    |    0.046s | 20.68MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard2.smt2 |    0.046s | 20.564MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6b_s.smt2                    |    0.047s | 20.492MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6n_s.smt2                    |    0.047s | 20.448MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7b_s.smt2                    |    0.047s | 20.696MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/simple_cyclic0.smt2             |    0.048s | 20.164MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery0.smt2 |    0.048s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7n_s.smt2                    |    0.048s | 20.408MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full0.smt2 |    0.049s | 20.104MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8_s.smt2                     |    0.049s | 20.192MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.600181.smt2       |    0.050s | 19.836MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/5s.smt2                        |    0.051s | 20.404MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9_s.smt2                     |    0.051s | 20.308MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/simple_cyclic1.smt2             |    0.052s | 20.688MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full1.smt2 |    0.052s | 20.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard3.smt2 |    0.052s | 20.352MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/6stage-flush.smt2              |    0.054s | 21.832MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/aodv/aodv0.smt2          |    0.054s | 20.316MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6i_s.smt2                    |    0.056s | 21.412MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.rf6.smt2                  |    0.057s | 20.616MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8b_s.smt2                    |    0.058s | 20.412MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/simple_cyclic2.smt2             |    0.059s | 20.82MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.877473.smt2       |    0.059s | 19.472MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard4.smt2 |    0.059s | 20.516MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/simple_cyclic3.smt2             |    0.061s | 20.872MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/27s.smt2                      |    0.062s | 21.004MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.883218.smt2       |    0.062s | 19.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded1.smt2 |    0.064s | 21.208MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10n_s.smt2                   |    0.064s | 20.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/49s.smt2                      |    0.065s | 19.996MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full2.smt2 |    0.065s | 20.656MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bi_s.smt2                   |    0.065s | 21.624MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/elf.rf7.smt2                  |    0.066s | 21.484MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO1.smt2            |    0.066s | 21.12MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded2.smt2 |    0.066s | 21.312MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full3.smt2 |    0.066s | 21.248MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/set_union1.smt2                 |    0.067s | 21.288MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/aodv/aodv3.smt2          |    0.067s | 21.384MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP1.smt2            |    0.067s | 21.456MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/reverse_acyclic0.smt2           |    0.068s | 20.144MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO2.smt2            |    0.068s | 21.436MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6id_s.smt2                   |    0.070s | 21.884MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/44s.smt2                      |    0.071s | 21.416MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/reverse_acyclic2.smt2           |    0.071s | 21.444MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6.smt2                       |    0.071s | 20.932MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/TwoSquares/smtlib.883188.smt2       |    0.071s | 19.524MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded3.smt2 |    0.072s | 21.404MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6n.smt2                      |    0.072s | 21.396MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/46s.smt2                      |    0.073s | 21.232MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP2.smt2            |    0.074s | 21.528MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard7.smt2 |    0.075s | 20.744MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/cache.inv8.smt2               |    0.076s | 21.232MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/reverse_acyclic4.smt2           |    0.077s | 21.704MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP5.smt2            |    0.077s | 21.504MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded4.smt2 |    0.077s | 21.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6i.smt2                      |    0.077s | 21.336MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/simple_cyclic4.smt2             |    0.078s | 20.764MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery1.smt2 |    0.078s | 21.4MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full4.smt2 |    0.078s | 21.732MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard6.smt2 |    0.078s | 20.584MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/aodv/aodv1.smt2          |    0.079s | 20.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP8.smt2            |    0.079s | 21.512MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP3.smt2            |    0.079s | 21.48MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/bug2.smt2                    |    0.079s | 21.348MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard5.smt2 |    0.079s | 20.492MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/set_union2.smt2                 |    0.081s | 21.74MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/DLX/DLX1C0.smt2          |    0.081s | 20.408MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP4.smt2            |    0.081s | 21.656MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete3/bug_file6.smt2                |    0.082s | 22.256MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP6.smt2            |    0.083s | 21.768MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bid_s.smt2                  |    0.083s | 21.98MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9b_s.smt2                    |    0.083s | 20.576MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.tag8.smt2                 |    0.084s | 21.176MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/reverse_acyclic6.smt2           |    0.085s | 21.64MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full6.smt2 |    0.085s | 22.236MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6nidw_s.smt2                 |    0.086s | 22.628MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bidw_s.smt2                 |    0.086s | 22.204MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery2.smt2 |    0.088s | 22.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP9.smt2            |    0.088s | 21.956MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/BRP/BRP7.smt2            |    0.088s | 21.544MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/reverse_acyclic5.smt2           |    0.089s | 21.68MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/dlx1c.smt2                    |    0.090s | 21.78MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/aodv/aodv2.smt2          |    0.090s | 20.988MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RTCL/b13_tf_100/b13_tf_100.smt2     |    0.091s | 23.696MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6.smt2                       |    0.091s | 21.436MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.rf7.smt2                  |    0.093s | 20.94MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6ni.smt2                     |    0.093s | 21.596MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/reverse_acyclic1.smt2           |    0.094s | 21.24MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6ni_s.smt2                   |    0.094s | 21.972MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6b.smt2                      |    0.097s | 21.276MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7idw_s.smt2                  |    0.098s | 22.616MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/reverse_acyclic3.smt2           |    0.099s | 21.448MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery3.smt2 |    0.100s | 22.46MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full5.smt2 |    0.101s | 22.468MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/set_union3.smt2                 |    0.102s | 22.12MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g7.smt2                       |    0.102s | 21.42MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/set_union5.smt2                 |    0.103s | 22.428MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8idw_s.smt2                  |    0.103s | 23.384MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded5.smt2 |    0.104s | 22.616MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7nidw_s.smt2                 |    0.104s | 22.992MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/37s.smt2                      |    0.105s | 22.748MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/DLX/DLX1C2.smt2          |    0.105s | 21.94MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7bidw_s.smt2                 |    0.105s | 22.904MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10idw_s.smt2                 |    0.109s | 24.324MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6idw_s.smt2                  |    0.109s | 21.98MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/dlx1c.rwmem1.smt2             |    0.110s | 21.596MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6i.smt2                      |    0.111s | 21.812MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/dlx1c.rwmem.smt2              |    0.112s | 21.348MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6b.smt2                      |    0.113s | 22.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6bi.smt2                     |    0.114s | 21.596MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/25s.smt2                      |    0.115s | 23.052MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/set_union4.smt2                 |    0.116s | 22.356MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/cxs.smt2                       |    0.117s | 21.436MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6idw.smt2                    |    0.118s | 22.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/fxs-safety.smt2                |    0.119s | 21.348MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/cxs-safety.smt2                |    0.120s | 21.068MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/aodv/aodv4.smt2          |    0.124s | 22.932MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc1.smt2 |    0.125s | 23.076MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10ni_s.smt2                  |    0.125s | 24.688MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/DLX/DLX1C1.smt2          |    0.128s | 22.168MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g7b.smt2                      |    0.128s | 21.972MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6ni.smt2                     |    0.128s | 22.492MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g7n.smt2                      |    0.129s | 21.836MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6bid.smt2                    |    0.129s | 22.148MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8bidw_s.smt2                 |    0.129s | 23.292MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full7.smt2 |    0.130s | 22.972MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6id.smt2                     |    0.130s | 21.88MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10i_s.smt2                   |    0.130s | 23.8MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9bidw_s.smt2                 |    0.130s | 23.944MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10id_s.smt2                  |    0.131s | 24.336MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/elf.rf8.smt2                  |    0.133s | 22.288MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/bug1.smt2                    |    0.134s | 23.068MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bid_s.smt2                 |    0.137s | 24.576MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bi_s.smt2                  |    0.139s | 24.08MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6nid.smt2                    |    0.139s | 22.172MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.tag10.smt2                |    0.140s | 22.368MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/fxs-bp-safety.smt2             |    0.140s | 22.18MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery6.smt2 |    0.140s | 23.668MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard9.smt2 |    0.140s | 20.908MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8nidw_s.smt2                 |    0.142s | 24.408MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery4.smt2 |    0.143s | 23.152MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full8.smt2 |    0.143s | 22.78MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard8.smt2 |    0.143s | 20.872MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6bi.smt2                     |    0.144s | 22.408MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10nid_s.smt2                 |    0.144s | 25.652MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6bidw.smt2                   |    0.144s | 22.408MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g8.smt2                       |    0.144s | 22.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7_i.smt2                     |    0.145s | 21.484MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO3.smt2            |    0.147s | 23.692MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bidw_s.smt2                |    0.147s | 24.668MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete3/bug_file1.smt2                |    0.149s | 25.412MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery5.smt2 |    0.150s | 23.264MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery7.smt2 |    0.151s | 23.636MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6nid_s.smt2                  |    0.151s | 22.924MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/fxs.smt2                       |    0.153s | 22.304MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/cxs-bp-safety.smt2             |    0.154s | 21.92MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded6.smt2 |    0.154s | 24.516MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6_i.smt2                     |    0.156s | 21.32MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9idw_s.smt2                  |    0.157s | 23.644MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g6nidw.smt2                   |    0.157s | 22.256MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard11.smt2 |    0.159s | 21.056MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/aodv/aodv5.smt2          |    0.160s | 23.428MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6n.smt2                      |    0.160s | 22.136MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery8.smt2 |    0.162s | 23.94MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/DLX/DLX1C3.smt2          |    0.165s | 23.7MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/fxs-bp-ex-safety.smt2          |    0.167s | 22.8MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded7.smt2 |    0.168s | 24.7MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.rf8.smt2                  |    0.169s | 21.68MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/cxs-bp.smt2                    |    0.170s | 21.88MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/cxs-bp-ex.smt2                 |    0.172s | 22.184MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/10stage-flush.smt2             |    0.178s | 29.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery9.smt2 |    0.179s | 24.148MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO4.smt2            |    0.179s | 24.764MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6.smt2                       |    0.181s | 21.256MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard10.smt2 |    0.181s | 21.244MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6id.smt2                     |    0.182s | 23.34MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9nidw_s.smt2                 |    0.182s | 25.336MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/fxs-bp-ex-inp-safety.smt2      |    0.185s | 23.272MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete3/bug_file2.smt2                |    0.185s | 26.536MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/DLX/DLX1C4.smt2          |    0.186s | 24.304MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g7idw.smt2                    |    0.186s | 22.956MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery13.smt2 |    0.188s | 25.036MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/fxs-bp.smt2                    |    0.190s | 23.22MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard15.smt2 |    0.192s | 21.56MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery11.smt2 |    0.193s | 24.592MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.tag12.smt2                |    0.195s | 24.08MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete3/bug_file3.smt2                |    0.201s | 27.32MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g8n.smt2                      |    0.202s | 22.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/IC-flush.smt2                  |    0.207s | 32.756MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/cxs-bp-ex-safety.smt2          |    0.208s | 22.208MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g8b.smt2                      |    0.208s | 22.644MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full9.smt2 |    0.209s | 22.944MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO5.smt2            |    0.212s | 25.748MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10nidw_s.smt2                |    0.213s | 26.232MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/aodv/aodv6.smt2          |    0.217s | 23.48MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery12.smt2 |    0.218s | 25.228MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8_i.smt2                     |    0.221s | 21.556MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/DLX/DLX1C5.smt2          |    0.222s | 24.808MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery10.smt2 |    0.225s | 24.564MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9_i.smt2                     |    0.225s | 21.656MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/fxs-bp-ex.smt2                 |    0.226s | 24.152MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO6.smt2            |    0.229s | 26.276MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery15.smt2 |    0.231s | 25.012MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l97.smt2    |    0.232s | 31.748MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9.smt2                       |    0.235s | 21.972MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded13.smt2 |    0.236s | 27.164MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7.smt2                       |    0.237s | 21.72MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7b_i.smt2                    |    0.238s | 21.968MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded11.smt2 |    0.239s | 26.58MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10.smt2                      |    0.239s | 22.12MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f7.smt2                       |    0.239s | 22.496MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10_i.smt2                    |    0.247s | 21.984MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8.smt2                       |    0.247s | 21.88MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6n_i.smt2                    |    0.248s | 21.84MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard12.smt2 |    0.249s | 21.448MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6nid.smt2                    |    0.252s | 24.132MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l32.smt2    |    0.257s | 32.16MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded9.smt2 |    0.257s | 26.504MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/fxs-bp-ex-inp.smt2             |    0.261s | 24.728MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6b_i.smt2                    |    0.262s | 21.868MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/cxs-bp-ex-inp.smt2             |    0.263s | 22.484MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g7bidw.smt2                   |    0.268s | 23.584MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery14.smt2 |    0.273s | 25.436MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO7.smt2            |    0.274s | 26.584MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded10.smt2 |    0.277s | 38.024MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/cxs-bp-ex-inp-safety.smt2      |    0.280s | 22.236MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f7b.smt2                      |    0.283s | 23.424MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc2.smt2 |    0.286s | 26.964MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery16.smt2 |    0.287s | 40.02MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g9.smt2                       |    0.287s | 22.72MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9b_i.smt2                    |    0.290s | 22.412MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete3/bug_file4.smt2                |    0.292s | 30.428MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g7nidw.smt2                   |    0.294s | 23.708MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l22.smt2    |    0.298s | 28.02MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded8.smt2 |    0.298s | 37.908MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO9.smt2            |    0.301s | 26.7MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/cache.inv10.smt2              |    0.303s | 22.456MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded15.smt2 |    0.303s | 27.492MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6bid.smt2                    |    0.304s | 23.972MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g8idw.smt2                    |    0.306s | 23.948MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard13.smt2 |    0.306s | 21.492MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/OOO/OOO8.smt2            |    0.311s | 26.82MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded12.smt2 |    0.318s | 37.98MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8b_i.smt2                    |    0.323s | 22.496MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l75.smt2    |    0.325s | 30.568MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l44.smt2    |    0.327s | 34.712MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded16.smt2 |    0.335s | 37.98MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8b.smt2                      |    0.337s | 22.86MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f7n.smt2                      |    0.338s | 23.384MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10.smt2                      |    0.354s | 23.288MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc8.smt2 |    0.361s | 29.344MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded14.smt2 |    0.364s | 38.156MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10b.smt2                     |    0.365s | 24.34MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7n_i.smt2                    |    0.366s | 22.432MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/43s.smt2                      |    0.367s | 24.26MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10n_i.smt2                   |    0.368s | 23.068MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9n_i.smt2                    |    0.369s | 22.924MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc3.smt2 |    0.374s | 29.6MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc6.smt2 |    0.374s | 29.144MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc4.smt2 |    0.378s | 29.668MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6n.smt2                      |    0.391s | 22.808MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7b.smt2                      |    0.391s | 22.372MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10b.smt2                     |    0.400s | 23.392MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8n_i.smt2                    |    0.403s | 22.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc5.smt2 |    0.406s | 29.288MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc7.smt2 |    0.411s | 29.528MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g9n.smt2                      |    0.413s | 23.56MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/8stage-flush.smt2              |    0.415s | 23.952MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8n.smt2                      |    0.417s | 23.632MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery17.smt2 |    0.421s | 25.424MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6b.smt2                      |    0.421s | 22.196MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10b_i.smt2                   |    0.425s | 23.036MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6idw.smt2                    |    0.428s | 24.692MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete3/bug_file5.smt2                |    0.429s | 27.912MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/elf.rf9.smt2                  |    0.436s | 24.464MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/bakery/LamportBakery18.smt2 |    0.436s | 38.784MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g8bidw.smt2                   |    0.436s | 24.908MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10n.smt2                     |    0.468s | 23.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7n.smt2                      |    0.471s | 22.756MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6nidw.smt2                   |    0.472s | 25.332MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l10.smt2    |    0.480s | 38.304MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l94.smt2    |    0.483s | 34.408MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9n.smt2                      |    0.485s | 23.98MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard16.smt2 |    0.485s | 21.916MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/RDS/sorted_list_insert_noalloc9.smt2 |    0.494s | 29.42MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f6bidw.smt2                   |    0.495s | 25.48MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/22s.smt2                      |    0.500s | 24.412MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.tag14.smt2                |    0.509s | 26.952MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10i.smt2                     |    0.514s | 24.296MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g8nidw.smt2                   |    0.521s | 24.82MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9b.smt2                      |    0.531s | 23.256MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l76.smt2    |    0.536s | 33.508MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard17.smt2 |    0.539s | 21.924MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard18.smt2 |    0.544s | 22.26MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6i.smt2                      |    0.548s | 22.84MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10idw.smt2                   |    0.552s | 26.736MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard14.smt2 |    0.553s | 21.824MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g9b.smt2                      |    0.577s | 23.752MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard19.smt2 |    0.582s | 22.244MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l08.smt2    |    0.610s | 37.204MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10bi.smt2                    |    0.620s | 25.344MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g9idw.smt2                    |    0.628s | 25.624MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/q2.10.smt2                    |    0.648s | 21.952MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6i_i.smt2                    |    0.652s | 22.872MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f8.smt2                       |    0.654s | 23.628MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l12.smt2    |    0.667s | 37.024MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g9bidw.smt2                   |    0.669s | 26.228MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l29.smt2    |    0.725s | 39.952MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10bid.smt2                   |    0.730s | 27.016MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6idw_i.smt2                  |    0.731s | 23.352MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6idw.smt2                    |    0.747s | 23.324MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l25.smt2    |    0.751s | 52.72MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6id.smt2                     |    0.772s | 23.264MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6id_i.smt2                   |    0.786s | 23.216MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l91.smt2    |    0.811s | 43.976MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l05.smt2    |    0.818s | 58.52MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/UCLID-pred/ibm_cache/ibm_cache_full_q_unbounded17.smt2 |    0.821s | 28.136MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l02.smt2    |    0.852s | 55.112MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.rf9.smt2                  |    0.855s | 22.96MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6nidw_i.smt2                 |    0.863s | 24.304MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/ooo.tag15.smt2               |    0.876s | 28.46MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l09.smt2    |    0.897s | 60.104MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g9nidw.smt2                   |    0.902s | 26.328MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10n.smt2                     |    0.910s | 24.972MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l06.smt2    |    0.917s | 60.176MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l00.smt2    |    0.917s | 54.376MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l01.smt2    |    0.919s | 54.172MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10bidw.smt2                  |    0.949s | 28.164MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bid_i.smt2                  |    0.967s | 24.048MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bi.smt2                     |    0.976s | 23.676MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bi_i.smt2                   |    0.982s | 23.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l03.smt2    |    0.984s | 56.52MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/mathsat/EufLaArithmetic/vhard/vhard20.smt2 |    1.004s | 22.724MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bidw.smt2                   |    1.021s | 24.524MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l04.smt2    |    1.023s | 58.26MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l50.smt2    |    1.028s | 62.832MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6nid_i.smt2                  |    1.045s | 24.328MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l38.smt2    |    1.048s | 59.2MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f8b.smt2                      |    1.051s | 25.312MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/ooo.tag17.smt2               |    1.059s | 31.14MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10nid.smt2                   |    1.063s | 27.324MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/cache.inv12.smt2              |    1.102s | 23.952MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10ni.smt2                    |    1.120s | 25.68MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10id.smt2                    |    1.131s | 26.304MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6nid.smt2                    |    1.166s | 25.192MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6ni_i.smt2                   |    1.199s | 23.864MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f7idw.smt2                    |    1.253s | 26.944MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bid.smt2                    |    1.253s | 24.3MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f7nidw.smt2                   |    1.270s | 27.984MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f7bidw.smt2                   |    1.275s | 28.172MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6nidw.smt2                   |    1.292s | 25.552MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l59.smt2    |    1.295s | 66.512MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete/9stage-flush.smt2              |    1.312s | 25.456MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f8n.smt2                      |    1.324s | 25.664MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7idw_i.smt2                  |    1.383s | 24.44MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6ni.smt2                     |    1.408s | 24.82MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l18.smt2    |    1.451s | 76.744MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l17.smt2    |    1.463s | 86.096MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l54.smt2    |    1.467s | 65.516MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l27.smt2    |    1.469s | 65.308MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7nidw.smt2                   |    1.524s | 25.936MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7idw.smt2                    |    1.547s | 24.616MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7bidw.smt2                   |    1.560s | 25.256MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l21.smt2    |    1.598s | 76.944MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l20.smt2    |    1.601s | 74.604MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l11.smt2    |    1.619s | 86.392MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c6bidw_i.smt2                 |    1.635s | 24.708MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7nidw_i.smt2                 |    1.638s | 25.596MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c7bidw_i.smt2                 |    1.660s | 25.296MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f9.smt2                       |    1.698s | 25.504MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l35.smt2    |    1.716s | 97.94MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l30.smt2    |    1.774s | 96.28MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l34.smt2    |    1.778s | 97.08MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l19.smt2    |    1.796s | 91.048MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l33.smt2    |    1.801s | 96.424MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l15.smt2    |    1.821s | 90.928MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8bidw.smt2                   |    1.879s | 26.524MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9idw_i.smt2                  |    1.921s | 26.24MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/g10nidw.smt2                  |    1.966s | 28.416MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l63.smt2    |    1.971s | 97.216MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8idw.smt2                    |    1.994s | 25.448MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8bidw_i.smt2                 |    2.099s | 26.232MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/ooo.tag19.smt2               |    2.155s | 35.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l56.smt2    |    2.200s | 109.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l45.smt2    |    2.297s | 105.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8idw_i.smt2                  |    2.297s | 25.376MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8nidw_i.smt2                 |    2.329s | 26.664MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l64.smt2    |    2.351s | 100.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l51.smt2    |    2.353s | 105.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l40.smt2    |    2.399s | 107.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9nidw_i.smt2                 |    2.470s | 27.524MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c8nidw.smt2                   |    2.518s | 28.148MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l42.smt2    |    2.585s | 114.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l41.smt2    |    2.689s | 109.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l37.smt2    |    2.783s | 109.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f8idw.smt2                    |    2.894s | 29.68MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bidw.smt2                  |    2.898s | 28.872MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10nid_i.smt2                 |    2.905s | 29.008MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l80.smt2    |    2.954s | 123.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9idw.smt2                    |    3.028s | 26.46MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l67.smt2    |    3.040s | 122.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10ni_i.smt2                  |    3.044s | 28.192MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f9n.smt2                      |    3.100s | 27.9MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l77.smt2    |    3.162s | 123.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l36.smt2    |    3.184s | 129.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l60.smt2    |    3.246s | 133.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10id.smt2                    |    3.352s | 27.452MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9bidw.smt2                   |    3.445s | 27.884MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l88.smt2    |    3.455s | 132.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bid_i.smt2                 |    3.457s | 28.712MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9bidw_i.smt2                 |    3.491s | 27.652MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l81.smt2    |    3.565s | 136.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l86.smt2    |    3.567s | 132.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l74.smt2    |    3.585s | 156.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10nidw.smt2                  |    3.611s | 30.772MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f9b.smt2                      |    3.685s | 27.924MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f8nidw.smt2                   |    3.685s | 31.668MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l95.smt2    |    3.731s | 180.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c9nidw.smt2                   |    3.734s | 30.24MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l84.smt2    |    3.759s | 172.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l78.smt2    |    3.782s | 178.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/q2.12.smt2                    |    3.817s | 23.352MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/elf.rf10.smt2                 |    3.920s | 27.788MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l61.smt2    |    3.924s | 183.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l69.smt2    |    3.965s | 172.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10nid.smt2                   |    3.993s | 31.192MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l92.smt2    |    4.029s | 198.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l85.smt2    |    4.050s | 175.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10idw_i.smt2                 |    4.094s | 27.7MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l99.smt2    |    4.118s | 192.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/ooo.rf10.smt2                 |    4.138s | 25.228MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bi_i.smt2                  |    4.194s | 28.408MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bidw_i.smt2                |    4.223s | 29.048MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10ni.smt2                    |    4.256s | 30.368MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l89.smt2    |    4.259s | 196.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l70.smt2    |    4.264s | 171.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l83.smt2    |    4.272s | 178.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10i_i.smt2                   |    4.332s | 27.024MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10idw.smt2                   |    4.339s | 27.972MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bid.smt2                   |    4.377s | 29.06MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l55.smt2    |    4.385s | 195.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l90.smt2    |    4.476s | 178.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l66.smt2    |    4.535s | 169.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l57.smt2    |    4.537s | 205.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l43.smt2    |    4.538s | 205.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10bi.smt2                    |    4.546s | 28.62MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10nidw_i.smt2                |    4.564s | 29.708MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l72.smt2    |    4.637s | 206.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f8bidw.smt2                   |    4.680s | 31.788MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10i.smt2                     |    4.773s | 28.952MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/cache.inv14.smt2              |    4.888s | 26.064MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10i.smt2                     |    5.042s | 27.12MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10.smt2                      |    5.652s | 28.356MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/c10id_i.smt2                  |    5.699s | 27.86MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l47.smt2    |    5.779s | 256.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f9bidw.smt2                   |    6.217s | 35.356MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10bi.smt2                    |    6.568s | 32.008MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10n.smt2                     |    6.569s | 31.38MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/q2.14.smt2                    |    6.592s | 24.572MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l79.smt2    |    7.210s | 360.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10b.smt2                     |    7.416s | 30.988MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f9idw.smt2                    |    8.059s | 33.62MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l96.smt2    |    8.091s | 412.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l46.smt2    |    8.244s | 41.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10ni.smt2                    |    8.350s | 32.14MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l53.smt2    |    8.590s | 425.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l28.smt2    |    9.010s | 35.456MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/cache.inv15.smt2             |    9.048s | 27.3MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l07.smt2    |    9.907s | 56.12MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f9nidw.smt2                   |   10.014s | 36.14MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l65.smt2    |   10.759s | 425.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10id.smt2                    |   11.727s | 34.124MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l93.smt2    |   12.746s | 493.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10bid.smt2                   |   13.610s | 37.476MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l87.smt2    |   13.630s | 667.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l98.smt2    |   13.872s | 738.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10nid.smt2                   |   14.322s | 38.288MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10bidw.smt2                  |   15.607s | 41.052MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/cache.inv16.smt2             |   18.538s | 29.348MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/ooo.rf11.smt2                |   18.896s | 28.284MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10nidw.smt2                  |   19.888s | 40.816MiB| unsat | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f40.smt2    |   20.012s | 36.704MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f59.smt2    |   20.012s | 34.552MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f24.smt2    |   20.014s | 35.376MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f07.smt2    |   20.014s | 31.708MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f53.smt2    |   20.014s | 32.888MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f50.smt2    |   20.014s | 34.572MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l52.smt2    |   20.016s | 66.82MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f36.smt2    |   20.016s | 38.48MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f45.smt2    |   20.017s | 34.228MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f43.smt2    |   20.018s | 32.96MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f82.smt2    |   20.018s | 84.128MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f95.smt2    |   20.018s | 65.912MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f56.smt2    |   20.021s | 57.412MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f34.smt2    |   20.022s | 31.932MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f64.smt2    |   20.022s | 36.196MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f77.smt2    |   20.022s | 38.16MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f16.smt2    |   20.027s | 34.032MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f63.smt2    |   20.027s | 34.336MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f05.smt2    |   20.028s | 36.776MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f35.smt2    |   20.028s | 32.912MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/q2.18.smt2                    |   20.030s | 29.528MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f37.smt2    |   20.030s | 32.652MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l26.smt2    |   20.035s | 68.112MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f28.smt2    |   20.035s | 37.372MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/cache.inv17.smt2             |   20.035s | 30.972MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/q2.40.smt2                    |   20.036s | 43.552MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f92.smt2    |   20.036s | 62.06MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/pete2/f10idw.smt2                   |   20.036s | 37.868MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f21.smt2    |   20.037s | 36.02MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f29.smt2    |   20.038s | 38.136MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f44.smt2    |   20.039s | 33.284MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f46.smt2    |   20.039s | 34.128MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f39.smt2    |   20.039s | 35.456MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/ooo.rf13.smt2                |   20.039s | 32.104MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/q2.20.smt2                    |   20.040s | 31.008MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f03.smt2    |   20.040s | 35.064MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f65.smt2    |   20.041s | 42.328MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f17.smt2    |   20.041s | 53.072MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f47.smt2    |   20.041s | 40.532MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f27.smt2    |   20.041s | 36.74MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f41.smt2    |   20.041s | 39.46MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f85.smt2    |   20.042s | 61.416MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f61.smt2    |   20.043s | 35.3MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f58.smt2    |   20.044s | 33.476MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l68.smt2    |   20.047s | 50.292MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l62.smt2    |   20.048s | 81.204MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l39.smt2    |   20.048s | 66.628MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f87.smt2    |   20.049s | 89.332MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f68.smt2    |   20.049s | 39.332MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l49.smt2    |   20.050s | 106.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f18.smt2    |   20.050s | 33.14MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f67.smt2    |   20.051s | 32.2MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f54.smt2    |   20.051s | 34.48MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f66.smt2    |   20.051s | 34.74MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f49.smt2    |   20.051s | 33.728MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f42.smt2    |   20.052s | 34.852MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f08.smt2    |   20.052s | 36.012MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f72.smt2    |   20.052s | 34.656MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/cache.inv18.smt2             |   20.052s | 32.068MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f71.smt2    |   20.053s | 41.012MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f94.smt2    |   20.053s | 57.06MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f51.smt2    |   20.053s | 37.376MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f57.smt2    |   20.053s | 34.468MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/ooo.rf12.smt2                |   20.053s | 30.16MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f78.smt2    |   20.054s | 39.104MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f20.smt2    |   20.054s | 37.808MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l13.smt2    |   20.054s | 64.568MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f15.smt2    |   20.054s | 32.788MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f00.smt2    |   20.054s | 35.764MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f30.smt2    |   20.055s | 37.292MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f93.smt2    |   20.055s | 56.908MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f80.smt2    |   20.055s | 39.076MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l48.smt2    |   20.055s | 80.656MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f84.smt2    |   20.055s | 35.644MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f38.smt2    |   20.055s | 53.236MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l14.smt2    |   20.055s | 68.572MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f76.smt2    |   20.056s | 33.684MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l24.smt2    |   20.056s | 54.444MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid2/elf.rf12.smt2                |   20.056s | 39.28MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/uclid/q2.30.smt2                    |   20.057s | 35.64MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f52.smt2    |   20.058s | 34.36MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l16.smt2    |   20.058s | 76.88MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f31.smt2    |   20.058s | 35.796MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f10.smt2    |   20.058s | 36.184MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f23.smt2    |   20.058s | 36.596MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l58.smt2    |   20.059s | 53.448MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f26.smt2    |   20.059s | 39.332MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l73.smt2    |   20.059s | 70.72MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f74.smt2    |   20.060s | 41.712MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l71.smt2    |   20.060s | 75.396MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f33.smt2    |   20.060s | 33.86MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f70.smt2    |   20.060s | 35.176MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f04.smt2    |   20.061s | 34.736MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f96.smt2    |   20.061s | 62.116MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f90.smt2    |   20.062s | 36.724MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f14.smt2    |   20.062s | 35.92MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f32.smt2    |   20.062s | 52.724MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f01.smt2    |   20.064s | 38.54MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f09.smt2    |   20.065s | 36.768MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f22.smt2    |   20.067s | 34.868MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f89.smt2    |   20.067s | 33.84MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f98.smt2    |   20.068s | 52.728MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f83.smt2    |   20.068s | 84.896MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f99.smt2    |   20.069s | 52.724MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f75.smt2    |   20.081s | 34.248MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f69.smt2    |   20.082s | 37.532MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f88.smt2    |   20.083s | 34.608MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f60.smt2    |   20.083s | 38.92MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f06.smt2    |   20.084s | 36.46MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f81.smt2    |   20.084s | 34.244MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f48.smt2    |   20.084s | 34.456MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f11.smt2    |   20.084s | 38.48MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f12.smt2    |   20.086s | 34.436MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l31.smt2    |   20.088s | 103.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f73.smt2    |   20.088s | 39.2MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f62.smt2    |   20.089s | 40.34MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f25.smt2    |   20.091s | 34.492MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f97.smt2    |   20.092s | 89.532MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f19.smt2    |   20.092s | 36.452MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l23.smt2    |   20.092s | 71.276MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f13.smt2    |   20.094s | 36.136MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f02.smt2    |   20.094s | 44.308MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f91.smt2    |   20.095s | 36.42MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f55.smt2    |   20.095s | 34.352MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f79.smt2    |   20.095s | 33.404MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_f86.smt2    |   20.103s | 188.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFIDL/20210312-Bouvier/vlsat3_l82.smt2    |   20.118s | 249.0MiB| timeout | 0 |  |  |
