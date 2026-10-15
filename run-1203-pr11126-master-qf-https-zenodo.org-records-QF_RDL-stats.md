# .

* SAT 94
* UNSAT 98
* TIMEOUT 63
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-15 01:17:30 UTC
Job description: PR11126 full QF baseline: pinned master f05a5c651802; paired smoke integrity gate passed | Benchmark suite: https://zenodo.org/records/16740866/files/QF_RDL.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: pr11126-master-qf-https-zenodo.org-records-QF_RDL
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_RDL.tar.zst?download=1
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
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-1.smt2 |    0.031s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking03.smt2 |    0.032s | 20.864MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-1.smt2             |    0.035s | 20.616MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-1.smt2             |    0.035s | 21.132MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-3.smt2             |    0.036s | 20.616MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-5.smt2             |    0.037s | 21.368MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-04.smt2 |    0.037s | 21.376MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-03.smt2 |    0.037s | 21.38MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-2.smt2             |    0.038s | 20.856MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-3.smt2             |    0.038s | 21.38MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-4.smt2             |    0.039s | 21.12MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-2.smt2 |    0.039s | 21.328MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-2.smt2             |    0.040s | 21.624MiB| unsat | 0 |  |
|non-incremental/QF_RDL/check/bignum_rdl1.smt2                |    0.040s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-matrix1x1.pddl.smt2 |    0.040s | 20.86MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-1.smt2             |    0.043s | 20.356MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking02.smt2 |    0.043s | 20.864MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-2.smt2             |    0.045s | 20.864MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-3.smt2             |    0.046s | 21.724MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_250.smt2             |    0.046s | 22.648MiB| unsat | 0 |  |
|non-incremental/QF_RDL/check/bignum_rdl2.smt2                |    0.046s | 20.604MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-4.smt2             |    0.047s | 22.244MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-6.smt2             |    0.047s | 21.38MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking01.smt2 |    0.047s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking04.smt2 |    0.051s | 21.364MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-4.smt2             |    0.053s | 21.468MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_800.smt2              |    0.053s | 23.648MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking05.smt2 |    0.053s | 21.376MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-7.smt2             |    0.054s | 21.712MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-2.smt2 |    0.054s | 21.368MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-8.smt2             |    0.055s | 21.956MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_330.smt2             |    0.055s | 23.668MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_700.smt2             |    0.055s | 23.164MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-5.smt2             |    0.056s | 21.988MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-09.smt2 |    0.057s | 22.496MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-06.smt2 |    0.058s | 22.14MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_700.smt2             |    0.059s | 23.644MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-07.smt2 |    0.061s | 22.484MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-5.smt2             |    0.064s | 22.496MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-08.smt2 |    0.066s | 22.236MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking08.smt2 |    0.066s | 22.012MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking14.smt2 |    0.067s | 22.98MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking12.smt2 |    0.068s | 22.916MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking07.smt2 |    0.068s | 22.14MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-6.smt2             |    0.069s | 22.244MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1000.smt2             |    0.069s | 23.896MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking06.smt2 |    0.069s | 21.632MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking09.smt2 |    0.070s | 22.144MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking10.smt2 |    0.072s | 22.568MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking13.smt2 |    0.072s | 22.728MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-6.smt2             |    0.073s | 23.344MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-05.smt2 |    0.073s | 21.868MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-10.smt2 |    0.076s | 22.748MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking16.smt2 |    0.076s | 23.268MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_850.smt2             |    0.080s | 23.944MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-7.smt2             |    0.083s | 22.744MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking11.smt2 |    0.085s | 22.476MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-9.smt2             |    0.086s | 21.956MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking17.smt2 |    0.088s | 23.268MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking15.smt2 |    0.092s | 23.3MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking21.smt2 |    0.096s | 24.196MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_800.smt2             |    0.097s | 23.944MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking22.smt2 |    0.097s | 24.244MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_700.smt2             |    0.099s | 24.192MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-10.smt2            |    0.102s | 21.952MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking20.smt2 |    0.102s | 23.916MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1400.smt2             |    0.104s | 24.652MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_800.smt2             |    0.105s | 24.192MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-3.smt2 |    0.106s | 23.96MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking18.smt2 |    0.108s | 23.344MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/cooking19.smt2 |    0.110s | 23.932MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_800.smt2             |    0.116s | 24.444MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-8.smt2             |    0.128s | 22.776MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_1100.smt2             |    0.141s | 24.664MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-11.smt2            |    0.154s | 22.772MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_550.smt2             |    0.160s | 25.484MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_1000.smt2            |    0.168s | 25.232MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-20.smt2 |    0.169s | 24.62MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-7.smt2             |    0.176s | 23.632MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-5.smt2              |    0.181s | 34.956MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-12.smt2            |    0.188s | 22.768MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_1000.smt2            |    0.202s | 25.176MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-8.smt2             |    0.229s | 24.22MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-13.smt2            |    0.245s | 23.188MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_1100.smt2            |    0.252s | 25.224MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_900.smt2             |    0.253s | 24.448MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_1000.smt2             |    0.268s | 25.172MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-5.base.cvc.smt2    |    0.279s | 50.312MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-9.smt2             |    0.279s | 24.136MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_900.smt2              |    0.282s | 24.92MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_800.smt2             |    0.299s | 24.916MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_1200.smt2            |    0.319s | 25.476MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-14.smt2            |    0.322s | 23.464MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_850.smt2             |    0.343s | 24.452MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-15.smt2            |    0.348s | 23.836MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-6.base.cvc.smt2    |    0.380s | 56.568MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_950.smt2             |    0.393s | 25.22MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_830.smt2             |    0.404s | 24.704MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-30.smt2 |    0.411s | 27.328MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1300.smt2             |    0.446s | 25.18MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-16.smt2            |    0.485s | 23.96MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-5.induction.cvc.smt2 |    0.526s | 67.328MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_900.smt2             |    0.540s | 25.472MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-7.base.cvc.smt2    |    0.542s | 60.696MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_900.smt2             |    0.549s | 25.208MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-17.smt2            |    0.558s | 23.968MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-3.smt2 |    0.587s | 24.72MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-40.smt2 |    0.610s | 28.472MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-18.smt2            |    0.612s | 25.012MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz6_943.smt2              |    0.617s | 25.168MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_1200.smt2            |    0.642s | 25.992MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-10.smt2            |    0.658s | 24.648MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_1100.smt2            |    0.674s | 26.252MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-10.smt2             |    0.687s | 60.264MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_1100.smt2            |    0.709s | 25.48MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-11.smt2            |    0.735s | 25.604MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-19.smt2            |    0.755s | 25.008MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-9.smt2             |    0.783s | 25.676MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-9.base.cvc.smt2    |    0.789s | 88.388MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-7.induction.cvc.smt2 |    0.791s | 86.24MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-12.smt2            |    0.804s | 25.136MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-6.induction.cvc.smt2 |    0.805s | 84.188MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-8.base.cvc.smt2    |    0.837s | 84.24MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_750.smt2               |    0.880s | 65.724MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_750.smt2               |    0.960s | 66.748MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_430.smt2             |    0.965s | 25.736MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer3-mutex-20.smt2            |    0.976s | 25.792MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_1000.smt2            |    0.994s | 25.996MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-13.smt2            |    1.082s | 26.116MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_1200.smt2            |    1.092s | 26.248MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_1000.smt2            |    1.145s | 25.996MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_1200.smt2            |    1.147s | 26.248MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-9.induction.cvc.smt2 |    1.158s | 129.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_900.smt2             |    1.216s | 25.72MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-8.induction.cvc.smt2 |    1.285s | 107.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_750.smt2               |    1.307s | 65.984MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-4.smt2 |    1.382s | 28.088MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-10.induction.cvc.smt2 |    1.614s | 128.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-10.base.cvc.smt2   |    1.716s | 98.568MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_500.smt2              |    1.833s | 49.816MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_1100.smt2            |    1.961s | 26.468MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-10.smt2            |    2.065s | 27.724MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-11.smt2            |    2.197s | 27.632MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-11.induction.cvc.smt2 |    2.270s | 156.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_900.smt2             |    2.316s | 25.988MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-13.induction.cvc.smt2 |    2.391s | 170.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-12.induction.cvc.smt2 |    2.424s | 163.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-60.smt2 |    2.481s | 49.26MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb09_934.smt2             |    2.541s | 26.252MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb02_888.smt2             |    2.546s | 26.492MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1200.smt2             |    2.593s | 26.328MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-14.smt2            |    2.634s | 27.664MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-14.induction.cvc.smt2 |    2.853s | 179.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-16.smt2            |    3.025s | 27.6MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-11.base.cvc.smt2   |    3.112s | 105.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-16.induction.cvc.smt2 |    3.232s | 239.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_1000.smt2            |    3.531s | 26.252MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_1100.smt2            |    3.539s | 26.916MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-80.smt2 |    3.655s | 45.352MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-15.induction.cvc.smt2 |    3.657s | 227.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb07_397.smt2             |    3.907s | 26.508MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-18.induction.cvc.smt2 |    4.222s | 262.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_900.smt2             |    4.320s | 27.228MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-15.smt2            |    4.375s | 28.756MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-12.smt2            |    4.611s | 28.996MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb10_944.smt2             |    4.759s | 27.012MiB| sat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-70.smt2 |    4.813s | 43.82MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-18.smt2            |    4.859s | 29.868MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb04_1005.smt2            |    4.863s | 27.264MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-12.base.cvc.smt2   |    4.950s | 111.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-17.induction.cvc.smt2 |    4.967s | 267.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_950.smt2             |    5.296s | 27.388MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_930.smt2             |    5.377s | 27.272MiB| sat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-19.induction.cvc.smt2 |    5.455s | 294.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-17.smt2            |    5.492s | 29.86MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-15.smt2             |    5.898s | 98.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-13.base.cvc.smt2   |    5.914s | 117.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_1100.smt2            |    6.256s | 27.776MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb08_888.smt2             |    7.303s | 27.752MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-20.induction.cvc.smt2 |    7.798s | 345.0MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-19.smt2            |    7.969s | 30.852MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-14.base.cvc.smt2   |    8.438s | 127.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-14.smt2            |    8.442s | 31.048MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tms-2-3-light-90.smt2 |    8.485s | 47.656MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb05_887.smt2             |    9.337s | 28.128MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/abz5_1234.smt2             |    9.601s | 28.504MiB| sat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-13.smt2            |   10.785s | 32.504MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer6-mutex-20.smt2            |   10.850s | 31.496MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-15.base.cvc.smt2   |   13.507s | 140.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-16.base.cvc.smt2   |   14.932s | 179.0MiB| unsat | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-16.smt2            |   18.035s | 36.6MiB| unsat | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-5.smt2 |   18.163s | 48.028MiB| sat | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_1000.smt2            |   20.022s | 30.568MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-19.smt2            |   20.024s | 37.152MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-18.smt2            |   20.024s | 38.416MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_700.smt2              |   20.024s | 74.912MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-15.smt2            |   20.025s | 34.976MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/orb03_1005.smt2            |   20.025s | 29.832MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-17.smt2            |   20.026s | 37.152MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_670.smt2              |   20.026s | 74.148MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_691.smt2              |   20.026s | 75.42MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_910.smt2               |   20.027s | 91.068MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_1059.smt2            |   20.027s | 29.832MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-4.smt2 |   20.027s | 46.912MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_860.smt2               |   20.028s | 88.26MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_600.smt2              |   20.029s | 67.996MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/orb01_1000.smt2            |   20.029s | 29.316MiB| timeout | 0 |  |
|non-incremental/QF_RDL/sal/fischer9-mutex-20.smt2            |   20.030s | 37.4MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/orb06_1010.smt2            |   20.030s | 30.084MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_862.smt2               |   20.030s | 86.98MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_969.smt2               |   20.031s | 94.652MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_827.smt2               |   20.032s | 82.628MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_1000.smt2              |   20.032s | 96.452MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_828.smt2               |   20.032s | 82.624MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_890.smt2               |   20.033s | 91.324MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_919.smt2               |   20.034s | 88.508MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-6.smt2 |   20.034s | 56.64MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa/skdmxa-3x3-20.smt2             |   20.035s | 133.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_894.smt2               |   20.035s | 89.788MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-5.smt2 |   20.035s | 80.7MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_850.smt2               |   20.036s | 80.324MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_887.smt2               |   20.037s | 88.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn4_950.smt2               |   20.038s | 92.092MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn2_950.smt2               |   20.038s | 95.168MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_850.smt2               |   20.040s | 84.412MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-matrix2x2.pddl.smt2 |   20.040s | 130.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_667.smt2              |   20.041s | 73.636MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-20.base.cvc.smt2   |   20.042s | 230.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn3_950.smt2               |   20.042s | 93.372MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/yn1_950.smt2               |   20.042s | 93.372MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-17.base.cvc.smt2   |   20.043s | 197.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_2988.smt2            |   20.044s | 245.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_2885.smt2            |   20.045s | 224.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-depth-7.smt2 |   20.045s | 88.716MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_2900.smt2            |   20.046s | 226.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/abz7_800.smt2              |   20.046s | 78.248MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_2972.smt2            |   20.046s | 237.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_2900.smt2            |   20.046s | 224.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/SMT-Temporal-Planning-Benchmarks/tempo-width-6.smt2 |   20.046s | 143.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-19.base.cvc.smt2   |   20.047s | 225.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv13_3200.smt2            |   20.047s | 246.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv13_3104.smt2            |   20.047s | 226.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_3050.smt2            |   20.048s | 227.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_2992.smt2            |   20.049s | 236.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_2800.smt2            |   20.049s | 226.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv11_2983.smt2            |   20.049s | 228.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_3050.smt2            |   20.050s | 239.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_3004.smt2            |   20.050s | 231.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv13_3000.smt2            |   20.050s | 236.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv13_3150.smt2            |   20.050s | 243.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/skdmxa2/skdmxa-3x3-18.base.cvc.smt2   |   20.051s | 196.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_2905.smt2            |   20.052s | 233.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_2895.smt2            |   20.052s | 231.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv12_2990.smt2            |   20.058s | 239.0MiB| timeout | 0 |  |
|non-incremental/QF_RDL/scheduling/swv14_3000.smt2            |   20.058s | 230.0MiB| timeout | 0 |  |
