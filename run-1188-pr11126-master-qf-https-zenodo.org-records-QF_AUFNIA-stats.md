# .

* SAT 5
* UNSAT 12
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-14 05:36:25 UTC
Job description: PR11126 full QF baseline: pinned master f05a5c651802; paired smoke integrity gate passed | Benchmark suite: https://zenodo.org/records/16740866/files/QF_AUFNIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: pr11126-master-qf-https-zenodo.org-records-QF_AUFNIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_AUFNIA.tar.zst?download=1
Z3 commit message: Add Tseitin/De Morgan QF_UF rewrite matchers (demorgan_and, implies_as_or, eq_not)

Adds op_and/op_or/op_implies interpreted Boolean operators and three new
soundness-proved rewrite matchers to Z3PatternMatchedProofChecker.fst,
closing the dominant QF_UF unmatched-leaf shapes found in the Round 2
revisit (De Morgan and-expansion and eq/not rearrangement from Z3's
Tseitin CNF normalization). Mirrored in extract_z3_proof.py.

Re-ran the 100-file QF_UF benchmark at 30s timeout: matched leaves rose
from 82/167 to 143/167 (49% -> 86%), unmatched leaves dropped from 85 to
24, with all 98 extracted proofs still passing end-to-end (no
regressions). Documented in QF_AX_QF_UF_BENCHMARK_REPORT.md's new
'Round 3' section.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.02_false-unreach-call.i.cil.c_71.smt2 |    0.042s | 22.912MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.03_false-unreach-call.i.cil.c_71.smt2 |    0.044s | 22.916MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.07_false-unreach-call.i.cil.c_0.smt2 |    0.046s | 22.912MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.01_false-unreach-call.i.cil.c_57.smt2 |    0.046s | 23.152MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.08_true-unreach-call.i.cil.c_1140.smt2 |    0.066s | 26.084MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.12_true-unreach-call.i.cil.c_0.smt2 |    0.068s | 25.72MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.10_true-unreach-call.i.cil.c_0.smt2 |    0.072s | 24.128MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.15_true-unreach-call.i.cil.c_1173.smt2 |    0.078s | 27.048MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.14_true-unreach-call.i.cil.c_959.smt2 |    0.083s | 27.512MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.16_true-unreach-call.i.cil.c_2036.smt2 |    0.085s | 28.108MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.06_true-unreach-call.i.cil.c_2803.smt2 |    0.097s | 29.196MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.06_true-unreach-call.i.cil.c.smt2 |    0.108s | 30.384MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.02_false-unreach-call.i.cil.c_0.smt2 |    0.469s | 28.812MiB| sat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.04_false-unreach-call.i.cil.c_0.smt2 |    0.493s | 29.008MiB| sat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.01_false-unreach-call.i.cil.c_0.smt2 |    0.656s | 28.932MiB| sat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.11_false-unreach-call.i.cil.c_0.smt2 |    1.803s | 35.38MiB| sat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.01_false-unreach-call.i.cil.c_0.smt2 |   13.613s | 46.652MiB| sat | 0 |  |
