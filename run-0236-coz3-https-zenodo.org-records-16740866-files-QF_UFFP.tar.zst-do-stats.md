# .

* SAT 0
* UNSAT 2
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFFP.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFFP.tar.zst-do
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: ef66acc6b54144f10d6f64e9b557142ff438a31a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFFP.tar.zst?download=1
Z3 commit message: change calculation of threads to use total threads indicated by parameter or processor count, subtract from worker threads based on backbone and core threads

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_UFFP/schanda/spark/O402-020_1.smt2        |    0.590s | 56.992MiB| unsat | 0 |  |  |
|non-incremental/QF_UFFP/schanda/spark/O402-020_2.smt2        |   11.480s | 61.936MiB| unsat | 0 |  |  |
