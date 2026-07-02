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
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_NIRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-gen-https-zenodo.org-records-QF_NIRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: e7e686127bbc65834c09fa85686fbe8e1c497adf
Z3 branch: generation
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_NIRA.tar.zst?download=1
Z3 commit message: fix signature regression for unit test

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_NIRA/UltimateAutomizer/test_union_cast-1_true-unreach-call.i.smt2 |    0.038s | 20.856MiB| unsat | 0 |  |  |
|non-incremental/QF_NIRA/LCTES/miniflight.smt2                |    3.262s | 123.0MiB| unsat | 0 |  |  |
|non-incremental/QF_NIRA/LCTES/miniflight.nosummaries.smt2    |   20.047s | 454.0MiB| timeout | 0 |  |  |
