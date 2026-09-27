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
Experiment date and time: 2026-09-27 23:58:46 UTC
Job description: Smoke test: verify benchmark miner trigger | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFFPDTNIRA.tar.zst?download=1 | Source list: benchmarks-smoke.txt
Job tag: coz3-https-zenodo.org-records-QF_UFFPDTNIRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 148e396f0d6b5d8c1e98429f53653ba5f25ad35b
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFFPDTNIRA.tar.zst?download=1
Z3 commit message: Restore array extensionality after value-root disequality shortcut (#10936)

Quantified array queries regressed after `is_diseq` began treating
distinct value roots as disequal. `already_diseq` then skipped
extensionality witnesses needed for E-matching and MBQI progress.

- **Scoped disequality check**
  - Keep the value-root shortcut as the default `is_diseq` behavior.
- Add `is_diseq_no_value_check` for callers that require
congruence/atom-based disequality only.

- **Array extensionality**
- Use the no-value-check variant exclusively in
`theory_array_base::already_diseq`.
- Prevent distinct select-value roots from suppressing an extensionality
witness.

```cpp
if (ctx.is_diseq_no_value_check(parent, other)) {
    return true;
}
```

- **Regression coverage**
- Add an SMT-context test asserting that a disequal array pair with
selects rooted at distinct values still emits an array extensionality
axiom.

<!-- START COPILOT CODING AGENT SUFFIX -->

- Fixes #10934

---------

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: NikolajBjorner <3085284+NikolajBjorner@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ef7f8e_generic_float_tests-T-defqtvc__00.smt2 |    0.023s | 19.648MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2cb2b4_generic_float_tests-T-defqtvc__00.smt2 |    0.023s | 19.772MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/OB04-007__float_conversion__floatround.adb_7_47_predicate_check___00.smt2 |    0.030s | 20.084MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_748b19_generic_float_tests-T-defqtvc__00.smt2 |    0.030s | 20.24MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_59_11_disjoint_contract_cases___00.smt2 |    0.038s | 20.048MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_05d13b_generic_float_tests-T-defqtvc__00.smt2 |    0.040s | 19.716MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_140_division_check___00.smt2 |    0.043s | 20.08MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S603-051__dimensions__a-ngelfu.ads_126_32_dimensions.ads_380_4_division_check___00.smt2 |    0.044s | 19.932MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_19_28_division_check___00.smt2 |    0.045s | 19.564MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d00566_generic_float_tests-T-defqtvc__00.smt2 |    0.049s | 19.724MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2d2300_generic_interval_tests-T-defqtvc__00.smt2 |    0.049s | 19.872MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_65_10_predicate_check___00.smt2 |    0.049s | 19.884MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8c7880_generic_float_tests-T-defqtvc__00.smt2 |    0.049s | 19.784MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/N313-022__float_div__example.adb_9_30_division_check___00.smt2 |    0.050s | 19.764MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_847dc3_generic_float_tests-T-defqtvc__00.smt2 |    0.050s | 19.852MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_af6b6a_generic_interval_tests-T-defqtvc__00.smt2 |    0.050s | 19.98MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_f5486e_generic_float_tests-T-defqtvc__00.smt2 |    0.051s | 19.94MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_59_11_complete_contract_cases___00.smt2 |    0.053s | 20.108MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8e174b_generic_float_tests-T-defqtvc__00.smt2 |    0.053s | 20.324MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S320-040__flow_blocking__a-ngelfu.ads_117_36_p.adb_7_4_division_check___00.smt2 |    0.053s | 19.892MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b2ba6e_generic_float_tests-T-defqtvc__00.smt2 |    0.054s | 28.152MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_32_99_division_check___00.smt2 |    0.055s | 19.888MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_420942_generic_float_tests-T-defqtvc__00.smt2 |    0.055s | 20.136MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_63_10_predicate_check___00.smt2 |    0.056s | 19.632MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_95a19c_generic_float_tests-T-defqtvc__00.smt2 |    0.056s | 19.908MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_140_division_check___00.smt2 |    0.057s | 20.332MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a95b75_generic_float_tests-T-defqtvc__00.smt2 |    0.057s | 20.376MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_128_fp_overflow_check___00.smt2 |    0.059s | 30.212MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_69_10_predicate_check___00.smt2 |    0.059s | 28.296MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d97549_generic_float_tests-T-defqtvc__00.smt2 |    0.060s | 19.828MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ef0acb_generic_float_tests-T-defqtvc__00.smt2 |    0.061s | 19.9MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_828bbe_generic_float_tests-T-defqtvc__00.smt2 |    0.062s | 19.756MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c87e9d_generic_float_tests-T-defqtvc__00.smt2 |    0.062s | 28.652MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5adf4f_generic_float_tests-T-defqtvc__00.smt2 |    0.062s | 20.1MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_323ba9_generic_float_tests-T-defqtvc__00.smt2 |    0.063s | 19.848MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_67_10_predicate_check___00.smt2 |    0.063s | 19.9MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S427-002__inline_dim__a-ngelfu.ads_117_36_dimensions.ads_379_4_division_check___00.smt2 |    0.064s | 19.828MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_15_8_complete_contract_cases___00.smt2 |    0.065s | 20.576MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3943ed_generic_float_tests-T-defqtvc__00.smt2 |    0.065s | 28.268MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-012__justification__numerics.ads_18_99_division_check___00.smt2 |    0.066s | 19.852MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_9_17_range_check___00.smt2 |    0.066s | 29.416MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_d9224d_generic_float_tests-T-defqtvc__00.smt2 |    0.068s | 19.872MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/N917-037__prover_sanity_cvc4__floats.adb_6_4_assert___00.smt2 |    0.069s | 28.3MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_eabfeb_generic_float_tests-T-defqtvc__00.smt2 |    0.069s | 20.76MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_127068_generic_float_tests-T-defqtvc__00.smt2 |    0.070s | 19.876MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_29_99_division_check___00.smt2 |    0.071s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_13fa28_generic_float_tests-T-defqtvc__00.smt2 |    0.071s | 20.136MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7d8b1a_generic_float_tests-T-defqtvc__00.smt2 |    0.072s | 30.384MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_9b4426_generic_float_tests-T-defqtvc__00.smt2 |    0.073s | 29.32MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O303-041__flow_pure_implies_null_global__a-ngelfu.ads_127_36_ff.ads_3_1_division_check___00.smt2 |    0.073s | 20.624MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_49_division_check___00.smt2 |    0.073s | 28.292MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_954270_generic_float_tests-T-defqtvc__00.smt2 |    0.073s | 30.248MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5636a0_generic_float_tests-T-defqtvc__00.smt2 |    0.074s | 32.452MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_44_division_check___00.smt2 |    0.077s | 28.172MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O401-034__float_pred__safety_pack.ads_15_8_disjoint_contract_cases___00.smt2 |    0.078s | 20.908MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3acf14_generic_float_tests-T-defqtvc__00.smt2 |    0.078s | 19.848MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_18_17_range_check___00.smt2 |    0.078s | 29.488MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b7409d_generic_float_tests-T-defqtvc__00.smt2 |    0.078s | 28.336MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_13_17_range_check___00.smt2 |    0.079s | 29.508MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_47e135_generic_float_tests-T-defqtvc__00.smt2 |    0.079s | 20.072MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_3a913e_generic_float_tests-T-defqtvc__00.smt2 |    0.079s | 30.136MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_128_fp_overflow_check___00.smt2 |    0.080s | 30.348MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S320-040__flow_blocking__a-ngelfu.ads_126_32_p.adb_7_4_division_check___00.smt2 |    0.084s | 19.836MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_eb2c36_generic_float_tests-T-defqtvc__00.smt2 |    0.084s | 19.844MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_6_17_range_check___00.smt2 |    0.086s | 29.396MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_12_4_precondition___00.smt2 |    0.086s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0c5b8b_generic_float_tests-T-defqtvc__00.smt2 |    0.086s | 32.488MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/proofinuse__lat_long__lat_long.ads_8_34_fp_overflow_check___00.smt2 |    0.090s | 30.108MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/S603-051__dimensions__a-ngelfu.ads_127_36_dimensions.ads_379_4_division_check___00.smt2 |    0.091s | 20.676MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_b0d547_generic_float_tests-T-defqtvc__00.smt2 |    0.094s | 30.544MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2bc3b7_generic_float_tests-T-defqtvc__00.smt2 |    0.095s | 32.464MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5b82bd_generic_float_tests-T-defqtvc__00.smt2 |    0.097s | 30.144MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_14_23_fp_overflow_check___00.smt2 |    0.097s | 30.316MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_26_99_fp_overflow_check___00.smt2 |    0.099s | 30.588MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e70d10_generic_float_tests-T-defqtvc__00.smt2 |    0.100s | 30.372MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_23_99_fp_overflow_check___00.smt2 |    0.100s | 30.284MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_6_20_fp_overflow_check___00.smt2 |    0.100s | 32.78MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_23_99_fp_overflow_check___00.smt2 |    0.100s | 30.276MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e09c2c_reduced_02-T-defqtvc__00.smt2 |    0.102s | 29.972MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c7ccdd_generic_float_tests-T-defqtvc__00.smt2 |    0.104s | 30.276MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_5fd830_generic_float_tests-T-defqtvc__00.smt2 |    0.105s | 28.82MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_1c77b0_generic_interval_tests-T-defqtvc__00.smt2 |    0.108s | 31.928MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/K720-019__floats__pack.adb_5_17_fp_overflow_check___00.smt2 |    0.108s | 30.348MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_15_16_fp_overflow_check___00.smt2 |    0.113s | 30.404MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_20_99_fp_overflow_check___00.smt2 |    0.114s | 30.168MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e14883_generic_interval_tests-T-defqtvc__00.smt2 |    0.115s | 32.132MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7dad36_generic_float_tests-T-defqtvc__00.smt2 |    0.115s | 29.34MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c5e486_generic_float_tests-T-defqtvc__00.smt2 |    0.120s | 32.6MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q522-022__codepeer__proof.ads_11_14_fp_overflow_check___00.smt2 |    0.131s | 30.592MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/O810-001__predicate__safety_pack.ads_17_16_fp_overflow_check___00.smt2 |    0.131s | 30.348MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M603-018__float__testfloat.adb_19_23_fp_overflow_check___00.smt2 |    0.141s | 33.692MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7a5c9e_generic_float_tests-T-defqtvc__00.smt2 |    0.141s | 30.264MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a3ff0b_generic_interval_tests-T-defqtvc__00.smt2 |    0.145s | 32.096MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0f94ea_generic_float_tests-T-defqtvc__00.smt2 |    0.155s | 32.624MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_c7dfd8_generic_interval_tests-T-defqtvc__00.smt2 |    0.163s | 33.42MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_7931fb_generic_interval_tests-T-defqtvc__00.smt2 |    0.163s | 33.44MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6e6909_generic_float_tests-T-defqtvc__00.smt2 |    0.165s | 30.372MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_9fa525_generic_float_tests-T-defqtvc__00.smt2 |    0.176s | 32.624MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/Q607-007__floats__normalize.adb_26_23_range_check___00.smt2 |    0.176s | 37.716MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_826a12_generic_float_tests-T-defqtvc__00.smt2 |    0.185s | 31.764MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a662fe_generic_float_tests-T-defqtvc__00.smt2 |    0.195s | 32.808MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6a34dd_generic_float_tests-T-defqtvc__00.smt2 |    0.201s | 58.04MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cdd303_generic_float_tests-T-defqtvc__00.smt2 |    0.201s | 30.46MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2d90c4_generic_float_tests-T-defqtvc__00.smt2 |    0.201s | 59.812MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0b5f28_generic_interval_tests-T-defqtvc__00.smt2 |    0.205s | 33.48MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_44_fp_overflow_check___00.smt2 |    0.216s | 63.628MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2629df_generic_float_tests-T-defqtvc__00.smt2 |    0.218s | 32.528MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_776e4a_foo-T-defqtvc__00.smt2 |    0.228s | 31.596MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_29f8e7_generic_float_tests-T-defqtvc__00.smt2 |    0.230s | 29.904MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8ff7c6_generic_float_tests-T-defqtvc__00.smt2 |    0.231s | 58.544MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_a33e16_generic_float_tests-T-defqtvc__00.smt2 |    0.232s | 32.732MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/OA26-022__gnatprove_crash__eg.adb_5_16_fp_overflow_check___00.smt2 |    0.235s | 33.44MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_26_99_fp_overflow_check___00.smt2 |    0.245s | 33.46MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ca225b_generic_interval_tests-T-defqtvc__00.smt2 |    0.252s | 32.392MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_29_99_fp_overflow_check___00.smt2 |    0.255s | 33.448MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_aa8637_generic_interval_tests-T-defqtvc__00.smt2 |    0.265s | 32.196MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2475ea_generic_float_tests-T-defqtvc__00.smt2 |    0.279s | 32.644MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cd606f_generic_interval_tests-T-defqtvc__00.smt2 |    0.285s | 48.5MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/MB01-007__floating_point__foo.adb_6_25_fp_overflow_check___00.smt2 |    0.305s | 41.26MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_81dd6b_generic_float_tests-T-defqtvc__00.smt2 |    0.336s | 40.272MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_60f230_generic_interval_tests-T-defqtvc__00.smt2 |    0.479s | 65.72MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_49eeb1_generic_interval_tests-T-defqtvc__00.smt2 |    0.497s | 65.636MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_e155c1_generic_float_tests-T-defqtvc__00.smt2 |    0.522s | 32.748MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_bac632_generic_interval_tests-T-defqtvc__00.smt2 |    0.527s | 61.408MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2b3a3d_generic_interval_tests-T-defqtvc__00.smt2 |    0.540s | 65.672MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_640d9e_generic_float_tests-T-defqtvc__00.smt2 |    0.549s | 36.476MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_74557c_generic_interval_tests-T-defqtvc__00.smt2 |    0.631s | 61.056MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_2df025_generic_float_tests-T-defqtvc__00.smt2 |    0.823s | 78.44MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_759541_generic_interval_tests-T-defqtvc__00.smt2 |    0.823s | 61.356MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_0a0ce0_generic_float_tests-T-defqtvc__00.smt2 |    0.828s | 41.88MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-014__numerics__numerics.ads_29_99_fp_overflow_check___00.smt2 |    0.842s | 80.56MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P912-012__float_zero_numerics__numerics.ads_32_99_fp_overflow_check___00.smt2 |    0.847s | 80.572MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ed697c_generic_float_tests-T-defqtvc__00.smt2 |    0.855s | 77.52MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P909-012__justification__numerics.ads_18_99_fp_overflow_check___00.smt2 |    0.861s | 80.532MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_dd5ac9_generic_float_tests-T-defqtvc__00.smt2 |    0.901s | 32.252MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_ccb32f_generic_float_tests-T-defqtvc__00.smt2 |    0.917s | 77.96MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_07a5cf_generic_interval_tests-T-defqtvc__00.smt2 |    0.928s | 78.836MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__original_sample.ads_15_140_fp_overflow_check___00.smt2 |    1.013s | 87.908MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P315-010__float__sample.ads_13_140_fp_overflow_check___00.smt2 |    1.039s | 87.74MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_382280_generic_float_tests-T-defqtvc__00.smt2 |    1.047s | 275.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_736933_generic_float_tests-T-defqtvc__00.smt2 |    1.161s | 280.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_6dccf4_generic_float_tests-T-defqtvc__00.smt2 |    1.227s | 280.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_cefa4e_generic_float_tests-T-defqtvc__00.smt2 |    1.580s | 131.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_da0327_generic_interval_tests-T-defqtvc__00.smt2 |    1.885s | 62.324MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_452d8a_generic_float_tests-T-defqtvc__00.smt2 |    4.258s | 382.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_8fc10b_generic_float_tests-T-defqtvc__00.smt2 |    4.575s | 380.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_f8d0db_generic_float_tests-T-defqtvc__00.smt2 |    5.876s | 388.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_53_fp_overflow_check___00.smt2 |    7.610s | 100.0MiB| sat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_30ca25_generic_float_tests-T-defqtvc__00.smt2 |   11.073s | 743.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/M809-005__float_basics__why_15e1a4_generic_interval_tests-T-defqtvc__00.smt2 |   16.144s | 407.0MiB| unsat | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_558_19_fp_overflow_check___00.smt2 |   20.047s | 120.0MiB| timeout | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_49_fp_overflow_check___00.smt2 |   20.096s | 400.0MiB| timeout | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_58_fp_overflow_check___00.smt2 |   20.107s | 452.0MiB| timeout | 0 |  |
|non-incremental/QF_UFFPDTNIRA/20200306-Kanig/spark2014bench/P201-069__simulink__simulink_functions.adb_567_19_fp_overflow_check___00.smt2 |   20.145s | 527.0MiB| timeout | 0 |  |
