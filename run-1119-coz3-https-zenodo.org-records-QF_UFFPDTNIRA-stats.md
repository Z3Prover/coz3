# .

* SAT 22
* UNSAT 128
* TIMEOUT 4
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-07 02:17:40 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFFPDTNIRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_UFFPDTNIRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFFPDTNIRA.tar.zst?download=1
Z3 commit message: Add witness_instantiation simplifier and fix TPTP preprocessing pipeline fixpoint (#10679)

## Summary

Adds a new opt-in TPTP preprocessing pass, ptp.witness_instantiation,
that synthesizes a fresh witness constant for any uninterpreted sort
with no ground term anywhere in the problem, and uses it to instantiate
orall-quantified formulas over that sort. This is sound (all sorts are
non-empty in classical logic) and lets search-time E-matching proceed on
axioms that would otherwise never fire for lack of a ground term to
match against (e.g. ANA068^1.p's FINITE_REAL_INTERVAL_1 axiom,
quantified over eal, with no ground eal term anywhere in the problem).

Also fixes a real bug in the TPTP preprocessing pipeline: it was only
flushed once before check_sat, so simplifiers that add new formulas
(lambda_reify_simplifier, leibniz_simplifier,
witness_instantiation_simplifier) never got a second pass to fully
simplify/rewrite those newly-added formulas. Adds an extra push()/pop(1)
flush right before check_sat, gated on any of these three passes being
enabled, guarded by a ry/catch so a latent lambda_reify_simplifier
sort-mismatch bug (which check_sat's own exception handling already
downgrades gracefully to GaveUp) can't escape as a hard crash.

This fixes ANA068^1.p and 3 of its 4 sibling problems with
ptp.reify_lambda_literals=true (previously all timed out).

## Testing

- Full 5,279-file TPTP higher-order regression confirms both
eify_lambda_literals and witness_instantiation have an inherent
net-negative trade-off at full corpus scale (gains on some problems,
timeouts on many others due to extra instantiation noise), consistent
with eify_lambda_literals's pre-existing regression profile
(historically measured at 53 gained / 591 lost even without this fix).
Both remain opt-in, default off; default behavior is unaffected.
- 99/99 unit tests pass (
inja test-z3 + ./test-z3.exe /a).
- Merged latest origin/master into this branch; rebuilt cleanly and
re-verified ANA068^1.p still solves.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b2ba6e_generic_float_tests-T-defqtvc__00.smt2 |    0.030s | 28.152MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_32_99_division_check___00.smt2 |    0.034s | 19.884MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/N917-037__prover_sanity_cvc4__floats.adb_6_4_assert___00.smt2 |    0.039s | 28.172MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_29_99_division_check___00.smt2 |    0.039s | 19.896MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8c7880_generic_float_tests-T-defqtvc__00.smt2 |    0.039s | 19.992MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_15_8_disjoint_contract_cases___00.smt2 |    0.039s | 20.768MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_127068_generic_float_tests-T-defqtvc__00.smt2 |    0.040s | 19.664MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_af6b6a_generic_interval_tests-T-defqtvc__00.smt2 |    0.041s | 19.992MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/K720-019__floats__pack.adb_5_17_fp_overflow_check___00.smt2 |    0.043s | 30.376MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3a913e_generic_float_tests-T-defqtvc__00.smt2 |    0.044s | 30.148MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b7409d_generic_float_tests-T-defqtvc__00.smt2 |    0.044s | 28.304MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2cb2b4_generic_float_tests-T-defqtvc__00.smt2 |    0.045s | 19.816MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_05d13b_generic_float_tests-T-defqtvc__00.smt2 |    0.046s | 19.676MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_828bbe_generic_float_tests-T-defqtvc__00.smt2 |    0.047s | 19.744MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_13fa28_generic_float_tests-T-defqtvc__00.smt2 |    0.047s | 19.876MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S427-002__inline_dim__a-ngelfu.ads_117_36_dimensions.ads_379_4_division_check___00.smt2 |    0.047s | 19.848MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-012__justification__numerics.ads_18_99_division_check___00.smt2 |    0.047s | 19.884MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_f5486e_generic_float_tests-T-defqtvc__00.smt2 |    0.047s | 19.756MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_47e135_generic_float_tests-T-defqtvc__00.smt2 |    0.047s | 19.892MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3acf14_generic_float_tests-T-defqtvc__00.smt2 |    0.048s | 20.08MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S603-051__dimensions__a-ngelfu.ads_126_32_dimensions.ads_380_4_division_check___00.smt2 |    0.049s | 19.824MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_63_10_predicate_check___00.smt2 |    0.050s | 19.732MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ef7f8e_generic_float_tests-T-defqtvc__00.smt2 |    0.050s | 19.692MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S320-040__flow_blocking__a-ngelfu.ads_126_32_p.adb_7_4_division_check___00.smt2 |    0.051s | 19.68MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_67_10_predicate_check___00.smt2 |    0.051s | 19.876MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d97549_generic_float_tests-T-defqtvc__00.smt2 |    0.051s | 19.932MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_19_28_division_check___00.smt2 |    0.052s | 19.604MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_95a19c_generic_float_tests-T-defqtvc__00.smt2 |    0.052s | 19.852MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2d2300_generic_interval_tests-T-defqtvc__00.smt2 |    0.053s | 19.98MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_954270_generic_float_tests-T-defqtvc__00.smt2 |    0.056s | 30.556MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_44_division_check___00.smt2 |    0.057s | 28.332MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_140_division_check___00.smt2 |    0.058s | 19.988MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8e174b_generic_float_tests-T-defqtvc__00.smt2 |    0.058s | 19.912MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_69_10_predicate_check___00.smt2 |    0.058s | 28.128MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_eb2c36_generic_float_tests-T-defqtvc__00.smt2 |    0.059s | 19.832MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_59_11_disjoint_contract_cases___00.smt2 |    0.060s | 20.128MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_140_division_check___00.smt2 |    0.060s | 20.344MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ef0acb_generic_float_tests-T-defqtvc__00.smt2 |    0.061s | 19.864MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_23_99_fp_overflow_check___00.smt2 |    0.061s | 30.568MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a95b75_generic_float_tests-T-defqtvc__00.smt2 |    0.062s | 19.88MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_15_8_complete_contract_cases___00.smt2 |    0.062s | 20.68MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_13_17_range_check___00.smt2 |    0.063s | 29.364MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_420942_generic_float_tests-T-defqtvc__00.smt2 |    0.063s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d9224d_generic_float_tests-T-defqtvc__00.smt2 |    0.063s | 19.88MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_15_16_fp_overflow_check___00.smt2 |    0.065s | 30.276MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_128_fp_overflow_check___00.smt2 |    0.066s | 30.196MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d00566_generic_float_tests-T-defqtvc__00.smt2 |    0.066s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_59_11_complete_contract_cases___00.smt2 |    0.066s | 20.104MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/OB04-007__float_conversion__floatround.adb_7_47_predicate_check___00.smt2 |    0.067s | 19.868MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c7ccdd_generic_float_tests-T-defqtvc__00.smt2 |    0.067s | 30.288MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/N313-022__float_div__example.adb_9_30_division_check___00.smt2 |    0.069s | 19.892MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5adf4f_generic_float_tests-T-defqtvc__00.smt2 |    0.069s | 19.656MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_323ba9_generic_float_tests-T-defqtvc__00.smt2 |    0.069s | 20.336MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_12_4_precondition___00.smt2 |    0.070s | 19.88MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e70d10_generic_float_tests-T-defqtvc__00.smt2 |    0.070s | 30.412MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S320-040__flow_blocking__a-ngelfu.ads_117_36_p.adb_7_4_division_check___00.smt2 |    0.070s | 19.804MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e09c2c_reduced_02-T-defqtvc__00.smt2 |    0.070s | 29.888MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7d8b1a_generic_float_tests-T-defqtvc__00.smt2 |    0.071s | 30.136MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5b82bd_generic_float_tests-T-defqtvc__00.smt2 |    0.071s | 30.096MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_18_17_range_check___00.smt2 |    0.072s | 29.42MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5fd830_generic_float_tests-T-defqtvc__00.smt2 |    0.072s | 28.92MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_9b4426_generic_float_tests-T-defqtvc__00.smt2 |    0.073s | 29.1MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_748b19_generic_float_tests-T-defqtvc__00.smt2 |    0.073s | 20.164MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_6_17_range_check___00.smt2 |    0.074s | 29.428MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S603-051__dimensions__a-ngelfu.ads_127_36_dimensions.ads_379_4_division_check___00.smt2 |    0.074s | 20.62MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O303-041__flow_pure_implies_null_global__a-ngelfu.ads_127_36_ff.ads_3_1_division_check___00.smt2 |    0.075s | 20.436MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3943ed_generic_float_tests-T-defqtvc__00.smt2 |    0.076s | 28.844MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_26_99_fp_overflow_check___00.smt2 |    0.077s | 30.568MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c87e9d_generic_float_tests-T-defqtvc__00.smt2 |    0.078s | 28.248MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_49_division_check___00.smt2 |    0.079s | 28.604MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5636a0_generic_float_tests-T-defqtvc__00.smt2 |    0.082s | 32.5MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_14_23_fp_overflow_check___00.smt2 |    0.083s | 30.74MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_9_17_range_check___00.smt2 |    0.086s | 29.332MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b0d547_generic_float_tests-T-defqtvc__00.smt2 |    0.087s | 30.24MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_128_fp_overflow_check___00.smt2 |    0.087s | 30.34MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7dad36_generic_float_tests-T-defqtvc__00.smt2 |    0.088s | 29.372MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_23_99_fp_overflow_check___00.smt2 |    0.090s | 30.164MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_65_10_predicate_check___00.smt2 |    0.095s | 19.608MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/proofinuse__lat_long__lat_long.ads_8_34_fp_overflow_check___00.smt2 |    0.095s | 30.116MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c5e486_generic_float_tests-T-defqtvc__00.smt2 |    0.096s | 32.712MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_847dc3_generic_float_tests-T-defqtvc__00.smt2 |    0.099s | 19.908MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_eabfeb_generic_float_tests-T-defqtvc__00.smt2 |    0.101s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_1c77b0_generic_interval_tests-T-defqtvc__00.smt2 |    0.103s | 31.908MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2bc3b7_generic_float_tests-T-defqtvc__00.smt2 |    0.105s | 32.78MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_17_16_fp_overflow_check___00.smt2 |    0.106s | 30.392MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_20_99_fp_overflow_check___00.smt2 |    0.106s | 30.608MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_6_20_fp_overflow_check___00.smt2 |    0.106s | 32.636MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e14883_generic_interval_tests-T-defqtvc__00.smt2 |    0.108s | 31.892MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7a5c9e_generic_float_tests-T-defqtvc__00.smt2 |    0.111s | 30.344MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q522-022__codepeer__proof.ads_11_14_fp_overflow_check___00.smt2 |    0.117s | 30.336MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0f94ea_generic_float_tests-T-defqtvc__00.smt2 |    0.124s | 32.26MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_19_23_fp_overflow_check___00.smt2 |    0.130s | 33.732MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0c5b8b_generic_float_tests-T-defqtvc__00.smt2 |    0.133s | 32.796MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6e6909_generic_float_tests-T-defqtvc__00.smt2 |    0.142s | 30.368MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c7dfd8_generic_interval_tests-T-defqtvc__00.smt2 |    0.162s | 33.472MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_826a12_generic_float_tests-T-defqtvc__00.smt2 |    0.168s | 31.616MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8ff7c6_generic_float_tests-T-defqtvc__00.smt2 |    0.169s | 59.228MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7931fb_generic_interval_tests-T-defqtvc__00.smt2 |    0.172s | 33.444MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cdd303_generic_float_tests-T-defqtvc__00.smt2 |    0.172s | 30.276MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a662fe_generic_float_tests-T-defqtvc__00.smt2 |    0.173s | 32.684MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_9fa525_generic_float_tests-T-defqtvc__00.smt2 |    0.175s | 32.968MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0b5f28_generic_interval_tests-T-defqtvc__00.smt2 |    0.179s | 33.416MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a3ff0b_generic_interval_tests-T-defqtvc__00.smt2 |    0.180s | 32.16MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6a34dd_generic_float_tests-T-defqtvc__00.smt2 |    0.189s | 58.288MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_26_23_range_check___00.smt2 |    0.204s | 37.676MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_29f8e7_generic_float_tests-T-defqtvc__00.smt2 |    0.207s | 29.924MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cd606f_generic_interval_tests-T-defqtvc__00.smt2 |    0.211s | 48.412MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a33e16_generic_float_tests-T-defqtvc__00.smt2 |    0.219s | 32.892MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/OA26-022__gnatprove_crash__eg.adb_5_16_fp_overflow_check___00.smt2 |    0.220s | 33.448MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2629df_generic_float_tests-T-defqtvc__00.smt2 |    0.237s | 32.304MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_44_fp_overflow_check___00.smt2 |    0.238s | 63.384MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_aa8637_generic_interval_tests-T-defqtvc__00.smt2 |    0.239s | 31.944MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_776e4a_foo-T-defqtvc__00.smt2 |    0.239s | 31.508MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_29_99_fp_overflow_check___00.smt2 |    0.241s | 33.448MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_26_99_fp_overflow_check___00.smt2 |    0.243s | 33.444MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2d90c4_generic_float_tests-T-defqtvc__00.smt2 |    0.246s | 59.824MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2475ea_generic_float_tests-T-defqtvc__00.smt2 |    0.268s | 32.712MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ca225b_generic_interval_tests-T-defqtvc__00.smt2 |    0.275s | 32.164MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_6_25_fp_overflow_check___00.smt2 |    0.287s | 41.232MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_81dd6b_generic_float_tests-T-defqtvc__00.smt2 |    0.345s | 40.216MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_60f230_generic_interval_tests-T-defqtvc__00.smt2 |    0.474s | 65.408MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_49eeb1_generic_interval_tests-T-defqtvc__00.smt2 |    0.485s | 65.536MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_bac632_generic_interval_tests-T-defqtvc__00.smt2 |    0.499s | 61.636MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_640d9e_generic_float_tests-T-defqtvc__00.smt2 |    0.520s | 36.408MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2b3a3d_generic_interval_tests-T-defqtvc__00.smt2 |    0.524s | 65.44MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e155c1_generic_float_tests-T-defqtvc__00.smt2 |    0.529s | 32.752MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_74557c_generic_interval_tests-T-defqtvc__00.smt2 |    0.638s | 61.076MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_759541_generic_interval_tests-T-defqtvc__00.smt2 |    0.821s | 61.556MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_32_99_fp_overflow_check___00.smt2 |    0.822s | 80.464MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-012__justification__numerics.ads_18_99_fp_overflow_check___00.smt2 |    0.822s | 80.524MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ed697c_generic_float_tests-T-defqtvc__00.smt2 |    0.837s | 77.452MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2df025_generic_float_tests-T-defqtvc__00.smt2 |    0.842s | 78.224MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_29_99_fp_overflow_check___00.smt2 |    0.847s | 80.784MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0a0ce0_generic_float_tests-T-defqtvc__00.smt2 |    0.853s | 41.372MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ccb32f_generic_float_tests-T-defqtvc__00.smt2 |    0.916s | 77.54MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_dd5ac9_generic_float_tests-T-defqtvc__00.smt2 |    0.921s | 31.976MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_07a5cf_generic_interval_tests-T-defqtvc__00.smt2 |    0.937s | 78.276MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_140_fp_overflow_check___00.smt2 |    0.960s | 87.72MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_382280_generic_float_tests-T-defqtvc__00.smt2 |    1.010s | 275.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_140_fp_overflow_check___00.smt2 |    1.021s | 87.736MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_736933_generic_float_tests-T-defqtvc__00.smt2 |    1.120s | 280.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6dccf4_generic_float_tests-T-defqtvc__00.smt2 |    1.166s | 279.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cefa4e_generic_float_tests-T-defqtvc__00.smt2 |    1.619s | 131.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_da0327_generic_interval_tests-T-defqtvc__00.smt2 |    1.881s | 62.34MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_452d8a_generic_float_tests-T-defqtvc__00.smt2 |    4.159s | 382.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8fc10b_generic_float_tests-T-defqtvc__00.smt2 |    4.558s | 381.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_f8d0db_generic_float_tests-T-defqtvc__00.smt2 |    5.777s | 388.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_53_fp_overflow_check___00.smt2 |    7.575s | 100.0MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_30ca25_generic_float_tests-T-defqtvc__00.smt2 |   10.841s | 743.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_15e1a4_generic_interval_tests-T-defqtvc__00.smt2 |   16.154s | 407.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_19_fp_overflow_check___00.smt2 |   20.069s | 118.0MiB| timeout | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_49_fp_overflow_check___00.smt2 |   20.096s | 400.0MiB| timeout | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_19_fp_overflow_check___00.smt2 |   20.102s | 527.0MiB| timeout | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_58_fp_overflow_check___00.smt2 |   20.124s | 451.0MiB| timeout | 0 |  |
