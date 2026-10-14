# .

* SAT 0
* UNSAT 2
* TIMEOUT 1
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-14 23:56:07 UTC
Job description: PR11126 full QF baseline: pinned master f05a5c651802; paired smoke integrity gate passed | Benchmark suite: https://zenodo.org/records/16740866/files/QF_NIRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: pr11126-master-qf-https-zenodo.org-records-QF_NIRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_NIRA.tar.zst?download=1
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
|non-incremental/QF_NIRA/UltimateAutomizer/test_union_cast-1_true-unreach-call.i.smt2 |    0.043s | 20.856MiB| unsat | 0 |  |
|non-incremental/QF_NIRA/LCTES/miniflight.smt2                |    2.834s | 131.0MiB| unsat | 0 |  |
|non-incremental/QF_NIRA/LCTES/miniflight.nosummaries.smt2    |   20.064s | 480.0MiB| timeout | 0 |  |
