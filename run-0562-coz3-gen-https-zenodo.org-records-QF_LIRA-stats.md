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
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_LIRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-gen-https-zenodo.org-records-QF_LIRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: e7e686127bbc65834c09fa85686fbe8e1c497adf
Z3 branch: generation
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_LIRA.tar.zst?download=1
Z3 commit message: fix signature regression for unit test

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_LIRA/LCTES/cruise-control.smt2            |    0.048s | 21.996MiB| unsat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/cruise-control.nosummaries.smt2 |    0.081s | 22.464MiB| unsat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/smtopt.smt2                    |    0.779s | 232.0MiB| sat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/fly-by-wire.locals.smt2        |    1.093s | 71.528MiB| unsat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/tdf.locals.smt2                |    1.115s | 232.0MiB| unsat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/tdf.locals.nosummaries.smt2    |   20.035s | 232.0MiB| timeout | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/fly-by-wire.locals.nosummaries.smt2 |   20.070s | 453.0MiB| timeout | 0 |  |  |
