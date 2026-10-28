# .

* SAT 0
* UNSAT 28
* TIMEOUT 3
* UNKNOWN 0

* ERRORS 3 (error-1:3)

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-28 17:56:32 UTC
Job description: Pinned full public LPAR cohort; triangular; pr-11415; 30s solver and Ramon limits; four independent solver workers; not the paper's filtered cohort or four-core portfolio
Job tag: lpar26-20261028-30s-r3-pr-11415-triangular
Runner: rise-runner-1
Z3 repo: Z3Prover/z3
Z3 commit: 2a48201a59c3ac36546b1fc02b4681e38d96e745
Z3 branch: improve/arithmetic-witness-instantiation
Z3 options: "-T:30 -st"
Ramon options: "--limit-time 30"
Input preset: alasca-lpar-2026
Z3 inputs: https://github.com/vprover/vampire_benchmarks/archive/6563baa75aea885cee14db5d6dfcff1cf3b50a1c.tar.gz alasca-lpar-2026/triangular
Z3 commit message: Add F* NFV audit for #11415 at 3082d1f94779
Report correction: regenerated from original raw logs with Z3Prover/bench@1250328d4; explicit Ramon time limits count as timeouts.
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|triangular/triangular02.smt2                                 |    0.013s | 20.04MiB| unsat | 0 |  |
|limit/lim_square.smt2                                        |    0.014s | 20.796MiB| error-1 | 1 | line 40 column 1: logic does not support nonlinear arithmetic |
|triangular/triangular04.smt2                                 |    0.015s | 21.06MiB| unsat | 0 |  |
|limit/lim_sub.smt2                                           |    0.016s | 20.304MiB| unsat | 0 |  |
|limit/lim_sum.smt2                                           |    0.016s | 20.544MiB| unsat | 0 |  |
|limit/limit-composite-continous.smt2                         |    0.016s | 20.552MiB| unsat | 0 |  |
|triangular/triangular11.smt2                                 |    0.016s | 20.816MiB| unsat | 0 |  |
|triangular/triangular05.smt2                                 |    0.016s | 21.024MiB| unsat | 0 |  |
|triangular/triangular08.smt2                                 |    0.016s | 20.784MiB| unsat | 0 |  |
|triangular/triangular09.smt2                                 |    0.018s | 21.056MiB| unsat | 0 |  |
|triangular/triangular07.smt2                                 |    0.018s | 21.06MiB| unsat | 0 |  |
|motivating-example/example10.smt2                            |    0.019s | 20.184MiB| unsat | 0 |  |
|motivating-example/example5.smt2                             |    0.021s | 20.296MiB| unsat | 0 |  |
|motivating-example/example8.smt2                             |    0.022s | 20.292MiB| unsat | 0 |  |
|motivating-example/example1.smt2                             |    0.026s | 20.276MiB| unsat | 0 |  |
|motivating-example/example11.smt2                            |    0.026s | 20.04MiB| unsat | 0 |  |
|limit/lim_k_fx.smt2                                          |    0.026s | 20.556MiB| unsat | 0 |  |
|motivating-example/example7.smt2                             |    0.027s | 20.316MiB| unsat | 0 |  |
|limit/lim_div.smt2                                           |    0.027s | 21.0MiB| error-1 | 1 | line 64 column 1: logic does not support nonlinear arithmetic |
|motivating-example/example4.smt2                             |    0.028s | 20.044MiB| unsat | 0 |  |
|limit/lim_x.smt2                                             |    0.029s | 20.816MiB| unsat | 0 |  |
|limit/lim_product.smt2                                       |    0.029s | 20.804MiB| error-1 | 1 | line 83 column 1: logic does not support nonlinear arithmetic |
|motivating-example/example3.smt2                             |    0.030s | 20.292MiB| unsat | 0 |  |
|motivating-example/example2.smt2                             |    0.031s | 20.292MiB| unsat | 0 |  |
|motivating-example/example9.smt2                             |    0.031s | 20.296MiB| unsat | 0 |  |
|motivating-example/example12.smt2                            |    0.031s | 20.288MiB| unsat | 0 |  |
|triangular/triangular10.smt2                                 |    0.032s | 20.812MiB| unsat | 0 |  |
|triangular/triangular03.smt2                                 |    0.032s | 20.04MiB| unsat | 0 |  |
|limit/lim_c.smt2                                             |    0.033s | 21.064MiB| unsat | 0 |  |
|triangular/triangular06.smt2                                 |    0.033s | 21.104MiB| unsat | 0 |  |
|triangular/triangular01.smt2                                 |    0.041s | 19.792MiB| unsat | 0 |  |
|limit/lim_fgx.smt2                                           |   30.006s | 65.552MiB| timeout | 143 | Ramon wall-clock time limit reached |
|limit/lim_fxy.smt2                                           |   30.013s | 199.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|limit/lim_f_greater_lim_greater.smt2                         |   30.015s | 207.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
