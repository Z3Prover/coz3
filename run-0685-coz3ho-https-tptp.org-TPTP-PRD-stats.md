# .

* SAT 0
* UNSAT 0
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

* SZS (TPTP) 3 (Success: 1, NoSuccess: 2) (Timeout:2, Theorem:1)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/PRD | Source list: benchmarks-tptp.txt
Job tag: coz3ho-https-tptp.org-TPTP-PRD
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 7040a74d1a55b3472ab10930d89aa350f039d06e
Z3 branch: master
Z3 options: "-T:20 model_validate=true smt.ho_matching=true"
Z3 inputs: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/PRD
Z3 commit message: defer ho-matching to lazy mam

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|PRD001+1.p                                                   |    0.354s | 75.764MiB| Theorem | 0 |  |  |
|PRD003+1.p                                                   |   20.037s | 280.0MiB| Timeout | 0 |  |  |
|PRD002+1.p                                                   |   20.050s | 283.0MiB| Timeout | 0 |  |  |
