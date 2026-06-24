# .

* SAT 3
* UNSAT 6
* TIMEOUT 48
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/AUFBVFP.tar.zst?download=1 | Source list: benchmarks-q.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-AUFBVFP.tar.zst-do
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 500ca46434627ddd2a2b2777edd5160dbbcff211
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/AUFBVFP.tar.zst?download=1
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
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/ph7/583_ph7.smt2 |    0.364s | 115.0MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/340_oggenc.smt2 |    0.368s | 36.336MiB| sat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/392_oggenc.smt2 |    1.241s | 44.996MiB| sat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/ph7/611_ph7.smt2     |    1.284s | 42.188MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/344_oggenc.smt2 |    2.317s | 47.264MiB| sat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/290_oggenc.smt2 |    2.374s | 338.0MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/455_oggenc.smt2 |    3.171s | 329.0MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/397_oggenc.smt2 |    6.143s | 60.728MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/sqlite3/816_sqlite3.smt2 |    7.597s | 67.168MiB| unsat | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/384_oggenc.smt2 |   20.021s | 111.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/351_oggenc.smt2 |   20.022s | 36.04MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/411_oggenc.smt2 |   20.030s | 47.732MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/434_oggenc.smt2 |   20.034s | 222.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/392_oggenc.smt2 |   20.046s | 120.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/ph7/568_ph7.smt2     |   20.047s | 89.208MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/ph7/757_ph7.smt2     |   20.055s | 107.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/374_oggenc.smt2 |   20.064s | 114.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/ph7/744_ph7.smt2 |   20.073s | 94.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/357_oggenc.smt2 |   20.073s | 119.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/322_oggenc.smt2 |   20.079s | 143.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/sqlite3/892_sqlite3.smt2 |   20.083s | 101.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/410_oggenc.smt2 |   20.087s | 221.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/bzip2/001_bzip2.smt2 |   20.092s | 130.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/277_oggenc.smt2 |   20.095s | 245.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/354_oggenc.smt2 |   20.102s | 241.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/295_oggenc.smt2 |   20.109s | 272.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/422_oggenc.smt2 |   20.110s | 167.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/324_oggenc.smt2 |   20.113s | 270.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/352_oggenc.smt2 |   20.116s | 252.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/445_oggenc.smt2 |   20.117s | 195.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/426_oggenc.smt2 |   20.118s | 339.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/356_oggenc.smt2 |   20.137s | 319.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/sqlite3/917_sqlite3.smt2 |   20.138s | 412.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/300_oggenc.smt2 |   20.144s | 479.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/449_oggenc.smt2 |   20.145s | 278.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/342_oggenc.smt2 |   20.151s | 272.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/293_oggenc.smt2 |   20.157s | 466.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/368_oggenc.smt2 |   20.160s | 460.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/288_oggenc.smt2 |   20.161s | 354.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/305_oggenc.smt2 |   20.170s | 458.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/343_oggenc.smt2 |   20.172s | 571.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/369_oggenc.smt2 |   20.190s | 555.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/303_oggenc.smt2 |   20.193s | 667.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/386_oggenc.smt2 |   20.193s | 664.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/459_oggenc.smt2 |   20.195s | 550.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/358_oggenc.smt2 |   20.214s | 808.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/409_oggenc.smt2 |   20.215s | 680.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/446_oggenc.smt2 |   20.262s | 1369.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/430_oggenc.smt2 |   20.485s | 3366.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/sqlite3/870_sqlite3.smt2 |   20.499s | 3276.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/453_oggenc.smt2 |   20.615s | 4307.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/461_oggenc.smt2 |   20.726s | 5338.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/385_oggenc.smt2 |   20.804s | 6065.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/346_oggenc.smt2 |   20.961s | 6895.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/oggenc/344_oggenc.smt2 |   21.045s | 7194.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2-partial-undef/ph7/489_ph7.smt2 |   21.058s | 8262.0MiB| timeout | 0 |  |  |
|non-incremental/AUFBVFP/20210301-Alive2/oggenc/457_oggenc.smt2 |   21.125s | 7895.0MiB| timeout | 0 |  |  |
