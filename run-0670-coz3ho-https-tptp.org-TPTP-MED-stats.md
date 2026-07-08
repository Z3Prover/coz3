# .

* SAT 0
* UNSAT 0
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

* SZS (TPTP) 12 (Success: 6, NoSuccess: 6) (Timeout:6, Theorem:5, CounterSatisfiable:1)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/MED | Source list: benchmarks-tptp.txt
Job tag: coz3ho-https-tptp.org-TPTP-MED
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 7040a74d1a55b3472ab10930d89aa350f039d06e
Z3 branch: master
Z3 options: "-T:20 model_validate=true smt.ho_matching=true"
Z3 inputs: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/MED
Z3 commit message: defer ho-matching to lazy mam

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|MED002+1.p                                                   |    0.026s | 20.6MiB| Theorem | 0 |  |  |
|MED003+1.p                                                   |    0.047s | 20.368MiB| Theorem | 0 |  |  |
|MED001+1.p                                                   |    0.048s | 20.864MiB| Theorem | 0 |  |  |
|MED004+1.p                                                   |    0.049s | 21.652MiB| CounterSatisfiable | 0 |  |  |
|MED008+1.p                                                   |    0.053s | 21.66MiB| Theorem | 0 |  |  |
|MED009+1.p                                                   |    0.059s | 22.148MiB| Theorem | 0 |  |  |
|MED006+1.p                                                   |   20.033s | 180.0MiB| Timeout | 0 |  |  |
|MED007+1.p                                                   |   20.052s | 173.0MiB| Timeout | 0 |  |  |
|MED010+1.p                                                   |   20.056s | 197.0MiB| Timeout | 0 |  |  |
|MED005+1.p                                                   |   20.066s | 310.0MiB| Timeout | 0 |  |  |
|MED012+1.p                                                   |   20.068s | 310.0MiB| Timeout | 0 |  |  |
|MED011+1.p                                                   |   20.183s | 2124.0MiB| Timeout | 0 |  |  |
