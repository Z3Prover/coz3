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
Job description: fstar-ulib-smt2-small, imp_fix, propagate_fixed_rows=true, -T:60, seed 3
Job tag: imp_fix_1p_on_fstar-ulib-smt2-small_60_rs_3
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 552abc0c032f57e86e040aa42d868ea19a34c7ed
Z3 branch: imp_fix
Z3 options: "-T:60 smt.random_seed=3 smt.arith.nl.propagate_fixed_rows=true"
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
|queries-FStar.UInt-264.smt2                                  |    1.192s | 85.54MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-285.smt2                           |    1.251s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-193.smt2                           |    1.294s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-10.smt2                  |    1.323s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-9.smt2                   |    1.346s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-202.smt2                           |    1.360s | 63.272MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-8.smt2                             |    1.361s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-69.smt2                      |    1.363s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-54.smt2                            |    1.374s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-173.smt2                           |    1.376s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-22.smt2                            |    1.401s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-321.smt2                           |    1.406s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-304.smt2                           |    1.462s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.PostProcess-5.smt2          |    1.466s | 165.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-182.smt2                           |    1.472s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-221.smt2                           |    1.490s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-269.smt2                           |    1.556s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-197.smt2                           |    1.582s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-64.smt2                            |    1.592s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-49.smt2                            |    1.625s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-95.smt2                      |    1.628s | 170.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-69.smt2                            |    1.654s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-136.smt2                     |    1.679s | 168.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-288.smt2                           |    1.708s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-139.smt2                     |    1.763s | 192.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-232.smt2                           |    1.824s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Typeclasses-9.smt2                     |    1.825s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-278.smt2                           |    1.836s | 151.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-61.smt2                            |    1.841s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-52.smt2                            |    1.863s | 38.052MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-313.smt2                           |    1.991s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-258.smt2                           |    2.091s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-200.smt2                           |    2.100s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-264.smt2                           |    2.191s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-275.smt2                           |    2.221s | 152.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-44.smt2                            |    2.239s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-17.smt2                            |    2.254s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-188.smt2                           |    2.477s | 44.156MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-165.smt2                           |    2.517s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-66.smt2                            |    2.534s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-266.smt2                           |    2.540s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-159.smt2                           |    2.547s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-224.smt2                           |    2.646s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-234.smt2                           |    2.885s | 58.068MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-228.smt2                           |    2.954s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-280.smt2                           |    3.209s | 159.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-155.smt2                           |    3.251s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-239.smt2                           |    3.300s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-310.smt2                           |    3.530s | 163.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-286.smt2                    |    4.805s | 159.0MiB| error-1 | 1 |  |  |
