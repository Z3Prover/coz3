# .

* SAT 1
* UNSAT 4
* TIMEOUT 2
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-01 19:33:19 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_LIRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_LIRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 532bff7af54bbe1359bc456c8060fe5fb545093b
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_LIRA.tar.zst?download=1
Z3 commit message: Removed misplaced comment
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_LIRA/LCTES/cruise-control.smt2            |    0.044s | 22.0MiB| unsat | 0 |  |
|non-incremental/QF_LIRA/LCTES/cruise-control.nosummaries.smt2 |    0.070s | 22.008MiB| unsat | 0 |  |
|non-incremental/QF_LIRA/LCTES/smtopt.smt2                    |    0.800s | 242.0MiB| sat | 0 |  |
|non-incremental/QF_LIRA/LCTES/tdf.locals.smt2                |    1.018s | 242.0MiB| unsat | 0 |  |
|non-incremental/QF_LIRA/LCTES/fly-by-wire.locals.smt2        |    1.292s | 70.932MiB| unsat | 0 |  |
|non-incremental/QF_LIRA/LCTES/tdf.locals.nosummaries.smt2    |   20.031s | 242.0MiB| timeout | 0 |  |
|non-incremental/QF_LIRA/LCTES/fly-by-wire.locals.nosummaries.smt2 |   20.047s | 242.0MiB| timeout | 0 |  |
