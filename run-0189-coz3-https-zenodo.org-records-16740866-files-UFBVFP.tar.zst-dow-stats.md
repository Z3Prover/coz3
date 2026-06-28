# .

* SAT 0
* UNSAT 1
* TIMEOUT 1
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/UFBVFP.tar.zst?download=1 | Source list: benchmarks-q.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-UFBVFP.tar.zst-dow
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 56bb49f8dcfbd1ecbb84e7e6364098361c833404
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/UFBVFP.tar.zst?download=1
Z3 commit message: lp: avoid per-call join allocation in explain_fixed_column (#9984)

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/UFBVFP/20210301-Alive2/oggenc/439_oggenc.smt2 |    0.163s | 29.052MiB| unsat | 0 |  |  |
|non-incremental/UFBVFP/20210301-Alive2-partial-undef/gzip/333_gzip.smt2 |   20.061s | 406.0MiB| timeout | 0 |  |  |
