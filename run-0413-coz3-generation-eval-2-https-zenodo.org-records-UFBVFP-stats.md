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
Job tag: coz3-generation-eval-2-https-zenodo.org-records-16740866-files-
Runner: rise-runner-1
Z3 repo: CanCebeci/z3
Z3 commit: 45940fec5efc590f50c1ea2786dfb989655405aa
Z3 branch: tune-mam-kiss
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/UFBVFP.tar.zst?download=1
Z3 commit message: Fix build errors

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/UFBVFP/20210301-Alive2/oggenc/439_oggenc.smt2 |    0.153s | 28.348MiB| unsat | 0 |  |  |
|non-incremental/UFBVFP/20210301-Alive2-partial-undef/gzip/333_gzip.smt2 |   20.042s | 406.0MiB| timeout | 0 |  |  |
