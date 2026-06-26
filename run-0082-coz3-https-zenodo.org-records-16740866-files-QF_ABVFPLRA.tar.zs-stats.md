# .

* SAT 56
* UNSAT 2
* TIMEOUT 16
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ABVFPLRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_ABVFPLRA.tar.zs
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ABVFPLRA.tar.zst?download=1
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
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_2.smt2 |    0.023s | 20.092MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_336.smt2 |    0.024s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter_iir_true-unreach-call.c_27.smt2 |    0.025s | 19.808MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_5.smt2 |    0.027s | 19.588MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_0.smt2 |    0.028s | 20.144MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0662a_true-unreach-call.c_1.smt2 |    0.028s | 19.724MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_5.smt2 |    0.031s | 20.468MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0883_true-unreach-call.c_1.smt2 |    0.044s | 20.072MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_2.smt2 |    0.052s | 20.836MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_alt_true-unreach-call.c_39.smt2 |    0.053s | 20.124MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0874_true-unreach-call.c_4.smt2 |    0.056s | 19.708MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0685a_true-unreach-call.c_9.smt2 |    0.061s | 19.86MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_185.smt2 |    0.061s | 19.632MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0876_true-unreach-call.c_5.smt2 |    0.062s | 19.604MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_9.smt2 |    0.062s | 20.584MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_0.smt2 |    0.062s | 19.868MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_184.smt2 |    0.062s | 19.632MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_10.smt2 |    0.063s | 20.352MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_3.smt2 |    0.063s | 20.136MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_8.smt2 |    0.063s | 19.648MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_186.smt2 |    0.063s | 19.596MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_alt_true-unreach-call.c_8.smt2 |    0.063s | 19.464MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_335.smt2 |    0.064s | 20.116MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_72.smt2 |    0.064s | 20.4MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_7.smt2 |    0.065s | 20.408MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_3.smt2 |    0.065s | 20.356MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_332.smt2 |    0.065s | 20.132MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_1.smt2 |    0.066s | 19.912MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0870b_true-unreach-call.c_4.smt2 |    0.066s | 19.596MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_182.smt2 |    0.066s | 19.88MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_73.smt2 |    0.066s | 20.416MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_324.smt2 |    0.066s | 19.588MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_56.smt2 |    0.066s | 19.6MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_320.smt2 |    0.066s | 19.616MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0680a_true-unreach-call.c_1.smt2 |    0.067s | 19.624MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_2.smt2 |    0.067s | 20.128MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_1.smt2 |    0.067s | 19.648MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0873a_true-unreach-call.c_2.smt2 |    0.067s | 19.844MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_334.smt2 |    0.067s | 20.596MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_1.smt2 |    0.068s | 20.608MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0660b_true-unreach-call.c_1.smt2 |    0.068s | 19.856MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0883_true-unreach-call.c_2.smt2 |    0.068s | 19.592MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_45.smt2 |    0.068s | 19.628MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_8.smt2 |    0.069s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_323.smt2 |    0.069s | 19.644MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter_iir_true-unreach-call.c_34.smt2 |    0.069s | 19.64MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_10.smt2 |    0.070s | 19.908MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/sin_interpolated_bigrange_loose_true-unreach-call.c_0.smt2 |    0.070s | 19.848MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_poly2_false-unreach-call.c_0.smt2 |    0.071s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0872a_true-unreach-call.c_6.smt2 |    0.072s | 20.412MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/sin_interpolated_index_false-unreach-call.c_0.smt2 |    0.073s | 19.836MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0220b_true-unreach-call.c_0.smt2 |    0.079s | 24.612MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0240b_true-unreach-call.c_0.smt2 |    0.094s | 24.616MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0620b_true-unreach-call.c_7.smt2 |    0.100s | 22.92MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c.p+cfa-reducer.c_AllErrorsAtOnce_Iteration4_TraceCheck_0.smt2 |    9.516s | 98.0MiB| unsat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation_true-unreach-call.c.p+cfa-reducer.c_1.smt2 |   14.726s | 109.0MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/inv_Newton_false-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |   18.390s | 101.0MiB| unsat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation2_true-unreach-call_true-termination.c_0.smt2 |   19.894s | 109.0MiB| sat | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_index_false-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.027s | 149.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_327.smt2 |   20.046s | 420.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation2_true-unreach-call.c.v+cfa-reducer.c_0.smt2 |   20.057s | 111.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter2_alt_true-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |   20.061s | 78.5MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/interpolation_true-unreach-call.c.p+cfa-reducer.c_0.smt2 |   20.063s | 116.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_index_true-unreach-call_true-termination.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.075s | 149.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_329.smt2 |   20.075s | 420.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sin_interpolated_smallrange_true-unreach-call.c_AllErrorsAtOnce_Iteration2_TraceCheck_0.smt2 |   20.084s | 233.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_poly2_false-unreach-call.c_AllErrorsAtOnce_Iteration1_TraceCheck_0.smt2 |   20.085s | 241.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_321.smt2 |   20.089s | 417.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_322.smt2 |   20.093s | 417.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_70.smt2 |   20.093s | 418.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_71.smt2 |   20.100s | 418.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_330.smt2 |   20.100s | 418.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20170501-Heizmann-UltimateAutomizer/filter2_iterated_true-unreach-call.c_331.smt2 |   20.101s | 418.0MiB| timeout | 0 |  |  |
|non-incremental/QF_ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/filter_iir_true-unreach-call.c_0.smt2 |   20.110s | 483.0MiB| timeout | 0 |  |  |
