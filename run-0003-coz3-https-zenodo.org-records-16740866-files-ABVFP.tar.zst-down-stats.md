# .

* SAT 30
* UNSAT 2
* TIMEOUT 9
* UNKNOWN 19

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/ABVFP.tar.zst?download=1 | Source list: benchmarks-q.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-ABVFP.tar.zst-down
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 500ca46434627ddd2a2b2777edd5160dbbcff211
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/ABVFP.tar.zst?download=1
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
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0530b_true-unreach-call.c_2.smt2 |    0.023s | 20.124MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_1.smt2 |    0.023s | 20.128MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_15.smt2 |    0.024s | 21.048MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0620b_true-unreach-call.c_6.smt2 |    0.026s | 20.364MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0663b.c_7.smt2 |    0.026s | 21.128MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_16.smt2 |    0.026s | 20.872MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_6.smt2 |    0.026s | 21.136MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_2.smt2 |    0.027s | 20.904MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/list_search-2.i_34.smt2 |    0.027s | 20.924MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0682a_true-unreach-call.c_13.smt2 |    0.028s | 20.916MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/test_mutex_unbounded-1.i_53.smt2 |    0.030s | 21.936MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/test_mutex_unbounded-2.i_63.smt2 |    0.034s | 22.92MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_0.smt2 |    0.039s | 22.612MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_4.smt2 |    0.041s | 20.356MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0620a_true-unreach-call.c_1.smt2 |    0.041s | 20.356MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_2.smt2 |    0.042s | 22.992MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0530b_true-unreach-call.c_5.smt2 |    0.047s | 20.36MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_1210_false-unreach-call.c_0.smt2 |    0.047s | 20.928MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_7.smt2 |    0.047s | 20.636MiB| unsat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0530b_true-unreach-call.c_8.smt2 |    0.047s | 20.872MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_14.smt2 |    0.047s | 20.872MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_4.smt2 |    0.048s | 21.376MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_13.smt2 |    0.048s | 20.908MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0530a_true-unreach-call.c_4.smt2 |    0.048s | 20.64MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0620a_true-unreach-call.c_3.smt2 |    0.048s | 20.396MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_3.smt2 |    0.048s | 20.628MiB| unsat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_0.smt2 |    0.048s | 20.656MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/sll2n_append_unequal.i_49.smt2 |    0.048s | 20.912MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0663b.c_8.smt2 |    0.048s | 21.192MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_1.smt2 |    0.049s | 20.9MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_17.smt2 |    0.049s | 20.9MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_4.smt2 |    0.050s | 21.592MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0530b_true-unreach-call.c_7.smt2 |    0.050s | 21.292MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_3.smt2 |    0.050s | 21.508MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_10.smt2 |    0.051s | 23.968MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/test_mutex_unbounded-2.i_64.smt2 |    0.053s | 21.896MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_4.smt2 |    0.057s | 21.132MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_1.smt2 |    0.061s | 22.24MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0663b.c_9.smt2 |    0.064s | 21.672MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/test_mutex_unbounded-1.i_52.smt2 |    0.064s | 22.668MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_8.smt2 |    0.089s | 24.564MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float_req_bl_1130a.c_1.smt2 |    0.144s | 23.452MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_6.smt2 |    0.155s | 25.232MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_9.smt2 |    0.156s | 25.08MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0670_true-unreach-call.c_5.smt2 |    0.272s | 28.044MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0670_true-unreach-call.c_8.smt2 |    0.373s | 28.24MiB| sat | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_12.smt2 |    0.495s | 35.04MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/double_req_bl_1300.c_2.smt2 |    2.779s | 49.512MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/double_req_bl_1300.c_3.smt2 |    3.260s | 49.508MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/double_req_bl_1300.c_1.smt2 |    4.072s | 50.872MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/double_req_bl_1300.c_4.smt2 |    4.449s | 52.72MiB| unknown | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_10.smt2 |   20.013s | 47.46MiB| timeout | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_13.smt2 |   20.013s | 47.092MiB| timeout | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_12.smt2 |   20.029s | 47.476MiB| timeout | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float_req_bl_1130a.c_0.smt2 |   20.029s | 63.596MiB| timeout | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_11.smt2 |   20.033s | 98.94MiB| timeout | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_5.smt2 |   20.038s | 62.136MiB| timeout | 0 |  |  |
|non-incremental/ABVFP/20190429-UltimateAutomizerSvcomp2019/float22_true-unreach-call_true-termination.i_5.smt2 |   20.042s | 178.0MiB| timeout | 0 |  |  |
|non-incremental/ABVFP/20230321-UltimateAutomizerSvcomp2023/float22.i_11.smt2 |   20.044s | 47.608MiB| timeout | 0 |  |  |
|non-incremental/ABVFP/20170501-Heizmann-UltimateAutomizer/filter_iir_true-unreach-call.c_35.smt2 |   20.049s | 146.0MiB| timeout | 0 |  |  |
