# .

* SAT 35
* UNSAT 9
* TIMEOUT 18
* UNKNOWN 16

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/ANIA.tar.zst?download=1 | Source list: benchmarks-q.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-ANIA.tar.zst-downl
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 500ca46434627ddd2a2b2777edd5160dbbcff211
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/ANIA.tar.zst?download=1
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
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/dancing.i_1.smt2 |    0.024s | 20.624MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/sll2n_insert_unequal.i_75.smt2 |    0.026s | 20.872MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/packet_filter.i_11.smt2 |    0.026s | 20.876MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0237.i_9.smt2 |    0.026s | 21.368MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/prefixsum_rec.c_12.smt2 |    0.029s | 20.888MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0237.i_6.smt2 |    0.029s | 20.612MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/packet_filter.i_8.smt2 |    0.030s | 20.856MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/packet_filter.i_10.smt2 |    0.031s | 20.62MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_8.smt2 |    0.031s | 20.876MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/prefixsum_rec.c_11.smt2 |    0.031s | 20.676MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_5.smt2 |    0.031s | 20.876MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2-2.i_1.smt2 |    0.032s | 21.156MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_4.smt2 |    0.033s | 20.904MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/sll2n_insert_unequal.i_74.smt2 |    0.035s | 20.876MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_7.smt2 |    0.037s | 21.12MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/packet_filter.i_7.smt2 |    0.037s | 20.624MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0237.i_7.smt2 |    0.038s | 20.88MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_4.smt2 |    0.039s | 20.892MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_53.smt2 |    0.040s | 20.92MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_52.smt2 |    0.041s | 21.128MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/sll2n_insert_unequal.i_76.smt2 |    0.041s | 20.996MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_10.smt2 |    0.042s | 20.616MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_1.smt2 |    0.042s | 20.92MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_57.smt2 |    0.043s | 21.124MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/packet_filter.i_6.smt2 |    0.043s | 20.772MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0237.i_17.smt2 |    0.044s | 20.872MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0237.i_16.smt2 |    0.044s | 20.912MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_56.smt2 |    0.044s | 20.904MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_55.smt2 |    0.045s | 20.604MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0237.i_20.smt2 |    0.045s | 20.92MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_5.smt2 |    0.046s | 20.896MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/packet_filter.i_4.smt2 |    0.046s | 20.612MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_9.smt2 |    0.046s | 20.62MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/packet_filter.i_5.smt2 |    0.046s | 20.768MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_9.smt2 |    0.046s | 20.876MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/running_example.i_6.smt2 |    0.047s | 21.128MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0234-1.i_50.smt2 |    0.047s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_2.smt2 |    0.047s | 20.884MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_11.smt2 |    0.047s | 21.068MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/packet_filter.i_9.smt2 |    0.047s | 20.616MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_10.smt2 |    0.048s | 20.84MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/test-0237.i_8.smt2 |    0.048s | 20.808MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_3.smt2 |    0.049s | 20.92MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2-2.i_2.smt2 |    0.049s | 21.108MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/vogal-2.i_6.smt2 |    0.051s | 20.916MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_4.smt2 |    0.054s | 20.896MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_3.smt2 |    0.067s | 21.112MiB| sat | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/array_1-2.c_AllErrorsAtOnce_Iteration3_0.smt2 |    0.084s | 21.652MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/mcmillan2006.i_AllErrorsAtOnce_Iteration5_0.smt2 |    0.116s | 22.916MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/condm.c_AllErrorsAtOnce_Iteration9_0.smt2 |    0.151s | 24.14MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/array_doub_access_init_const.c_AllErrorsAtOnce_Iteration3_0.smt2 |    0.223s | 26.608MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/array_shadowinit.i_AllErrorsAtOnce_Iteration5_0.smt2 |    0.800s | 30.02MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2-2.i_0.smt2 |    1.002s | 102.0MiB| sat | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/benchmark52_polynomial.i_AllErrorsAtOnce_Iteration3_0.smt2 |    1.044s | 22.196MiB| unknown | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/dll2n_insert_unequal.i_5.smt2 |    1.486s | 35.096MiB| sat | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/sep20-1.i_AllErrorsAtOnce_Iteration23_0.smt2 |    1.736s | 41.44MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/dll2n_insert_unequal.i_7.smt2 |    1.952s | 36.248MiB| sat | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/skipped.c_AllErrorsAtOnce_Iteration5_0.smt2 |    1.988s | 29.664MiB| unsat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/dll2n_insert_unequal.i_8.smt2 |    2.745s | 37.148MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/dll2n_insert_unequal.i_10.smt2 |    7.958s | 50.644MiB| sat | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/tree_max.c_0.smt2 |   20.012s | 37.044MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/prefixsum_rec.c_25.smt2 |   20.016s | 40.632MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/linear_sea.ch.c_AllErrorsAtOnce_Iteration8_0.smt2 |   20.017s | 43.828MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/sorting_selectionsort_ground-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.018s | 47.136MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/dll2n_insert_unequal.i_9.smt2 |   20.020s | 79.72MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/aiob_4.c.v+lh-reducer.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.020s | 65.08MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/array_4.i_AllErrorsAtOnce_Iteration5_0.smt2 |   20.021s | 123.0MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/aiob_4.c.v+cfa-reducer.c_AllErrorsAtOnce_Iteration4_0.smt2 |   20.024s | 69.104MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/flag_loopdep_simple.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.031s | 97.492MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/aiob_3.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.031s | 72.948MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20230321-UltimateAutomizerSvcomp2023/dll2n_insert_unequal.i_6.smt2 |   20.035s | 84.824MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/aiob_2.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.036s | 119.0MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/rew.c_AllErrorsAtOnce_Iteration5_0.smt2 |   20.043s | 43.748MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/array_of_struct_break.i_AllErrorsAtOnce_Iteration5_0.smt2 |   20.044s | 62.468MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/partial_lesser_bound-1.i_AllErrorsAtOnce_Iteration3_0.smt2 |   20.045s | 76.416MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/rewnifrev2.c_AllErrorsAtOnce_Iteration5_0.smt2 |   20.045s | 301.0MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/nr2.c_AllErrorsAtOnce_Iteration8_0.smt2 |   20.054s | 191.0MiB| timeout | 0 |  |  |
|non-incremental/ANIA/20240413-AutomizerLoopAcceleration/aiob_4.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.088s | 429.0MiB| timeout | 0 |  |  |
