# .

* SAT 20
* UNSAT 0
* TIMEOUT 1
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-16 14:09:25 UTC
Job description: PR11126 diagnostic: pinned master only on no_init_simple_delete35.smt2; sequential default control plus paired smt.random_seed and sat.random_seed values 0-19; original benchmark bytes retained after seed prefixes
Job tag: pr11126-master-seeds-0-19
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: inputs/pr11126-master-seeds
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
|000-control.smt2                                             |    0.393s | 60.888MiB| sat | 0 |  |
|seed-000.smt2                                                |    0.396s | 60.892MiB| sat | 0 |  |
|seed-014.smt2                                                |    1.804s | 62.036MiB| sat | 0 |  |
|seed-015.smt2                                                |    1.896s | 62.032MiB| sat | 0 |  |
|seed-013.smt2                                                |    1.972s | 61.144MiB| sat | 0 |  |
|seed-011.smt2                                                |    2.156s | 61.512MiB| sat | 0 |  |
|seed-001.smt2                                                |    2.883s | 62.808MiB| sat | 0 |  |
|seed-005.smt2                                                |    3.255s | 62.544MiB| sat | 0 |  |
|seed-009.smt2                                                |    3.961s | 63.052MiB| sat | 0 |  |
|seed-007.smt2                                                |    4.640s | 64.592MiB| sat | 0 |  |
|seed-008.smt2                                                |    6.033s | 64.332MiB| sat | 0 |  |
|seed-003.smt2                                                |    9.114s | 62.288MiB| sat | 0 |  |
|seed-019.smt2                                                |    9.279s | 62.28MiB| sat | 0 |  |
|seed-016.smt2                                                |   10.505s | 69.7MiB| sat | 0 |  |
|seed-018.smt2                                                |   10.530s | 61.532MiB| sat | 0 |  |
|seed-012.smt2                                                |   10.734s | 63.56MiB| sat | 0 |  |
|seed-002.smt2                                                |   10.794s | 65.608MiB| sat | 0 |  |
|seed-017.smt2                                                |   12.803s | 77.808MiB| sat | 0 |  |
|seed-010.smt2                                                |   13.306s | 64.324MiB| sat | 0 |  |
|seed-006.smt2                                                |   17.914s | 83.696MiB| sat | 0 |  |
|seed-004.smt2                                                |   20.032s | 80.632MiB| timeout | 0 |  |
