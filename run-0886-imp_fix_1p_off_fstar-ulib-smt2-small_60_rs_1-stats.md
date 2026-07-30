# .

* SAT 0
* UNSAT 0
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 50 (error-1:50)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: fstar-ulib-smt2-small, imp_fix, propagate_fixed_rows=false, -T:60, seed 1
Job tag: imp_fix_1p_off_fstar-ulib-smt2-small_60_rs_1
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 552abc0c032f57e86e040aa42d868ea19a34c7ed
Z3 branch: imp_fix
Z3 options: "-T:60 smt.random_seed=1 smt.arith.nl.propagate_fixed_rows=false"
Z3 inputs: inputs/fstar-ulib-smt2-small
Z3 commit message: nla: record why the two fstar regressions are not worth tuning away

queries-Pulse.Lib.HashTable.Spec-1 and FStar.Matrix-2 cost more with
propagate_fixed_rows on. Neither is a defect in the derivation; both are
second-order interactions with the bounded nlsat escalation in check(),
and they pull in opposite directions.

On HashTable.Spec the escalation is pure waste. All 11 nra calls come from
the should_run_bounded_nlsat() site, and with arith.nl.nra=false the file
is still unsat and the pass is a 15% win: 223336 rlimit on master, 472374
with the pass, 189730 with the pass and no nra. The pass has already
derived what monomial_bounds and add_bounds would have, so no_effect()
holds while monomials still need refining, and m_nlsat_delay starts at 0,
so nlsat runs at the first opportunity. On Matrix-2 the escalation is
load-bearing instead: with arith.nl.nra=false that file goes from 765543
to unknown.

Both global levers were measured and both fail the same way, fixing
HashTable.Spec and turning Matrix-2 from unsat into unknown:

  initial m_nlsat_delay   0 (now)      5        10       50    exponential
    HashTable.Spec         472374   557081   189730   189730   429941
    Matrix-2              1603093  unknown  unknown  unknown   unknown

  bounded nlsat rlimit   100000 (now)   25000     5000
    HashTable.Spec             472374   382601   194730
    Matrix-2                  1603213  2307249  unknown

There is no local signal to separate the two cases either: at the
escalation point the bounded runs are equally productive, 6 conflicts and
5 inconclusive on HashTable.Spec against 6 and 9 on Matrix-2. What differs
is whether the rest of the search would have closed the goal anyway, which
is not knowable there.

Comment only, no functional change.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|queries-FStar.ModifiesGen-285.smt2                           |    1.215s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-193.smt2                           |    1.245s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-10.smt2                  |    1.318s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-9.smt2                   |    1.331s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-202.smt2                           |    1.363s | 63.608MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-69.smt2                      |    1.363s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-54.smt2                            |    1.381s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-8.smt2                             |    1.383s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.UInt-264.smt2                                  |    1.384s | 87.028MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-22.smt2                            |    1.408s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-321.smt2                           |    1.412s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-173.smt2                           |    1.414s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-304.smt2                           |    1.428s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-182.smt2                           |    1.501s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.PostProcess-5.smt2          |    1.504s | 165.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-221.smt2                           |    1.507s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-64.smt2                            |    1.548s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-269.smt2                           |    1.607s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-136.smt2                     |    1.637s | 168.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-197.smt2                           |    1.643s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-288.smt2                           |    1.660s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-49.smt2                            |    1.663s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-69.smt2                            |    1.676s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-95.smt2                      |    1.687s | 170.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-139.smt2                     |    1.751s | 192.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-232.smt2                           |    1.778s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-278.smt2                           |    1.837s | 151.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-61.smt2                            |    1.841s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Typeclasses-9.smt2                     |    1.875s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-258.smt2                           |    1.967s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-313.smt2                           |    1.975s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-200.smt2                           |    2.019s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-275.smt2                           |    2.156s | 152.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-264.smt2                           |    2.178s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-17.smt2                            |    2.212s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-44.smt2                            |    2.282s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-188.smt2                           |    2.458s | 44.452MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-165.smt2                           |    2.470s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-159.smt2                           |    2.485s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-266.smt2                           |    2.513s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-66.smt2                            |    2.580s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-224.smt2                           |    2.659s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-228.smt2                           |    2.771s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-234.smt2                           |    3.048s | 57.98MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-52.smt2                            |    3.219s | 38.3MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-239.smt2                           |    3.249s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-155.smt2                           |    3.309s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-280.smt2                           |    3.510s | 159.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-310.smt2                           |    3.589s | 163.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-286.smt2                    |    4.425s | 159.0MiB| error-1 | 1 |  |  |
