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
Job tag: pre-derive-https-zenodo.org-records-16740866-files-QF_LIRA.tar.
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6fd303c4b987e1df8d13fe01243edceb701f9a97
Z3 branch: 6fd303c4b987e1df8d13fe01243edceb701f9a97
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_LIRA.tar.zst?download=1
Z3 commit message: set the auf flag to false in all cases

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_LIRA/LCTES/cruise-control.smt2            |    0.044s | 22.092MiB| unsat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/cruise-control.nosummaries.smt2 |    0.075s | 21.74MiB| unsat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/smtopt.smt2                    |    0.802s | 232.0MiB| sat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/tdf.locals.smt2                |    1.082s | 232.0MiB| unsat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/fly-by-wire.locals.smt2        |    2.710s | 71.356MiB| unsat | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/tdf.locals.nosummaries.smt2    |   20.031s | 232.0MiB| timeout | 0 |  |  |
|non-incremental/QF_LIRA/LCTES/fly-by-wire.locals.nosummaries.smt2 |   20.046s | 453.0MiB| timeout | 0 |  |  |
