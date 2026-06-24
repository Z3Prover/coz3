# .

* SAT 59
* UNSAT 4
* TIMEOUT 0
* UNKNOWN 14

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/ABVFPLRA.tar.zst?download=1 | Source list: benchmarks-q.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-ABVFPLRA.tar.zst-d
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 500ca46434627ddd2a2b2777edd5160dbbcff211
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/ABVFPLRA.tar.zst?download=1
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
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_10.smt2 |    0.024s | 20.86MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_16.smt2 |    0.024s | 20.92MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_1032d_true-unreach-call.c_0.smt2 |    0.025s | 20.364MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_12.smt2 |    0.025s | 20.876MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_17.smt2 |    0.026s | 21.124MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0880_true-unreach-call.c_18.smt2 |    0.026s | 21.136MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0876_true-unreach-call.c_24.smt2 |    0.026s | 20.84MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0875_true-unreach-call.c_29.smt2 |    0.028s | 21.136MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_8.smt2 |    0.028s | 20.768MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_13.smt2 |    0.029s | 20.928MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_14.smt2 |    0.029s | 20.828MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0832_true-unreach-call.c_4.smt2 |    0.030s | 21.54MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_9.smt2 |    0.031s | 20.74MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0880_true-unreach-call.c_15.smt2 |    0.031s | 20.9MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_18.smt2 |    0.032s | 21.144MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0883_true-unreach-call.c_5.smt2 |    0.033s | 20.688MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0876_true-unreach-call.c_21.smt2 |    0.036s | 21.572MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0875_true-unreach-call.c_23.smt2 |    0.036s | 21.612MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0876_true-unreach-call.c_25.smt2 |    0.042s | 21.588MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_8.smt2 |    0.042s | 20.624MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_11.smt2 |    0.048s | 20.928MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_11.smt2 |    0.048s | 20.892MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_7.smt2 |    0.049s | 20.64MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_3.smt2 |    0.049s | 21.14MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0875_true-unreach-call.c_28.smt2 |    0.049s | 21.088MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0832_true-unreach-call.c_9.smt2 |    0.050s | 20.608MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_15.smt2 |    0.050s | 21.404MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_7.smt2 |    0.050s | 20.648MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_9.smt2 |    0.050s | 20.944MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0682a_true-unreach-call.c_1.smt2 |    0.050s | 20.912MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_5.smt2 |    0.051s | 20.58MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_6.smt2 |    0.051s | 20.584MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_7.smt2 |    0.051s | 20.84MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_19.smt2 |    0.051s | 20.928MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_15.smt2 |    0.051s | 21.14MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_15.smt2 |    0.052s | 21.208MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_15.smt2 |    0.052s | 20.62MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_9.smt2 |    0.052s | 21.12MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_4.smt2 |    0.052s | 20.664MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_5.smt2 |    0.053s | 20.876MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0880_true-unreach-call.c_11.smt2 |    0.053s | 21.448MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0877_true-unreach-call.c_12.smt2 |    0.053s | 20.944MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_10.smt2 |    0.053s | 21.108MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0876_true-unreach-call.c_23.smt2 |    0.053s | 20.892MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0877_true-unreach-call.c_13.smt2 |    0.053s | 21.144MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_8.smt2 |    0.053s | 21.948MiB| unsat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_12.smt2 |    0.054s | 20.892MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_10.smt2 |    0.054s | 21.652MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0832_true-unreach-call.c_5.smt2 |    0.054s | 21.248MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0880_true-unreach-call.c_12.smt2 |    0.054s | 21.476MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0875_true-unreach-call.c_27.smt2 |    0.054s | 21.132MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0832_true-unreach-call.c_6.smt2 |    0.055s | 20.768MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_12.smt2 |    0.055s | 21.44MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0880_true-unreach-call.c_13.smt2 |    0.055s | 21.156MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20230321-UltimateAutomizerSvcomp2023/float_req_bl_0683a.c_7.smt2 |    0.055s | 20.632MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0876_true-unreach-call.c_22.smt2 |    0.056s | 21.896MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20230321-UltimateAutomizerSvcomp2023/float_req_bl_0683a.c_5.smt2 |    0.056s | 20.62MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_8.smt2 |    0.057s | 21.656MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0880_true-unreach-call.c_14.smt2 |    0.057s | 21.424MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_18.smt2 |    0.058s | 21.112MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_6.smt2 |    0.058s | 20.696MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0877_true-unreach-call.c_11.smt2 |    0.058s | 21.692MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0875_true-unreach-call.c_22.smt2 |    0.062s | 21.176MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0875_true-unreach-call.c_24.smt2 |    0.062s | 21.66MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_14.smt2 |    0.064s | 21.172MiB| unknown | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_16.smt2 |    0.065s | 23.724MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0875_true-unreach-call.c_21.smt2 |    0.065s | 21.956MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_13.smt2 |    0.069s | 23.844MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_18.smt2 |    0.081s | 24.928MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_17.smt2 |    0.168s | 27.432MiB| unsat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_19.smt2 |    0.215s | 30.856MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621a_true-unreach-call.c_20.smt2 |    0.356s | 31.636MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_13.smt2 |    0.465s | 32.304MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_16.smt2 |    0.474s | 33.856MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0621b_true-unreach-call.c_11.smt2 |    0.684s | 33.836MiB| sat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_14.smt2 |    2.527s | 52.812MiB| unsat | 0 |  |  |
|non-incremental/ABVFPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0684a_true-unreach-call.c_17.smt2 |    4.552s | 126.0MiB| unsat | 0 |  |  |
