# .

* SAT 0
* UNSAT 53
* TIMEOUT 64
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/AUFBVFPDTNIRA.tar.zst?download=1 | Source list: benchmarks-q.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-AUFBVFPDTNIRA.tar.
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 500ca46434627ddd2a2b2777edd5160dbbcff211
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/AUFBVFPDTNIRA.tar.zst?download=1
Z3 commit message: Refresh README build badges to active GitHub Actions workflows (#9936)

README build ribbons had drifted from current workflow reality (e.g.,
removed `pyodide.yml`, disabled workflows still shown). This updates the
badge matrix to reflect active workflows only.

- **Scheduled workflows**
- Replaced deprecated `pyodide.yml` badge with active `pyodide-pypi.yml`
(`Pyodide Wheel (PyPI)`).

- **Disabled workflow cleanup**
- Removed badges for disabled workflows, including `coverage.yml` and
disabled agentic workflows (`a3-python`, `build-warning-fixer`,
`code-conventions-analyzer`, `csa-analysis`, `ostrich-benchmark`,
`tactic-to-simplifier`, `zipt-code-reviewer`).

- **Agentic workflows alignment**
- Kept active lock workflows and refreshed the section to include
currently active entries such as `specbot-crash-analyzer`,
`smtlib-benchmark-finder`, and `tptp-benchmark`.

```md
- | [![Pyodide Build](.../pyodide.yml/badge.svg)](.../pyodide.yml) |
+ | [![Pyodide Wheel (PyPI)](.../pyodide-pypi.yml/badge.svg)](.../pyodide-pypi.yml) |
```

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_42_35_range_check___00.smt2 |    0.029s | 20.14MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_46_31_range_check___00.smt2 |    0.032s | 20.044MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_12_27_range_check___00.smt2 |    0.040s | 19.828MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_24_61_range_check___00.smt2 |    0.043s | 19.776MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_18_50_range_check___00.smt2 |    0.043s | 19.704MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_11_19_precondition___00.smt2 |    0.045s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_25_64_contract_case___00.smt2 |    0.045s | 21.468MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_46_20_length_check___00.smt2 |    0.045s | 19.964MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_10_4_precondition___00.smt2 |    0.045s | 20.224MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_9_23_precondition___00.smt2 |    0.046s | 20.424MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_12_4_precondition___00.smt2 |    0.046s | 19.868MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_42_35_length_check___00.smt2 |    0.047s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MC12-026__overlayed_objects__p2.ads_27_22_postcondition___00.smt2 |    0.048s | 20.34MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_24_18_unreachable_branch___00.smt2 |    0.049s | 19.884MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_45_27_range_check___00.smt2 |    0.050s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_45_85_range_check___00.smt2 |    0.050s | 19.872MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_68_78_range_check___00.smt2 |    0.051s | 19.904MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_14_4_precondition___00.smt2 |    0.052s | 20.344MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_38_22_assert___00.smt2 |    0.055s | 20.612MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_26_36_loop_invariant_init___00.smt2 |    0.056s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_50_25_assert___00.smt2 |    0.057s | 21.372MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_62_14_postcondition___00.smt2 |    0.057s | 19.864MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_58_25_assert___00.smt2 |    0.058s | 21.604MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_51_30_unreachable_branch___00.smt2 |    0.058s | 21.128MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_23_9_disjoint_contract_cases___00.smt2 |    0.059s | 20.84MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_22_53_unreachable_branch___00.smt2 |    0.060s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_18_16_loop_invariant_init___00.smt2 |    0.060s | 21.0MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_49_31_range_check___00.smt2 |    0.061s | 21.068MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_19_61_range_check___00.smt2 |    0.061s | 19.9MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_37_76_range_check___00.smt2 |    0.061s | 20.968MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/P816-010__prefix_inline__generic_queue.ads_73_32_some_package.adb_8_4_range_check___00.smt2 |    0.062s | 20.884MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_16_28_assert___00.smt2 |    0.063s | 19.864MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_22_18_unreachable_branch___00.smt2 |    0.063s | 19.872MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_39_13_range_check___00.smt2 |    0.065s | 20.112MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_40_85_range_check___00.smt2 |    0.066s | 19.852MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MC12-024__flow_crash__p.ads_27_22_postcondition___00.smt2 |    0.067s | 20.396MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_52_25_assert___00.smt2 |    0.067s | 21.128MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_54_25_assert___00.smt2 |    0.068s | 21.932MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_73_78_range_check___00.smt2 |    0.069s | 19.844MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_25_22_unreachable_branch___00.smt2 |    0.072s | 19.86MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_15_23_precondition___00.smt2 |    0.072s | 19.912MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/P816-010__prefix_inline__generic_queue.ads_65_32_some_package.adb_8_4_range_check___00.smt2 |    0.073s | 20.916MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/P816-010__prefix_inline__generic_queue.ads_46_32_some_package.adb_8_4_range_check___00.smt2 |    0.086s | 20.876MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_67_66_range_check___00.smt2 |    0.087s | 21.64MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_39_73_range_check___00.smt2 |    0.087s | 21.864MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_13_19_precondition___00.smt2 |    0.089s | 20.084MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_42_23_length_check___00.smt2 |    0.092s | 25.3MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_26_36_loop_invariant_preserv___00.smt2 |    0.094s | 20.072MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_18_16_loop_invariant_preserv___00.smt2 |    0.096s | 21.636MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_29_80_contract_case___00.smt2 |    0.103s | 21.696MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_44_23_length_check___00.smt2 |    0.125s | 25.736MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_38_61_range_check___00.smt2 |    0.128s | 22.152MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_34_19_contract_case___00.smt2 |    0.137s | 21.9MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_29_39_unreachable_branch___00.smt2 |   20.013s | 24.892MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_41_22_assert___00.smt2 |   20.013s | 34.928MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_63_62_unreachable_branch___00.smt2 |   20.013s | 55.704MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MC12-026__overlayed_objects__p2.adb_40_28_fp_overflow_check___00.smt2 |   20.015s | 31.636MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_40_27_unreachable_branch___00.smt2 |   20.015s | 34.172MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_43_52_unreachable_branch___00.smt2 |   20.024s | 25.056MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_28_17_dead_code___00.smt2 |   20.025s | 24.104MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MC12-024__flow_crash__p.adb_32_28_fp_overflow_check___00.smt2 |   20.026s | 31.16MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_35_14_unreachable_branch___00.smt2 |   20.027s | 25.604MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_40_17_unreachable_branch___00.smt2 |   20.027s | 38.84MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_65_27_unreachable_branch___00.smt2 |   20.028s | 55.688MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_43_17_unreachable_branch___00.smt2 |   20.028s | 30.248MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_26_14_unreachable_branch___00.smt2 |   20.030s | 29.06MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_30_14_unreachable_branch___00.smt2 |   20.030s | 25.156MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_68_30_unreachable_branch___00.smt2 |   20.031s | 55.576MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_49_10_dead_code___00.smt2 |   20.031s | 55.252MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_9_23_dead_code___00.smt2 |   20.032s | 30.104MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_39_22_assert___00.smt2 |   20.032s | 34.356MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_45_17_unreachable_branch___00.smt2 |   20.034s | 24.952MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_15_19_assert___00.smt2 |   20.038s | 27.612MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_12_16_dead_code___00.smt2 |   20.041s | 33.08MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_58_17_unreachable_branch___00.smt2 |   20.041s | 42.016MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_65_69_range_check___00.smt2 |   20.042s | 55.576MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_11_16_dead_code___00.smt2 |   20.043s | 31.208MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_73_30_unreachable_branch___00.smt2 |   20.045s | 55.904MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_9_19_assert___00.smt2 |   20.045s | 30.292MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_59_54_unreachable_branch___00.smt2 |   20.046s | 55.452MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_53_30_unreachable_branch___00.smt2 |   20.046s | 55.056MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_39_14_unreachable_branch___00.smt2 |   20.046s | 31.676MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_37_14_unreachable_branch___00.smt2 |   20.047s | 28.24MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_57_17_unreachable_branch___00.smt2 |   20.048s | 56.168MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_78_10_dead_code___00.smt2 |   20.048s | 55.1MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_7_7_dead_code___00.smt2 |   20.048s | 23.496MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_43_27_unreachable_branch___00.smt2 |   20.049s | 35.1MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_13_19_assert___00.smt2 |   20.049s | 25.248MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_71_30_unreachable_branch___00.smt2 |   20.051s | 55.436MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_19_18_unreachable_branch___00.smt2 |   20.051s | 33.336MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.ads_50_55_range_check___00.smt2 |   20.052s | 30.728MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MC12-026__overlayed_objects__p2.adb_39_28_overflow_check___00.smt2 |   20.052s | 30.484MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_69_34_unreachable_branch___00.smt2 |   20.053s | 55.932MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_71_65_unreachable_branch___00.smt2 |   20.053s | 55.424MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.ads_50_9_postcondition___00.smt2 |   20.054s | 49.952MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_41_21_unreachable_branch___00.smt2 |   20.054s | 28.588MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_61_28_unreachable_branch___00.smt2 |   20.054s | 56.116MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_62_25_assert___00.smt2 |   20.058s | 55.828MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_67_27_unreachable_branch___00.smt2 |   20.059s | 55.832MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_74_34_unreachable_branch___00.smt2 |   20.061s | 56.084MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_63_17_unreachable_branch___00.smt2 |   20.061s | 26.976MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_46_21_unreachable_branch___00.smt2 |   20.061s | 28.816MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_57_28_unreachable_branch___00.smt2 |   20.062s | 55.824MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_39_52_unreachable_branch___00.smt2 |   20.064s | 33.564MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_14_16_dead_code___00.smt2 |   20.064s | 29.092MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_56_14_postcondition___00.smt2 |   20.064s | 41.24MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_42_28_unreachable_branch___00.smt2 |   20.066s | 34.404MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_61_14_inconsistent_pre___00.smt2 |   20.077s | 23.74MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_7_7_precondition___00.smt2 |   20.077s | 24.836MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_42_59_unreachable_branch___00.smt2 |   20.078s | 34.108MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MC12-026__overlayed_objects__p.adb_31_28_overflow_check___00.smt2 |   20.078s | 30.384MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_20_22_unreachable_branch___00.smt2 |   20.079s | 34.736MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_76_25_assert___00.smt2 |   20.080s | 56.276MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.adb_55_54_unreachable_branch___00.smt2 |   20.080s | 55.52MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__test_failsafe.adb_11_19_assert___00.smt2 |   20.117s | 523.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/MA16-036__flow_missing_globals__logger.adb_46_20_range_check___00.smt2 |   20.129s | 1049.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFPDTNIRA/20200306-Kanig/spark2014bench/OA21-028__failsafe__failsafe.ads_55_13_inconsistent_pre___00.smt2 |   20.441s | 4180.0MiB| timeout | 0 |  |  |
