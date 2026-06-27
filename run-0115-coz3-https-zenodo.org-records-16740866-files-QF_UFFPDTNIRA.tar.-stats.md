# .

* SAT 22
* UNSAT 126
* TIMEOUT 6
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFFPDTNIRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFFPDTNIRA.tar.
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFFPDTNIRA.tar.zst?download=1
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
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_127068_generic_float_tests-T-defqtvc__00.smt2 |    0.027s | 19.784MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d97549_generic_float_tests-T-defqtvc__00.smt2 |    0.030s | 19.904MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_63_10_predicate_check___00.smt2 |    0.031s | 19.572MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_15_8_complete_contract_cases___00.smt2 |    0.033s | 20.448MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5b82bd_generic_float_tests-T-defqtvc__00.smt2 |    0.045s | 30.34MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_828bbe_generic_float_tests-T-defqtvc__00.smt2 |    0.050s | 19.608MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c7ccdd_generic_float_tests-T-defqtvc__00.smt2 |    0.052s | 30.192MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2d2300_generic_interval_tests-T-defqtvc__00.smt2 |    0.054s | 19.844MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_13fa28_generic_float_tests-T-defqtvc__00.smt2 |    0.054s | 20.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_32_99_division_check___00.smt2 |    0.055s | 19.884MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d00566_generic_float_tests-T-defqtvc__00.smt2 |    0.055s | 19.776MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_140_division_check___00.smt2 |    0.056s | 20.14MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_59_11_disjoint_contract_cases___00.smt2 |    0.060s | 19.904MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_44_division_check___00.smt2 |    0.060s | 28.284MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-012__justification__numerics.ads_18_99_division_check___00.smt2 |    0.060s | 19.88MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_420942_generic_float_tests-T-defqtvc__00.smt2 |    0.061s | 20.376MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_19_28_division_check___00.smt2 |    0.061s | 19.588MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_af6b6a_generic_interval_tests-T-defqtvc__00.smt2 |    0.062s | 19.848MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_69_10_predicate_check___00.smt2 |    0.062s | 28.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_12_4_precondition___00.smt2 |    0.062s | 19.624MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S427-002__inline_dim__a-ngelfu.ads_117_36_dimensions.ads_379_4_division_check___00.smt2 |    0.062s | 19.872MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S603-051__dimensions__a-ngelfu.ads_126_32_dimensions.ads_380_4_division_check___00.smt2 |    0.063s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ef0acb_generic_float_tests-T-defqtvc__00.smt2 |    0.064s | 19.804MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_15_8_disjoint_contract_cases___00.smt2 |    0.064s | 20.764MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d9224d_generic_float_tests-T-defqtvc__00.smt2 |    0.065s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S603-051__dimensions__a-ngelfu.ads_127_36_dimensions.ads_379_4_division_check___00.smt2 |    0.066s | 20.628MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_47e135_generic_float_tests-T-defqtvc__00.smt2 |    0.066s | 19.848MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_49_division_check___00.smt2 |    0.067s | 28.3MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b2ba6e_generic_float_tests-T-defqtvc__00.smt2 |    0.068s | 28.332MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_748b19_generic_float_tests-T-defqtvc__00.smt2 |    0.069s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_eb2c36_generic_float_tests-T-defqtvc__00.smt2 |    0.069s | 19.628MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_847dc3_generic_float_tests-T-defqtvc__00.smt2 |    0.069s | 20.068MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_67_10_predicate_check___00.smt2 |    0.070s | 19.836MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8c7880_generic_float_tests-T-defqtvc__00.smt2 |    0.070s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S320-040__flow_blocking__a-ngelfu.ads_126_32_p.adb_7_4_division_check___00.smt2 |    0.071s | 20.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_05d13b_generic_float_tests-T-defqtvc__00.smt2 |    0.071s | 20.28MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_13_17_range_check___00.smt2 |    0.072s | 29.312MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/OB04-007__float_conversion__floatround.adb_7_47_predicate_check___00.smt2 |    0.074s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/N917-037__prover_sanity_cvc4__floats.adb_6_4_assert___00.smt2 |    0.075s | 28.152MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_eabfeb_generic_float_tests-T-defqtvc__00.smt2 |    0.076s | 20.804MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S320-040__flow_blocking__a-ngelfu.ads_117_36_p.adb_7_4_division_check___00.smt2 |    0.076s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O303-041__flow_pure_implies_null_global__a-ngelfu.ads_127_36_ff.ads_3_1_division_check___00.smt2 |    0.076s | 20.624MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_29_99_division_check___00.smt2 |    0.077s | 20.088MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a95b75_generic_float_tests-T-defqtvc__00.smt2 |    0.080s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8e174b_generic_float_tests-T-defqtvc__00.smt2 |    0.083s | 19.756MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_23_99_fp_overflow_check___00.smt2 |    0.086s | 30.052MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_18_17_range_check___00.smt2 |    0.087s | 29.5MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_6_17_range_check___00.smt2 |    0.088s | 29.248MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q522-022__codepeer__proof.ads_11_14_fp_overflow_check___00.smt2 |    0.088s | 30.076MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_59_11_complete_contract_cases___00.smt2 |    0.089s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_128_fp_overflow_check___00.smt2 |    0.092s | 30.024MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3acf14_generic_float_tests-T-defqtvc__00.smt2 |    0.094s | 19.624MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/proofinuse__lat_long__lat_long.ads_8_34_fp_overflow_check___00.smt2 |    0.094s | 30.092MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/N313-022__float_div__example.adb_9_30_division_check___00.smt2 |    0.095s | 19.864MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_323ba9_generic_float_tests-T-defqtvc__00.smt2 |    0.095s | 19.876MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b0d547_generic_float_tests-T-defqtvc__00.smt2 |    0.095s | 30.108MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5adf4f_generic_float_tests-T-defqtvc__00.smt2 |    0.096s | 19.896MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e70d10_generic_float_tests-T-defqtvc__00.smt2 |    0.097s | 30.188MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_26_99_fp_overflow_check___00.smt2 |    0.097s | 30.424MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_140_division_check___00.smt2 |    0.097s | 20.076MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_65_10_predicate_check___00.smt2 |    0.097s | 19.9MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_95a19c_generic_float_tests-T-defqtvc__00.smt2 |    0.099s | 19.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_14_23_fp_overflow_check___00.smt2 |    0.100s | 30.344MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2cb2b4_generic_float_tests-T-defqtvc__00.smt2 |    0.100s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b7409d_generic_float_tests-T-defqtvc__00.smt2 |    0.101s | 28.316MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/K720-019__floats__pack.adb_5_17_fp_overflow_check___00.smt2 |    0.101s | 30.184MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c87e9d_generic_float_tests-T-defqtvc__00.smt2 |    0.102s | 28.336MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e09c2c_reduced_02-T-defqtvc__00.smt2 |    0.103s | 29.836MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_1c77b0_generic_interval_tests-T-defqtvc__00.smt2 |    0.103s | 32.008MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3943ed_generic_float_tests-T-defqtvc__00.smt2 |    0.104s | 28.304MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3a913e_generic_float_tests-T-defqtvc__00.smt2 |    0.104s | 30.136MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0c5b8b_generic_float_tests-T-defqtvc__00.smt2 |    0.106s | 31.976MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e14883_generic_interval_tests-T-defqtvc__00.smt2 |    0.107s | 31.916MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_23_99_fp_overflow_check___00.smt2 |    0.107s | 30.3MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_9b4426_generic_float_tests-T-defqtvc__00.smt2 |    0.108s | 29.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_6_20_fp_overflow_check___00.smt2 |    0.108s | 32.42MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2bc3b7_generic_float_tests-T-defqtvc__00.smt2 |    0.110s | 32.168MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_9_17_range_check___00.smt2 |    0.111s | 29.328MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_17_16_fp_overflow_check___00.smt2 |    0.117s | 30.236MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_954270_generic_float_tests-T-defqtvc__00.smt2 |    0.118s | 30.004MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_128_fp_overflow_check___00.smt2 |    0.127s | 30.144MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5fd830_generic_float_tests-T-defqtvc__00.smt2 |    0.127s | 28.84MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_15_16_fp_overflow_check___00.smt2 |    0.128s | 30.452MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_20_99_fp_overflow_check___00.smt2 |    0.128s | 30.108MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5636a0_generic_float_tests-T-defqtvc__00.smt2 |    0.140s | 32.02MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7dad36_generic_float_tests-T-defqtvc__00.smt2 |    0.148s | 29.472MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7931fb_generic_interval_tests-T-defqtvc__00.smt2 |    0.156s | 32.724MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7a5c9e_generic_float_tests-T-defqtvc__00.smt2 |    0.156s | 30.232MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c7dfd8_generic_interval_tests-T-defqtvc__00.smt2 |    0.158s | 32.896MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a3ff0b_generic_interval_tests-T-defqtvc__00.smt2 |    0.160s | 32.164MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0f94ea_generic_float_tests-T-defqtvc__00.smt2 |    0.160s | 32.1MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_19_23_fp_overflow_check___00.smt2 |    0.160s | 33.44MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6e6909_generic_float_tests-T-defqtvc__00.smt2 |    0.176s | 30.192MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0b5f28_generic_interval_tests-T-defqtvc__00.smt2 |    0.180s | 32.924MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_26_23_range_check___00.smt2 |    0.182s | 37.652MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8ff7c6_generic_float_tests-T-defqtvc__00.smt2 |    0.187s | 59.164MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_826a12_generic_float_tests-T-defqtvc__00.smt2 |    0.195s | 31.236MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/OA26-022__gnatprove_crash__eg.adb_5_16_fp_overflow_check___00.smt2 |    0.196s | 32.864MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_26_99_fp_overflow_check___00.smt2 |    0.201s | 32.68MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cdd303_generic_float_tests-T-defqtvc__00.smt2 |    0.202s | 30.224MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_29f8e7_generic_float_tests-T-defqtvc__00.smt2 |    0.205s | 30.316MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_29_99_fp_overflow_check___00.smt2 |    0.215s | 32.732MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_776e4a_foo-T-defqtvc__00.smt2 |    0.229s | 31.52MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a662fe_generic_float_tests-T-defqtvc__00.smt2 |    0.240s | 32.72MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ca225b_generic_interval_tests-T-defqtvc__00.smt2 |    0.242s | 31.908MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_9fa525_generic_float_tests-T-defqtvc__00.smt2 |    0.248s | 32.42MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a33e16_generic_float_tests-T-defqtvc__00.smt2 |    0.260s | 32.372MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2629df_generic_float_tests-T-defqtvc__00.smt2 |    0.263s | 32.288MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6a34dd_generic_float_tests-T-defqtvc__00.smt2 |    0.276s | 58.752MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_6_25_fp_overflow_check___00.smt2 |    0.284s | 38.42MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ed697c_generic_float_tests-T-defqtvc__00.smt2 |    0.285s | 59.712MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_f5486e_generic_float_tests-T-defqtvc__00.smt2 |    0.320s | 31.864MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_81dd6b_generic_float_tests-T-defqtvc__00.smt2 |    0.323s | 38.032MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e155c1_generic_float_tests-T-defqtvc__00.smt2 |    0.348s | 32.38MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2475ea_generic_float_tests-T-defqtvc__00.smt2 |    0.360s | 32.42MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_49eeb1_generic_interval_tests-T-defqtvc__00.smt2 |    0.499s | 65.184MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_bac632_generic_interval_tests-T-defqtvc__00.smt2 |    0.505s | 61.544MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_60f230_generic_interval_tests-T-defqtvc__00.smt2 |    0.519s | 65.368MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_aa8637_generic_interval_tests-T-defqtvc__00.smt2 |    0.519s | 31.94MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_74557c_generic_interval_tests-T-defqtvc__00.smt2 |    0.538s | 60.708MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cd606f_generic_interval_tests-T-defqtvc__00.smt2 |    0.554s | 61.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_640d9e_generic_float_tests-T-defqtvc__00.smt2 |    0.581s | 35.772MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2b3a3d_generic_interval_tests-T-defqtvc__00.smt2 |    0.608s | 65.16MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2d90c4_generic_float_tests-T-defqtvc__00.smt2 |    0.698s | 73.908MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_140_fp_overflow_check___00.smt2 |    0.842s | 86.516MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-012__justification__numerics.ads_18_99_fp_overflow_check___00.smt2 |    0.846s | 81.92MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_29_99_fp_overflow_check___00.smt2 |    0.854s | 81.896MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_44_fp_overflow_check___00.smt2 |    0.865s | 80.34MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_32_99_fp_overflow_check___00.smt2 |    0.870s | 81.872MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2df025_generic_float_tests-T-defqtvc__00.smt2 |    0.871s | 78.3MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0a0ce0_generic_float_tests-T-defqtvc__00.smt2 |    0.879s | 41.136MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_07a5cf_generic_interval_tests-T-defqtvc__00.smt2 |    0.881s | 77.788MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_140_fp_overflow_check___00.smt2 |    0.915s | 86.532MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ccb32f_generic_float_tests-T-defqtvc__00.smt2 |    0.943s | 78.176MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_dd5ac9_generic_float_tests-T-defqtvc__00.smt2 |    0.996s | 32.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_382280_generic_float_tests-T-defqtvc__00.smt2 |    1.093s | 330.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_452d8a_generic_float_tests-T-defqtvc__00.smt2 |    1.195s | 330.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_736933_generic_float_tests-T-defqtvc__00.smt2 |    1.266s | 330.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ef7f8e_generic_float_tests-T-defqtvc__00.smt2 |    1.302s | 40.212MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cefa4e_generic_float_tests-T-defqtvc__00.smt2 |    1.623s | 131.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_53_fp_overflow_check___00.smt2 |    1.705s | 98.9MiB| sat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6dccf4_generic_float_tests-T-defqtvc__00.smt2 |    4.221s | 390.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8fc10b_generic_float_tests-T-defqtvc__00.smt2 |    4.437s | 386.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_15e1a4_generic_interval_tests-T-defqtvc__00.smt2 |    5.462s | 397.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_759541_generic_interval_tests-T-defqtvc__00.smt2 |    5.545s | 64.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_f8d0db_generic_float_tests-T-defqtvc__00.smt2 |    5.685s | 386.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_30ca25_generic_float_tests-T-defqtvc__00.smt2 |   10.144s | 745.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_da0327_generic_interval_tests-T-defqtvc__00.smt2 |   10.387s | 66.648MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7d8b1a_generic_float_tests-T-defqtvc__00.smt2 |   20.065s | 45.432MiB| timeout | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_19_fp_overflow_check___00.smt2 |   20.073s | 118.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_49_fp_overflow_check___00.smt2 |   20.082s | 399.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c5e486_generic_float_tests-T-defqtvc__00.smt2 |   20.090s | 58.54MiB| timeout | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_58_fp_overflow_check___00.smt2 |   20.117s | 452.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_19_fp_overflow_check___00.smt2 |   20.118s | 511.0MiB| timeout | 0 |  |  |
